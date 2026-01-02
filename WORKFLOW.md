# Human-AI Collaboration Workflow

**Version 1.0.0**

This document defines the collaboration model between humans and AI systems when using Bricks.

---

## Philosophy

> The AI never decides alone. The human retains control.

Bricks enables a collaboration where:
- **Human** provides vision, validates choices, controls quality
- **AI** executes technically, proposes solutions, respects constraints

---

## Roles and Responsibilities

### Human (Pilot)

| Responsibility | Actions |
|----------------|---------|
| **Vision** | Defines concept, target audience, tone |
| **Content** | Provides or validates text and images |
| **Validation** | Tests output, approves or reports issues |
| **Decision** | Makes choices when multiple options exist |

### AI (Executor)

| Responsibility | Actions |
|----------------|---------|
| **Technical** | Creates files, assembles bricks |
| **Proposal** | Suggests structures, requests validation |
| **Documentation** | Maintains project state |
| **Quality** | Follows workflow, doesn't take shortcuts |

---

## Workflow Phases

```
1. Brief      →  Human provides, AI documents
2. Planning   →  AI proposes structure, Human validates
3. Data       →  AI requests content, Human provides/approves
4. Build      →  AI creates, Human tests
5. Iterate    →  Human reports issues, AI fixes
6. Next       →  Human says "ok", AI continues
```

### Phase 1: Brief

**Human** describes the project:
- Concept and goals
- Target audience
- Pages needed
- Visual style preferences

**AI** produces a structured brief document:
```markdown
# Project Name

## Concept
[Description]

## Pages
- Page 1: [description]
- Page 2: [description]

## Theme
- Colors: [palette]
- Typography: [fonts]
- Tone: [voice]
```

### Phase 2: Planning

**AI** proposes:
- Brick selection for each page
- Data structure
- Component hierarchy

**Human** validates or adjusts the plan.

### Phase 3: Data Collection

**Critical Rule**: Always ask before inventing.

**AI** (before creating a page):
```
I'm building [page.html]. I need content for:
- Hero section: title, subtitle, CTA
- Cards section: 4 items with title + description
- FAQ section: 5 questions/answers

Options:
1. You provide the content
2. I draft content for your review
3. We skip this section
```

**Human** chooses and provides content if needed.

**AI** adds content to data file BEFORE creating HTML.

### Phase 4: Page Creation

**AI** creates one page at a time:
1. Verifies data availability
2. Creates composition JSON
3. Generates output (HTML)
4. Confirms completion

```
**Done:**
- ✅ compositions/about.json
- ✅ about.html
- ✅ Progress updated (3/10)

You can test `about.html`.
```

### Phase 5: Validation

**Human**:
- Opens page in browser
- Tests rendering (desktop + mobile)
- Says "ok" OR reports issue

**If issue**:
- Human describes precisely (screenshot if possible)
- AI fixes
- Return to validation

**If ok**:
- AI proceeds to next page

---

## Communication Patterns

### AI → Human (Before Page Creation)

```
I'm building the contact page.

Required content:
- Page title and intro
- Contact details (address, phone, email)
- Form fields
- FAQ (optional)

Do you want to provide this content or should I draft it?
```

### AI → Human (After Page Creation)

```
**Done:**
- ✅ compositions/contact.json
- ✅ contact.html
- ✅ Progress: 8/10

You can test `contact.html`.
```

### Human → AI (Validation)

Approval:
```
ok
```

Issue report:
```
Bug: The submit button is not aligned on mobile.
[screenshot]
```

### AI → Human (After Fix)

```
Fixed: Added `text-align: center` to `.form__actions` for mobile.

Please re-test.
```

---

## Rules for AI

### 1. Ask Before Inventing

Never create content inline in HTML. Always:
1. Identify missing content
2. Ask the human
3. Add to data file
4. Then create the page

### 2. One Page = One Cycle

```
Create → Confirm → Wait for "ok" → Next page
```

Never batch multiple pages without validation.

### 3. Maintain Documentation

After each validated page:
- Update progress tracker
- Document decisions made

### 4. No Shortcuts

- Use theme classes
- Avoid inline `style=""`
- Respect the design system

### 5. Sync Files

When modifying theme files, update consolidated files too (and vice versa).

---

## Rules for Humans

### 1. Test Before Approving

Actually open the page in a browser. Check:
- Visual rendering
- Interactions (clicks, accordions, etc.)
- Responsive behavior (mobile)

### 2. Report Precisely

❌ "It doesn't work"

✅ "The accordion doesn't open when I click the second question"

✅ Screenshot + description of expected vs actual behavior

### 3. One Issue at a Time

Allows clean fixes without mixing problems.

### 4. Validate Data

If content seems invented, ask to see the data file.

---

## Anti-patterns

| Anti-pattern | Problem | Solution |
|--------------|---------|----------|
| Batch 5 pages at once | No validation, accumulated bugs | One page at a time |
| Invent content in HTML | Scattered data, inconsistencies | Data file first |
| "I did everything, it's done" | No visibility on work | Detailed action list |
| Modify theme without sync | Desynchronization | Update all related files |
| Ignore reported bugs | Frustration, degraded quality | Fix before continuing |

---

## Checklist

### For Each Page

**AI:**
- [ ] Did I ask about missing content?
- [ ] Did I add data BEFORE HTML?
- [ ] Did I use theme classes?
- [ ] Did I list what I did?
- [ ] Did I wait for "ok"?

**Human:**
- [ ] Did I test in browser?
- [ ] Did I check desktop + mobile?
- [ ] Did I report bugs clearly?
- [ ] Did I say "ok" when satisfied?

---

## Summary

```
Human pilots, AI executes.
Ask before inventing.
One page at a time.
"ok" to continue.
```

---

*One brick at a time.*
