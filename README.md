# Antigravity Skills Collection

A curated collection of production-grade Agent Skills for [Google Antigravity](https://deepmind.google/technologies/gemini/). These skills extend the AI pair programmer with structured, evidence-based workflows for software engineering, quality assurance, infrastructure review, and portfolio releases.

---

## 📦 Included Skills

| Skill | Description | Primary Invocations |
|---|---|---|
| **[`github-portfolio-release`](skills/github-portfolio-release)** | Plans and executes a safe, professional release workflow for software portfolio projects: pre-push audit, cleanup, validation, GitHub publishing, Vercel deployment, and portfolio optimization. | `/github-portfolio-release`<br>`/github-portfolio-release audit`<br>`/github-portfolio-release full-pipeline` |
| **[`infrastructure-reliability-review`](skills/infrastructure-reliability-review)** | Acts as a Senior SRE & Platform Engineer to audit architecture, compute limits, database capacity, CI/CD resilience, failure scenarios, and performance bottlenecks. | `/infrastructure-reliability-review` |
| **[`read-markdown-docs`](skills/read-markdown-docs)** | Strictly read-only documentation reader and analyzer. Scans and synthesizes all `.md` files in a project folder to build complete architecture and requirements context. | Ask agent to scan/read docs folder |
| **[`software-quality-pipeline`](skills/software-quality-pipeline)** | Phased 5-stage software quality lifecycle: (1) Codebase Audit, (2) Systematic Testing, (3) Root-cause Debugging, (4) Regression Testing, and (5) Final Verification. | `/software-quality-pipeline`<br>`/software-quality-pipeline audit`<br>`/software-quality-pipeline full-pipeline` |

---

## 🚀 Installation

You can install these skills either globally (available across all workspaces) or per-project.

### Option A: Global Installation (All Projects)

Copy the desired skill folder(s) into your Antigravity global configuration directory:

#### Windows (PowerShell)
```powershell
# Copy all skills globally
Copy-Item -Path "skills\*" -Destination "$env:USERPROFILE\.gemini\config\skills\" -Recurse -Force

# Or copy a specific skill
Copy-Item -Path "skills\software-quality-pipeline" -Destination "$env:USERPROFILE\.gemini\config\skills\" -Recurse -Force
```

#### macOS / Linux (Bash)
```bash
# Copy all skills globally
cp -r skills/* ~/.gemini/config/skills/

# Or copy a specific skill
cp -r skills/software-quality-pipeline ~/.gemini/config/skills/
```

### Option B: Workspace / Project Installation

To make skills available only within a specific repository, place them in `.agents/skills/`:

```bash
# Inside your project root:
mkdir -p .agents/skills
cp -r /path/to/antigravity-skills/skills/* .agents/skills/
```

---

## 🔒 Security & Operating Principles

All skills in this repository follow strict Agentic safety standards:
- **Read-Only by Default**: Audit and inspection phases never mutate source code or cloud environments without explicit approval.
- **Zero Secrets**: Strict guidelines preventing any credentials, API keys, or private environment variables from being echoed into logs, diffs, or reports.
- **Evidence-Driven**: Claims, bottlenecks, and defects must cite concrete files, logs, or reproduced test results.
- **Progressive Disclosure**: Detailed phase guidelines are stored in `references/` subdirectories to keep context window overhead minimal.

---

## 📄 License

MIT License. See individual skill files for additional attribution and details.
