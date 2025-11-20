# Using BMad in ChatGPT, Claude, and Gemini

Use BMad Method anywhere with web bundles - standalone XML files containing complete agents that work in ChatGPT, Claude Projects, and Gemini Gems **without any local installation**.

## Quick Links

Choose your platform:

- **[Using BMad in ChatGPT →](./using-bmad-in-chatgpt.md)** - Custom GPTs setup and usage
- **[Using BMad in Claude →](./using-bmad-in-claude.md)** - Claude Projects integration
- **[Using BMad in Gemini →](./using-bmad-in-gemini.md)** - Gemini Gems configuration (recommended)

## What Are Web Bundles?

Web bundles are self-contained XML files that package everything an agent needs:

- ✅ Complete agent persona and instructions
- ✅ All workflows and dependencies
- ✅ Interactive menu system
- ✅ Party mode for multi-agent collaboration
- ✅ No external files required

**Perfect for:** Cost-effective planning phases before local implementation

## Platform Comparison

| Feature | Gemini Gems | Claude Projects | ChatGPT Custom GPTs |
|---------|-------------|-----------------|---------------------|
| **Recommended** | ⭐⭐⭐ Best | ⭐⭐ Excellent | ⭐ Good |
| XML Handling | Excellent | Excellent | Good |
| Context Window | Large (best) | Large | Medium |
| Team Bundles | ✅ Yes (2.5 Pro+) | ✅ Yes (Sonnet+) | ❌ Not recommended |
| Document Generation | Code Execution | Artifacts | Canvas |
| Best For | All workflows | Complex workflows | Simple workflows |
| Cost | $$$ | $$$ | $$ |

### Platform Recommendations

**Use Gemini Gems when:**
- You want the best overall experience
- Working with team bundles (multiple agents)
- Need complex multi-step workflows
- Handling large documents (PRD, Architecture)

**Use Claude Projects when:**
- You prefer Claude's reasoning style
- Need strong analytical capabilities
- Working on technical architecture
- Want excellent artifact generation

**Use ChatGPT Custom GPTs when:**
- You're most familiar with ChatGPT
- Need quick prototyping
- Working with creative workflows (CIS)
- Using simple, focused agents

## Getting Web Bundles

### Option A: Download Pre-Built Bundles (Recommended)

Download ready-to-use bundles automatically updated with every commit:

