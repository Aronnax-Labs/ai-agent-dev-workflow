# AI Development Lifecycle — Human-Centered, GitHub-as-Hub

This document outlines the **big-picture workflow** for a human-centered AI-assisted development process.  
The goal is to create a sustainable loop where AI tools augment human developers while maintaining transparency, control, and traceability — all centered around **GitHub**.

---

## 🧭 The Lifecycle Overview

**Intent → Spec → Build → Verify → Review → Merge → Deploy → Learn**

Every step is visible in GitHub Issues, PRs, and Actions.  
Agents and automations exist to *assist*, not replace, the human developer.

---

### 1. **Intent**
- **Owner:** Human (Zack)
- **Description:** The human defines what they want built in natural language.  
- **Example:**  
  “Create a simple website that lists events from a DynamoDB table.”

- **Tool Candidates:**
  - **Cursor** — chat-based IDE that plans and executes code changes.
  - **Amazon Q CLI** — for scripted, command-line task generation.
  - **Claude / ChatGPT / Copilot Workspace** — for brainstorming or initial planning.

- **Output:** A high-level description that becomes GitHub Issues.

---

### 2. **Spec**
- **Owner:** Planner Agent (or you via Cursor)
- **Description:** Converts intent into small, actionable Issues with clear scope.
- **Example:**  
  “Implement `/api/events` endpoint.”  
  “Create `EventList` page in Next.js.”  

- **Tool Candidates:**
  - **GitHub Issue Forms / Templates** — ensure consistent formatting.
  - **Q CLI or Claude** — can generate Issue specs directly from prompts.
  - **Cursor** — writes structured acceptance criteria.

- **Exit Criteria:** Issue labeled `ready-for-dev`.

---

### 3. **Build**
- **Owner:** Build Agent (Cursor)
- **Description:** Executes approved specs by creating feature branches, commits, and PRs.  
- **Human Role:** Approve plans before execution.

- **Tool Candidates:**
  - **Cursor** — main coding environment and PR generator.
  - **Aider / Claude Code / OpenDevin** — alternative local agents.
  - **GitHub CLI** — branch, commit, and PR automation.

- **Output:** PR with changes linked to an Issue.

---

### 4. **Verify**
- **Owner:** QA Agent(s) (CI / Actions)
- **Description:** Automated checks run on every PR: tests, linting, docs validation, etc.
- **Human Role:** Read comments, make judgment calls on ambiguous results.

- **Tool Candidates:**
  - **GitHub Actions** — CI/CD runner for tests, lint, security scans.
  - **ESLint / Prettier / Jest / Pytest** — code quality tools.
  - **lychee-action** — link checker for websites.
  - **markdown-lint** — keeps docs clean.

- **Output:** Comment summarizing QA results (`qa-pass` or `qa-fail` label).

---

### 5. **Review**
- **Owner:** Human (Zack)
- **Description:** The human reviews diffs, test results, and design choices.
- **Action:** Approve or request changes.

- **Tools:**
  - **GitHub PR Review**
  - **Cursor Chat View** (inline explanations)

- **Output:** Human approval before merge.

---

### 6. **Merge**
- **Owner:** Human (Zack)
- **Description:** Merges PRs that have passed QA and review.  
  Protected branch policies enforce checks.

- **Tool Candidates:**
  - **GitHub Branch Protection Rules**
  - **Actions Required Checks**

- **Output:** Clean, approved commits in `main`.

---

### 7. **Deploy**
- **Owner:** Deploy Agent (CI/CD)
- **Description:** Automatic deployment triggered on merge.
- **Tool Candidates:**
  - **GitHub Pages** — simplest option for static sites.
  - **Vercel / Netlify** — for Next.js or serverless apps.
  - **AWS Amplify / S3 Static Hosting** — cloud-native alternative.

- **Output:** Live deployment + generated release notes.

---

### 8. **Learn**
- **Owner:** Human + Planner Agent
- **Description:** Evaluate results, gather insights, open new Issues.
- **Tools:**
  - **GitHub Discussions / Issues**
  - **Q CLI / Cursor / Claude** for postmortems and reflections.

- **Output:** New or refined Intent → loop restarts.

---

## 🔄 Example Full-Circle Workflow

### Scenario: Simple GitHub Pages Site (Completed Nov 1, 2025)

**Setup:** Agent verified PAT configured, MCP working, permissions set.

1. **Intent** — PM: "Build a simple static site demonstrating AI agent workflow"

