# Using BMad in ChatGPT (Custom GPTs)

Create Custom GPTs powered by BMad Method agents for analysis, planning, and creative workflows - no local installation required.

## Overview

Custom GPTs let you use BMad agents directly in ChatGPT by uploading web bundle XML files. Perfect for planning phases and creative work, though Gemini Gems and Claude Projects offer better performance for complex workflows.

### When to Use ChatGPT

**✅ Good For:**
- Simple, focused workflows (product briefs, brainstorming)
- Creative agents (CIS - brainstorming, storytelling, etc.)
- Quick prototyping and exploration
- Familiar ChatGPT interface
- Easy team sharing via link

**⚠️ Limitations:**
- Smaller context window than Gemini/Claude
- Character limit on instructions (large bundles may not fit)
- **NOT recommended for team bundles** (use Gemini 2.5 Pro+ instead)
- Canvas feature less mature than Gemini's Code Execution

**💡 Recommendation:** Use ChatGPT for individual agents only. For team bundles or complex workflows, use [Gemini Gems](./using-bmad-in-gemini.md) or [Claude Projects](./using-bmad-in-claude.md).

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

### Step 2: Create a Custom GPT

1. **Go to ChatGPT**
   - Visit [ChatGPT](https://chat.openai.com/)
   - Click your profile icon
   - Select "My GPTs"
   - Click "Create a GPT"

2. **Configure GPT**
   - **Name:** Choose a descriptive name (e.g., "BMad Product Manager")
   - **Description:** Brief description (e.g., "AI product planning agent powered by BMad Method")
   - **Instructions:** See Step 3 below

3. **Add Instructions**

   In the **Instructions** field, add this setup text FIRST:

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

4. **Add Agent XML**

   **Below the config text**, paste the entire contents of ONE XML file.

   **CRITICAL:** Upload exactly ONE agent XML file. Do NOT combine multiple agents.

   **Example:** For PM agent, paste all of `pm.xml`

5. **Enable Canvas**

   - In GPT settings, enable "Canvas" if available
   - This improves document generation for PRD, Architecture, etc.

6. **Set Capabilities** (Optional)

   - Enable "Web Browsing" for research workflows
   - Enable "DALL-E" for visual brainstorming (optional)
   - Enable "Code Interpreter" if available

7. **Save and Test**

   - Click "Create" or "Save"
   - Test by typing `*help` to see the agent's menu

## Recommended Agents for ChatGPT

### Best Performing

These agents work excellently in Custom GPTs:

**BMad Method (BMM):**
- ⭐ **analyst.xml** - Analysis and requirements gathering
- ⭐ **pm.xml** - Product planning and PRD creation
- ⭐ **sm.xml** - Scrum facilitation
- ⭐ **ux-designer.xml** - UX design workflows

**Creative Intelligence Suite (CIS):**
- ⭐ **brainstorming-coach.xml** - Creative brainstorming (Carson)
- ⭐ **storyteller.xml** - Narrative and storytelling (Sophia)
- ⭐ **design-thinking-coach.xml** - Design thinking (Maya)
- ⭐ **creative-problem-solver.xml** - Problem solving (Dr. Quinn)
- ⭐ **innovation-strategist.xml** - Innovation strategy (Victor)

**BMad Builder (BMB):**
- ⭐ **bmad-builder.xml** - Create custom agents and workflows

### Moderate Performance

These work but may hit context limits:

- architect.xml - Technical architecture (better in Gemini/Claude)
- dev.xml - Development (requires local IDE for implementation)
- tech-writer.xml - Documentation

### NOT Recommended for ChatGPT

**❌ Team Bundles:**
- team-fullstack.xml
- team-gamedev.xml
- creative-squad.xml

**Why:** ChatGPT's context window is too small for multiple agents. Use [Gemini Gems](./using-bmad-in-gemini.md) with 2.5 Pro+ instead.

## Usage Examples

### Example 1: Create a Product Brief

**Agent:** PM

**Setup:**
1. Create Custom GPT with `pm.xml`
2. Enable Canvas
3. Name it "BMad Product Manager"

**Usage:**
```
User: I want to create a product brief for a task management app

PM: I'll guide you through creating a comprehensive product brief. Let's run the product brief workflow.

[Asks questions about target audience, problems, solutions, features...]

[Generates product brief document in Canvas]

User: Can you adjust the target audience section?

PM: [Updates document based on feedback]
```

**Result:** Complete product brief ready to export

### Example 2: Brainstorming Session

**Agent:** Brainstorming Coach (Carson)

**Setup:**
1. Create Custom GPT with `brainstorming-coach.xml`
2. Name it "Carson - Brainstorming Coach"

**Usage:**
```
User: *brainstorming

Carson: Let's get those creative juices flowing! What are we brainstorming about today?

User: Features for a fitness app

Carson: Exciting! Let's use the SCAMPER technique to generate innovative features...

[Guides through creative brainstorming process]
```

**Result:** List of creative feature ideas with analysis

### Example 3: UX Design Workflow

**Agent:** UX Designer

**Setup:**
1. Create Custom GPT with `ux-designer.xml`
2. Enable Canvas
3. Name it "BMad UX Designer"

**Usage:**
```
User: I have a PRD for my app. Can you help design the UX?

UX Designer: I'd be happy to help! Share your PRD and let's run the UX design workflow.

[User shares PRD]

UX Designer: *create-ux-design

[Asks questions about user flows, wireframes, design system...]

[Generates UX design document in Canvas]
```

**Result:** Complete UX design specification

## Key Workflows for ChatGPT

### Analysis & Planning (BMM)

**Product Brief** (`*product-brief`)
- Define product vision
- Identify target audience
- Outline key features
- **Time:** 15-20 minutes

**Product Requirements** (`*prd`)
- Comprehensive requirements document
- User stories and acceptance criteria
- Technical considerations
- **Time:** 30-45 minutes
- **Note:** May need multiple conversations due to length

**Research** (`*research`)
- Market analysis
- Competitor research
- Technical research
- **Time:** 20-30 minutes
- **Requires:** Web browsing enabled

### Creative Workflows (CIS)

**Brainstorming** (`*brainstorming`)
- 36 creative techniques
- Structured ideation
- Idea evaluation
- **Time:** 20-40 minutes

**Storytelling** (`*storytelling`)
- Narrative frameworks
- Pitch development
- Story structure
- **Time:** 30-45 minutes

**Design Thinking** (`*design-thinking`)
- 5-phase human-centered process
- User empathy
- Rapid prototyping
- **Time:** 45-60 minutes

**Problem Solving** (`*problem-solving`)
- Root cause analysis
- Solution generation
- Impact assessment
- **Time:** 30-45 minutes

**Innovation Strategy** (`*innovation-strategy`)
- Business model innovation
- Disruptive thinking
- Strategic planning
- **Time:** 40-60 minutes

### Builder Workflows (BMB)

**Create Agent** (`*create-agent`)
- Design custom agents
- Define persona and commands
- Integration planning
- **Time:** 30-60 minutes

**Create Workflow** (`*create-workflow`)
- Design structured workflows
- Define steps and outputs
- Configuration setup
- **Time:** 30-45 minutes

## Tips for Success

### 1. Start Simple

Begin with focused agents like PM or Brainstorming Coach. Get comfortable before trying complex workflows.

### 2. Use Shortcuts

All commands work with `*` prefix:
- `*help` - Show menu
- `*prd` - Start PRD workflow
- `*brainstorming` - Start brainstorming

Or use natural language:
- "Let's create a PRD"
- "I want to brainstorm features"

### 3. Break Into Conversations

For long workflows (PRD, Architecture):
- Split into multiple conversations if hitting limits
- Export sections as you go
- Combine artifacts later

### 4. Enable Canvas

Canvas dramatically improves document generation:
- Settings → Canvas → Enable
- Documents appear in structured format
- Easy editing and export

### 5. Provide Context

Give the agent relevant information:
- Share existing docs (PRD, specs)
- Explain your goals clearly
- Answer questions thoroughly

### 6. Export Regularly

Save generated documents frequently:
- Copy from Canvas to your editor
- Download as Markdown
- Save to project `docs/` folder

### 7. Combine with Local

Use web planning + local implementation:
1. Create PRD in ChatGPT Custom GPT
2. Export to `docs/prd.md`
3. Install BMad locally: `npx bmad-method@alpha install`
4. Run `*workflow-init` - detects artifacts
5. Implement with Developer agent locally

**Cost Savings:** 60-70%

## Troubleshooting

### GPT Not Responding Correctly

**Problem:** Agent doesn't follow persona or workflows

**Solutions:**
- Verify entire XML was pasted (check for truncation)
- Ensure setup instructions were added at the top
- Try refreshing and creating a new conversation
- Test with `*help` to verify menu appears

### Character Limit Errors

**Problem:** XML file too large for instructions field

**Solutions:**
- Use a simpler agent (analyst, pm instead of team bundles)
- Try [Gemini Gems](./using-bmad-in-gemini.md) instead (no character limits)
- Split agent into multiple GPTs if possible

### Menu Commands Not Working

**Problem:** Typing `*prd` doesn't trigger workflow

**Solutions:**
- Ensure you're using the `*` prefix
- Try natural language: "Let's create a PRD"
- Type `*help` to see all available commands
- Verify agent XML was properly loaded

### Canvas Not Working

**Problem:** Documents not appearing in Canvas

**Solutions:**
- Enable Canvas in ChatGPT settings
- Ask agent to "create document in Canvas"
- Update to latest ChatGPT version
- Try regular chat if Canvas unavailable

### Workflow Stopped Mid-Process

**Problem:** Agent stops before completing workflow

**Solutions:**
- Prompt to continue: "Please continue"
- Break into smaller sections
- Use Gemini or Claude for longer workflows
- Export progress and restart if needed

### Context Window Exceeded

**Problem:** "Maximum length exceeded" or similar errors

**Solutions:**
- This is a ChatGPT limitation
- Switch to [Gemini Gems](./using-bmad-in-gemini.md) (larger context)
- Use individual agents (not team bundles)
- Break workflow into multiple conversations

## Limitations & Workarounds

### Character Limits

**Issue:** ChatGPT has instruction length limits

**Workaround:**
- Use individual agents only
- Use Gemini for team bundles or large agents

### Context Window

**Issue:** Smaller than Gemini/Claude

**Workaround:**
- Split long workflows across conversations
- Export and restart with context summary
- Use Gemini/Claude for complex workflows

### Team Bundles

**Issue:** Don't work well in ChatGPT

**Solution:**
- ❌ Don't use team bundles in ChatGPT
- ✅ Use [Gemini Gems](./using-bmad-in-gemini.md) 2.5 Pro+ instead

### Implementation Workflows

**Issue:** Development requires codebase access

**Solution:**
- Use ChatGPT for planning only
- Switch to local BMad installation for implementation
- See [hybrid workflow guide](./USING_WEB_BUNDLES.md#cost-saving-strategy-web--local-hybrid)

## Sharing Your Custom GPTs

Custom GPTs can be shared with others:

1. **Open Your GPT**
2. **Click Share Icon**
3. **Choose Sharing Option:**
   - Only you
   - Anyone with the link
   - Public (listed in GPT store)
4. **Copy and Share Link**

**Note:** Others need ChatGPT Plus to use shared GPTs.

## Best Practices

1. ✅ **One Agent Per GPT** - Don't combine multiple XML files
2. ✅ **Enable Canvas** - Essential for document workflows
3. ✅ **Add Setup Instructions** - Use the config block provided
4. ✅ **Start Simple** - Try PM or CIS agents first
5. ✅ **Export Frequently** - Save your work regularly
6. ✅ **Use Natural Language** - Commands or conversation both work
7. ✅ **Provide Context** - Share relevant information
8. ❌ **Avoid Team Bundles** - Use individual agents instead
9. ❌ **Don't Implement in Web** - Use local IDE for coding
10. ✅ **Combine with Local** - Plan in web, build locally

## Example Custom GPT Setup

### BMad Product Manager GPT

**Name:** BMad Product Manager

**Description:** AI-powered product planning agent using BMad Method. Creates PRDs, product briefs, epics, and stories.

**Instructions:**
```
All of your operating instructions and resources are contained in the XML file attached. Read in the initial agent block and instructions to understand it. You will not deviate from the character and rules outlined in the attached!

CONFIG.YAML Values:
- user_name: Alex Smith
- communication_language: English
- user_skill_level: Intermediate
- document_output_language: English
- bmm-workflow-status: standalone (no workflow)

[Paste entire pm.xml contents below this line]
```

**Capabilities:**
- ✅ Web Browsing
- ✅ Code Interpreter
- ✅ Canvas

**Conversation Starters:**
- "Help me create a product brief"
- "Let's write a PRD for my project"
- "I need to create user stories"
- "Show me the menu"

## Next Steps

1. **Download Bundles** - Get from [BMad Web Bundles](https://bmad-code-org.github.io/bmad-bundles/)
2. **Create Your First GPT** - Start with PM or Brainstorming Coach
3. **Test with** `*help` - Verify setup
4. **Run a Workflow** - Try `*product-brief` or `*brainstorming`
5. **Export Results** - Save generated documents
6. **Go Local for Implementation** - Install BMad locally when ready to code

## Additional Resources

- **[Web Bundles Overview](./USING_WEB_BUNDLES.md)** - Compare all platforms
- **[Using BMad in Gemini](./using-bmad-in-gemini.md)** - Better for complex workflows
- **[Using BMad in Claude](./using-bmad-in-claude.md)** - Alternative platform
- **[BMM Documentation](../src/modules/bmm/docs/README.md)** - All workflows explained
- **[BMad Discord](https://discord.gg/gk8jAdXWmj)** - Get help and share Custom GPTs

---

**Ready to create your first Custom GPT?** Download an agent bundle and follow the steps above!
