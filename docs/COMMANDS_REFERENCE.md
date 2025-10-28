# PrivateStudio Commands & Agents Reference

Quick reference guide for all slash commands and agents in the project.

---

## 🏗️ Building Commands (Initial Setup)

These create the website from scratch.

| Command | What It Does | When to Use |
|---------|--------------|-------------|
| `/install-dependencies` | Installs Tailwind, GSAP, and required packages | First time setup |
| `/create-design-tokens` | Creates CSS variables for colors, fonts, spacing | Before building components |
| `/create-animations` | Creates animation keyframes and utilities | Before building components |
| `/create-base-layout` | Creates main HTML structure and layout | Start of build |
| `/create-nav` | Creates responsive navigation with hamburger menu | After base layout |
| `/create-footer` | Creates footer with contact info and social links | After base layout |
| `/create-hero` | Creates hero section with HYPER BIG typography | First major section |
| `/create-horizontal-scroll` | Creates horizontal text scroll on vertical scroll | After hero |
| `/create-team-section` | Creates 4 barbers showcase with Booksy links | Main content section |
| `/create-products-section` | Creates STMNT products grid | Main content section |
| `/create-about-section` | Creates about section with text effects | Main content section |
| `/extract-images` | Sets up placeholder image structure | Before finalizing |

---

## ✏️ Modification Commands (Changing Existing Code)

These update or modify what's already built.

| Command | What It Does | When to Use |
|---------|--------------|-------------|
| `/update-hero-text` | Changes hero headline and CTA text | Content updates |
| `/update-colors` | Modifies color scheme (backgrounds, accents) | Design tweaks |
| `/update-typography` | Changes font sizes, weights, or families | Typography adjustments |
| `/update-spacing` | Adjusts padding, margins, and gaps | Layout refinement |
| `/update-animations` | Modifies animation speeds or effects | Animation tuning |
| `/replace-images` | Swaps placeholder images with real photos | Adding real content |
| `/update-barber-info` | Modifies barber names, photos, or Booksy links | Team updates |
| `/update-products` | Changes product names, descriptions, prices | Product catalog updates |
| `/update-footer-info` | Updates contact info, hours, or address | Business info changes |
| `/fix-responsive-issue` | Fixes mobile/tablet layout problems | Bug fixes |

---

## 🧪 Testing Commands (Quality Assurance)

These test the site and report issues (don't fix automatically).

| Command | What It Does | When to Use |
|---------|--------------|-------------|
| `/test-responsive` | Tests all breakpoints (mobile, tablet, desktop) | After building/changes |
| `/test-lighthouse` | Runs Google Lighthouse performance audit | Before deployment |
| `/test-accessibility` | Checks WCAG compliance and a11y issues | Before deployment |
| `/test-links` | Validates all internal and external links work | Before deployment |
| `/optimize-images` | Checks and compresses images for web | Performance optimization |
| `/test-cross-browser` | Tests in Chrome, Firefox, Safari, Edge | Before deployment |

---

## 🤖 Agents (Specialized Workflows)

### QA Tester Sub-Agent
**Location:** `.claude/agents/qa-tester.md`

**Purpose:** Runs comprehensive testing in parallel

**When to use:**
- Say: "Test the PrivateStudio website"
- Say: "Run QA tests"
- Before deployment
- After major changes

**What it does:**
1. Runs all 6 testing commands in parallel
2. Collects results from each test
3. Categorizes issues by severity
4. Provides actionable recommendations
5. Reports findings without fixing

**Output:** Comprehensive test report with priority issues

---

## 🎯 The Skill (Orchestrator)

### PrivateStudio Manager Skill
**Location:** `.claude/skills/privatestudio-manager/`

**Purpose:** Coordinates all commands and manages the entire site

**When to use:**
- Say: "Build the complete PrivateStudio website" (initial build)
- Say: "Add a new barber named [Name]" (content updates)
- Say: "Update the accent color to [color]" (design changes)
- Say: "Test the website" (runs QA sub-agent)

**What it does:**
- Decides which commands to run
- Executes them in the right order
- Handles complex multi-step workflows
- Coordinates between building, modifying, and testing

**The skill is the "manager"** - it uses all the commands as building blocks.

---

## 📋 Quick Workflows

### Initial Build Workflow
```
"Build the complete PrivateStudio website"
```
**Skill runs:** All building commands → Creates complete site

---

### Testing Workflow
```
"Test the PrivateStudio website"
```
**Skill spawns:** QA Tester sub-agent → Runs all tests in parallel

---

### Content Update Workflow
```
"Add a new barber named Carlos García, specialty 'Fade Expert'"
```
**Skill runs:** `/update-barber-info` → Updates team section

---

### Design Change Workflow
```
"Change the accent color to gold and make the hero text bigger"
```
**Skill runs:** `/update-colors` + `/update-typography` → Updates design

---

### Fix Issue Workflow
```
"The team section is breaking on mobile"
```
**Skill runs:** `/fix-responsive-issue` → Diagnoses and fixes

---

## 🎓 Dan's Hierarchy Applied

```
                 SKILL
        (privatestudio-manager)
                   |
      ┌────────────┼────────────┐
      |            |            |
   BUILDING     MODIFYING    TESTING
   COMMANDS     COMMANDS     COMMANDS
      |            |            |
   (12 cmds)    (10 cmds)    (6 cmds)
                               |
                          QA SUB-AGENT
                         (parallel tests)
```

### The Pattern:
1. **Slash Commands** = Primitives (single operations)
2. **Sub-Agent** = Parallel isolated work (testing)
3. **Skill** = Manager (coordinates everything)

---

## 💡 When to Use What

### Use Individual Commands When:
- Testing a specific feature: `/test-responsive`
- Making a small change: `/update-hero-text`
- Building one component: `/create-hero`
- Experimenting with settings

### Use the Sub-Agent When:
- You want comprehensive testing
- You need parallel execution
- You want a full QA report

### Use the Skill When:
- Building the entire site
- Making multiple related changes
- You want automatic coordination
- You're not sure which commands to use

---

## 🚀 Most Common Commands

**For beginners, you'll mainly use:**

1. **"Build the complete PrivateStudio website"** → Full site build
2. **"Test the website"** → QA report
3. **"Update [something]"** → Content/design changes
4. **"Fix [issue]"** → Bug fixes

The skill figures out the rest!

---

## 📦 File Locations

```
privatestudio-v2/
├── .claude/
│   ├── commands/          ← 28 slash commands here
│   ├── agents/            ← QA tester sub-agent here
│   └── skills/            ← PrivateStudio manager skill here
└── docs/
    ├── IMPLEMENTATION_PLAN.md
    └── COMMANDS_REFERENCE.md  ← You are here
```

---

## 🎯 Success Metrics

After running commands, check:
- ✅ Lighthouse score 90+
- ✅ Mobile responsive (all breakpoints)
- ✅ All links work
- ✅ Accessibility WCAG AA
- ✅ No console errors
- ✅ Smooth 60fps animations

---

**Need help?** Just ask the skill in natural language - it knows which commands to use!
