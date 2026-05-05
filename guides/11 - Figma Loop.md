I won't be writing a guide for each tool as they fundamentally work the same way. If you need help setting up your MCP connections, refer to the earlier guides in the course.

The Figma loop refers to getting content in and out of Figma using their MCP server and skills. Figma offers a plugin I would highly recommend installing as it includes skills that will improve the quality of your integration.

There are two main workflows Figma MCP enables, with some sub elements:

1) Use Figma design to write code
2) Use code to update Figma designs

This guide will focus on the second workflow.

## Use code to update Figma designs

To get started, find the Figma plugin in your tool of choice. In Claude Code, it can be found in the Customize menu under Plugins --> Browse Plugins.

![](<screenshots/CleanShot 2026-05-02 at 16.22.56.png>)

Now, let's assume we have a prototype of some kind, like this:
![](<screenshots/CleanShot 2026-05-02 at 17.20.28.png>)


We can move it back into Figma in two ways:
1) Using our design system
2) As a raw frame

To move it back in with the design system, open a page in Figma in a project that has the design system enabled. This can be the design system file itself, or any file where the design system is accessible across your team.

Use a prompt like this:
> move this into figma using my design system. if you cant find the design system elements, tell me.
>
[https://www.figma.com/design/NtgkbJYyusdRMBgLSy5k57/Stride-%E2%80%94-Design-System?node-id=83-2&p=f&t=oFb8tdGpDphIPaVQ-0](https://www.figma.com/design/NtgkbJYyusdRMBgLSy5k57/Stride-%E2%80%94-Design-System?node-id=83-2&p=f&t=oFb8tdGpDphIPaVQ-0)

This should invoke a few figma skills and the MCP server to explore your page:
![](<screenshots/CleanShot 2026-05-02 at 16.55.48.png>)

This will result in many tool calls and can take 30+ minutes. Here is the final result:
![](<screenshots/CleanShot 2026-05-02 at 17.21.51.png>)

Importantly, if we inspect the file, we'll see that it uses the design system elements like the primary color and button component.
![](<screenshots/CleanShot 2026-05-02 at 17.22.41.png>)


The second workflow available is to not use the design system. This will be much faster and almost an exact replica, but obviously won't use any of the design system elements.

![](<screenshots/CleanShot 2026-05-02 at 17.32.44.png>)

You can see that the exported frame is visually similar to the prior output, with some minor modifications. Inspecting the elements would show that there is no variables or components being used in this design.


## Use Figma design to write code

If you're curious about using a Figma design to implement code, it's a much simpler workflow. You copy the frame you want to implement, paste it in your coding agent of choice and prompt to implement.

Usually challenges come from mapping Figma elements to your codebase components and variables, which you can improve by creating your own skill that documents these mappings.