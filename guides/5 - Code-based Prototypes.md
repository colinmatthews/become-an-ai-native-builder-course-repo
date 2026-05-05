There are two ways to set up code-based prototypes at your organziation.

1. Create a components-only github repo
2. Use your production repo

Using production often comes with challenges when running your backend application, including environment variables, services, and sometimes your computer can't physically run all of the software at once.

This guide will focus on using a component repo for Stride.

It is recommended to install Github Desktop before starting this process.
You must have Magic Patterns set up with the MCP (see 10 - Connect Magic Patterns MCP).

## 1) Download from Github

Download the components repo from github.
If you're using Github desktop, you can do this directly from Code --> Open with Github Desktop.
![[CleanShot 2026-05-01 at 16.25.27.png]]

If you have git, you can clone with `git clone colinmatthews/stride-components.
Finally, you can also download the zip using the same menu above.

Once you have the files in a folder on your computer, open Claude Code / Codex / Cursor on that folder.

You can optionally install the plugin in the plugins folder to improve your use of Magic Patterns.
For Codex and Cursor. Use a prompt like this:
`Install the Magic patterns plugin from plugins/codex-magic-patterns`

If you have any problems with visibility, you can prompt something like `its not visible in the list of plugins`. Remember to restart your app after it is installed.

![[CleanShot 2026-05-01 at 17.28.21.png]]


Once complete, you can use the plugin to build your prototype. Simply select the plugin and prompt for a new feauture.

eg: `Build me a better analytics view for run performance`

![[CleanShot 2026-05-01 at 17.38.57.png]]

And the resulting prototype:
![[CleanShot 2026-05-01 at 17.39.44.png]]

