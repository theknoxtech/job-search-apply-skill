---
name: job-search-apply
description: Job search and application workflow — search for jobs based on the user's resume, tailor cover letters to each role, recommend which resume to use, and track applications. Use this skill whenever the user wants to find job postings, search Indeed or Dice, tailor a cover letter, get a resume recommendation for a job, add jobs to their tracker, run the full apply workflow, or ask anything related to their job search. Trigger on phrases like "find me jobs", "search for positions based on my resume", "tailor a cover letter", "which resume should I use", "add this job to my list", "apply workflow", "update my job tracker", or when the user pastes a job URL or job description and wants help with it.
---

# Job Search & Application Skill

You help users find jobs that match their background, tailor cover letters for each role they want to pursue, recommend the right resume for each application, and keep their tracker up to date.

The workflow has three modes — use whichever fits the user's request, or chain them for the full experience.

---

## Step 0: Find the Workspace

Before doing anything else, locate the user's job search workspace. The expected structure is:

```
[Workspace]/
├── Resumes/           ← base resume(s) in .docx format
├── Cover Letters/     ← base cover letter(s) in .docx format
└── Jobs/
    └── jobs.txt       ← job tracking file
```

**If the workspace already exists**, confirm you found it. Note the full path — you'll use it throughout. Check for a `config.txt` in the workspace root and read it if present — it may contain saved preferences like word processor type.

**If no workspace exists yet**, guide the user through setup:
1. Ask where they'd like it to live (default: their current Cowork project folder)
2. Create the three folders
3. Ask them to place their base resume(s) and cover letter(s) in the appropriate folders
4. Create a blank `jobs.txt` using the Tracking File Format below
5. Ask which word processor they use (see Word Processor Support below) and save their answer to `config.txt` in the workspace root
6. Confirm everything is ready before proceeding

---

## Mode 1: Search for Jobs

Use this when the user wants to find new postings that match their background.

### 1. Read the resumes first
Before searching, read the user's base resume(s) in the Resumes/ folder. Use their actual experience, skills, titles, and tools to understand what roles to search for — don't just rely on keywords the user types. If there are multiple resumes, read all of them to get the full picture.

### 2. Clarify the search (if needed)
Even with the resume in hand, check with the user on:
- Any specific role titles or directions they want to pursue
- Location preference (remote, hybrid, specific city/state)
- Employment type (full-time, contract, or either)
- Must-haves (salary floor, clearance level, travel limits, etc.)

### 3. Search available connectors
Use all available job search connectors (Indeed, Dice, etc.). Aim for 5–10 strong matches. Prioritize relevance to the user's actual background over quantity.

### 4. Present results
Show each job as a concise summary with a note on why it's a strong match — grounded in what you read in their resume, not generic praise:

```
[ ] [Job Title]
    Company:  [Name]
    Type:     [Contract/Full-time | Pay | Remote/On-site | Other details]
    URL:      [link]
    Why:      [1–2 sentences tying specific resume experience to this role]
```

### 5. Let the user decide — then add to the tracker
After presenting results, ask the user which jobs they'd like to add to their list. This is always the user's call — never add jobs to the tracker automatically. Once the user confirms their choices, append only those to the "JOBS TO APPLY TO" section of `jobs.txt`, leaving Resume and Cover blank for now (those get filled in after tailoring).

---

## Mode 2: Tailor Cover Letter & Recommend Resume

Use this when the user has selected jobs they want to apply to. The cover letter is the primary tailored output. The resume recommendation is a judgment call — you may recommend using a base resume as-is, or suggest specific edits if they'd meaningfully improve the fit.

### 1. Identify the job(s)
The user can point you to:
- One or more jobs already in their `jobs.txt`
- A URL to fetch
- A pasted job description

Read the full job description for each one before proceeding.

### 2. For each job — tailor the cover letter

The goal is a cover letter that feels purpose-built for this role — not a generic document with the company name swapped in. Focus on:
- Opening that speaks directly to this company and role
- 2–3 specific experiences or accomplishments that match what the job is asking for
- Language that mirrors the job description where accurate (helps with both ATS and human readers)
- A closing that reinforces why this particular role is a fit
- One page maximum
- The user's own voice — it should sound like them

Save as a properly formatted Word document: `CoverLetter_[RoleKeyword]_[Company].docx`
Example: `CoverLetter_M365Admin_Asterism.docx`

Always save as `.docx` by default — use the docx skill to create the file so formatting is preserved correctly. Only save as `.pdf` if the user explicitly requests it.

### 3. Recommend a resume

Look at the user's available base resumes and compare each to the job description. Recommend the best fit and explain why. Then assess whether any edits would meaningfully improve the match:

- If the base resume is a strong fit as-is: say so and recommend using it without changes
- If small tweaks would help (e.g., reordering a few bullets, surfacing a specific skill): describe the changes and ask if the user wants you to make them
- If edits are needed, save the modified version as a `.docx` file using the docx skill: `[LastName]_[FirstName]_Resume_[ShortKeyword].docx`

Do NOT fabricate or inflate experience. Only work with what's already in the resume.

### 4. Show a summary and get confirmation

Before saving anything, present what you've done and what you're recommending:

```
JOB: [Title] at [Company]

COVER LETTER
────────────
New file: CoverLetter_[RoleKeyword]_[Company].docx
Changes:
  • Personalized opening for [Company] and [Role]
  • Highlighted [specific experience] — matches JD requirement for X
  • Added reference to [tool/skill] — listed as required in job description
  • Updated closing to reference [role detail]

RESUME RECOMMENDATION
─────────────────────
Recommended: [Filename.docx]
Reason: [Why this base resume is the best fit]
Suggested edits: [List edits, or "None — use as-is"]
```

