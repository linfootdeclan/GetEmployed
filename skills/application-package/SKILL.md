---
name: application-package
description: Builds a tailored résumé and a cover letter for one specific job. Use when the user says "apply package for [job]," "write a cover letter for," "tailor my résumé for," "help me apply to," or points at a job in the Job Search Tracker, a posting link, or pasted job text.
---

# Application Package

Produce a tailored résumé and a cover letter for one job, using only what is true on the user's résumé.

## 1. Identify the job

The user may give a tracker row (company + title), a posting link, or pasted posting text.

- For a tracker row, find it in `claude/Job_Search_Tracker.md` and read its description paragraph and link.
- Fetch the live posting (Indeed `get_job_details`, WebFetch, or the company career page). If the posting can't be fetched, ask the user to paste it, and say the tracker entry may be stale.
- Pull out: exact title, company, location, pay, required and preferred qualifications, duties, tools or systems named, and the company's tone (formal corporate, casual, mission-driven).
- Note the requisition number if there is one. Application confirmations have repeatedly named a different title than the tracker row, so confirm which role the user is applying for if the posting and the tracker disagree.

## 2. Load source material

- Read the user's master résumé (the most complete version in the project; ask which one if unclear). Read `claude/Job_Description.md` if it exists, to see what the user is aiming for. Also check the project's shorter and corporate résumé versions to see which framing fits the job.
- Read any project summary notes and relevant memory notes for background, and `claude/Cold_Outreach_Log.md` if the company was already contacted, so the letter doesn't contradict earlier outreach.
- Never copy personal contact details from anywhere except the résumé header.

## 3. Match analysis (brief, for the user)

Before drafting, work out:

- The 4-6 strongest matches between the posting and the résumé, each with a specific, quantified proof point (for example, catering margins up to 87%, commissary waste cut by 70%, a team of 15 serving 200-450 customers daily, 20-volunteer weekly team leadership).
- Real gaps (a required license, years of experience, a sector the résumé never touches). State gaps plainly to the user. Do not hide them, and do not invent experience to cover them.
- Any alignment the résumé doesn't show on its face, such as a shared mission or values, a nonprofit cause, or a role close to home. Ask the user before leaning on personal values in the letter.

## 4. Tailor the résumé

Rules:

- Use only facts present in the master résumé. No invented titles, dates, numbers, tools, or certifications.
- Keep dates and employers exactly as in the master. If the master is ambiguous, ask.
- Reorder and reword to fit the posting: rewrite the professional summary for this role, lead with the most relevant bullets, mirror the posting's own keywords where they truthfully apply, and trim bullets that don't help.
- Keep it to one page when possible, two at most.
- Choose the framing that fits: the strengths on the résumé that the posting asks for most (for example, team leadership for a management role, vendor and inventory work for a supply role, teaching for an education role), and so on.

## 5. Write the cover letter

- Address a named hiring contact if the posting gives one. Otherwise use "Hiring Team."
- Open with the specific role and one concrete reason this company and role (not a generic opener).
- Body: two short paragraphs tying 2-3 proof points directly to the posting's top requirements. Use numbers from the résumé.
- If there is a gap, address it briefly and constructively (a transferable skill, willingness to get the credential) rather than ignoring it or apologizing at length.
- Close with a clear, confident next step. Under one page, around 250-350 words.
- Use plain, natural language in the user's voice: direct and warm, not buzzword-heavy. Avoid clichés ("passionate team player," "dynamic self-starter") and avoid repeating the résumé line for line.

## 6. Deliver

- **Default format: PDF.** The user's standing preference is PDF for the résumé and the cover letter. Deliver both as PDF files.
- Build each as an editable document first (a Docs artifact type if the session offers one, otherwise the docx skill), review it, then export or convert it to PDF (the Docs export, or the pdf skill). Check the PDF renders correctly: one page where possible, no cut-off text, fonts and spacing intact, text selectable (not an image), so applicant tracking systems can read it.
- Keep the editable version available and offer it (Word or the document) if the user wants changes. Send a Word file as well only if the user asks or the application portal requires it. If the user names another format for a specific job, use that one for that job only.
- Name files clearly: `<First>_<Last>_Resume_<Company>.pdf` and `<First>_<Last>_Cover_Letter_<Company>.pdf` (the user's name from the résumé header).
- **Save the files on the user's computer, in `Job Searching with Claude`.** Put every file this skill creates (the PDFs, and the editable versions if you made them) in a folder named `Job Searching with Claude` inside the user's Downloads folder (`~/Downloads/Job Searching with Claude/`). Steps:
  1. Send each file to the user with SendUserFile (so it also appears in the conversation). Then write it to the folder through the device bridge (`device_commit_files`, using each file's `fileUuid`).
  2. If the folder isn't reachable, **ask for it**: request access to the Downloads folder with `device_request_folder_access` (one request, with a one-line reason: "to save your résumé and cover letter in a folder called Job Searching with Claude"). Create the folder `Job Searching with Claude` there if it doesn't exist. Reuse it on later runs; don't make a second one.
  3. If the user declines, or no computer is linked, don't retry. Keep the files in the conversation, say they weren't saved to the computer and why, and how to link a computer.
  4. Never delete or overwrite a file in that folder that this run didn't create. If the same file name exists, add the date to the new name (`..._2026-10-10.pdf`) unless the user says to replace it.
  5. Tell the user the exact folder and file names, so they can find them for uploads on application forms.
- Save a short record in the tracker: add a line to the job's row or description paragraph noting that a package was prepared and which résumé version it used. Do not change Status to "Applied" until the user says they applied.
- A references sheet, if the project has one, is a separate document. Only mention it if the posting asks for references.

## 7. Boundaries

- Never submit an application, send an email, or fill in a form without the user's explicit yes. Drafts only.
- Never enter passwords or account details into application portals.
- If the user wants to email the package, create a Gmail draft and show them before anything is sent.

## 8. Reply

Give the user, in this order: the 3-5 strongest matches, any gaps or questions that need their input (one short list), and the delivered files. Keep it short. Offer to adjust tone, length, or which résumé version was used, and offer to fill out the actual application from the package (the `application-form-fill` skill).
