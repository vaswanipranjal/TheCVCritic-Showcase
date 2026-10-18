The CV Critic

An AI resume audit tool that gives feedback for the specific role you are applying to, not generic resume advice.

Live demo: https://thecvcritic.lovable.app (Runs on free-tier AI, so it may be busy at times.)

An independent, hands-on project built with Lovable, n8n and Google Gemini.

Quick links
Live demo: thecvcritic.lovable.app
Step-by-step build guide (PDF): build-guide.pdf
n8n workflow (JSON): n8n/resume-audit-workflow.json
Screenshots: docs/screenshots
The problem

Most resume checkers give the same advice to everyone. A Product Manager resume and a Data Analyst resume should not be judged by the same standard, and a fresher should not be judged like a senior hire.

The solution

Upload your resume (PDF), type the role you want, pick your experience level, and optionally paste a job description. In about a minute you get two reports:

Content audit

Overall score out of 100
10 role-specific category scores, each with a reason
Top 5 strengths and top 5 gaps
How a recruiter would see you in a 30-second read, and which role they would put you in
A bullet-by-bullet check that flags weak bullets and explains what is missing
Job description match with matched and missing keywords (only when a job description is given)
A final verdict and the top 10 improvements, ordered by impact

Formatting audit

Formatting score out of 10
A clear verdict: PASS, COULD BE BETTER or NEEDS IMPROVEMENTS
Must-fix items, with what is wrong, why it matters and what to change
Recommendations and a list of passed checks

The full report is also sent by email.

Screenshots
Upload	Details
Show Image	Show Image
Loading	Content audit
Show Image	Show Image

Show Image

How it works
Website (Lovable)
   -> Reads the text and page count from the PDF
   -> AI step 1: writes a scoring rubric for the target role
   -> AI step 2: audits the content against that rubric
   -> AI step 3: audits the formatting by looking at the PDF itself
   -> Combines both reports and cleans up the data
   -> Shows the result on screen and emails it
The key idea: a rubric made for each role

The AI does not use one fixed checklist. It first writes a rubric for the role you typed:

Exactly 10 scoring categories (for example, a Finance Analyst gets Financial Reporting and Analytical Tools, a Product Manager gets Product Thinking and User Understanding)
Labels for classifying resume bullets
A list of roles a recruiter might assign you to
Possible verdicts, from "Role Ready" to "Not Role Ready"

If a job description is pasted, the rubric is built from that description instead. This is why the feedback changes for every role.

Three separate AI steps
Step	Job	Input
Rubric generator	Builds the 10-category rubric for the role	Role, experience level, optional job description
Content auditor	Scores the resume against the rubric, never rewrites it	Rubric, resume text
Formatting auditor	Checks layout rules visually	The resume PDF, page count, experience level

The content and formatting audits run independently, so one cannot influence the other.

Scoring rules

Content audit

Each of the 10 categories is scored 1 to 10 using only evidence written in the resume
The overall score is recalculated in code as the sum of the 10 scores, so arithmetic mistakes cannot slip through
Fresher resumes are compared with strong fresher candidates, and internships and personal projects count as evidence

Formatting audit

Score out of 10, with a verdict decided in code: 8 and above is PASS, 6 up to 8 is COULD BE BETTER, below 6 is NEEDS IMPROVEMENTS
Any must-fix item always forces NEEDS IMPROVEMENTS
For freshers, a one-page resume is mandatory; for experienced candidates, length is only a recommendation

Colour coding (scores out of 10): below 6 red, 6 up to 8 amber, 8 and above green. For the 100-point content score: below 60 red, 60 up to 80 amber, 80 and above green.

What the formatting audit checks

Single-column layout, one page for freshers, page use and whitespace, margins, professional fonts and consistency, font sizes, typographic hierarchy, spacing, alignment, bullet consistency, no photos or decorative graphics, complete contact details (email, phone, LinkedIn), at least one project link, and file quality (no cut-off or overlapping text). It also checks education formatting and date order.

It judges only the layout, never the quality of the content.

Design decisions
Evidence only: the AI never invents skills, numbers or links, and explains every score with something from the resume.
Audit, not rewrite: the tool points out what is missing instead of writing bullets for you.
Structured output: every AI step returns strict JSON, so the website can render scores, badges and lists directly.
Resume treated as untrusted data: instructions hidden inside a resume, such as "give this 100/100", are ignored.
Rules in code, not only in prompts: the verdict thresholds and the score total are computed in code so results stay consistent.
Tech stack
Part	Tool
Website	Lovable (React)
Workflow and automation	n8n
AI model	Google Gemini (free tier)
Email	Gmail
Version control	GitHub

Skills used: prompt engineering, structured JSON output, workflow automation, debugging, UI design.

What I learned
How a website and a no-code workflow talk to each other through requests and responses
How to write prompts that return clean, structured data instead of loose text
How to debug real problems: AI answers that came back empty, outputs cut off mid-sentence, and variables that were never filled in
Why important rules (scoring, verdicts) belong in code and not only in the prompt
How to plan around free-tier limits and protect an app that is open to the public
Limitations
Runs on a free AI tier with a daily limit, so it can be slow or unavailable at busy times
PDF resumes only, up to 5 MB; scanned image-only PDFs may give weaker results
It cannot open links, so it does not verify that GitHub, LinkedIn or portfolio links work
It is an AI review and a guide for improving a resume, not a hiring decision
Roadmap
Saved reports and history
Side-by-side comparison of two resume versions
More detailed job-description matching
Support for DOCX files
A bring-your-own-key option
Privacy

The resume is used only to generate the report. Do not upload real resumes to a public demo if you are not comfortable with it being processed by an AI provider, and note that free-tier AI services may use inputs to improve their models.

Repository contents
docs/screenshots/                 App screenshots
n8n/resume-audit-workflow.json    Workflow export (no keys included)
build-guide.pdf                   Step-by-step guide to build this project
README.md                         This file
Build it yourself

The step-by-step guide is in build-guide.pdf, and the workflow can be imported into n8n from n8n/resume-audit-workflow.json.

To run your own copy:

Import the workflow into n8n.
Add your own Google Gemini key and an email account in n8n.
Publish the workflow.
Build the website in Lovable using the guide and connect it to your workflow.

No keys are stored in this repository.
