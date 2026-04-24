---
name: codebase-to-course-salesforce-po
description: "Turn any Salesforce codebase into a beautiful, interactive single-page HTML course tailored for a Salesforce Product Owner with junior Apex experience and no LWC/JavaScript background. Use this skill whenever the user wants to understand a Salesforce project they've vibe-coded, explain an LWC/Apex codebase interactively, or fill gaps in their Salesforce frontend knowledge. Trigger on: 'turn this Salesforce project into a course,' 'explain this LWC codebase,' 'teach me how this Apex project works,' 'make a course from this Salesforce repo,' 'I vibe-coded this and want to understand it.' Produces a self-contained HTML course with scroll-based modules, Mermaid call graphs, animated visualizations, embedded quizzes, code-anchored explanations, and Apex-to-LWC concept bridges."
---

# Codebase-to-Course (Salesforce PO Edition)

Transform any Salesforce codebase into a stunning, interactive course built for a **Salesforce Product Owner with junior Apex experience and no LWC/JavaScript background**. The output is a **directory** containing a pre-built `styles.css`, `main.js`, per-module HTML files, and an assembled `index.html` — open it directly in the browser with no setup required (external dependencies: Google Fonts CDN + Mermaid.js CDN). The course teaches how the code works through scroll-based modules, rendered Mermaid call graphs, animated visualizations, embedded quizzes, and code-anchored plain-English translations.

## First-Run Welcome

When the skill is first triggered and the user hasn't specified a codebase yet, introduce yourself and explain what you do:

> **I can turn any Salesforce codebase into an interactive course that teaches how it works — built specifically for someone who knows Apex but is new to LWC and JavaScript.**
>
> Just point me at a project:
> - **A local folder** — e.g., "turn ./my-sf-project into a course"
> - **A GitHub link** — e.g., "make a course from https://github.com/user/repo"
> - **The current project** — if you're already in a codebase, just say "turn this into a course"
>
> I'll read through the code, figure out how everything fits together, and generate a beautiful single-page HTML course with Mermaid call graphs, plain-English code explanations anchored to real lines of code, and interactive quizzes. Every LWC concept gets explained by bridging from Apex you already know. The whole thing runs in your browser — no setup needed.

If the user provides a GitHub link, clone the repo first (`git clone <url> /tmp/<repo-name>`) before starting the analysis. If they say "this codebase" or similar, use the current working directory.

---

## Who This Is For

The target learner is a **Salesforce Product Owner who vibe-codes** — they instruct AI coding tools to build Salesforce features and can read and write basic Apex, but have little to no practical experience with LWC, JavaScript, HTML, or CSS. They may have built this project themselves by prompting AI, or found an interesting open-source Salesforce project they want to understand.

**Assumed knowledge (use freely, no need to gloss):**
- Salesforce platform: orgs, sandboxes, objects, fields, relationships, record types, profiles, permission sets
- Apex fundamentals: classes, methods, instance variables, constructors, interfaces, triggers, SOQL, DML, governor limits, async Apex (future, queueable, batch), Apex tests
- Salesforce architecture concepts: metadata, deployment, packages, managed vs. unmanaged
- Business process tools: Flows, Process Builder, validation rules, formula fields

**Unknown territory (explain everything, no assumptions):**
- LWC: component structure, lifecycle hooks, reactive properties, decorators (`@api`, `@track`, `@wire`), slots, templates
- JavaScript: variables (`const`/`let`), arrow functions, promises, async/await, modules, destructuring, array methods
- HTML/CSS: the DOM, box model, flexbox, grid, overflow, z-index, specificity, pseudo-classes
- Browser concepts: event bubbling, event delegation, shadow DOM, rendering pipeline

**The key bridging principle:** Every unfamiliar concept gets anchored to an Apex equivalent the learner already knows before introducing the new idea. Never explain LWC or JavaScript in a vacuum.

