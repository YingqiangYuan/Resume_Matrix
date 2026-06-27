# AI Solutions Engineer, New Grad

**Requisition ID**: CHI-2026-AISE-014
**Posted**: 2026-02-15
**Team**: Customer-Embedded Engineering, within Customer Success
**Reporting line**: Senior AI Solutions Engineer
**Employment type**: Full-time, permanent

---

## About Cascadia Health Insights

Cascadia Health Insights is the Pacific Northwest's largest independent healthcare data and AI platform. For over two decades we have helped regional hospital systems, integrated delivery networks, and provider organizations across Washington, Oregon, Idaho, and parts of British Columbia turn their clinical operations data into decisions that materially improve patient care. We currently serve approximately 80 client institutions ranging from 8-bed rural critical-access hospitals to 1,800-bed tertiary academic medical centers.

In 2025 we launched Cascadia Insight Assistant, our agentic analytics platform, which puts a natural-language BI interface in front of each client's clinical operations data warehouse. Insight Assistant is now in deployment at 12 client sites with a target of 30 by end of FY26. Our Customer-Embedded Engineering team is the group that takes Insight Assistant from "general product" to "this hospital's working tool" at each client site.

---

## Purpose

As an AI Solutions Engineer on the Customer-Embedded Engineering team, you will work directly with hospital clients to design, build, deploy, and operate custom natural-language BI agents on top of Cascadia Insight Assistant. Reporting to a Senior AI Solutions Engineer, you will be the technical voice in the room when a client's clinical operations leader asks how to make their data work for them. This role is for someone who is comfortable being client-facing, writes clean Python, can hold their own against a senior clinical analyst on metric definitions, and is excited about applied generative AI in a regulated domain.

This is a New Grad opening. We do not expect you to arrive with deep production experience on every part of the stack. We do expect you to be fluent in Python and SQL, comfortable with one LLM application framework, comfortable in AWS, and able to learn fast on the parts you have not seen yet.

---

## Accountability

- Lead the technical build-out of a Cascadia Insight Assistant deployment for one to three concurrent client engagements. You own the semantic layer, the Knowledge Retrieval corpus, the evaluation harness, and the audit trail for each engagement.
- Translate client clinical operations questions (shift handover, room availability, length-of-stay forecasting, high-risk patient alerting, order scheduling, and similar) into agent designs covering semantic layer, retrieval, response shaping, evaluation, and audit trail.
- Partner with each client's senior clinical analyst to codify their internal metric definitions into the semantic layer YAML. Resolve definitional conflicts before they become production confusion.
- Run UAT (User Acceptance Testing) cycles with Charge Nurses, Floor Managers, and Clinical Operations Directors. Track thumbs-up rate, surface wording and information-density adjustments, and iterate until acceptance crosses contractually-agreed thresholds.
- Deploy and operate the agent infrastructure on AWS (Bedrock, AgentCore Runtime, Lambda, ECS, CloudWatch) using Cascadia's standard CDK templates. Customize the templates only when client constraints require it.
- Author audit-trail and compliance documentation aligned to HIPAA, TJC hand-off communication standards, and client-specific governance requirements. Coordinate with client compliance officers on dry-runs.
- Provide post-deployment support including evaluation framework runs, drift monitoring on the Knowledge Base corpus, and incremental updates as the client's metric definitions evolve.
- Document technical decisions and produce post-deployment write-ups that feed back into product engineering so the platform itself improves over time.
- Travel to client sites within the Pacific Northwest approximately two to four days per month during active engagements.

---

## Education, Experience, and Other Information

