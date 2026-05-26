# LinkedIn Outreach Agent

An AI agent that generates personalized LinkedIn connection requests and multi-step follow-up sequences using Claude. All outreach is logged locally to `outreach_log.json` for tracking.

## What it does

1. **Connection Request** — generates a personalized note under LinkedIn's 300-character limit based on the target's name, title, company, and your reason to connect.
2. **Follow-up Sequence** — generates a 3-step message sequence to send after the connection is accepted: warm opener → value-add → soft CTA.
3. **Outreach Log** — saves every generated message and its status (`pending / accepted / replied / closed`) to a local JSON file.

## Quick Start

```bash
# 1. Install dependencies
pip install -r requirements.txt

# 2. Set your API key
cp .env.example .env
# Edit .env and add your ANTHROPIC_API_KEY

# 3. Run the demo
python run_demo.py
```

### Expected output

```
============================================================
  LinkedIn Outreach Agent — Demo Run
============================================================

[Agent] Building outreach for Priya Sharma @ Accenture...

--- Connection Request (189 chars) ---
Hi Priya, your work on LLM-based enterprise agents at Accenture directly aligns with what I'm building. I'd love to exchange ideas — your recent posts on agent frameworks were really insightful!

--- Follow-up Sequence ---

[STEP1]
Great to connect, Priya! ...

[STEP2]
...

[STEP3]
...
```

## Run Tests

```bash
pytest tests/ -v
```

No API key needed — tests mock the Claude API.

## Usage in code

```python
from linkedin_outreach_agent import run_outreach, update_status

profile = {
    "name": "Jane Doe",
    "title": "AI Research Lead",
    "company": "DeepMind",
    "reason": "Shared interest in multi-agent coordination",
    "goal": "discuss open-source agent tooling",
}

entry = run_outreach(profile)
# Later, after they accept:
update_status("Jane Doe", "DeepMind", "accepted")
```

## Runtime

- Runs in under 30 seconds per profile on any machine with internet access.
- No GPU required. Pure API calls to Claude.

## Ethical Considerations

- This agent generates message **drafts** — always review before sending.
- Do not use for mass automated messaging; LinkedIn's Terms of Service prohibit automation of messaging at scale.
- Personalize further before sending — the agent provides a strong starting point, not a finished message.
- Do not input sensitive personal data beyond publicly available profile information.

## Safety Notes

- API keys are loaded from `.env` and never logged or printed.
- No data is sent to any service other than the Anthropic API.
- `outreach_log.json` is local-only; add it to `.gitignore` to avoid committing personal outreach data.