Examples:
- "An LWC component is like an Apex class — it has properties (like instance variables) and methods — but instead of processing data on the server, it controls what the user sees and interacts with in the browser."
- "The `@wire` decorator is like a lazy SOQL query that Salesforce runs automatically when the component loads, the same way you'd call a method in a constructor — except the platform handles the timing and caching for you."
- "CSS `overflow: hidden` is like setting a container's size in a Visualforce page and telling it to clip anything that doesn't fit, instead of expanding to show all the content."
- "JavaScript `async/await` works like Apex's `@future` — the work happens outside the main thread, but `await` lets you write the code as if it were synchronous, just like you can write sequential DML after a future call returns."

**Their goals are practical, not academic:**
- Have enough technical knowledge to effectively **steer AI coding tools** on Salesforce projects — make better architecture decisions (when to use LWC vs. Flow vs. Apex, where logic belongs)
- **Detect when AI generates bad Salesforce patterns** — spot logic that should be in Apex leaking into LWC, SOQL in loops, shared state abuse, missing `@AuraEnabled(cacheable=true)` annotations
- **Intervene when AI gets stuck** — debug "it's not showing up" LWC rendering issues, break out of wire service loops
- Build more advanced Salesforce software with **production-level quality** — understand why certain patterns exist (bulkification, separation of concerns, error handling)
- Be **technically fluent** enough to discuss decisions with Salesforce developers confidently — know when to say "the wire adapter" instead of "that thing that loads data automatically"
- **Fill LWC/JavaScript gaps** without abandoning Apex fluency — add the frontend layer to an already strong backend mental model

**They are NOT trying to become full-stack Salesforce developers.** They want to understand the code they've vibe-coded well enough to direct AI better, catch problems earlier, and have credible technical conversations with their dev team.

---

## Why This Approach Works

This skill builds on existing Apex knowledge rather than starting from scratch. The learner already has a strong server-side mental model — Apex classes, methods, data access. The course's job is to show them that LWC is the same conceptual structure applied to the browser layer, with a different runtime environment and a few new patterns on top.

Every module answers **"why should I care?"** before "how does it work?" The answer is always practical: *because this knowledge helps you steer AI better, debug faster, or make smarter Salesforce architecture decisions.*

**Code-first, always.** Every concept — including JavaScript ones — must be explained by pointing at an actual line or block from the codebase being analyzed. Abstract explanations without a real code anchor are forbidden. The pattern is always: show the snippet → explain what it does in plain English → explain why it's written that way → connect to the Apex equivalent if one exists.

---

## The Process

### Phase 1: Codebase Analysis

Before writing course HTML, deeply understand the codebase. Read all the key files, trace the data flows between LWC components and Apex classes, identify the "cast of characters," and map how they communicate. For Salesforce projects, pay particular attention to:

**What to extract:**
- The main actors: which LWC components exist, which Apex classes back them, which objects they operate on
- The primary user journey: what happens end-to-end when a user performs the core action in this app (from button click through wire/imperative call through Apex through DML and back)
- Communication patterns: `@wire` vs. imperative calls, component events vs. pubsub, parent-child vs. sibling communication
- Data model: which objects and fields are central to this app, and how they relate
- Clever patterns: caching strategy, error handling, bulkification, separation of concerns
- Gotchas: any anti-patterns visible in the code (SOQL in LWC, business logic in the component, missing error handling)
- Tech stack decisions: why LWC vs. Aura vs. Visualforce, any third-party libraries, any integration points

**Figure out what the app does yourself** by reading the README, the main component's HTML template, and the Apex `@AuraEnabled` methods. Don't ask the user to explain the product. The course should open by explaining what the app does in plain language before diving into how it works. The first module should start with a concrete user action — "imagine you click this button — here's exactly what happens, step by step, through five layers of code."

### Phase 2: Curriculum Design

Structure the course as **4-7 modules**. Salesforce full-stack projects typically need 5-6. Fewer, better modules beat more, thinner ones.

The arc always starts from what the learner already understands (the Salesforce platform and Apex backend) and moves toward what they don't (LWC, JavaScript, CSS, browser rendering). Think of it as crossing a bridge from the server side they know to the client side they don't.

