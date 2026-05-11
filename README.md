# Job Search & Apply — Claude Skill

A skill for [Claude Cowork](https://claude.ai) that automates your job search and application workflow. It reads your resume, finds matching jobs on Indeed and Dice, tailors cover letters to each role, recommends which resume to use, and keeps your application tracker up to date.

---

## What It Does

**Search for jobs** — Claude reads your actual resume files before searching, so matches are grounded in your real experience rather than generic keywords. Results include a "Why" note for each job explaining the specific match.

**Tailor documents** — For each job you want to apply to, Claude writes a purpose-built cover letter and recommends the best resume from your library. It shows you exactly what it changed and why before saving anything.

**Track applications** — A simple `jobs.txt` file tracks everything: jobs to apply to, which resume and cover letter to use for each, and a completed section for jobs you've already submitted.

You stay in control at every step — Claude never adds jobs to your list, saves files, or moves entries without your explicit approval.

---

## Prerequisites

- [Claude Cowork](https://claude.ai) (desktop app)
- **Indeed** and/or **Dice** connected as connectors in Cowork (Settings → Connectors)
- Microsoft Word, LibreOffice, or Apple Pages to open the generated `.docx` files
  - Don't have any of these? [LibreOffice](https://www.libreoffice.org) is free and works great.

---

## Installation

1. Download **[job-search-apply.skill](../../releases/latest)** from the Releases page
2. Double-click the `.skill` file — Cowork will install it automatically
3. That's it

---

## Workspace Setup

On first use, Claude will guide you through this automatically. But if you'd like to set it up in advance, create this folder structure in your Cowork project directory:

```
Your Project Folder/
├── Resumes/
│   └── YourName_Resume_Specialty.docx   ← your base resume(s)
├── Cover Letters/
│   └── BaseCoverLetter.docx              ← your base cover letter template
└── Jobs/
    └── jobs.txt                          ← auto-created on first run
```

You can have multiple base resumes (e.g. one for each specialty) — Claude will pick the best fit for each job.

---

## How to Use

Once installed, just talk to Claude naturally in any Cowork session. The skill triggers automatically. Some examples:

| What you say | What happens |
|---|---|
| *"Find me jobs that match my resume"* | Reads your resumes, searches Indeed/Dice, presents matches with fit notes |
| *"Search for remote M365 admin roles, no clearance"* | Targeted search with your filters applied |
| *"Tailor a cover letter for the Leidos job in my list"* | Reads the job description, writes a tailored cover letter, recommends a resume |
| *"Get me ready to apply to the best jobs on my list"* | Full workflow — search, select, tailor, track |
| *"Which resume should I use for this posting?"* | Compares your resumes to the job and gives a recommendation with reasoning |

---

## The Three Modes

**Search** — Claude reads your resume(s), asks for any filters (remote only, salary floor, employment type, etc.), searches available connectors, and presents results. You choose which ones to add to your tracker.

**Tailor** — Point Claude at a job (from your tracker, a URL, or a pasted description). It writes a tailored cover letter, recommends a resume, shows a change summary, and saves everything once you approve.

**Full Workflow** — Chains search and tailor together in one session. Claude finds the jobs, you pick which ones you like, then it preps the documents for each.

---

## Application Tracker Format

Your `jobs.txt` stays in this format automatically:

```
JOBS TO APPLY TO
================
Remove a job from this list once you've submitted your application.
────────────────────────────────────────────────────────────────
[ ] Senior M365 Administrator
    Company:  Leidos
    Type:     Contract | Remote | Public Trust Required
    URL:      https://www.dice.com/job-detail/...
    Resume:   Witherspoon_Jonathan_Resume_M365Admin.docx
    Cover:    CoverLetter_M365Admin_Leidos.docx
────────────────────────────────────────────────────────────────
ALREADY APPLIED
===============
[X] M365 Admin L3
    Company:  Asterism IT Solutions
    Type:     Full-time | $110,000–$120,000/yr | 100% Remote
    URL:      https://www.dice.com/job-detail/...
    Resume:   Witherspoon_Jonathan_Resume_MSP.docx
    Cover:    CoverLetter_M365AdminL3_Asterism.docx
    Applied:  May 2026
```

---

## File Naming

Claude follows these conventions automatically:

- **Resume**: `LastName_FirstName_Resume_Keyword.docx`
- **Cover Letter**: `CoverLetter_RoleKeyword_Company.docx`

---

## Built With

- [Claude Cowork](https://claude.ai) — Anthropic's desktop AI assistant
- [Claude Skill SDK](https://docs.claude.ai) — skill framework for custom workflows
- Indeed and Dice MCP connectors for job search

---

## License

MIT — use it, fork it, improve it.