2. **Spec** — Agent creates Issue #2 with:
   - Clear acceptance criteria
   - Technical constraints
   - Success metrics

3. **Build** — Agent:
   - Creates feature branch
   - Codes `index.html` + `styles.css`
   - Sets up GitHub Pages workflow
   - Opens PR #3

4. **Verify** — GitHub Actions run deployment workflow (few iterations to get Pages config right)

5. **Review** — PM + Copilot review, approve PR

6. **Merge** — Agent merges to `main` (squash merge)

7. **Deploy** — GitHub Pages publishes site live

8. **Learn** — Team creates retrospective, identifies improvements (Issue #4)

**Lessons Applied:**
- Authentication friction required troubleshooting
- Agent identity attribution needs improvement
- Context management strategies needed
- See [Project Retrospective](../project-retrospective.md) for full details

Cycle repeats with improved process.

---

## 🧩 Minimal Agent Stack (Plug-and-Play)

| Stage | Primary Tool | Alternative / Future Option |
|-------|---------------|-----------------------------|
| Intent | Cursor | Q CLI, Claude, Copilot Workspace |
| Spec | GitHub Issues / Templates | Q CLI + Claude |
| Build | Cursor | Aider, Claude Code, OpenDevin |
| Verify | GitHub Actions | Jenkins, n8n |
| Review | GitHub UI | Cursor inline review |
| Merge | GitHub Protected Branch | - |
| Deploy | GitHub Pages | Vercel, Netlify, Amplify |
| Learn | GitHub Discussions | Slack / Notion sync |

---

## 🧱 Suggested First Project

**Name:** `ai-dev-loop-demo`  
**Goal:** Prove the workflow before scaling.

**Stack:**
- Static HTML/CSS site (hero + links)
- GitHub Pages for deploy
- Cursor for coding
- GitHub Issues for specs
- QA Action (lint + link check)
- Manual human review

**Why:**  
- Zero infrastructure  
- Tests the entire workflow (Intent → Learn)  
- Fast feedback and iteration

---

## 🏁 North Star

Keep the loop **visible, simple, and reversible**.

> **Intent → Spec → Build → Verify → Review → Merge → Deploy → Learn**

GitHub is the central nervous system.  
AI agents are your specialized limbs.  
You stay the brain — making the final calls and keeping the team on course.

---

## 🔧 Setup & Configuration

Before starting any project, ensure proper setup to avoid friction during execution.

### Pre-flight Checklist

- [ ] **Authentication:** PAT configured with appropriate scopes (`repo`, `workflow`, etc.)
- [ ] **MCP Server:** GitHub MCP configured and tested
- [ ] **Git Credentials:** CLI fallback works for emergencies
- [ ] **Permissions:** Verify agent can create branches, issues, PRs
- [ ] **Environment:** Test basic API calls (get user, list repos)

### Common Setup Issues

**Problem:** CLI git push fails with "could not read Username"  
**Solution:** Use MCP tools or configure git credentials properly

**Problem:** MCP returns 401/403 errors  
**Solution:** Check PAT scopes and token expiration

**Problem:** Workflows fail with permission errors  
**Solution:** Verify Actions permissions in repo settings

---

## 👤 Agent Identity Management

**Current Challenge:** All agent activity appears under human's account, causing attribution confusion.

### Approaches to Explore

1. **GitHub Bot Accounts**
   - Create dedicated bot user for agent activity
   - Pros: Clear attribution, separation of concerns
   - Cons: Extra account management, potential cost

2. **GitHub App Tokens**
   - Use GitHub App for machine access
   - Pros: Fine-grained permissions, better security
   - Cons: More complex setup

3. **Service Accounts**
   - Organization-level service account
   - Pros: Central management
   - Cons: Still under organization, not individual bots

### Best Practices (Current)

- Document agent work clearly in PR descriptions
- Use consistent commit message format: `feat: [Agent] Description`
- Add attribution notes in code comments when appropriate
- Consider signing commits with distinct GPG key

---

## 🚨 Troubleshooting Guide

### Authentication Failures

**Symptom:** MCP tools return 401/403 errors

**Steps:**
1. Verify PAT is valid and not expired
2. Check PAT scopes in GitHub settings
3. Restart MCP server/Cursor
4. Test with simple API call (get user)

**Symptom:** CLI git commands fail

**Steps:**
1. Check git remote configuration
2. Try using MCP tools instead
3. Configure git credentials helper
4. Use SSH instead of HTTPS if applicable

### Deployment Issues

**Symptom:** GitHub Pages deployment fails

**Steps:**
1. Verify repo is public or org has Pages enabled
2. Check workflow permissions (`pages: write`)
3. Ensure Pages is enabled in repo settings
4. Review workflow logs for specific errors

**Symptom:** Workflow runs but site not accessible

**Steps:**
1. Check Pages source branch is correct
2. Verify index.html is in root or correct directory
3. Wait for DNS propagation (can take 10+ min)
4. Clear browser cache

### General Debugging

When in doubt:
1. Check GitHub Actions logs
2. Test API calls manually via MCP or curl
3. Verify file permissions and paths
4. Ask PM for guidance on unclear requirements

---

## 📚 Context & Knowledge Management

**Critical Question:** Where is the source of truth for project knowledge?

### Recommended Structure

1. **README.md** - Quick start, overview, architecture
2. **Issues** - Task-specific context and discussions
3. **Project Board** - Visual workflow and timeline
4. **Wiki/Discussions** - Durable knowledge base
5. **Code Comments** - Inline documentation

### Maintaining Context Across Sessions

- **Session Continuity:** Reference previous issues/PRs in new work
- **Consistent Patterns:** Follow established conventions
- **Clear Communication:** Explain decisions in PR descriptions
- **Documentation First:** Update docs as code changes

### Agent-Agent Collaboration

When multiple agents might work on same project:
- Claim issues explicitly
- Leave detailed PR comments
- Update progress in issue threads
- Keep README current

---

## 🔍 Enhanced Verify Stage

Expand automated quality checks before merge.

### Quality Gates

**Code Quality:**
- Linting (ESLint, Pylint, etc.)
- Formatting (Prettier, Black, etc.)
- Type checking where applicable

**Security:**
- Dependency vulnerability scans
- Secrets detection
- Security linters

**Functional:**
- Unit tests (if applicable)
- Link checking for web projects
- HTML/CSS validation for web projects
- Build verification

**Documentation:**
- Markdown linting
- Dead link detection
- Doc completeness checks

### Sample Workflow

```yaml
# .github/workflows/quality-checks.yml
name: Quality Checks
on: [pull_request]

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run linter
        run: npm run lint # or project-specific command
  
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run tests
        run: npm test
```

---

## 🚀 Enhanced Deployment

### Pre-Deploy Validation

Before deploying:
- [ ] Review deployment target (staging vs production)
- [ ] Verify environment variables configured
- [ ] Check dependency versions
- [ ] Confirm build passes locally

### Deployment Workflow

```yaml
# Enhanced deployment workflow
jobs:
  deploy:
    environment:
      name: production  # or staging
      url: ${{ steps.deployment.outputs.url }}
    steps:
      - name: Pre-deploy checks
        run: |
          npm audit --audit-level=high
          npm run build
      
      - name: Deploy
        uses: actions/deploy-pages@v4
      
      - name: Post-deploy verification
        run: |
          curl -f ${{ steps.deployment.outputs.url }}
```

### Rollback Procedures

- **GitHub Actions:** Revert commit, re-trigger deployment
- **Infrastructure:** Document rollback steps per platform
- **Communication:** Notify team of rollbacks via issue/PR comment

### Monitoring Post-Deploy

- Verify site is accessible
- Check for console errors
- Monitor error rates (if applicable)
- Test critical user flows

---

## 🔒 Enhanced Merge Stage

Add guardrails to prevent problematic merges.

### Branch Protection Rules

Recommended settings:
- [ ] Require pull request reviews before merging
- [ ] Require status checks to pass before merging
- [ ] Require branches to be up to date before merging
- [ ] Include administrators in protection rules

### Required Checks

Configure required checks to include:
- Quality gates (lint, test)
- Security scans
- Build verification
- Deployment readiness (if applicable)

### Merge Strategies

- **Squash:** Clean history, single commit per PR (recommended for most cases)
- **Merge:** Preserves full PR history
- **Rebase:** Linear history, more complex conflict resolution

---

## 📖 Additional Resources

- [Project Retrospective](../project-retrospective.md) - Lessons from first project
- [Improvement Issue](#4) - Tracking ongoing enhancements
- [GitHub Pages Docs](https://docs.github.com/en/pages)
- [GitHub Actions Docs](https://docs.github.com/en/actions)

---

## 🔄 Continuous Improvement

This workflow is a living document. After each project:
1. Document what worked
2. Identify friction points
3. Update these guidelines
4. Share learnings with team

---

*Last updated: Based on first project retrospective (Nov 1, 2025)*

---