| Module Position | Focus | Depth for this learner |
|---|---|---|
| 1 | What this app does + the end-to-end user journey | Start with the product, trace one core action through all layers — ground everything in something concrete |
| 2 | Data model + Apex backend | **Light-to-medium** — familiar territory; focus on any non-obvious patterns or anti-patterns in this specific codebase |
| 3 | LWC component structure + HTML templates | **Deep** — this is the first big unknown; use Apex analogies throughout; explain every decorator and lifecycle hook |
| 4 | JavaScript in LWC: reactivity, events, async | **Deep** — JavaScript semantics need full explanation; anchor every concept to Apex equivalents |
| 5 | Component communication + data flow | **Deep** — wire vs. imperative, parent-child events, how data moves across the component tree |
| 6 | Platform concerns: permissions, limits, deployment | **Medium** — mix of familiar (governor limits) and new (LWC security, LockerService, namespace isolation) |
| 7 | When things break: debugging LWC | **Medium** — practical debugging skills; browser DevTools, wire service failures, Apex exceptions surfaced in LWC |

This is a **menu, not a checklist**. A simple single-component utility might need only modules 1, 3, and 4. Adapt the arc to the codebase's complexity and where the learning gaps are largest.

**The key principle:** Every module should connect back to a practical skill — steering AI, debugging, making decisions. If a module doesn't help the learner DO something better, cut it or reframe it until it does.

**Each module should contain:**
- 3-6 screens (sub-sections that flow within the module)
- At least one code-with-English translation (real code, real file path, real line numbers)
- At least one Mermaid diagram (call graph, sequence diagram, or component tree)
- At least one interactive element (quiz, visualization, or animation)
- One or two "aha!" callout boxes — prioritize insights that bridge Apex knowledge to LWC
- A metaphor that grounds the technical concept in everyday life OR in Apex — but NEVER reuse the same metaphor across modules. Pick metaphors that feel *inevitable* for the concept.

**Mandatory interactive elements (every course must include ALL of these):**
- **Group Chat Animation** — at least one across the course. iMessage/WeChat-style conversations between components (e.g., LWC component messaging its Apex controller, or a child component talking to a parent). One of the most engaging elements — always include it.
- **Message Flow / Data Flow Animation** — at least one across the course. The step-by-step animation showing data moving from user interaction → LWC → Apex → database → back. Every Salesforce app has this flow somewhere.
- **Code ↔ English Translation Blocks** — at least one per module. Non-negotiable. Always reference real code from the actual codebase with file path and line number. Never invent example code when real code exists.
- **Quizzes** — at least one per module (multiple-choice, scenario, drag-and-drop, or spot-the-bug). Quiz questions should test the practical skill, not just recall.
- **Glossary Tooltips** — on every technical term, first use per module. For LWC/JS/CSS terms, the tooltip must include the Apex bridge phrase where one exists.
- **Mermaid Diagrams** — at least one per module. Use `sequenceDiagram` for data flows, `classDiagram` for component/class relationships, `graph TD` for call graphs and decision trees. These are rendered via Mermaid.js CDN — write valid Mermaid syntax inside `<div class="mermaid">` blocks.

These six element types are the backbone of every course. Other interactive elements (architecture diagrams, layer toggles, pattern cards, etc.) are optional and should be added when they fit. But the six above must ALWAYS be present — no exceptions.

**Do NOT present the curriculum for approval — just build it.** The user wants a course, not a planning document. Design the curriculum internally, then go straight to building. If they want changes, they'll tell you after seeing the result.

**After designing the curriculum, decide which build path to use:**

- **Simple codebase** (single LWC component with one Apex class, or a small utility, 5 or fewer modules) → go directly to Phase 3 Sequential.
- **Complex codebase** (multiple LWC components, multiple Apex classes/services, full feature with data model, 6+ modules) → go to Phase 2.5 first, then Phase 3 Parallel.

### Phase 2.5: Module Briefs (complex codebases only)

For complex codebases, write a brief for each module before writing any HTML. This is the critical step that enables parallel writing — each brief gives an agent everything it needs without re-reading the codebase.

Read `references/module-brief-template.md` for the template structure. Read `references/content-philosophy.md` for the content rules that should guide brief writing.