- Bachelor's degree in Computer Science, Data Science, Health Informatics, or a related field. A Master's degree is an asset, particularly with coursework in machine learning, natural language processing, or distributed systems.
- 0 to 2 years of post-graduation experience. New grad applications are welcome and explicitly encouraged.
- Required: strong Python proficiency including at least one production-grade or production-adjacent codebase. We will ask to discuss a code sample during the technical loop.
- Required: SQL proficiency including window functions and basic warehouse query optimization. Snowflake or BigQuery exposure is preferred; comparable experience with PostgreSQL plus a small Snowflake project is acceptable.
- Strongly preferred: hands-on experience with at least one LLM application framework. Strand Agents, LangChain, LlamaIndex, Haystack, or comparable. We use Strand Agents in production; experience with any well-known equivalent will transfer.
- Strongly preferred: hands-on experience deploying to AWS, especially Bedrock, Lambda, and ECS. Familiarity with AWS CDK (Python) is a meaningful plus because every Cascadia deployment ships through CDK.
- Strongly preferred: experience with at least one retrieval-augmented-generation (RAG) implementation including evaluation. We will probe how you measured RAG quality.
- Preferred: prior exposure to healthcare data formats (FHIR, HL7) or comparable regulated data environments (financial services, public sector, life sciences). If you have not worked in healthcare before, please be specific about what regulated-data experience you do have.
- Required: strong written communication skills. AI Solutions Engineers at Cascadia produce roughly ten to fifteen pages of client-facing documentation per quarter; this is not negotiable.
- Required: comfort with ambiguity. Each client engagement starts with an under-defined business problem. You will be asked in interviews to walk through a time you converted an ambiguous request into a shipped artifact.
- Required: willingness to travel to client sites two to four days per month within the Pacific Northwest (Portland, Spokane, Boise, Bend, Vancouver BC, plus occasional Eastern Washington).
- Working knowledge of compliance frameworks (HIPAA, TJC hand-off communication, SOC 2 Type II) is a plus. We will train you on what we use day to day.

---

## Working Conditions

- Hybrid arrangement: three days per week at Cascadia Seattle HQ (1700 7th Avenue, Seattle, WA 98101), two days remote.
- Approximately two to four days per month of travel to client sites within the Pacific Northwest. Travel is reimbursed per Cascadia's standard policy.
- Standard business hours, Pacific Time. Occasional after-hours coverage during client go-live windows; these windows are pre-scheduled and rotated across the team.
- Cascadia provides equipment (laptop, peripherals, secure-access tokens) and a home-office stipend.

---

## Compensation and Benefits

- Base salary range for this role: USD 95,000 to USD 120,000 depending on prior experience and the technical loop signal.
- Annual performance bonus target: 8 percent of base, paid in Q1 of the following fiscal year.
- Restricted stock unit grant (Cascadia is privately held; grants are valued against the most recent 409A appraisal).
- Health, dental, vision, life, and disability insurance, with employer contribution.
- 401(k) with 4 percent employer match, immediately vested.
- 25 days of paid time off per year plus 11 paid holidays.
- USD 2,500 per year professional development budget (conferences, courses, certifications).

---

## Location

USA: Washington: Seattle.

This role is hybrid Seattle. Cascadia Health Insights does not offer fully remote employment for this position because regular co-location with the Customer-Embedded Engineering team and frequent client travel are core to the role.

---

## Application Process

- Submit your application through cascadiahealthinsights.com/careers. Please include a resume and a one-page cover note explaining why this role is a fit. Portfolio links to relevant code samples are welcome.
- The interview process is: recruiter screen (30 min) → take-home Python and SQL exercise (3 hours, completed within one week) → technical phone screen with a Senior AI Solutions Engineer (60 min) → on-site (Seattle, half day, four interviews including a system design and a client-facing case study). Total elapsed time from application to decision is typically three to four weeks.

Cascadia Health Insights is committed to maintaining an inclusive and accessible workplace. All qualified applicants will receive consideration for employment without regard to race, color, religion, gender, gender identity or expression, sexual orientation, national origin, genetics, disability, age, or veteran status. We thank all applicants for their interest in a career at Cascadia Health Insights. Only those selected for an interview will be contacted.
