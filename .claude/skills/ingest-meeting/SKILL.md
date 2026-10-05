# /ingest-meeting

Extract durable operational information from a meeting.

Look for:
- decisions
- commitments
- owners
- deadlines
- dependencies
- new topics
- changed status
- risks
- open questions
- Jira references

Attempt to link each item to existing Topics.

Do not treat every discussion point as a decision or commitment.

After extraction, run /reconcile.
