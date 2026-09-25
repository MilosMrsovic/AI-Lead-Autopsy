# AI Lead Autopsy

**Every lost deal gets a cause of death.**

---

## Case #1: Ana, $5,000

Monday, 09:14. Ana fills out a Facebook lead form and asks for a quote that week.

Nobody is assigned to her.

Wednesday, 14:05. Someone finally calls. Ana politely explains that she signed with another agency on Tuesday. They called her back the same day.

In the CRM, this deal says one word: **Lost.**

No reason. No lesson. Next week it happens again.

This workflow exists so that it doesn't.

---

## What it does

Every morning at 8, it opens the file on every deal that was marked Lost in the last 24 hours. It reads what the CRM knows about each one: the timestamps, the source, the owner (or the lack of one), the tags, the notes, the tasks nobody finished.

Then it asks one question per deal: **what actually went wrong?**

Not "lead lost interest". A real answer, with the evidence next to it, and an honest label for how sure it is.

By the time the sales lead opens their inbox, the report is waiting.

![Lead Autopsy Overview](lead_autopsy_overview.png)

---

## The morning report

One email. The numbers first, the full report attached.

![Report Email](screenshots/email_report.png)

Inside the PDF, every deal gets its own page section: what happened, the primary failure, the evidence behind it, what else went wrong, and one fix that would have saved it.

![Daily Lead Autopsy Report](screenshots/sample_report.png)

The real PDF from the test run: [`sample_report.pdf`](sample_report.pdf)

---

## A doctor who is always sure is a bad doctor

This is the part that matters most.

A generic AI summary will give every lost deal a confident explanation. That is worse than no explanation, because people act on it.

So every cause of death in this report carries one of three labels, and the AI is not allowed to pick the strongest one without proof:

| Label | When it is used |
| --- | --- |
| **Confirmed by data** | A note or field shows what killed the deal |
| **Likely inference** | Something clearly went wrong, but nothing proves it was the cause |
| **Insufficient data** | The CRM does not say enough, and the report admits it |

A missing owner or a missing loss reason is never treated as proof on its own. It shows up as a secondary failure, or as something the team should start logging.

Here is how the three test cases came out:

| Deal | What the report concluded | Label |
| --- | --- | --- |
| Ana, $5,000 | A competitor called her back the same day. We called after 53 hours. | Confirmed by data |
| Stefan, $12,000 | Strong fit, proposal sent, one follow-up, then silence. | Likely inference |
| Marko, $8,900 | Never assigned to anyone, no notes, re-engagement tasks created after he was already lost. | Likely inference |

One confirmed cause. Two honest "probably". Zero invented stories.

---

## Under the hood

![n8n Workflow](screenshots/n8n_workflow.png)

**Collect.** A scheduled n8n workflow pulls lost opportunities from the GoHighLevel API together with their notes and tasks, drops everything older than 24 hours, and turns each deal into a clean evidence file ([`sample_lost_deal.json`](sample_lost_deal.json)).

**Examine.** An AI agent looks at one deal at a time. A structured output parser forces every answer into the same shape: failure, confidence, evidence, data completeness, fix and severity ([`sample_autopsy.json`](sample_autopsy.json)).

**Report.** All findings go into a single HTML report, PDFShift turns it into a PDF, and Gmail delivers it.

![Technical Architecture](lead_autopsy_technical.png)

---

## The patients

The three test deals, straight from the Lost column in GoHighLevel. The report above was written about them.

![GoHighLevel Lost Deals](screenshots/ghl_lost_deals.png)

---

## What it can't do

It can only read what the team writes down. A call that never made it into the CRM is invisible here, and a team that logs nothing will get a lot of "Insufficient data" (which is also useful to know).

Recoverable revenue is an estimate, not a promise. And anything marked as inference should be checked by a human before someone gets a hard conversation about it.

---

Built with n8n · GoHighLevel API · GPT-5 mini · PDFShift · Gmail

Works with GoHighLevel out of the box. The same idea works with HubSpot, Salesforce or any CRM that has an API.

**Stop treating "Lost" as a closed case. Start treating it as evidence.**
