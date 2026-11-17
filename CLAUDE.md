# PROJECT: Resume Screening Agent Loop with Bias & Fairness Checks

## GOAL
An AI loop that scores resumes against a job description, checks its own work for unsupported claims, bias and inconsistency, revises flagged results, and hands a ranked shortlist with rationales to a human recruiter. It never rejects anyone automatically.

## LOOP
1. **Ingest:** load one job description and a folder of resumes (plain text or PDF).
2. **Score:** rate each resume 0-100 against the job's must-have and nice-to-have requirements. For every score, give a rationale that quotes evidence from the resume.
3. **Check:**
   a. **Evidence check:** every quoted piece of evidence must appear verbatim in the resume. Check this in code, not with the AI.
   b. **Bias check (counterfactual):** create versions of the same resume that differ only in name, gender signals and age signals. Score each version. Flag the resume if scores differ by more than 5 points.
   c. **Consistency check:** score each resume 3 times. Flag it if the spread is more than 5 points.
4. **Revise:** re-score flagged resumes using an anonymized version (name, gender, age, photo and address removed). Maximum 3 rounds.
5. **Hand off:** output a ranked shortlist with scores, rationales, any remaining flags and a "needs human review" marker.

## RULES
- Use synthetic resumes only. Never use real people's data.
- Python 3.11+, Anthropic API (key from the `ANTHROPIC_API_KEY` environment variable), Streamlit for the UI.
- Log every step and every score to a JSON audit file.
- Keep the code simple and well commented. The audience is HR hiring managers, not engineers.
- Write tests for the deterministic checks.
- Before writing code for each phase, show the plan and wait for approval.
