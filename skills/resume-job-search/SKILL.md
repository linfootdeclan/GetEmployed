---
name: resume-job-search
description: Searches for current job openings that match the résumé in the project, skips anything already logged, and adds verified leads to the Job Search Tracker. Use when the user asks to find jobs, run a job search, "find me N more leads," look for openings in a sector or city, or run the scheduled search.
---

# Résumé-Based Job Search

Find live job openings that fit the résumé stored in the project, score them, and log them in the project's Job Search Tracker.

## 1. Load context before searching

1. Read the user's Job Description profile, `claude/Job_Description.md`. It holds the search area, shift limits, sectors, hard filters, and what each 1-5 score means for this user. **If it doesn't exist, stop and run the `job-description` skill first**, then continue. Don't search on guessed preferences.
2. Read the user's most complete résumé from the project (a master or "complete" version if there is one; otherwise ask which one). Fall back to the PDF or DOCX résumés in the project. Pull out titles held, scope (team sizes, volume), tools and systems, certifications, education, and quantified wins.
3. Read `claude/Job_Search_Tracker.md` in full (if it doesn't exist yet, you'll create it in section 5), especially any standing-instructions note at the end. Build a list of every company + title + location already logged, including Passed, Applied, and removed rows, so nothing is re-listed.
4. Check memory notes about the job search for current goals and anything newer than the Job Description profile. If a note conflicts with the profile, ask the user which is current.
5. If the user gave a focus (a sector, a city, a count), use it. Otherwise use the broad target below.

## 2. Search rules (from the Job Description profile)

Apply the profile exactly. Don't add rules of your own.

- **Hard filters:** drop any lead that breaks one (excluded area, shift, sector, or pay structure). Check shift and hours before logging, not just title and pay.
- **Geography:** the area and commute limit the user stated. Always check the state or country code on a city match (the same city name often exists in several states).
- **Lanes:** the titles, sectors, and level the user stated, plus adjacent roles where the skills clearly transfer, if the profile allows adjacent roles.
- Do not re-chase sectors the tracker already marks as exhausted unless there is reason to think something changed.
- Never resurface a company + title + location the user asked to remove.

## 3. Search

- Use the Indeed connector (`search_jobs`, `get_job_details`) first. Also use JobsPipe and WebSearch/WebFetch for company career pages.
- Run each company, sector, and city as its own search. Do not blend queries.
- LinkedIn blocks direct fetches. Prefer company-hosted or Indeed `viewjob?jk=` links. Avoid `rc/clk` tracking links and `to.indeed.com` short links when a stable URL exists. If only a short link or search page is available, say so in the row.
- Open the actual posting for each candidate. Aggregator snippets (hotel and catering especially) go stale fast.

## 3b. Verify every lead before it is logged

A lead is not logged until it passes both checks below. Do this for every job found on a job board (Indeed, ZipRecruiter, Glassdoor, LinkedIn mirrors, CareerBuilder, JobsPipe, and so on).

**Check 1: Cross-reference the company's own website.**
- Find the employer's official careers page (or the franchisee's or operator's page when the brand is franchised). Do not trust a job board's copy of the company name alone.
- Confirm the same role is posted there: same title, location, and requisition number or pay if shown. A franchisee "hiring across many locations" does not prove the opening exists in one city, so confirm the specific location.
- Prefer the company-hosted link as the primary link in the tracker. Keep the board link as a backup.
- If the company's site doesn't show the role, mark the lead unverified, say why (not listed, careers page unreachable, role under a different title), and do not present it as a confirmed opening. Check for a title mismatch before dropping it.
- Confirm the employer is who the board says. For a recruiter or agency posting where the real employer isn't named, say so.

**Check 2: Check every link, on every board, for a live and active posting.**
- Open each link (the primary link and any backup link) and confirm: it resolves (no 404/410), it shows the same job, and it is still accepting applications (an Apply button or a working application page, no "expired," "closed," "no longer accepting," or "filled" notice).
- A plain page fetch can't see notices that a page adds after it loads. When the built-in browser or Claude in Chrome is available, load the page in the browser and read the rendered text and the Apply button, which catches closed listings a fetch misses. If no browser is available, say the check was fetch-only.
- Reject or mark dead: tracking redirects that no longer resolve, search pages standing in for a posting, listings older than their stated close date, and listings that disagree between the board and the company site.
- Link types: prefer the company careers page, then an Indeed `viewjob?jk=` URL. Avoid `rc/clk` tracking links and `to.indeed.com` short links. LinkedIn blocks direct fetches, so check the same role on the company site or Indeed instead.

**Record the result for each lead:**
- A **Verified** note in the Status cell: `Verified [date]: company site ✓ / board ✓ (browser)` or the equivalent, with the method (browser or fetch-only).
- Use the flag `🔗?` in the Quick view when any link could not be confirmed, and add the reason in the job description paragraph.
- Leads that fail either check go in the "checked and not logged" paragraph with the reason (expired, not on company site, link dead), not in the table.

