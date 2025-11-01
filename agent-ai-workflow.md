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

### Scenario: Simple GitHub Pages Site

1. **Intent** — Zack chats with Cursor:  
   “Let’s build a simple static site with a hero section and two links.”

2. **Spec** — Cursor creates a GitHub Issue with:
   - Outcome: Minimal static site with hero + links.
   - Constraints: Pure HTML/CSS, under 50 LOC.
   - Acceptance: Deployed via GitHub Pages, passes link check.

3. **Build** — Cursor codes `index.html` + `styles.css`, commits to branch, opens PR.

4. **Verify** — GitHub Actions run:
   - Markdown lint, HTML link checks.
   - Comments summary on PR.

5. **Review** — Zack checks diff and approves.

6. **Merge** — Branch protection enforces checks, merge to `main`.

7. **Deploy** — GitHub Pages Action publishes site live.

8. **Learn** — Zack inspects live page, opens a new Issue:  
   “Add a contact form.”

Cycle repeats.

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
