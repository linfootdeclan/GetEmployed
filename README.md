# GetEmployed

A plugin that runs a job search from start to finish. It starts by learning what job you actually want, then finds matching openings, keeps them in a Job Search Tracker, builds tailored applications, fills the forms for your review, follows up with hiring contacts, and tracks every reply.

## Install

```
/plugin marketplace add linfootdeclan/GetEmployed
/plugin install get-employed@get-employed
```

Requires Claude Code 2.1.275 or later, or the Claude desktop app with plugins turned on.

## Requirements

- **Connections:** Gmail and Google Calendar (status checks, drafts), Indeed and JobsPipe (job search, optional but recommended), and Google Drive or a Claude project to keep your files.
- **Your computer:** the Claude desktop app linked to your computer, so the plugin can save files to your Downloads folder and use the built-in browser for applications.
- **A résumé** you can attach or paste. The plugin never invents anything that is not on it.
- **A Job Description profile.** Run `job-description` first; every other skill reads it.
- **A daily scheduled task (optional).** The status check offers to set one up, and only does so after you say yes.

## Start here

Run `job-description` first. It asks about where you want to work, pay, schedule, skills, the kind of employer, and interests you'd like to use at work, and saves your answers as a Job Description profile. Every other skill reads that profile, so searches, scores, and applications follow your own rules, not defaults.

## Skills

| Skill | What it does | Try saying |
| --- | --- | --- |
| `job-description` | Interviews you about the job you want and saves it as your profile, including what a 1 to 5 rating means for you on pay, location, workload, skills, and satisfaction. Run first. | "Set up my job search" |
| `resume-job-search` | Searches for current openings that fit your résumé and your profile, skips anything already logged, drops anything that breaks your deal-breakers, scores each lead, and adds verified ones to the tracker. Ends by offering to schedule a daily application status check. | "Find me 10 new leads" |
| `application-package` | Builds a tailored résumé and a cover letter for one job, using only what is true on your résumé, and flags any gaps. Saves the PDFs in a `Job Searching with Claude` folder in your Downloads. | "Build an application package for this job" |
| `application-form-fill` | Finds the real application page, fills it out accurately from the tailored résumé, asks you about any question it can't answer, and stops at a final review so you submit. | "Fill out the application for this job" |
| `hiring-contact-outreach` | After you apply, finds the right person to contact (hiring manager, recruiter, location leader) from public sources and drafts a short, specific email with your PDFs attached. Drafts only. | "Find the hiring manager for this job" |
| `application-status-check` | Checks Gmail, Calendar, and company sites for application news and updates the tracker: received, action needed, interview (with round, such as Round 2 of 3), offer, or rejection. Offers to add each scheduled interview to your calendar. Keeps a `Job Search Tracker with Claude.xlsx` spreadsheet up to date in the same Downloads folder. Can run every day as a scheduled task, set up after you say yes. | "Any updates on my applications?" |
| `interview-prep` | Once an interview is scheduled, researches the posting and the company, then builds a step-by-step guide: what to know, what to have ready, likely questions with your own stories, questions to ask, and a thank-you note. | "Prep me for my interview" |

## Shorthand scorecard

Every lead gets a one-glance score line: `P-L-W-S-Sat` (Pay, Location, Workload, Skills, Satisfaction), an average, a few flags, and a one-line verdict. The scores come from the rating scale in your Job Description profile.

| Job @ Company | City | Code | Avg | Flags | Verdict |
| --- | --- | --- | --- | --- | --- |
| Example role @ Example Co | Example City | 4-4-3-5-4 | 4.0 | ✓ close | Strong fit, nearby, solid reviews |

Flags: `⚠ rating`, `⚠ hours`, `⚠ gap`, `$est`, `✓ close`, `✓ values`, `★ interest`, `🔗?`. Ask for "a quick view of the tracker" any time to see every open lead this way, best first.

## Verification

Before a lead is logged, the search skill cross-references the job on the company's own careers site and checks every link on every board for a live, active posting. It uses a real browser to read the rendered page and Apply button when one is available, because a plain page fetch can miss closed listings. Leads that fail either check are listed as not logged, with the reason.

## What it uses

- Your Job Description profile, your master résumé, and a `Job_Search_Tracker.md`, all kept in the project
- Indeed, JobsPipe, and web search for postings
- Gmail (read-only) and Google Calendar for status updates. Calendar events are only created after you say yes
- Gmail drafts, only when you ask, with your résumé and cover letter attached. Nothing is sent without your approval

## Where your files go

Your résumés, cover letters, and the tracker spreadsheet are saved in `Downloads/Job Searching with Claude/` on your computer. The plugin asks for access to your Downloads folder the first time, and it never deletes or overwrites a file it didn't create. Your Job Description profile is copied there too. The full tracker, profile, and notes are also kept in your Claude project, so create a project called Job Search and start your job-search chats from it (the plugin can't create the project for you). The folder on your computer is used either way.

## Ground rules built in

- Your own Job Description profile sets the rules: search area, shifts, sectors, pay, and what counts as a good score
- Résumés and cover letters are delivered as PDFs by default (Word on request or when a portal requires it)
- Outreach is drafted after you apply, to publicly listed business contacts only; never sent without your yes. Wording fits the day the email will be sent
- You attach your résumé on application forms yourself, on a computer; the plugin fills everything else, including follow-up forms, and checks every date it fills
- Never invents experience, titles, numbers, or certifications
- Never submits an application without your explicit yes for that specific job
- Never creates accounts, enters passwords, ID numbers, or payment details, or solves CAPTCHAs; you do those steps
- Always asks you about pay expectations, work authorization, background and demographic questions, and agreements
- A link that "looks live" is not proof a posting is open; your own look wins

## Planned next

- Mock interviews with feedback
- Follow-up and thank-you note drafts

## Privacy and data

The plugin is written instructions only: no code, hooks, servers, or analytics, and the author receives none of your data. Search terms go to the job-search connectors you enable (Indeed, JobsPipe) and to web search; your résumé goes only to employers you apply to, and only after you approve each application. Full details: [Privacy](PRIVACY.md).

## License and author

MIT License (see `LICENSE`). By Declan Linfoot, linfoot.declan@gmail.com, https://github.com/linfootdeclan. Issues and suggestions are welcome on the GitHub repository.
