---
name: job-description
description: Interviews the user about the job they actually want (where, pay, schedule, skills, employer and culture) and saves it as their Job Description profile, which defines what a 1 to 5 rating means for them. Run first, before any other GetEmployed skill. Use when the user starts a job search, says "set up my job search," "define what I'm looking for," "update my job description," or when another skill finds no profile.
---

# Job Description (run this first)

Learn what the user wants in their next job, and turn their answers into a saved profile. Every other skill in this plugin reads that profile to decide what to search for, what to skip, and what a score of 1 or 5 means for this person. Nothing is assumed: the user's answers are the rules.

## 0. Set up a project to keep everything together (first run only)

Check whether this session is attached to a Claude project (the project tools will show it). Everything the plugin makes (profile, tracker, outreach log, status notes) is kept there so it stays in one place and every future session can find it.

- **Attached:** carry on. Say in one line that your documents will be saved to the project.
- **Not attached:** suggest it before the interview, once: "I recommend creating a project called Job Search in Claude and starting your job-search chats from it, so your profile, tracker, and notes all live together and every session can find them. I can't create the project for you; you make it in Claude, then start a new chat inside it." Don't block on it. Continue the interview, and keep the files in the computer folder below until a project exists. When the user later starts a session in a project, copy the profile and any notes into it.
- **The computer folder is always used, project or not.** Whatever the plugin saves to the project is also kept as a copy in `~/Downloads/Job Searching with Claude/` (request Downloads access once if needed, as `application-package` does). Never overwrite or delete a file the plugin didn't create.

## 1. Check what already exists

1. Look in the project for `claude/Job_Description.md`. If it exists, read it, show a short summary, and ask what the user wants to change. Update only those parts and keep the rest.
2. Read the user's résumé and `claude/Job_Search_Tracker.md` if they exist, plus any project notes about the search. Use them to propose answers ("your résumé shows 8 years in food service: stay in this field?") instead of asking cold. Never treat a guess as an answer: the user confirms.
3. If there is nothing, start the interview in section 2. Say in one line what this is for and that it takes a few minutes.

## 2. Interview

Ask in short batches (up to four questions per AskUserQuestion call), with two to four concrete options each plus the user's own words. Skip anything the user has already answered in this conversation or in the project. Don't ask all of these if the user gives a direction ("keep it simple," "just remote jobs"): fill the rest with sensible defaults and say so.

Each topic below ties to a score on the 1-5 rating scale. The answers become the anchors in section 3.

**A. Location (scores Location)**
- Where do you live, or where would you start from? (city and state is enough; no street address)
- Where are you willing to work: which towns, regions, or states? Anywhere to exclude?
- How far will you commute each way? (minutes or miles)
- Remote, hybrid, or on-site? Is relocation an option?

**B. Pay (scores Pay)**
- What pay do you need at minimum, and what would you be happy with? (annual or hourly; the user may skip a number and say "market rate")
- How do you want to be paid: salary, hourly, base plus commission, tips? Anything you won't accept (commission only, for example)?
- Benefits that matter: health insurance, paid time off, retirement match, bonus, flexibility.

**C. Workload and schedule (scores Workload)**
- Which days and hours can you work? Which shifts are off the table (overnight, early morning, weekends, holidays)?
- What is the most hours per week you'd take on? Is on-call, travel, or a long season acceptable?
- Is predictable scheduling or work-life balance a top priority or a nice-to-have?

**D. Role and skills (scores Skills)**
- What titles or kinds of work are you targeting? Which sectors are in, and which are out?
- What level: same as now, a step up, or open to a step down for the right place? How many years of a requirement can be missing before you'd call it a stretch?
- What do you want to do more of, and what do you never want to do again?
- Licenses, certifications, or tools you hold, and any you would be willing to get.

**E. Employer and satisfaction (scores Satisfaction)**
- Company size and type you prefer (small local business, franchise, large employer, nonprofit, public sector)?
- The lowest employer review you'd accept (for example 3.5 of 5 on Glassdoor or Indeed) and how much weight reviews should get.
- Values, mission, or culture that matter (for example mission-driven work, a faith or community fit, growth and training). Anything that would make you pass.

