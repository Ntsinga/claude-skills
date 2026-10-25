# claude-skills
This repository is a collection of Claude skills/workflows that i create to automate my life

## Skills

| Skill | What it does |
|---|---|
| [space-plan-visualizer](skills/space-plan-visualizer/SKILL.md) | Describe a physical space (room, office, furniture, sockets, wiring) with optional photos or video. Claude confirms its understanding with a top-down sketch, then answers each step with text, a top-down diagram and an interactive 3D model. |

## Install a skill in Claude

1. Download the skill's `.zip` (for example [`skills/space-plan-visualizer.zip`](skills/space-plan-visualizer.zip)).
2. In Claude, open **Customize > Skills**, click **+**, then **Upload a skill**, and choose the zip.
3. Make sure the skill is switched on. Claude uses it automatically when a request matches its description.

To change a skill, edit its `SKILL.md`, zip the folder, and upload it again.