**[→ Download Web Bundles](https://bmad-code-org.github.io/bmad-bundles/)**

Navigate to: `module folder → agents folder → download .xml file`

### Option B: Generate from Local Installation

```bash
# Generate all bundles
npm run bundle

# Generate specific module
node tools/cli/bundlers/bundle-web.js module bmm

# Generate specific agent
node tools/cli/bundlers/bundle-web.js agent bmm pm
```

**Output location:** `web-bundles/` directory

## Available Bundles

### BMad Method (BMM) - Agile Development

**Individual Agents:**
- `analyst.xml` - Requirements gathering and analysis
- `pm.xml` - Product management and planning
- `architect.xml` - System architecture and design
- `dev.xml` - Full-stack development
- `sm.xml` - Scrum master and agile facilitation
- `tea.xml` - Test architecture and QA
- `ux-designer.xml` - User experience design
- `tech-writer.xml` - Technical documentation
- `game-designer.xml` - Game design
- `game-dev.xml` - Game development
- `game-architect.xml` - Game architecture

**Team Bundles** (Gemini 2.5 Pro+ or Claude Projects only):
- `team-fullstack.xml` - Complete development team

### BMad Builder (BMB) - Agent Creation

- `bmad-builder.xml` - Create custom agents, workflows, and modules

### Creative Intelligence Suite (CIS) - Creative Facilitation

- `brainstorming-coach.xml` - Creative brainstorming (Carson)
- `design-thinking-coach.xml` - Human-centered design (Maya)
- `creative-problem-solver.xml` - Problem solving (Dr. Quinn)
- `innovation-strategist.xml` - Innovation strategy (Victor)
- `storyteller.xml` - Narrative and storytelling (Sophia)

**Team Bundle:**
- `creative-squad.xml` - All CIS agents together

### BMad Game Development (BMGD)

**Team Bundle:**
- `team-gamedev.xml` - Game development team

## Cost-Saving Strategy: Web + Local Hybrid

**Save 60-80% on costs** by doing planning in web bundles, then switching to local for implementation.

### Recommended Workflow

**Phases 1-3: Use Web Bundles** (Analysis, Planning, Architecture)
1. **Analysis** - Brainstorm, research, product brief
2. **Planning** - Create PRD, define epics and stories
3. **Solutioning** - Design architecture and UX

**Export Artifacts** - Download generated documents (PRD, Architecture, UX Design)

**Phase 4: Switch to Local** (Implementation)
1. Save artifacts to project `docs/` folder
2. Install BMad locally: `npx bmad-method@alpha install`
3. Run `*workflow-init` to detect artifacts
4. Implement with Developer agent and full IDE capabilities

**Why this works:**
- Planning workflows don't need code context (perfect for web)
- Implementation needs full codebase access (requires local IDE)
- Best of both worlds: Cost savings + full capabilities

## Common Use Cases

### Use Case 1: New Product Planning

**Goal:** Plan a new SaaS product before building

**Setup:**
1. Download `pm.xml`, `architect.xml`, `ux-designer.xml`
2. Create separate instances (Gem/Project/GPT for each)
3. Enable document generation (Code Execution/Artifacts/Canvas)

**Workflow:**
1. PM: `*product-brief` → Product vision
2. PM: `*prd` → Requirements document
3. UX Designer: `*create-ux-design` → UX specifications
4. Architect: `*architecture` → Technical design
5. Export all documents
6. Switch to local for implementation

**Cost Savings:** ~70%

### Use Case 2: Creative Brainstorming

**Goal:** Generate innovative ideas for a feature

**Setup:**
1. Download `brainstorming-coach.xml`
2. Create Gemini Gem or Claude Project

**Workflow:**
1. Run `*brainstorming`
2. Choose technique (SCAMPER, Mind Mapping, etc.)
3. Generate and refine ideas
4. Export results

**Time:** 30-60 minutes

### Use Case 3: Architecture Review

**Goal:** Review and improve existing architecture

**Setup:**
1. Download `architect.xml`
2. Create Claude Project (excellent for technical work)

**Workflow:**
1. Share existing architecture docs
2. Run `*architecture`
3. Discuss and refine design
4. Export updated architecture

### Use Case 4: Complete Game Design

**Goal:** Design a game from concept to technical specs

**Setup:**
1. Download `team-gamedev.xml`
2. Create Gemini Gem (2.5 Pro+) - team bundles require large context

**Workflow:**
1. Run `*game-brief` → Game concept
2. Run `*game-architecture` → Technical design
3. Export all game design documents
4. Implement locally

## Critical Setup Rules

Follow these rules for best results:

1. ✅ **One file per instance** - Upload exactly ONE XML file per Gem/Project/GPT
2. ✅ **Use setup instructions** - Add configuration prompts (see platform guides)
3. ✅ **Enable document generation** - Code Execution (Gemini), Artifacts (Claude), Canvas (ChatGPT)
4. ✅ **Prefer Gemini or Claude** - Best performance for BMad workflows
5. ✅ **Team bundles need premium models** - Gemini 2.5 Pro+ or Claude Sonnet 3.5+
6. ✅ **Create separate instances per agent** - Don't combine agents manually

## Basic Usage

### 1. Load the Agent

Upload or paste XML into your chosen platform. The agent introduces itself with a menu.

### 2. Choose a Workflow

Use natural language or shortcuts:

```
"Create a PRD for my project"
*prd

"Let's brainstorm ideas"
*brainstorm-project

"Show the menu"
*help
```

### 3. Follow the Workflow

The agent guides you step-by-step, asking questions and creating deliverables.

### 4. Export Results

Copy or download generated documents to your project's `docs/` folder.

## Advanced Features

### Party Mode

All bundles include party mode for multi-agent collaboration:

```
*party
```

Multiple agent personas collaborate on your task, providing diverse perspectives.

### Context Loading

```
*workflow-init     # Initialize project workflow
*document-project  # Analyze existing codebase (limited in web)
```

### Dynamic Menus

Agents adapt menus based on project phase and available workflows.

## Troubleshooting

**Agent not responding correctly?**
- Verify entire XML was uploaded (no truncation)
- Check setup instructions were added
- Try a simpler agent first (analyst, pm)

**Menu items not working?**
- Use `*` prefix: `*prd` not `prd`
- Or natural language: "Run the PRD workflow"
- Check menu: `*help`

**Workflows failing?**
- Some workflows expect project files (use local installation)
- Use planning/analysis workflows in web
- Implementation workflows require local IDE

**File too large?**
- Use Gemini or Claude (better than ChatGPT for large files)
- Use individual agents instead of team bundles
- Split into multiple conversations

## Platform-Specific Guides

For detailed setup instructions, see:

- **[ChatGPT Setup Guide →](./using-bmad-in-chatgpt.md)**
- **[Claude Projects Setup Guide →](./using-bmad-in-claude.md)**
- **[Gemini Gems Setup Guide →](./using-bmad-in-gemini.md)**

## Best Practices

1. **One File Per Instance** - Never combine multiple agent XMLs
2. **Enable Document Generation** - Code Execution/Artifacts/Canvas
3. **Use Premium Models** - Better performance, especially for team bundles
4. **Plan in Web, Build Locally** - Maximize cost savings
5. **Export Artifacts** - Save generated docs to project folder
6. **Test with Simple Agents First** - PM, Analyst work great
7. **Keep Bundles Updated** - Download latest from web or rebuild

## Next Steps

1. **Choose your platform** - Review comparison above
2. **Download bundles** - Get from [BMad Bundles](https://bmad-code-org.github.io/bmad-bundles/)
3. **Follow platform guide** - See links above
4. **Start with Analysis/Planning** - Try `*product-brief` or `*brainstorm-project`
5. **Export and go local** - Switch to local IDE for implementation

## Additional Resources

- **[Web Bundle Technical Docs](./web-bundles-gemini-gpt-guide.md)** - Comprehensive technical guide
- **[Agent Customization](./agent-customization-guide.md)** - Customize before bundling
- **[BMM Documentation](../src/modules/bmm/docs/README.md)** - All workflows explained
- **[BMad Discord](https://discord.gg/gk8jAdXWmj)** - Get help and share experiences

---

**Ready to start?** Choose your platform above and follow the setup guide!