**F. Interests that could become job skills (scores Skills or Satisfaction)**
Ask exactly two questions, built from what the résumé, the project notes, and the earlier answers actually show about the user. Don't guess interests from nothing, and don't pull in anything sensitive.
1. Offer three or four interests the user's own history suggests (for example, tutoring and training from a teaching background, event planning from catering, content creation from design or video work, community leadership from volunteering). Ask which they'd want to use in their next job, with a free-text option. If the history shows nothing, ask an open question instead: "What do you enjoy doing that you'd like part of your work?"
2. Ask how much interests should count: a bonus to the Skills or Satisfaction score, a tiebreaker between similar jobs, a target to build the search around, or only material for cover letters and interviews.
Record the answer under Preferences, and add the `★ interest` flag for roles that clearly use one.

**G. The search itself**
- How urgent is it (already unemployed, employed and exploring, a target start date)?
- How many leads at a time do you want, and how often?
- Any companies to avoid or to prioritize?

Never ask for, and never store: government ID numbers, financial account details, passwords, health information, or demographic information. If the user volunteers something like that, don't write it down. Pay minimums and pay targets are the user's own preference: store them only if the user says to.

## 3. Turn the answers into a rating scale

Write an anchor for each score from 1 to 5 in the user's own terms. Show the draft and let the user adjust it before it is saved. Examples of how answers become anchors:

| Score | Location (from "where, how far") | Pay (from "minimum, target") |
|---|---|---|
| 5 | Inside the preferred area, under 15 minutes | At or above the target |
| 4 | Inside the preferred area, 15-30 minutes | Between minimum and target |
| 3 | At the edge of the stated commute limit | At the minimum |
| 2 | Slightly over the limit but reachable | Under the minimum by a little, or pay unknown |
| 1 | Outside the area or past the limit | Well under the minimum |

Do the same for Workload (matches the stated days, hours, and shifts exactly = 5; breaks a stated shift limit = 1), Skills (meets nearly every requirement = 5; a real gap in a core requirement = 1), and Satisfaction (rating at or above the preferred floor and matching the stated values = 5; below the floor or conflicting with a stated value = 1). Use the user's numbers and words. If the user gave no number for something, say the anchor is qualitative, and don't invent a figure.

## 4. Deal-breakers and flags

Separate **hard filters** (the lead is dropped, never scored) from **preferences** (they move the score). Typical hard filters: excluded shifts, excluded areas, excluded sectors, commission-only pay, a wage below the stated minimum. Also record which flag labels apply to this user, for example `✓ close` (inside the preferred distance), `✓ values` (matches a stated value), and `★ interest` (the role clearly uses an interest the user wants to use), plus the standard `⚠ rating`, `⚠ hours`, `⚠ gap`, `$est`, and `🔗?`.

## 5. Save it

Confirm the summary with the user, then write `claude/Job_Description.md` with `project_write` (read first and write the whole file if it exists). Use this layout:

```
# Job Description
Last updated: <date>

## Summary
One short paragraph in the user's words: the job they want and where.

## Hard filters (drop, don't score)
- ...

## Preferences
Location / Pay / Workload / Skills and role / Employer and values / Interests / Search logistics

## Rating scale (1-5)
| Score | Pay | Location | Workload | Skills | Satisfaction |
| 5 | ... |
| 4 | ... |
| 3 | ... |
| 2 | ... |
| 1 | ... |

## Flags in use
...

## Change log
- <date>: created / what changed
```

Also save a copy of the profile to `~/Downloads/Job Searching with Claude/Job Description.md` (find the folder first; if the file already exists and is one of the plugin's, update it in place; if the computer isn't linked, skip and say so). Tell the user in two or three lines what was saved and where (project and folder), and that every skill will now follow it. Offer the next step: `resume-job-search` to find leads that match, or `application-package` if they already have a job in mind. If the user changes their mind later ("I'll work weekends now"), update the file and add a change-log line. Existing tracker scores are not re-scored unless the user asks.

## 6. How the other skills use it

- `resume-job-search` takes its search area, shift limits, sectors, and scoring from this file. It stops and runs this skill first when the file is missing.
- `application-package`, `interview-prep`, and `hiring-contact-outreach` use the summary, the role target, and the stated values to choose what to emphasize.
- `application-form-fill` offers the stated schedule, availability, and start-date preferences as suggested answers. The user still confirms them, and still answers pay, authorization, background, and demographic questions personally.

Keep the file short and plain enough that the user can read and edit it themselves.
