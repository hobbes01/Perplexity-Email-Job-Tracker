Daily job-email processing for <MAILBOX@gmail.com> address.
Process the Gmail inbox via the gcal connector for <MAILBOX@gmail.com>. 
Eligible messages are INBOX messages carrying none of the user labels 
4.Great match, 3.Good match, 2.Match, 1.Weak match, 0.Disregard, or 0.Other 
(system labels do not disqualify a message).

For each eligible message:
1. Identify advertised jobs it contains. Do not merge by title alone;
   deduplicate jobs using board-qualified job IDs or canonical vacancy URLs.
   If those are absent, document the fallback used (for example, a stable
   board name plus employer plus role title).
2. For each advertised job, assess its match against the CVs stored in
   Google Drive folder JobSearch/<JOBNAME>: file IDs <GOOGLE FILE ID1>
   (<CV1.md>) and <GOOGLE FILE ID2> (<CV2.md>). Read the actual CV content
   and the advertised requirements; do not infer preferences not in those CVs.
   Retrieve a linked vacancy page when the email does not give enough evidence
   to judge it. If France Travail extraction returns only metadata, read the
   fully rendered vacancy in a cloud browser; its descriptif-offre web
   component contains the requirements. Metadata-only output does
   not establish missing requirements.
3. Write the job to the Google Sheets spreadsheet with ID <GOOGLE FILE ID3>,
   worksheet "Jobs". Preserve exactly these 21 columns in this order: Job ID;
   Recruitment agency; Company; City; Job Title; Short Job Description;
   Date first identified in emails; Date last identified in emails;
   Number of times identified in emails; Job board name; Matching level with CV;
   Min Salary; Max Salary; Main contact; Reasons for applying;
   Reasons for not applying; Vacancy URL; Source email ID; Application;
   Application comment; Max Salary Num.
   The Recruitment agency holds the recruiting firm when one is identifiable
   (for example Adsearch, Randstad Digital, Akkodis, Amiltone recruiting on
   behalf of an employer); record Unknown when the job is posted directly by
   the employer or no agency is identifiable. The Company holds the hiring
   employer when identifiable from the email or vacancy, and the City holds
   the job's city or location when stated (for example Amiltone in Lyon becomes
   Company Amiltone and City Lyon); record Unknown for either when not stated.
   Record the Vacancy URL (the canonical vacancy URL used for deduplication, or
   Unknown when genuinely unstated) and the Source email ID (the Gmail message
   ID of the first email that advertised this job, for traceability).
   Distinguish salary from referral/recruitment bonuses: if a salary header
   conflicts with the description, preserve and flag the ambiguity rather
   than treating the bonus as salary. Record salary currency and pay period
   alongside the numbers when stated; record the main contact and salary as
   Unknown where genuinely unstated rather than omitting the row. Record
   reasons grounded in the CVs and vacancy details. The final Application,
   Application comment and Max Salary Num columns (S, T, U) are user-managed:
   the user records when they apply to a vacancy. Never populate, clear,
   normalize, or overwrite Application and Application comment, including
   existing dates, text, or formulas. Leave Application and Application
   comment blank for new jobs by writing only the automated columns A:R;
   update existing jobs only within A:R. Never rewrite entire rows or
   clear/replace the worksheet in a way that touches columns S and T.
   If a job row must be moved, preserve its Application and Application
   comment cells with the same Job ID. Read back and verify existing
   Application and Application comment entries remain unchanged after updates.
4. Persist each (Job ID, Gmail message ID) occurrence so repeated emails
   increment the count exactly once, and derive Date first/last identified
   from those email dates. Retry after a partial write must not double-count.
5. Commit order per message: extract every advertised job → upsert job rows
   and occurrence records → read back and verify → apply the label → archive
   by removing INBOX. If extraction or a write is incomplete, leave that
   message unlabelled and in the inbox for the next run rather than labeling
   it partially.

Labels and archiving:
- Apply 4.Great match for strong documented alignment with the CVs and no known
  critical mismatch; 3.Good match for solid documented alignment with only
  minor gaps; 2.Match for plausible alignment with meaningful gaps or
  uncertainty; 1.Weak match for alignment on only part of the role, for
  example the right skills but a salary or seniority clearly below the CV's
  level; 0.Disregard for a clear material mismatch. Write these exact prefixed
  strings into the Matching level with CV column so reports sort naturally. Use
  the strongest matching advertised job in a multi-job message to choose its label.
- For non-job-board email, apply 0.Other. The explicit non-job-board rule takes
  precedence: only a message whose sender is a job board receives the job-match
  labels; direct recruiter mail counts as non-job-board and gets 0.Other.
- Archive successfully processed messages, including 0.Other, by removing INBOX
  and adding exactly one of the six labels (4.Great match, 3.Good match, 2.Match,
  1.Weak match, 0.Disregard, 0.Other). The Gmail label names and the Matching
  level with CV values match, so Gmail and spreadsheet reports sort identically
  by importance.
- Never delete messages, send messages or applications, or modify anything else
  in the mailbox, Drive, or the spreadsheet outside the "Jobs" and occurrence
  storage.

Response format: report how many eligible messages were processed, how many new 
versus updated job rows, how many messages were archived under each label, and 
any message left for retry and why.

Verified during the 2026-10-13 manual run: the Gmail connector's text-only body 
strips per-job URLs from Hellowork digest emails, so record the Vacancy URL as 
Unknown with the documented fallback and rely on the board-qualified fallback ID; 
France Travail bodies include offer numbers (Offre n°NNNNNNN) usable as Job IDs; 
LinkedIn alert bodies keep canonical /jobs/view/NNNNNNNN/ URLs usable as Job IDs; 
keep occurrence storage in the "Occurrences" worksheet of the same spreadsheet 
(columns: Job ID, Gmail message ID, Email date) and read it before appending to 
merge counts.

Verified during the 2026-10-14 run: Indeed alert headers can claim a larger total 
(for example "21 nouveaux emplois") than the jobs actually embedded in the message 
body, so extract and record only the jobs present in the body and note in the run 
report that the alert's remaining jobs are only reachable via the alert's web link; 
Indeed bodies embed per-job redirect URLs whose jk parameter serves as a stable 
Job ID; use the cts.indeed.com tracking link as the Vacancy URL when the canonical 
job URL is not exposed.

Verified during the 2026-10-18 run: The user's Job mail tracker Gmail connector 
can return false-empty results for in:anywhere and label-filter searches. For 
archived-message verification, use in:all, optionally with distinctive subject 
text such as CompanyX; this retrieves archived messages and exposes their actual 
labels for commit verification.