Only the user's own look at a listing, or an employer confirmation or rejection email, overrides these results.

## 4. Qualify each lead

Keep a lead only if it is a real, open, non-duplicate posting that fits the résumé without a licensure or experience gap that would block it. Check for:

- Parent-brand duplicates (the same requisition cross-posted under two names).
- Title mismatches (listing title vs. what the posting body says).
- Pay: use the posted figure. If none is posted, give a labeled estimate and say so.

Score each lead 1-5 on Pay, Location, Workload, Skills, and Satisfaction, using the rating scale anchors in the Job Description profile (for example, what counts as a 5 for location is whatever distance the user stated). Skills compares the lead to the résumé; Satisfaction uses Glassdoor and Indeed employer ratings and the user's stated values. If rating data is thin or the employer is unnamed, say so and use a cautious middle score. Never invent precision. Flag a poor employer rating or a demanding schedule plainly even when the skills fit is strong. Apply the interest weighting the profile states (for example, a bonus of about a point on Skills or Satisfaction when a role clearly uses a listed interest, never past 5). Call out alignments the scores don't capture, such as a stated mission or values fit.

### Shorthand scorecard

Give every lead a compact, scannable score line alongside the full table row:

- **Code:** `P-L-W-S-Sat`, the five scores in that fixed order (Pay, Location, Workload, Skills, Satisfaction). Example: `4-3-2-5-3`.
- **Avg:** the mean of the five scores, one decimal (`3.4`). If a score is blank or unconfirmed, average only the scored fields and add `*`.
- **Flags** (only when they apply, at most three per lead): `⚠ rating` (employer rated poorly), `⚠ hours` (heavy or irregular schedule), `⚠ gap` (a real qualification gap), `$est` (pay is an estimate), `✓ close` (inside the user's preferred distance), `✓ values` (matches a value the user stated), `★ interest` (the role clearly uses an interest the user listed), `🔗?` (link not stable or unverified).
- **Verdict:** one line, under 12 words, saying why it's worth or not worth the user's time.

Sort quick views by Avg, highest first, then by Location.

## 5. Log results

At the top of each new batch, before the full table, add a **Quick view** table:

| # | Job @ Company | City | Code (P-L-W-S-Sat) | Avg | Flags | Verdict |
|---|---|---|---|---|---|---|

The full table and job descriptions stay below it as the detailed record. If the user asks for a quick view of the whole tracker, build one from every row with a status of Interested (or the statuses they name), sorted by Avg, without rewriting the earlier batches.


Append a new dated batch to `claude/Job_Search_Tracker.md`. Never overwrite earlier batches. Use the existing format:

- A table: Job | Company | Location | Pay | Location★ | Workload | Skills | Satisfaction | Status | Link
- Status defaults to "Interested." If the user reports a status change (applied, interviewed, passed), update that row in place. If asked to remove a job, delete its row and note the removal so it isn't resurfaced.
- A "Job descriptions" section with one short paragraph per lead: what the role is, pay basis, fit vs. gaps, employer rating, any schedule caveat.
- A short "checked and not logged" paragraph listing what was searched and dropped (expired, filled, wrong level, too far) so future runs don't repeat it.

Project docs have no in-place patch: read the whole tracker, add the batch, and write the full file back with `project_write`.

Report honestly when a search comes up short. Give the real count rather than padding with unverifiable leads.

## 6. Verification caveat

A fetch that shows no "expired" notice proves the link isn't dead, not that the posting still accepts applications. Section 3b describes the stronger check (company site plus a rendered-page check in a browser). Treat the user's own look at a listing, or an employer confirmation or rejection email, as the final word on status. Statuses can also be reconciled against the user's Gmail. Convert UTC timestamps to the user's time zone, and read confirmation emails for the actual title and requisition number.

## 7. Reply

Lead with the number of new leads, then show the shorthand Quick view for the top 3-5 by Avg (job @ company, city, code, avg, flags, verdict). Keep it to a compact table the user can read on a phone. Say what was ruled out in a sentence, including how many leads failed verification and how the rest were checked (company site, and browser or fetch-only). Link each job title to its apply link. Do not recap the steps. Offer to build an application package (tailored résumé and cover letter) for any lead the user wants to pursue.

## 8. Offer a daily status check (last step, every time)

End every search by prompting the user to schedule the automatic status check, so applications get tracked without being asked. Don't skip this step, but don't nag:

1. Check whether a scheduled application status check already exists for this user. If it does, skip the prompt (a single line, "your daily status check is still scheduled," is enough).
2. If none exists, ask once, with a short AskUserQuestion: "Want me to check your email, calendar, and company sites for application updates every day and keep your tracker current?" Options: yes, daily at a time they choose (suggest a weekday morning, such as 8:00 am); yes, but a different schedule; not now.
3. On a yes, follow `application-status-check` section 6b to create the scheduled task. On "not now," say it's available any time ("set up my daily status check") and don't ask again in the same session.
4. Keep this prompt separate from the results, after the Quick view and the offer to build an application package.
