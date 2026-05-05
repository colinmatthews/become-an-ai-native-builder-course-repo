In this exercise you'll build a code-based prototype against the **Stride components repo**. Code-based prototypes use real components from your codebase, which means what you build looks and behaves like the actual product — not a generic mockup.

We're using a components-only repo (not production) so you don't need environment variables, services, or a backend to run it. We're also doing this **fully locally** — no Magic Patterns, no extra MCPs. Just your editor, the repo, and the existing component library.

## Before you start

You should have:

- **Github Desktop** installed (recommended — easiest way to clone and pull)
- Claude Code, Codex, or Cursor installed and ready

## 1) Clone the components repo

Pick whichever method you have set up.

**Github Desktop** (easiest): On the stride-components repo page, click **Code → Open with Github Desktop**.

![](<screenshots/CleanShot 2026-05-01 at 16.25.27.png>)

**Git CLI**: `git clone https://github.com/colinmatthews/stride-components`

**Zip**: Same Code dropdown → **Download ZIP**, then unzip it somewhere you'll find it.

## 2) Open the repo

Open Claude Code, Codex, or Cursor on the folder you just cloned. Then start the local dev server so you can see your changes render as you go (you can ask the agent to run these commands)

```
npm install
npm run dev
```

## 3) Orient your agent

Before asking for a feature, make sure your agent has read the codebase. In a fresh chat, prompt:

> Read through the components in `src/components/` and the existing pages so you understand the design language and the components available.

This step matters. Without it, the agent tends to invent its own components from scratch instead of reusing what's already there, and the prototype ends up looking nothing like the rest of the app.

## 4) Build your prototype

Prompt for a new feature. For example:

> Build me a better analytics view for run performance. Reuse the existing components in `src/components/` wherever possible and match the visual style of the rest of the app. Add it as a new page.


The agent should:

1. Reuse components from `src/components/` (cards, charts, layout primitives) instead of building new ones
2. Add a new page/route for the analytics view
3. Drop in realistic-looking mock data so the view actually renders

eg:
![](<screenshots/CleanShot 2026-05-01 at 17.39.44.png>)

## 5) Iterate

The first generation is rarely the final one. Look at what rendered and pick one specific thing to change. Be concrete:

> The chart is too small and the metric cards look generic. Make the chart full-width and use the existing `MetricCard` component from `src/components/` for the four stats above it.

Repeat until it feels close to shippable. Two or three iterations is normal.
