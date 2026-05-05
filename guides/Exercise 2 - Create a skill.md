In Exercise 1 you connected the course MCP server. The problem: your agent doesn't know *when* to reach for it, *which* tool to use first, or *how* to chain them. Without guidance, it tends to skip the MCP entirely.

In this exercise you'll fix that by writing a skill that wraps the course MCP. The skill will teach your agent the right call pattern and the right escalation paths.

## What the skill should do

The course MCP exposes these tools:

- `search_course_documents` — keyword search across course articles
- `fetch_course_document` — fetch one article by id (returns text + an image manifest)
- `fetch_course_image` / `show_image` — pull and display screenshots referenced by an article
- `get_help` / `submit_help` — open and submit a help request to the course team
- `submit_feedback` — log feedback on the answer the student got

A good skill for this MCP enforces a few opinions:

1. **Always search before fetching.** Never guess an article id — call `search_course_documents` first, then `fetch_course_document` with the top match.
2. **Show, don't just describe.** When an article includes images relevant to the answer, call `show_image` so the student actually sees the screenshot.
3. **Escalate on a miss.** If search returns nothing useful, or the student is stuck after a fetched article, offer `submit_help` rather than guessing.
4. **Close the loop.** After a substantive answer, offer `submit_feedback` so the course can improve.
5. **Trigger on course-shaped questions.** Course concepts, exercise blockers, error messages from the lessons — anything that smells like "this is about the course."

## Build the skill

Pick the platform you're using.

### Claude Cowork or Code

In a new chat, prompt Claude to use the `skill-creator` skill to build a skill called `course-mcp` that wraps the *Become an AI-Native Builder* MCP server. Paste in the five opinions from above so the skill captures them.


>Make a skill for Become an AI Native Builder that:
>
>1. Always search before fetching. Never guess an article id — call `search_course_documents` first, then `fetch_course_document` with the top match.
>2. Show, don't just describe. When an article includes images relevant to the answer, call `show_image` so the student actually sees the screenshot.
>3. Escalate on a miss. If search returns nothing useful, or the student is stuck after a fetched article, offer `submit_help` rather than guessing.
>4. Close the loop. After a substantive answer, offer `submit_feedback` so the course can improve.
>5. Trigger on course-shaped questions. Course concepts, exercise blockers, error messages from the lessons — anything that smells like "this is about the course."


![[CleanShot 2026-05-02 at 14.17.45.png]]

Claude will ask follow-ups — answer with the tool list and the call order (search → fetch → show_image; help/feedback as escalations).

![[CleanShot 2026-04-28 at 15.55.58.png]]

Once the skill is generated, you can install it with Save Skill.
![[CleanShot 2026-05-02 at 14.21.11.png]]

Then create a new chat and give your skill a try.
![[CleanShot 2026-05-02 at 14.22.54.png]]


The response should now include images directly in the result. These images were always available but not called by Claude as it didn't understand how to use the tool.
![[CleanShot 2026-05-02 at 14.24.38.png]]


## Codex

In a new chat, prompt Codex to use the `skill-creator` skill by typing @skill creator.
Enter a prompt to build a skill called `course-mcp` that wraps the *Become an AI-Native Builder* MCP server. Paste in the five opinions from above so the skill captures them.


>Make a skill for Become an AI Native Builder that:
>
>1. Always search before fetching. Never guess an article id — call `search_course_documents` first, then `fetch_course_document` with the top match.
>2. Show, don't just describe. When an article includes images relevant to the answer, call `show_image` so the student actually sees the screenshot.
>3. Escalate on a miss. If search returns nothing useful, or the student is stuck after a fetched article, offer `submit_help` rather than guessing.
>4. Close the loop. After a substantive answer, offer `submit_feedback` so the course can improve.
>5. Trigger on course-shaped questions. Course concepts, exercise blockers, error messages from the lessons — anything that smells like "this is about the course."

![[CleanShot 2026-05-02 at 14.29.01.png]]

Once complete, your skill will be installed.
![[CleanShot 2026-05-02 at 14.33.31.png]]

Restart Codex and then type `/`. Look for a skill with the a name similar to "Become an AI Native Builder." 
![[CleanShot 2026-05-02 at 14.32.18.png]]

Prompt with a question and check out the results!
![[CleanShot 2026-05-02 at 14.38.46.png]]


## Cursor

In a new chat, prompt Cursor to use the `skill-create` skill.
Enter a prompt to build a skill called `course-mcp` that wraps the *Become an AI-Native Builder* MCP server. Paste in the five opinions from above so the skill captures them.


>Make a skill for Become an AI Native Builder mcp that:
>
>1. Always search before fetching. Never guess an article id — call `search_course_documents` first, then `fetch_course_document` with the top match.
>2. Show, don't just describe. When an article includes images relevant to the answer, call `show_image` so the student actually sees the screenshot.
>3. Escalate on a miss. If search returns nothing useful, or the student is stuck after a fetched article, offer `submit_help` rather than guessing.
>4. Close the loop. After a substantive answer, offer `submit_feedback` so the course can improve.
>5. Trigger on course-shaped questions. Course concepts, exercise blockers, error messages from the lessons — anything that smells like "this is about the course."


Restart Cursor then prompt with a question in a new chat.
![[CleanShot 2026-05-02 at 14.41.10.png]]

Here's an example result:
![[CleanShot 2026-05-02 at 14.41.47.png]]