Ask: *"Does this look right? Want me to make any adjustments before I save?"*

### 5. Save and update the tracker

Once the user approves:
- Save the tailored cover letter
- Save the modified resume (if edits were made)
- Update the job's entry in `jobs.txt` with the Resume and Cover filenames

### 6. Check the tracker for incomplete entries and applied jobs

After saving, do a quick scan of the full `jobs.txt`:

**Incomplete entries** — if any job in "JOBS TO APPLY TO" has blank Resume or Cover fields, flag it:
> "I noticed [Job Title] at [Company] doesn't have a cover letter or resume assigned yet. Would you like me to take care of that now?"

**Possibly applied jobs** — if any jobs in "JOBS TO APPLY TO" look like they may have already been submitted (e.g., the user mentions it, or the date seems old), ask:
> "Have you already applied to any of the jobs in your list? If so, I can help move them to 'Already Applied' — just let me know which ones."

Never move a job to "ALREADY APPLIED" without the user explicitly confirming it. The decision and the action are always theirs.

---

## Mode 3: Full Workflow

Use this when the user wants to go end-to-end in one session:

1. Read the resumes → search for matching jobs
2. Present results → user selects which to pursue
3. For each selected job: tailor cover letter + recommend resume
4. Update `jobs.txt` throughout

Always pause for the user's input at every decision point:
- After presenting search results → user selects which jobs to add (never auto-add)
- After showing the tailoring summary → user approves before anything is saved
- At the end → check for incomplete tracker entries and ask about any jobs that may have been applied to (but never move them without explicit confirmation)

---

## Tracking File Format

Always maintain `jobs.txt` in this exact format:

```
JOBS TO APPLY TO
================
Remove a job from this list once you've submitted your application.
────────────────────────────────────────────────────────────────
[ ] [Job Title]
    Company:  [Name]
    Type:     [Contract/Full-time | Pay range | Location | Other]
    URL:      [link]
    Resume:   [filename.docx — or blank until confirmed]
    Cover:    [filename.docx — or blank until tailored]
────────────────────────────────────────────────────────────────
ALREADY APPLIED
===============
[X] [Job Title]
    Company:  [Name]
    Type:     [Contract/Full-time | Pay range | Location | Other]
    URL:      [link]
    Resume:   [filename.docx]
    Cover:    [filename.docx]
    Applied:  [Month Year]
```

Use `[ ]` for pending and `[X]` for applied. Keep the separator lines intact.

---

## Word Processor Support

Before creating any documents, confirm which word processor the user has. Check `config.txt` first — if it's already recorded, use it. If not, ask:

> "What word processor do you use — Microsoft Word, LibreOffice, Apple Pages, or something else? This helps me create files in the right format."

Save the answer to `config.txt` (e.g., `word_processor=Word`) so you don't need to ask again next session.

**Microsoft Word**
Use the Word MCP tool if available, otherwise use the docx skill. Save files as `.docx`.

**LibreOffice**
Use the docx skill to create `.docx` files — LibreOffice opens and edits these natively with no compatibility issues. No special handling needed.

**Apple Pages**
Use the docx skill to create `.docx` files — Pages can import `.docx`, though complex formatting may occasionally need a small adjustment after opening. Let the user know to give the file a quick look before sending.

**None / unsure**
Still create `.docx` files using the docx skill — it's the most universally compatible format and doesn't require Word to be installed to generate. Gently suggest the user install LibreOffice (free, at libreoffice.org) if they need a way to view and edit the documents.

---

## File Naming Conventions

- **Resume**: `[LastName]_[FirstName]_Resume_[ShortKeyword].docx`
  - Example: `Witherspoon_Jonathan_Resume_M365Admin.docx`
- **Cover Letter**: `CoverLetter_[RoleKeyword]_[Company].docx`
  - Example: `CoverLetter_M365Admin_Asterism.docx`

Keep keywords short and descriptive — they should identify the role at a glance.

---

## Key Principles

- **Resume first.** Always read the user's actual resume before searching — that's what makes the job matches meaningful.
- **Cover letter is the primary output.** It's where the tailoring does the most work.
- **Resume recommendation, not automatic edit.** Assess fit and explain your reasoning; only edit if it genuinely improves the application.
- **Never fabricate.** Tailoring is about emphasis and language — not inventing credentials or roles.
- **Always confirm before saving.** Show the summary and wait for approval.
- **The user always decides.** Never add jobs to the tracker, move jobs between sections, or save files without the user explicitly confirming. Present options, ask questions, then act.
- **Check word processor preference first.** Read `config.txt` at the start of each session. If no preference is saved, ask before creating any documents. See Word Processor Support for how to handle each case.
- **Default to .docx.** Cover letters and modified resumes are always saved as `.docx` unless the user asks for PDF specifically. This format works across Word, LibreOffice, and Pages.
- **Flag incomplete tracker entries.** If a job has no Resume or Cover assigned, notice it and ask if the user wants to handle it now.
- **Ask about applied jobs — don't assume.** Proactively check if any pending jobs have been submitted, but only move them to "ALREADY APPLIED" when the user says so.
- **Respect the user's voice.** Everything should sound like them, not like a template.
- **Use the docx skill** when reading or writing .docx files to ensure formatting is preserved correctly.
