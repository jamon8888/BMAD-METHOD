# Using BMad in Gemini Gems (Recommended)

Create Gemini Gems powered by BMad Method agents for the best web bundle experience - no local installation required.

## Overview

Gemini Gems provide the **best platform for BMad web bundles**, offering excellent XML handling, large context windows, and powerful Code Execution for document generation.

### Why Gemini Gems? (Strongly Recommended)

**⭐⭐⭐ Best Overall Platform for BMad**

**✅ Excellent For:**
- All BMad workflows (individual agents and team bundles)
- Complex multi-step workflows
- Large document generation (PRDs, Architecture, UX)
- Team bundles with multiple agents (2.5 Pro+)
- Long conversations with context retention
- All modules: BMM, BMB, CIS, BMGD

**⭐ Key Advantages:**
- **Best XML parsing** - Handles large bundles perfectly
- **Largest context window** - Better than ChatGPT
- **Code Execution** - Excellent for document generation
- **Team bundles work great** - Gemini 2.5 Pro+ recommended
- **Reliable performance** - Consistent results
- **Fast responses** - Quick workflow execution

**💡 Recommendation:** Gemini Gems are the preferred platform for BMad web bundles. Use Gemini 2.5 Pro for best results.

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

### Step 2: Create a Gemini Gem

