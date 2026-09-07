# Security Policy

## Supported version

Security fixes currently target the latest commit on `main`.

## Reporting a vulnerability

Do not report vulnerabilities, API keys, tokens, recordings, phone numbers, or other sensitive information in a public GitHub issue.

Use GitHub's private vulnerability reporting feature when it is enabled for the repository. If private reporting is unavailable, contact the maintainer through a private channel listed on the maintainer's GitHub profile and include only the minimum information needed to reproduce the issue.

Please describe the affected component, impact, reproduction steps, and any suggested mitigation. Remove real credentials and customer data from logs or examples.

## Operational guidance

This repository is a local-first reference implementation. The HTTP API has no authentication and must not be exposed directly to the public internet without authentication, authorization, TLS termination, rate limiting, and an appropriate data-retention policy.
