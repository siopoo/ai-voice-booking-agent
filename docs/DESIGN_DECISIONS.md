# Design Decisions

This document records the architectural choices that shape AI Voice Booking Agent. They are constraints, not accidental implementation details.

## Why LangGraph?

Booking is a stateful workflow with explicit stages: collect required information, validate availability, identify a specific existing appointment, request confirmation, perform one mutation, and report the result. Prompt-only tool calling makes those transitions implicit and lets model behavior vary across providers or model versions.

LangGraph makes the control flow inspectable and testable. Conditional edges decide which states can reach mutation nodes, while node-level guards repeat the authorization check. The model remains useful for language understanding without becoming the workflow engine.

## Why can't the LLM create bookings directly?

An LLM is a probabilistic component, not a trusted authorization boundary. Prompt instructions can be misunderstood, tool arguments can be malformed, and generated tool results can be forged in a compromised or poorly behaved model response.

The LLM-facing tool list therefore contains only fact lookup, availability lookup, appointment lookup, and draft updates. Create, reschedule, and cancel functions are invoked by deterministic graph nodes only after state guards verify the latest real user confirmation. The service layer validates the request again, and SQLite remains the final authority for slot conflicts.

## Why SQLite?

SQLite supports the current local-first demo and single-instance deployment with no external service. It still provides transactions, indexes, and a partial unique constraint that prevents two confirmed appointments from occupying the same slot.

It is not presented as a distributed production database. Multi-instance deployments should provide a PostgreSQL repository and appropriate transaction or locking semantics while preserving the service interface.

## Why idempotency?

Voice and web clients retry. Users also repeat confirmation when a response is delayed. Without idempotency, either case can create duplicate appointments even when every individual request is valid.

The booking API accepts an `Idempotency-Key`. The repository stores the first immutable response for that key and replays it on retry. Graph state also retains `booking_result`, so repeated confirmation in the same conversation does not invoke the service twice. The database constraint adds a final independent layer of protection.

## Why separate Service and Repository?

The service layer owns domain rules: booking windows, opening hours, service duration, confirmation requirements, and conflict translation. The repository owns SQL, transactions, and data mapping.

This separation keeps HTTP handlers and Agent tools thin, allows deterministic tests without a network model, and creates a clear path to replace SQLite without rewriting the booking workflow.

## Reliable mode vs Agent mode

Reliable mode does not call an LLM. It provides deterministic request interpretation and uses the same services, repository, configuration, and database as Agent mode. This makes the booking lifecycle usable when credentials are absent or a provider is unavailable, and provides a reproducible baseline for CI and Agent evaluation.

Agent mode adds OpenAI-compatible language understanding and natural responses. It does not receive additional business authority: both modes converge on the same LangGraph and service boundaries.

## Why keep compatibility modules and environment variables?

`server.py` remains the composition root and executable entry point. `booking_agent.py`, `booking_workflow.py`, and `business_config.py` preserve public imports used by the application and tests. Moving them in a documentation-focused release would create avoidable breakage.

For the same reason, existing `PAWPILOT_*` environment variables remain supported. They are legacy identifiers rather than the project brand. Generic aliases can be introduced with a documented deprecation window in a future major release.