1. **Go to Google AI Studio**
   - Visit [Google AI Studio](https://aistudio.google.com/)
   - Sign in with your Google account
   - Click "New Gem" or "Create Gem"

2. **Configure Gem**
   - **Name:** Choose a descriptive name (e.g., "BMad Product Manager")
   - **Description:** Brief description (optional)
   - **Model:** Select Gemini 2.5 Pro (recommended) or higher

3. **Add System Instructions**

   In the **System Instructions** field, add this EXACT text:

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

4. **Enable Code Execution**

   **CRITICAL for document workflows:**
   - Toggle "Code execution" to ON
   - This is essential for PRD, Architecture, UX Design workflows
   - Enables structured document generation

5. **Upload Agent XML**

   - Click "Add file" or the upload icon
   - Select ONE XML file (e.g., `pm.xml`)
   - Wait for upload to complete

   **CRITICAL:** Upload exactly ONE agent XML file per Gem. Do NOT combine multiple agents unless using an official team bundle.

6. **Save and Test**

   - Click "Save" or "Create Gem"
   - Start a conversation
   - Type `*help` to see the agent's menu

## Recommended Gemini Models

### Gemini 2.5 Pro (Best Choice)

**⭐⭐⭐ Recommended for all BMad workflows**

**Best for:**
- All individual agents
- Team bundles (required for good performance)
- Complex workflows
- Long documents
- Multi-step processes

**Context:** 1M tokens (excellent)
**Performance:** Fast, reliable, high quality

### Gemini 2.0 Flash

**⭐⭐ Good for simple workflows**

**Best for:**
- Quick tasks
- Simple agents (PM, Analyst)
- Budget-conscious use
- Rapid prototyping

**Note:** Faster and cheaper, but less capable for complex workflows. Not recommended for team bundles.

### Gemini 1.5 Pro

**⭐ Acceptable fallback**

**Best for:**
- When 2.5 Pro unavailable
- Individual agents
- Standard workflows

**Note:** Works but 2.5 Pro significantly better. Don't use for team bundles.

**Recommendation:** Always use Gemini 2.5 Pro or higher for BMad Gems.

## Recommended Agents for Gemini Gems

### All Agents Work Excellently

Gemini Gems handle all BMad agents superbly:

**BMad Method (BMM):**
- ⭐⭐⭐ **All individual agents** work excellently
  - analyst.xml
  - pm.xml
  - architect.xml
  - dev.xml
  - sm.xml
  - tea.xml
  - ux-designer.xml
  - tech-writer.xml
  - game-designer.xml
  - game-dev.xml
  - game-architect.xml

**Team Bundles** (Gemini 2.5 Pro required):
- ⭐⭐⭐ **team-fullstack.xml** - Complete development team
- ⭐⭐⭐ **team-gamedev.xml** - Game development team

**BMad Builder (BMB):**
- ⭐⭐⭐ **bmad-builder.xml** - Create custom agents and workflows

**Creative Intelligence Suite (CIS):**
- ⭐⭐⭐ **All CIS agents** work excellently
  - brainstorming-coach.xml (Carson)
  - design-thinking-coach.xml (Maya)
  - creative-problem-solver.xml (Dr. Quinn)
  - innovation-strategist.xml (Victor)
  - storyteller.xml (Sophia)

**Team Bundle:**
- ⭐⭐⭐ **creative-squad.xml** - All CIS agents together

**BMad Game Development (BMGD):**
- ⭐⭐⭐ **team-gamedev.xml** - Game development team

**Verdict:** Use Gemini for any BMad agent or team bundle.

## Usage Examples

### Example 1: Complete Product Planning

**Agent:** PM

**Setup:**
1. Create Gemini Gem: "BMad PM"
2. Upload `pm.xml`
3. Enable Code Execution
4. Use Gemini 2.5 Pro

**Usage:**
```
User: I want to plan a new mobile fitness app

PM: Excellent! I'll guide you through comprehensive product planning. Let's start with a product brief to establish the vision.

*product-brief

[Asks questions about target users, problems, solutions...]

PM: [Generates product brief with Code Execution]

User: Great! Now let's create a full PRD

PM: *prd

[Detailed questioning about features, requirements, success metrics...]

PM: [Generates comprehensive PRD document with structured sections]

User: Can we break this into epics and stories?

PM: *create-epics-and-stories

[Creates epic and story breakdown]
```

**Result:** Complete product planning package ready to export

### Example 2: Team Collaboration

**Agent:** Team Fullstack

**Setup:**
1. Create Gemini Gem: "BMad Dev Team"
2. Upload `team-fullstack.xml`
3. Enable Code Execution
4. Use Gemini 2.5 Pro (required)

**Usage:**
```
User: I need help architecting and planning a new microservices platform

Team: I'll bring in the relevant team members for this. *party

PM: I can help define the product requirements and success criteria.

Architect: And I'll design the technical architecture for your microservices platform.

User: We need to support 100K concurrent users

PM: Let me capture that as a key requirement...

Architect: For that scale, I recommend an event-driven architecture with...

SM: From a delivery perspective, we should break this into 3 main epics...

[Multi-agent collaboration continues]

[Generates comprehensive planning and architecture documents]
```

**Result:** Well-rounded analysis from multiple expert perspectives

### Example 3: Creative Brainstorming

**Agent:** Brainstorming Coach (Carson)

**Setup:**
1. Create Gemini Gem: "Carson Brainstorming"
2. Upload `brainstorming-coach.xml`
3. Enable Code Execution

**Usage:**
```
User: *brainstorming

Carson: 🎨 Let's ignite those creative sparks! What are we brainstorming today?

User: Innovative features for a productivity app

Carson: Awesome! We have 36 proven brainstorming techniques. For productivity apps, I'd suggest starting with SCAMPER. Sound good?

User: Yes!

Carson: [Guides through SCAMPER technique]
- Substitute: What if we replaced the traditional task list with...
- Combine: What if we merged calendar and task tracking...
- Adapt: How can we adapt gaming mechanics...
- Modify: What if we changed the time tracking approach...

[Generates list of creative ideas with analysis]
```

**Result:** Innovative feature ideas with structured evaluation

### Example 4: UX Design Workflow

**Agent:** UX Designer

**Setup:**
1. Create Gemini Gem: "BMad UX"
2. Upload `ux-designer.xml`
3. Enable Code Execution
4. Upload existing PRD as reference

**Usage:**
```
User: I have a PRD for my app. Can you design the UX?

UX Designer: I'd love to help! I see you've uploaded the PRD. Let me run the UX design workflow.

*create-ux-design

[Reviews PRD, asks questions about user flows, interactions, design preferences...]

UX Designer: [Generates comprehensive UX design document with Code Execution]
- User personas and journeys
- Information architecture
- Wireframe descriptions
- Interaction patterns
- Design system recommendations
- Accessibility considerations
```

**Result:** Complete UX specification ready for design tools

## Key Workflows for Gemini Gems

### All Workflows Work Excellently

Gemini Gems handle all BMad workflows superbly. Here are the most popular:

### Planning Workflows (BMM)

**Product Brief** (`*product-brief`)
- Product vision and strategy
- Target audience
- Key features
- **Time:** 15-20 minutes

**Product Requirements** (`*prd`)
- Comprehensive requirements document
- User stories and acceptance criteria
- Success metrics
- **Time:** 45-60 minutes
- **Output:** Detailed PRD with Code Execution formatting

**Create Epics and Stories** (`*create-epics-and-stories`)
- Epic breakdown
- User story creation
- Acceptance criteria
- **Time:** 30-45 minutes

### Technical Workflows (BMM)

**Architecture** (`*architecture`)
- System architecture design
- Component specifications
- Technology stack
- Architecture Decision Records
- **Time:** 45-60 minutes
- **Output:** Comprehensive architecture document

**Test Architecture** (`*test-architecture`)
- Testing strategy
- Test coverage planning
- QA framework
- **Time:** 30-45 minutes

### Design Workflows (BMM)

**UX Design** (`*create-ux-design`)
- User experience specifications
- Wireframe descriptions
- Interaction patterns
- **Time:** 45-60 minutes

**Game Design** (`*game-brief`)
- Game concept
- Mechanics and systems
- Technical requirements
- **Time:** 45-60 minutes

### Research & Analysis (BMM)

**Research** (`*research`)
- Market analysis
- Technical research
- Competitive landscape
- **Time:** 30-60 minutes

**Brainstorm Project** (`*brainstorm-project`)
- Ideation and exploration
- Feature brainstorming
- Problem-solution fit
- **Time:** 30-45 minutes

### Creative Workflows (CIS)

**Brainstorming** (`*brainstorming`)
- 36 creative techniques
- Structured ideation
- Idea evaluation
- **Time:** 30-60 minutes

**Design Thinking** (`*design-thinking`)
- 5-phase process
- User empathy
- Prototyping
- **Time:** 45-75 minutes

**Problem Solving** (`*problem-solving`)
- Root cause analysis
- Solution generation
- Decision making
- **Time:** 30-45 minutes

**Innovation Strategy** (`*innovation-strategy`)
- Business model innovation
- Blue Ocean strategy
- Disruptive thinking
- **Time:** 45-60 minutes

**Storytelling** (`*storytelling`)
- Narrative frameworks
- Pitch development
- Story structure
- **Time:** 30-45 minutes

### Builder Workflows (BMB)

**Create Agent** (`*create-agent`)
- Custom agent design
- Persona development
- Command structure
- **Time:** 45-75 minutes

**Create Workflow** (`*create-workflow`)
- Workflow design
- Step-by-step process
- Configuration
- **Time:** 30-60 minutes

**Create Module** (`*create-module`)
- Complete module creation
- Multi-agent systems
- Installation automation
- **Time:** 60-90 minutes

## Gemini Features & Benefits

### Code Execution

**Best feature for BMad workflows**

**Benefits:**
- Structured document generation
- Clean formatting
- Tables, lists, hierarchies
- Export-ready documents

**Activates automatically for:**
- PRDs
- Architecture documents
- UX specifications
- Research reports
- Any long-form content

**How to use:**
- Just enable in Gem settings
- Works automatically
- No special commands needed

### Large Context Window

**Gemini 2.5 Pro: 1M tokens**

**Benefits:**
- Handle very large documents
- Team bundles with multiple agents
- Long conversation threads
- Comprehensive workflows

**Perfect for:**
- Complex PRDs (10K+ words)
- Detailed architecture (15K+ words)
- Multi-agent team discussions
- Iterative refinement

### File Upload & Management

**Benefits:**
- Upload reference documents
- Attach PRDs, specs, designs
- Agent uses them in context

**Supported formats:**
- Markdown (.md)
- Text files (.txt)
- PDFs (.pdf)
- Images (for wireframes, mockups)

### Conversation Management

**Benefits:**
- Named conversations
- Organized workflow threads
- Easy reference

**Usage:**
- Rename conversations descriptively
- Create new threads for different workflows
- Keep related work together

## Tips for Success with Gemini Gems

### 1. Always Enable Code Execution

**Critical for document workflows:**
- PRD, Architecture, UX Design
- Research reports
- Documentation
- Any long-form output

**How to enable:**
- Gem settings → Toggle "Code execution" ON
- Must be enabled before starting workflows

### 2. Use Gemini 2.5 Pro

**Best performance:**
- Highest quality outputs
- Best XML handling
- Required for team bundles
- Fastest processing

### 3. Upload Reference Documents

**Provide context:**
- Existing PRDs
- Technical specs
- Design mockups
- Research findings

**Agent uses them:**
- References in workflows
- Builds on existing work
- Maintains consistency

### 4. Use Team Bundles

**Gemini excels at team bundles:**
- team-fullstack.xml
- team-gamedev.xml
- creative-squad.xml

**Why:**
- Large context window
- Handles multiple agent personas
- Clean multi-agent dialogue

### 5. Leverage Natural Language

**Commands or conversation:**
```
*prd                          (command shortcut)
"Let's create a PRD"          (natural language)
"Run the PRD workflow"        (explicit request)
```

All work perfectly in Gemini.

### 6. Iterate and Refine

**Gemini is excellent at refinement:**
```
"Expand the architecture section"
"Add more user stories for authentication"
"Revise the PRD with focus on security"
```

### 7. Export and Save

**Download generated documents:**
- Copy from Gemini interface
- Save to project `docs/` folder
- Use for local implementation phase

### 8. Create Multiple Gems

**Organize by agent/purpose:**
- "BMad PM" - Product planning
- "BMad Architect" - Technical design
- "BMad Dev Team" - Team collaboration
- "Carson Brainstorming" - Creative ideation

## Combining Gemini Gems with Local Development

### Recommended Hybrid Workflow

**Phases 1-3: Gemini Gems** (Analysis, Planning, Architecture)

**Maximum cost savings - do all planning in Gemini:**

1. **Create Gems** for planning agents:
   - PM Gem (`pm.xml`)
   - Architect Gem (`architect.xml`)
   - UX Designer Gem (`ux-designer.xml`)

2. **Enable Code Execution** in all Gems

3. **Run workflows:**
   - PM: `*product-brief` → Product vision
   - PM: `*prd` → Comprehensive requirements
   - PM: `*create-epics-and-stories` → Story breakdown
   - Architect: `*architecture` → Technical design
   - UX Designer: `*create-ux-design` → UX specifications

4. **Export artifacts** - Copy all generated documents

**Phase 4: Local Development** (Implementation)

5. **Setup local BMad:**
   - Install: `npx bmad-method@alpha install`
   - Save exported docs to `docs/` folder
   - Load Developer agent in IDE
   - Run: `*workflow-init` → Detects artifacts

6. **Implement:**
   - `*sprint-planning` → Plan sprint
   - `*dev-story` → Implement features
   - Full IDE capabilities with codebase access

**Cost Savings:** 70-80% by planning in Gemini before local implementation

### Why This Works

**Planning in Gemini:**
- ✅ No codebase access needed
- ✅ Document-heavy workflows (perfect for Code Execution)
- ✅ Lower cost per token
- ✅ Excellent for ideation and specification

**Implementation locally:**
- ✅ Full codebase access
- ✅ IDE tools and debugging
- ✅ Real-time code execution
- ✅ Git integration

**Best of both worlds!**

## Troubleshooting

### Gem Not Responding Correctly

**Problem:** Agent doesn't follow BMad persona

**Solutions:**
- Verify entire XML uploaded (check file size)
- Ensure system instructions added correctly
- Recreate Gem if needed
- Try with Gemini 2.5 Pro

### Code Execution Not Enabled

**Problem:** Documents not formatted properly

**Solutions:**
- Check Gem settings
- Toggle "Code execution" to ON
- Save Gem and restart conversation
- Essential for document workflows

### Menu Commands Not Working

**Problem:** `*prd` doesn't trigger workflow

**Solutions:**
- Ensure `*` prefix used
- Try natural language: "Create a PRD"
- Type `*help` to verify menu
- Check XML uploaded correctly

### Team Bundle Performance Issues

**Problem:** Multiple agents causing confusion

**Solutions:**
- Verify using Gemini 2.5 Pro (required)
- NOT 2.0 Flash or 1.5 Pro
- Be explicit: "PM, please analyze this"
- Use `*party` to activate team mode

### Upload Failed or Incomplete

**Problem:** XML file not uploading properly

**Solutions:**
- Check file size (should be < 10MB)
- Try re-downloading XML
- Verify file integrity
- Try uploading again

### Documents Not Generating

**Problem:** Workflow doesn't produce document

**Solutions:**
- Enable Code Execution (critical!)
- Use Gemini 2.5 Pro
- Ask explicitly: "Generate the document"
- Check conversation for errors

## Best Practices

1. ✅ **Use Gemini 2.5 Pro** - Best performance, required for teams
2. ✅ **Enable Code Execution** - Essential for documents
3. ✅ **One Agent Per Gem** - Or use official team bundles
4. ✅ **Add System Instructions** - Use config block provided
5. ✅ **Upload Reference Docs** - Provide context
6. ✅ **Create Organized Gems** - One per agent/purpose
7. ✅ **Export Regularly** - Save generated documents
8. ✅ **Use Team Bundles** - Gemini handles them excellently
9. ✅ **Iterate and Refine** - Gemini is great at improvement
10. ✅ **Plan in Gemini, Build Locally** - Maximum cost savings

## Comparison: Gemini vs. Others

| Feature | Gemini Gems | Claude Projects | ChatGPT GPTs |
|---------|-------------|-----------------|--------------|
| **Overall Rating** | ⭐⭐⭐ Best | ⭐⭐ Excellent | ⭐ Good |
| XML Handling | ⭐⭐⭐ Excellent | ⭐⭐⭐ Excellent | ⭐⭐ Good |
| Context Window | ⭐⭐⭐ 1M tokens | ⭐⭐⭐ 200K | ⭐⭐ Medium |
| Documents | ⭐⭐⭐ Code Exec | ⭐⭐⭐ Artifacts | ⭐⭐ Canvas |
| Team Bundles | ⭐⭐⭐ Excellent | ⭐⭐⭐ Excellent | ❌ No |
| Speed | ⭐⭐⭐ Fast | ⭐⭐ Good | ⭐⭐ Good |
| Reliability | ⭐⭐⭐ High | ⭐⭐⭐ High | ⭐⭐ Moderate |
| Cost | $$$ | $$$ | $$ |

**Choose Gemini Gems for:**
- ✅ Best overall BMad experience
- ✅ All workflows and agents
- ✅ Team bundles
- ✅ Large documents
- ✅ Complex workflows
- ✅ Maximum reliability

## Example Gemini Gem Setups

### Setup 1: Product Planning Gem

**Name:** BMad Product Manager

**Description:** AI product planning agent for PRDs, epics, and stories using BMad Method

**Model:** Gemini 2.5 Pro

**System Instructions:**
```
All of your operating instructions and resources are contained in the XML file attached. Read in the initial agent block and instructions to understand it. You will not deviate from the character and rules outlined in the attached!

CONFIG.YAML Values:
- user_name: Alex Johnson
- communication_language: English
- user_skill_level: Intermediate
- document_output_language: English
- bmm-workflow-status: standalone (no workflow)
```

**Settings:**
- ✅ Code execution: ON
- Upload: `pm.xml`

### Setup 2: Full Dev Team Gem

**Name:** BMad Development Team

**Description:** Complete AI development team (PM, Architect, Dev, SM, TEA) for collaborative planning

**Model:** Gemini 2.5 Pro (required for team)

**System Instructions:**
```
All of your operating instructions and resources are contained in the XML file attached. Read in the initial agent block and instructions to understand it. You will not deviate from the character and rules outlined in the attached!

CONFIG.YAML Values:
- user_name: Morgan Lee
- communication_language: English
- user_skill_level: Expert
- document_output_language: English
- bmm-workflow-status: standalone (no workflow)
```

**Settings:**
- ✅ Code execution: ON
- Upload: `team-fullstack.xml`

### Setup 3: Creative Brainstorming Gem

**Name:** Carson - Brainstorming Coach

**Description:** Energetic brainstorming facilitator with 36 creative techniques

**Model:** Gemini 2.5 Pro

**System Instructions:**
```
All of your operating instructions and resources are contained in the XML file attached. Read in the initial agent block and instructions to understand it. You will not deviate from the character and rules outlined in the attached!

CONFIG.YAML Values:
- user_name: Sam Rivera
- communication_language: English
- user_skill_level: Beginner
- document_output_language: English
- bmm-workflow-status: standalone (no workflow)
```

**Settings:**
- ✅ Code execution: ON
- Upload: `brainstorming-coach.xml`

## Next Steps

1. **Go to Google AI Studio** - [aistudio.google.com](https://aistudio.google.com/)
2. **Download Bundles** - Get from [BMad Web Bundles](https://bmad-code-org.github.io/bmad-bundles/)
3. **Create Your First Gem** - Start with PM or Architect
4. **Enable Code Execution** - Critical for document workflows
5. **Select Gemini 2.5 Pro** - Best performance
6. **Test with** `*help` - Verify setup
7. **Run a Workflow** - Try `*prd` or `*brainstorming`
8. **Export Results** - Save generated documents
9. **Go Local for Implementation** - Install BMad when ready to code

## Additional Resources

- **[Web Bundles Overview](./USING_WEB_BUNDLES.md)** - Compare all platforms
- **[Using BMad in Claude](./using-bmad-in-claude.md)** - Alternative platform
- **[Using BMad in ChatGPT](./using-bmad-in-chatgpt.md)** - Another option
- **[BMM Documentation](../src/modules/bmm/docs/README.md)** - All workflows explained
- **[Google AI Studio](https://aistudio.google.com/)** - Create Gems
- **[Gemini Documentation](https://ai.google.dev/docs)** - Learn about Gemini
- **[BMad Discord](https://discord.gg/gk8jAdXWmj)** - Get help and share Gems

---

**Ready to create your first Gemini Gem?** It's the best platform for BMad - follow the steps above to get started!
