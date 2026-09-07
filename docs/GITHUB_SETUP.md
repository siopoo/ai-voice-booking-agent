# GitHub Repository Setup

These settings require repository-owner access and should be configured manually after reviewing the changes.

## Suggested identity

- **Repository name:** `ai-voice-booking-agent`
- **Description:** Production-oriented AI voice booking agent built with LangGraph, deterministic workflows and configurable LLM/STT providers.

Renaming is optional. If performed, verify redirects, badges, clone instructions, container references, and release links afterward. This repository's Git remote has intentionally not been changed.

## Suggested topics

```text
ai-agent
voice-ai
langgraph
llm
speech-to-text
appointment-booking
python
sqlite
docker
ai-agents
workflow
open-source
```

## Recommended repository settings

- Enable Issues and Discussions if the maintainer plans to support community use.
- Enable private vulnerability reporting under **Security → Advisories**.
- Protect `main` and require the `quality` and `container` CI jobs before merge.
- Require pull-request review and dismiss stale approvals after new commits.
- Enable secret scanning and push protection where available.
- Create a `v0.1.0` release/tag before relying on the changelog comparison links.
- Add a real demo GIF and architecture image only after capturing them from a verified build.

## Social preview

Once real media exists, use `docs/assets/architecture.png` or a purpose-built social card as the repository social preview. Do not use generated screenshots that imply unimplemented features.
