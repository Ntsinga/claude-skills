---
name: "space-plan-visualizer"
description: "Use when the user describes a physical space (room, office, furniture, fixtures, wiring, layout) with or without photos/video and wants it understood, sketched, planned and shown in 2D and 3D."
---

# Space plan visualizer

Turn a spoken/typed description of a real space (plus any photos or video) into a confirmed understanding, a practical plan, and three outputs every reply: short text, a top-down diagram, and an interactive 3D model.

## Workflow

1. **Read the visual support first** (if any).
   - Images: view directly.
   - Video: extract frames into contact sheets, then view them:
     `ffmpeg -v error -i IN.mp4 -vf "fps=1/2,scale=480:-1,tile=4x2" frames/grid_%02d.jpg`
   - Note walls, fixtures (sockets, windows, doors), furniture shape, panels that reach the floor, gaps, materials.

2. **Confirm understanding before planning.**
   - Restate the scene in a few bullets (layout, numbered seats/items, fixture positions, what is near/far).
   - Draw a top-down sketch (inline SVG) of the WHOLE scene: room outline, furniture, numbered items, fixtures, chairs, a near/far indicator, and a legend.
   - Ask the user to correct anything. Apply every correction to all later visuals.

3. **Plan.** Research online when real products, standards or safety matter (cite sources). Give specific parts, quantities, approximate lengths, steps and safety/load checks. Respect the user's constraints (e.g. keep things under the desk, no clutter); if you disagree, say so with concrete reasons and propose a compromise.

4. **Every reply after confirmation contains, in this order:**
   - **Text:** concise, specific (what, where, how, parts, safety). No padding.
   - **Diagram:** top-down SVG of the whole scene with the plan overlaid. Same orientation and numbering every time. Legend at bottom. Dashed = hidden/under/behind. Colors: green = main feed/OK, red dashed = problem/too far, amber = endpoints/leads, coral = hub/central unit.
   - **3D model:** interactive three.js scene of the whole space with the plan built in. Add a side/section view only when height or mounting matters.

5. **Iterate.** When the user corrects something, update the diagram and 3D model, not just the text.

## 3D model rules (three.js, inline widget)

- Load `https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js`.
- Wrap ALL code in an IIFE `(function(){ ... })();`. Never declare globals named `top`, `parent`, `name`, `self`, `status`, `length`, `close` etc. A global `const top` throws and leaves a blank canvas.
- Don't rely on `ctx.roundRect`; draw rounded rects with `arcTo`.
- Units in metres; floor y=0, desk height ~0.75, chairs, walls ~1.6 high (omit the near wall).
- Helpers: `box()` for solids, cylinder `seg()`/`path()` for cables, `TorusGeometry` for holes/grommets, `EdgesGeometry` line boxes for trays, canvas-texture sprites for labels (`depthTest:false`).
- Custom orbit controls: pointer drag rotate, wheel + two-finger pinch zoom, `touch-action:none`.
- Controls under the canvas: see-through toggle (transparent desks/panels), labels toggle, preset views (room, close-up, top).
- Transparent renderer background; size via ResizeObserver with fallback width/height.

## Diagram rules (SVG)

- viewBox width 680; room outline, then furniture, then fixtures, then plan overlay, then legend.
- Labels never overlap lines or boxes; keep text 12–14px, sentence case.
- Mark fixtures on the correct wall exactly as the user described; re-check after every correction.

## Tone

Concise. Ask at most one clarifying question per turn, alongside a best-guess visual.