## Claude
You can create skills in Claude Chat, Cowork, or Code. The main difference is where they get stored.

With Chat or Cowork, they will be stored within Claude's virtual machine. They're easy to setup and use, but you can't edit them directly. Skills can also be created in Cloud Code. These skills can be scoped to you as a user or to the project that you're in.

If you scope them to the project, they'll be stored in your local file system in that directory and can be shared across GitHub and other tools. If you scope them to the user, they'll be stored on your device and are still editable, but will not be as easy to find.

Let's walk through each one.

### Chat
To create a skill, ask claude to create a skill. It's that easy! You can be specific and ask for the `create-skill` skill if you want. This is a skill that ships with claude and helps it create good skills.

The skill itself is pretty long, so I'll share just the opening section. The full skill can be found [here](https://github.com/anthropics/skills/blob/main/skills/skill-creator/SKILL.md).
```

---
name: skill-creator
 
 description: Create new skills, modify and improve existing skills, and measure skill performance. Use when users want to create a skill from scratch, edit, or optimize an existing skill, run evals to test a skill, benchmark skill performance with variance analysis, or optimize a skill's description for better triggering accuracy.
---


# Skill Creator

A skill for creating new skills and iteratively improving them.

At a high level, the process of creating a skill goes like this:

- Decide what you want the skill to do and roughly how it should do it
- Write a draft of the skill
- Create a few test prompts and run claude-with-access-to-the-skill on them
- Help the user evaluate the results both qualitatively and quantitatively
  - While the runs happen in the background, draft some quantitative evals if there aren't any (if there are some, you can either use as is or modify if you feel something needs to change about them). Then explain them to the user (or if they already existed, explain the ones that already exist)
  - Use the `eval-viewer/generate_review.py` script to show the user the results for them to look at, and also let them look at the quantitative metrics
- Rewrite the skill based on feedback from the user's evaluation of the results (and also if there are any glaring flaws that become apparent from the quantitative benchmarks)
- Repeat until you're satisfied
- Expand the test set and try again at larger scale

Your job when using this skill is to figure out where the user is in this process and then jump in and help them progress through these stages. So for instance, maybe they're like "I want to make a skill for X". You can help narrow down what they mean, write a draft, write the test cases, figure out how they want to evaluate, run all the prompts, and repeat.

On the other hand, maybe they already have a draft of the skill. In this case you can go straight to the eval/iterate part of the loop.

Of course, you should always be flexible and if the user is like "I don't need to run a bunch of evaluations, just vibe with me", you can do that instead.

Then after the skill is done (but again, the order is flexible), you can also run the skill description improver, which we have a whole separate script for, to optimize the triggering of the skill.

Cool? Cool.

```

Let's try it out by making a skill for writing great prompts for image generation.

![[CleanShot 2026-04-28 at 15.54.32.png]]

Claude has some follow up questions after reading the skill, which we'll answer below.
![[CleanShot 2026-04-28 at 15.55.58.png]]


Once complete, we can see Claude create the skill.
![[CleanShot 2026-04-28 at 16.37.32.png]]


Claude will then run tests to determine if the skill is providing high quality outputs. In Claude Chat, it does this as part of the chat with the prior context of the conversation stored. Here, you can continue to iterate on the skill with Claude.

When complete, Claude Chat cannot install its own skills. Instead, you will need to manually install the skill by asking for a file to download and uploading in Settings, Skills.

### Cowork
Creating a skill works the same as above with Claude Chat. The major differences are that Cowork can create subagents for testing and can directly install skills within its virtual machine.

Here you can see the Skill file created inline and the ability to install directly.
![[CleanShot 2026-04-28 at 19.10.14.png]]

Once installed, you can access your skills via the Customize menu.
![[CleanShot 2026-04-28 at 19.11.53 1.png]]



### Claude Code
Claude Code skills are independent of Claude Chat and Cowork skills. They can live in three places:

* **User Level**: ~/.claude/skills/ 
* **Project Level:** ~/project/.claude/skills/
* **Plugins**: Skills installed via Plugins 

To create a skill with Claude Code, you should start by installing the skill creator skill. You can do this from the Skill marketplace.

![[CleanShot 2026-04-28 at 19.38.40.png]]


The skill directory within Claude Code desktop is more limited than the terminal. 
You should never trust skills downloaded from the internet without reading them first in a code editor. Skills can include hidden instructions to send sensitive information off of your machine.

Once you have the skill creator skill installed, you can use it within Claude Code desktop or terminal to create a skill local to your project. It will be stored within your project files and is best accessed with a code editor like Cursor or VS Code.

![[CleanShot 2026-04-28 at 19.41.28.png]]



## Codex

Codex comes with a built in skill creator skill. You can access it with `/Skill Creator`

![[CleanShot 2026-04-28 at 19.49.00.png]] 

Skills are stored on your local machine at a user level in the Codex directory.
![[CleanShot 2026-04-28 at 19.51.11.png]]


Once you complete installation steps, you can trigger your skill with a `/` command.
![[CleanShot 2026-04-28 at 19.54.15.png]]