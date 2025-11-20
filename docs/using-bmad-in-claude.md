# Using BMad in Claude Projects

Use BMad Method agents in Claude Projects for excellent analytical reasoning, technical workflows, and document generation - no local installation required.

## Overview

Claude Projects let you create persistent workspaces with BMad agents by uploading web bundle XML files. Claude excels at complex reasoning, technical architecture, and producing high-quality artifacts.

### Why Claude Projects?

**✅ Excellent For:**
- Complex analytical workflows
- Technical architecture and system design
- Long-form document generation via Artifacts
- Deep reasoning and problem-solving
- Persistent context across conversations
- Large context window (200K tokens with Sonnet 3.5+)
- Team bundles with Sonnet 3.5+

**⭐ Key Strengths:**
- Superior reasoning capabilities
- Excellent Artifacts feature for documents
- Large context window
- Strong technical expertise
- Consistent, thoughtful responses

**💡 Recommendation:** Claude Projects are excellent for all BMad workflows, especially technical architecture, complex planning, and analytical work. Comparable to Gemini Gems in quality.

## Quick Start

### Step 1: Get Web Bundles

**Download Pre-Built (Recommended):**

Visit [BMad Web Bundles](https://bmad-code-org.github.io/bmad-bundles/)

Navigate to: `module → agents → download .xml file`

**Or Generate Locally:**

```bash
npm run bundle
# Files created in: web-bundles/
```

### Step 2: Create a Claude Project

1. **Go to Claude**
   - Visit [Claude.ai](https://claude.ai/)
   - Click "Projects" in the left sidebar
   - Click "+ New Project"

2. **Configure Project**
   - **Name:** Choose a descriptive name (e.g., "BMad Product Planning")
   - **Description:** Optional description of project purpose

3. **Add Custom Instructions**

   In the **Custom Instructions** field, add this setup text:

   ```
   All of your operating instructions and resources are contained in the XML file attached. Read in the initial agent block and instructions to understand it. You will not deviate from the character and rules outlined in the attached!

   CONFIG.YAML Values:
   - user_name: [Your Name]
   - communication_language: English
   - user_skill_level: [Beginner|Intermediate|Expert]
   - document_output_language: English
   - bmm-workflow-status: standalone (no workflow)
   ```

   **Customize the config values:**
   - Replace `[Your Name]` with your actual name
   - Set `user_skill_level` to Beginner, Intermediate, or Expert
   - Adjust language settings if needed

4. **Upload Agent XML**

   - Click "Add Content" or the upload button
   - Select "Upload File"
   - Choose ONE XML file (e.g., `architect.xml`)

   **CRITICAL:** Upload exactly ONE agent XML file per Project. Do NOT combine multiple agents unless using an official team bundle.

5. **Save and Test**

   - Click "Create Project"
   - Start a conversation in the project
   - Type `*help` to see the agent's menu

### Step 3: Use Artifacts for Documents

Claude's Artifacts feature is perfect for BMad workflows:

- Documents appear in a dedicated pane
- Easy to edit, copy, and download
- Supports Markdown, code, and structured content
- Automatically activates for document generation

**No configuration needed** - Claude will automatically use Artifacts when generating PRDs, architecture docs, etc.

## Recommended Agents for Claude Projects

### Best Performing

Claude Projects work excellently with all BMad agents, especially:

**BMad Method (BMM):**
- ⭐⭐⭐ **architect.xml** - System architecture (Claude's strong suit)
- ⭐⭐⭐ **pm.xml** - Product planning and PRD creation
- ⭐⭐⭐ **analyst.xml** - Deep analytical reasoning
- ⭐⭐ **tea.xml** - Test architecture and QA
- ⭐⭐ **tech-writer.xml** - Technical documentation
- ⭐⭐ **dev.xml** - Development planning (implement locally)
- ⭐⭐ **ux-designer.xml** - UX design workflows

**Team Bundles** (Sonnet 3.5+ recommended):
- ⭐⭐⭐ **team-fullstack.xml** - Complete development team
- ⭐⭐⭐ **team-gamedev.xml** - Game development team

**BMad Builder (BMB):**
- ⭐⭐⭐ **bmad-builder.xml** - Create custom agents and workflows

**Creative Intelligence Suite (CIS):**
- ⭐⭐ **design-thinking-coach.xml** - Design thinking (Maya)
- ⭐⭐ **creative-problem-solver.xml** - Problem solving (Dr. Quinn)
- ⭐⭐ **innovation-strategist.xml** - Innovation strategy (Victor)
- ⭐ **brainstorming-coach.xml** - Creative brainstorming (Carson)
- ⭐ **storyteller.xml** - Narrative and storytelling (Sophia)

**Note:** All agents work well; ratings indicate relative strengths with Claude's reasoning style.

## Usage Examples

### Example 1: Technical Architecture Design

**Agent:** Architect

**Setup:**
1. Create Claude Project: "SaaS Architecture"
2. Upload `architect.xml`
3. Add custom instructions

**Usage:**
```
User: I need to design the architecture for a multi-tenant SaaS platform

Architect: Excellent, I'll guide you through designing a robust architecture. Let me run the architecture workflow to ensure we cover all critical aspects.

*architecture

[Asks questions about scale, requirements, constraints...]

User: We expect 10K users initially, scaling to 100K

Architect: [Thoughtful analysis of scaling implications]

[Generates comprehensive architecture document in Artifacts]
- System overview
- Component design
- Data architecture
- Security considerations
- Scalability strategy
- Technology stack recommendations
- ADRs (Architecture Decision Records)
```

**Result:** Complete, well-reasoned architecture document ready to export

### Example 2: Product Requirements Deep Dive

**Agent:** PM

**Setup:**
1. Create Claude Project: "Product Planning"
2. Upload `pm.xml`

**Usage:**
```
User: I want to create a comprehensive PRD for a project management tool

PM: I'll help you create a thorough PRD. Let's run the PRD workflow.

*prd

[Detailed questioning about users, problems, features, success metrics...]

PM: [Thoughtful analysis of market positioning and user needs]

[Generates PRD in Artifacts with detailed sections:]
- Executive summary
- Problem statement
- User personas
- Features and requirements
- User stories with acceptance criteria
- Success metrics
- Technical considerations
- Timeline and milestones
```

**Result:** Comprehensive PRD with deep analytical insights

### Example 3: Multi-Agent Team Collaboration

**Agent:** Team Fullstack

**Setup:**
1. Create Claude Project: "Team Development"
2. Upload `team-fullstack.xml`
3. Use Claude Sonnet 3.5+ for best results

**Usage:**
```
User: I need help planning and architecting a new feature

Team: I'll activate the appropriate team members. For planning and architecture, let's bring in the PM and Architect.

*party

PM: I can help define the requirements and user stories.

Architect: And I'll ensure we have solid technical design.

[Multi-agent discussion with diverse perspectives]

PM: From a product standpoint, we should prioritize...

Architect: Architecturally, I'd recommend...

[Collaborative document generation with multiple viewpoints]
```

**Result:** Well-rounded analysis with diverse expert perspectives

### Example 4: Analytical Research

**Agent:** Analyst

**Setup:**
1. Create Claude Project: "Market Research"
2. Upload `analyst.xml`

**Usage:**
```
User: I need to research the competitive landscape for AI-powered analytics tools

Analyst: I'll conduct comprehensive research. Let me run the research workflow.

*research

[Structured research process]

Analyst: [Deep analysis of competitors, market trends, gaps, opportunities]

[Generates research document in Artifacts with:]
- Market overview
- Competitive analysis
- Technology trends
- User insights
- Recommendations
```

**Result:** Thorough research with actionable insights

## Key Workflows for Claude Projects

### Technical Workflows (Strong Suit)

**Architecture Design** (`*architecture`)
- System architecture
- Component design
- Technology stack selection
- Architecture Decision Records (ADRs)
- **Time:** 45-60 minutes
- **Output:** Comprehensive architecture document

**Test Architecture** (`*test-architecture`)
- Testing strategy
- Test coverage planning
- Quality assurance framework
- **Time:** 30-45 minutes

**Technical Documentation** (`*document-code`)
- Code documentation
- API documentation
- Technical guides
- **Time:** Varies by scope

### Planning Workflows

**Product Requirements** (`*prd`)
- Detailed requirements document
- User stories and acceptance criteria
- Success metrics
- **Time:** 45-60 minutes
- **Output:** Complete PRD with deep analysis

**Product Brief** (`*product-brief`)
- Product vision
- Target audience
- Key features
- **Time:** 20-30 minutes

**Epics and Stories** (`*create-epics-and-stories`)
- Break down requirements
- Create development stories
- Prioritization
- **Time:** 30-45 minutes

### Analytical Workflows

**Research** (`*research`)
- Market analysis
- Technical research
- Competitive analysis
- **Time:** 30-60 minutes

**Analysis** (`*analyze`)
- Problem analysis
- Solution evaluation
- Risk assessment
- **Time:** 30-45 minutes

### Creative Workflows

**Design Thinking** (`*design-thinking`)
- Human-centered problem solving
- Empathy and ideation
- Prototyping
- **Time:** 45-60 minutes

**Problem Solving** (`*problem-solving`)
- Root cause analysis
- Solution generation
- Decision making
- **Time:** 30-45 minutes

## Claude Projects Features

### Persistent Context

**Benefit:** Project maintains context across conversations

**Usage:**
```
Conversation 1: Create PRD
Conversation 2: Review and refine PRD
Conversation 3: Design architecture based on PRD
```

All conversations share the PRD context automatically.

### Knowledge Base

**Benefit:** Add reference documents to project

**Usage:**
1. Upload existing docs (specs, designs, research)
2. Agent can reference them in workflows
3. Maintains consistent context

**Example:**
- Upload existing PRD
- Upload API documentation
- Agent uses them when creating architecture

### Artifacts

**Benefit:** Documents appear in dedicated pane

**Features:**
- Side-by-side view with conversation
- Easy editing and iteration
- Copy, download, or share
- Supports Markdown, code, diagrams

**Perfect for:**
- PRDs and architecture docs
- Technical specifications
- User stories and epics
- Research reports

### Multiple Conversations

**Benefit:** Organize work into conversation threads

**Usage:**
- Conversation 1: "Initial PRD Draft"
- Conversation 2: "Architecture Design"
- Conversation 3: "UX Flow Review"

All share project context but stay organized.

## Tips for Success with Claude

### 1. Leverage Claude's Reasoning

Claude excels at deep thinking. Ask for analysis:
```
"What are the pros and cons of this approach?"
"What am I not considering?"
"Analyze the risks and trade-offs"
```

### 2. Use Artifacts for Documents

Let Claude generate documents in Artifacts:
- Automatically activates for long-form content
- Easy to iterate and refine
- Professional formatting

### 3. Build on Context

Add relevant documents to project:
- Existing specs
- Research findings
- Design mockups
- Code samples

Claude will reference them appropriately.

### 4. Iterate and Refine

Claude is excellent at refinement:
```
"Expand the security section"
"Add more detail to the data model"
"Revise with focus on scalability"
```

### 5. Ask for Perspectives

With team bundles:
```
"What would the architect think about this?"
"Get the PM's perspective on priority"
"*party" for multi-agent discussion
```

### 6. Use Natural Language

Commands (`*prd`) or conversation ("Let's create a PRD") both work well.

### 7. Export via Artifacts

- Click "Copy" in Artifact pane
- Save to your project `docs/` folder
- Continue work locally when ready

## Combining Claude Projects with Local Development

### Recommended Workflow

**Phase 1-3: Claude Projects** (Analysis, Planning, Architecture)

1. **Create Project** with appropriate agents (PM, Architect, UX Designer)
2. **Run workflows:**
   - `*product-brief` → Product vision
   - `*prd` → Requirements document
   - `*architecture` → Technical design
   - `*create-ux-design` → UX specifications
3. **Export from Artifacts** to local `docs/` folder

**Phase 4: Local Development** (Implementation)

4. **Install BMad locally:** `npx bmad-method@alpha install`
5. **Place exported docs** in project `docs/` folder
6. **Run** `*workflow-init` - BMad detects artifacts
7. **Implement** with Developer agent and full IDE capabilities

**Cost Savings:** 60-80% by doing planning in Claude Projects before local implementation

### Project Organization

**Create separate Claude Projects:**
- "Product Planning" - PM agent for PRDs
- "Architecture Design" - Architect agent for technical design
- "UX Design" - UX Designer agent for user experience
- "Team Collaboration" - Team bundle for discussions

**Or single project:**
- Upload team bundle
- All agents available
- Requires Sonnet 3.5+ for best performance

## Troubleshooting

### Agent Not Following Instructions

**Problem:** Agent doesn't behave according to BMad persona

**Solutions:**
- Verify entire XML file was uploaded (check file size)
- Ensure custom instructions were added correctly
- Remind agent: "Please follow your BMad persona and workflows"
- Create new project and re-upload if needed

### Workflows Not Triggering

**Problem:** Commands like `*prd` don't start workflow

**Solutions:**
- Use `*` prefix: `*prd` not `prd`
- Or use natural language: "Let's create a PRD"
- Type `*help` to see available commands
- Verify XML contains workflow definitions

### Artifacts Not Appearing

**Problem:** Documents generated in chat instead of Artifacts

**Solutions:**
- Ask explicitly: "Generate this in an Artifact"
- Artifacts auto-activate for long-form content
- Try regenerating the document
- Check that document is substantial enough (>200 words)

### Context Not Persisting

**Problem:** Agent doesn't remember previous conversations

**Solutions:**
- Verify you're in the same Project
- Check files are still uploaded to Project
- Reference previous work: "Based on the PRD we created..."
- Re-upload critical documents if needed

### Team Bundle Performance Issues

**Problem:** Multiple agents causing confusion or slowness

**Solutions:**
- Use Claude Sonnet 3.5+ (required for good performance)
- Upgrade to Pro plan for faster responses
- Use individual agents instead if experiencing issues
- Be explicit about which agent to activate: "PM, please analyze this"

## Claude Model Recommendations

### Claude 3.5 Sonnet (Recommended)

**Best for:**
- All individual agents
- Team bundles
- Complex workflows
- Long documents

**Performance:** Excellent reasoning, large context (200K tokens)

### Claude 3 Opus

**Best for:**
- Maximum reasoning capability
- Very complex problems
- Highest quality outputs

**Note:** Slower and more expensive, but highest quality

### Claude 3.5 Haiku

**Best for:**
- Simple queries
- Quick tasks
- Budget-conscious use

**Note:** Faster and cheaper, but less capable for complex workflows

**Recommendation:** Use Sonnet 3.5 for BMad workflows - best balance of quality, speed, and cost.

## Best Practices

1. ✅ **One Agent Per Project** - Or use official team bundles
2. ✅ **Add Custom Instructions** - Use the config block provided
3. ✅ **Upload Reference Docs** - Give agent context
4. ✅ **Use Artifacts** - Perfect for BMad documents
5. ✅ **Leverage Reasoning** - Ask "why" and "what if"
6. ✅ **Organize Conversations** - Name threads clearly
7. ✅ **Export Regularly** - Save Artifacts to local files
8. ✅ **Use Sonnet 3.5+** - Best performance for BMad
9. ✅ **Iterate and Refine** - Claude excels at improvement
10. ✅ **Combine with Local** - Plan in Claude, build locally

## Comparison: Claude vs. Others

| Feature | Claude Projects | Gemini Gems | ChatGPT GPTs |
|---------|----------------|-------------|--------------|
| Reasoning | ⭐⭐⭐ Excellent | ⭐⭐⭐ Excellent | ⭐⭐ Good |
| Context | ⭐⭐⭐ 200K | ⭐⭐⭐ Large | ⭐⭐ Medium |
| Documents | ⭐⭐⭐ Artifacts | ⭐⭐⭐ Code Execution | ⭐⭐ Canvas |
| Team Bundles | ⭐⭐⭐ Yes | ⭐⭐⭐ Yes | ❌ No |
| Technical | ⭐⭐⭐ Strong | ⭐⭐⭐ Strong | ⭐⭐ Moderate |
| Creative | ⭐⭐ Good | ⭐⭐ Good | ⭐⭐⭐ Strong |

**Choose Claude when:**
- You need deep reasoning and analysis
- Working on technical architecture
- Want excellent Artifacts for documents
- Need persistent project context
- Prefer Claude's thoughtful responses

## Example Claude Project Setup

### BMad Technical Architecture Project

**Name:** BMad Technical Architecture

**Description:** System architecture design and technical planning using BMad Method Architect agent

**Custom Instructions:**
```
All of your operating instructions and resources are contained in the XML file attached. Read in the initial agent block and instructions to understand it. You will not deviate from the character and rules outlined in the attached!

CONFIG.YAML Values:
- user_name: Jordan Chen
- communication_language: English
- user_skill_level: Expert
- document_output_language: English
- bmm-workflow-status: standalone (no workflow)
```

**Uploaded Files:**
- `architect.xml` (BMad Architect agent)
- `existing-prd.md` (reference document)
- `api-specs.yaml` (reference document)

**Conversation Starters:**
- "Let's design the system architecture"
- "I need to review and improve our architecture"
- "Help me make architecture decisions"
- "*architecture - run the full workflow"

## Next Steps

1. **Download Bundles** - Get from [BMad Web Bundles](https://bmad-code-org.github.io/bmad-bundles/)
2. **Create Claude Project** - Start with PM or Architect
3. **Upload Agent XML** - Add custom instructions
4. **Test with** `*help` - Verify setup
5. **Run a Workflow** - Try `*prd` or `*architecture`
6. **Export from Artifacts** - Save generated documents
7. **Go Local for Implementation** - Install BMad locally when ready to code

## Additional Resources

- **[Web Bundles Overview](./USING_WEB_BUNDLES.md)** - Compare all platforms
- **[Using BMad in Gemini](./using-bmad-in-gemini.md)** - Alternative platform
- **[Using BMad in ChatGPT](./using-bmad-in-chatgpt.md)** - Another option
- **[BMM Documentation](../src/modules/bmm/docs/README.md)** - All workflows explained
- **[Claude Documentation](https://docs.anthropic.com/claude/docs)** - Learn about Claude
- **[BMad Discord](https://discord.gg/gk8jAdXWmj)** - Get help and share Projects

---

**Ready to create your first Claude Project?** Download an agent bundle and follow the steps above!