**For each module, write a brief to `course-name/briefs/0N-slug.md` containing:**
- Teaching arc (Apex bridge metaphor, opening hook, key insight)
- Pre-extracted code snippets (copy-pasted from the codebase with file paths and line numbers — both the LWC and the Apex side where relevant)
- Mermaid diagram source for each planned diagram (fully written out so the writing agent doesn't need to read the codebase)
- Interactive elements checklist with enough detail to build them
- Which sections of which reference files the writing agent needs
- What the previous and next modules cover (for transitions)

The code snippets and Mermaid diagrams are the critical token-saving steps. By pre-extracting them into the brief, writing agents never need to read the codebase at all.

### Phase 3: Build the Course

The course output is a **directory**, not a single file. CSS and JS are pre-built reference files — never regenerate them. Your job is to write only the HTML content.

**Output structure:**
```
course-name/
  styles.css       ← copied verbatim from references/styles.css
  main.js          ← copied verbatim from references/main.js
  _base.html       ← customized shell (title, accent color, nav dots, Mermaid CDN)
  _footer.html     ← copied verbatim from references/_footer.html
  build.sh         ← copied verbatim from references/build.sh
  briefs/          ← module briefs (complex codebases only, can delete after build)
  modules/
    01-intro.html
    02-data-model.html
    ...
  index.html       ← assembled by build.sh (do not write manually)
```

**Step 1 (both paths): Setup** — Create the course directory. Copy these four files verbatim using Read + Write (do not regenerate their contents):
- `references/styles.css` → `course-name/styles.css`
- `references/main.js` → `course-name/main.js`
- `references/_footer.html` → `course-name/_footer.html`
- `references/build.sh` → `course-name/build.sh`

**Step 2 (both paths): Customize `_base.html`** — Read `references/_base.html`, then write it to `course-name/_base.html` with these substitutions:
- Both instances of `COURSE_TITLE` → the actual course title
- The four `ACCENT_*` placeholders → the chosen accent color values (pick one palette from the comments in `_base.html`)
- `NAV_DOTS` → one `<button class="nav-dot" ...>` per module
- Add the following two script tags in the `<head>`, **before** the closing `</head>` tag:

```html
<!-- Mermaid.js for call graphs and sequence diagrams -->
<script src="https://cdn.jsdelivr.net/npm/mermaid@10/dist/mermaid.min.js"></script>
<script>
  mermaid.initialize({
    startOnLoad: true,
    theme: 'base',
    themeVariables: {
      primaryColor: '#1E1E2E',
      primaryTextColor: '#CDD6F4',
      primaryBorderColor: '#89B4FA',
      lineColor: '#89B4FA',
      secondaryColor: '#313244',
      tertiaryColor: '#45475A',
      background: '#1E1E2E',
      mainBkg: '#1E1E2E',
      nodeBorder: '#89B4FA',
      clusterBkg: '#313244',
      titleColor: '#CDD6F4',
      edgeLabelBackground: '#313244',
      fontFamily: 'JetBrains Mono, monospace'
    }
  });
</script>
```

This theme matches the course's dark code block aesthetic (Catppuccin Mocha). The `startOnLoad: true` setting means any `<div class="mermaid">` block in a module will be automatically rendered when the page loads.

**Step 3: Write modules** — This is where the paths diverge.

#### Sequential path (simple codebases)

Read `references/content-philosophy.md` and `references/gotchas.md`. Then write modules one at a time. For each module, write `course-name/modules/0N-slug.html` containing only the `<section class="module" id="module-N">` block and its contents. Do not include `<html>`, `<head>`, `<body>`, `<style>`, or `<script>` tags.

Read `references/interactive-elements.md` for HTML patterns for each interactive element type. Read `references/design-system.md` for visual conventions.

**Mermaid diagram pattern** (use in modules, requires no additional JS wiring):
```html
<div class="code-block mermaid-wrapper">
  <div class="mermaid">
sequenceDiagram
    participant User
    participant LWC as LWC Component
    participant Apex as Apex Controller
    participant DB as Salesforce DB
    User->>LWC: clicks button
    LWC->>Apex: callApexMethod(recordId)
    Apex->>DB: SELECT Id, Name FROM Account WHERE Id = :recordId
    DB-->>Apex: Account record
    Apex-->>LWC: AccountWrapper result
    LWC->>LWC: set reactive property → re-render
    LWC-->>User: updated UI
  </div>
  <div class="diagram-caption">Call graph: what happens when the user clicks the button</div>
</div>
```

Wrap the `.mermaid` div in a `code-block` container so it inherits the dark background and border-radius from the design system. Add a `.diagram-caption` beneath for a plain-English label.

#### Parallel path (complex codebases)

Dispatch modules to subagents in batches of up to 3. Each agent receives:
- Its module brief (from `course-name/briefs/`) — which already contains pre-extracted code snippets and Mermaid source
- `references/content-philosophy.md` and `references/gotchas.md`
- Only the sections of `references/interactive-elements.md` and `references/design-system.md` listed in the brief
- The Mermaid diagram pattern shown above (copy it into the agent's prompt)

Each agent writes its module file(s) to `course-name/modules/`. Short modules (3 screens, one quiz) can be paired — two briefs given to one agent.

**What agents do NOT receive:** the full codebase (snippets are in the brief), SKILL.md, other modules' briefs, or unneeded reference file sections.

After all agents finish, do a quick consistency check in the main context: nav dots match modules, transitions between modules are coherent, Apex bridge metaphors are consistent, no obvious tone shifts.

**Step 4 (both paths): Assemble** — Run `build.sh` from the course directory:
```bash
cd course-name && bash build.sh
```
This produces `index.html`. Open it in the browser and verify that Mermaid diagrams render (they appear as plain text until the CDN script runs — check in a real browser, not a local file preview that blocks CDN).

**Critical rules:**
- **Never regenerate** `styles.css` or `main.js` — always copy from references
- Module files contain only `<section>` content — no boilerplate
- Use CSS `scroll-snap-type: y proximity` (NOT `mandatory`)
- Use `min-height: 100dvh` with `100vh` fallback on `.module`
- Interactive element JS is in `main.js`; wire up via `data-*` attributes and CSS class names as shown in `references/interactive-elements.md`
- Chat containers need `id` attributes; flow animations need `data-steps='[...]'` JSON on `.flow-animation`
- **Every code snippet must include the real file path and line number** — no invented examples when real code exists
- **Every LWC/JavaScript/CSS term that appears for the first time in a module must have a glossary tooltip** AND, where an Apex equivalent exists, the tooltip must include the bridge phrase

### Phase 4: Review and Open

After running `build.sh`, open `index.html` in the browser. Verify that:
- Mermaid diagrams rendered (not showing raw syntax)
- All code snippets reference real file paths and line numbers from the analyzed codebase
- Every LWC/JS/CSS term has a tooltip on first use
- At least one Apex bridge phrase appears per module that introduces new frontend concepts

Walk the user through what was built and ask for feedback on content, depth, and accuracy of the Apex bridges.

---

## Content Rules Specific to This Learner

### Code-First Explanations (non-negotiable)

Every concept must be demonstrated with a real snippet from the codebase before any explanation is given. The pattern is always:

1. **Show the code** — the actual snippet, with file path and line numbers
2. **Plain-English translation** — what this code does, as if explaining to someone who can read Apex but not JavaScript
3. **Why it's written this way** — the reason this pattern exists, not just what it does
4. **Apex bridge** (for LWC/JS concepts) — "this is like [Apex equivalent] because..."

Never do step 2 without step 1. Never invent a simpler example when the real code can be explained directly.

### Apex Bridge Phrases

Every new LWC/JS/CSS concept needs a bridge phrase connecting it to Apex. Write these naturally, not formulaically. Some patterns:

| New concept | Apex bridge |
|---|---|
| LWC component | "Like an Apex class, but for the browser layer" |
| `@wire` decorator | "Like a lazy SOQL query in a constructor — runs automatically when the component loads" |
| `@api` property | "Like a method parameter — the parent component passes values in" |
| `@track` (legacy) | "Signals to the framework that this variable should trigger a re-render when it changes — like how changing a property in Apex triggers downstream logic" |
| `connectedCallback()` | "Like the constructor in Apex — runs once when the component is first created" |
| `disconnectedCallback()` | "Like an Apex finalizer — runs when the component is removed from the page" |
| JavaScript Promise | "Like a future method return — the work happens asynchronously, and you handle the result when it arrives" |
| `async/await` | "Like writing synchronous Apex, but the runtime knows to wait for the async result before continuing" |
| Event bubbling | "Like how a trigger can fire parent-object automation — an event starts at the child and travels up the component tree" |
| Shadow DOM | "Like namespace isolation in managed packages — each component's styles and HTML are scoped so they don't bleed into other components" |
| CSS `overflow` | "Like a container that clips its content — if a child element is bigger than its parent, you control whether it's hidden, scrollable, or allowed to overflow" |
| `const` / `let` | "`const` is a final variable (can't be reassigned); `let` is a regular variable. Both are block-scoped — like a local variable inside an Apex method." |
| Arrow function | "A shorthand way to write a method — `(x) => x * 2` is the same as a one-line Apex method that takes one parameter and returns a result" |

Add new bridge phrases whenever a concept not in this table appears in the codebase.

### Mermaid Diagram Types

Choose the diagram type that best matches the concept being illustrated:

- **`sequenceDiagram`** — for data flows: user action → LWC → Apex → DB → back. Use for any request/response or event chain.
- **`classDiagram`** — for component trees and class relationships. Show which LWC components are parents/children, which Apex classes are called by which components.
- **`graph TD`** — for call graphs and decision trees. Show which method calls which method, or which code path executes under which condition.
- **`flowchart LR`** — for left-to-right architecture overviews. Good for showing the platform layer stack (Browser → LWC → Apex → Database → External System).

Always place diagrams inside a `.code-block.mermaid-wrapper` container. Always add a `.diagram-caption` with a one-sentence plain-English description of what the diagram shows.

---

## Design Identity

The visual design should feel like a **beautiful developer notebook** — warm, inviting, and distinctive. Read `references/design-system.md` for the full token system, but here are the non-negotiable principles:

- **Warm palette**: Off-white backgrounds (like aged paper), warm grays, NO cold whites or blues
- **Bold accent**: One confident accent color (vermillion, coral, teal — NOT purple gradients). For Salesforce projects, Salesforce blue (`#0176D3`) can be used as a subtle secondary accent for platform-layer UI elements, but the main course accent should remain a warm, distinct color.
- **Distinctive typography**: Display font with personality for headings (Bricolage Grotesque, or similar bold geometric face — NEVER Inter, Roboto, Arial, or Space Grotesk). Clean sans-serif for body (DM Sans or similar). JetBrains Mono for code and Mermaid diagrams.
- **Generous whitespace**: Modules breathe. Max 3-4 short paragraphs per screen.
- **Alternating backgrounds**: Even/odd modules alternate between two warm background tones for visual rhythm
- **Dark code blocks**: IDE-style with Catppuccin-inspired syntax highlighting on deep indigo-charcoal (#1E1E2E) — Mermaid diagrams inherit this same dark background via the theme config in `_base.html`
- **Depth without harshness**: Subtle warm shadows, never black drop shadows

---

## Reference Files

The `references/` directory contains detailed specs. **Read them only when you reach the relevant phase** — not upfront. This keeps context lean.

- **`references/content-philosophy.md`** — Visual density rules, metaphor guidelines, quiz design, tooltip rules, code translation guidance. Read during Phase 2.5 (briefs) and Phase 3 (writing modules).
- **`references/gotchas.md`** — Common failure points checklist. Read during Phase 3 and Phase 4 (review).
- **`references/module-brief-template.md`** — Template for Phase 2.5 module briefs. Read only for complex codebases using the parallel path.
- **`references/design-system.md`** — Complete CSS custom properties, color palette, typography scale, spacing system, shadows, animations, scrollbar styling. Read during Phase 3 when writing module HTML.
- **`references/interactive-elements.md`** — Implementation patterns for every interactive element: drag-and-drop quizzes, multiple-choice quizzes, code↔English translations, group chat animations, message flow visualizations, architecture diagrams, pattern cards, callout boxes. Read the relevant sections during Phase 3.
