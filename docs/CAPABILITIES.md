# Available Capabilities

This is a capability inventory for the orchestrator. It describes what is available, not what must be used.

## AI / model capabilities

### OpenRouter
- API access and key are available.
- Multiple models can be selected.
- Use when model diversity, reasoning, classification, semantic matching, or other model capabilities materially improve the task.
- Consider model cost, latency, rate limits, and reliability.

### JEV
- Type-safe AI capability is available with one model/API key.
- Consider it for structured or deterministic AI workflows where typed inputs/outputs and predictable interfaces are valuable.
- Do not assume JEV is appropriate until the architecture evaluates the task.

## Engineering / platform capabilities

### GitHub
- Repository, version control, persistent project artifacts, and collaboration.

### Web / external APIs
- External web access and APIs may be available where appropriate.
- Prefer official and reliable APIs when possible.

### Scheduling / deployment
- Scheduling and deployment mechanisms are available and should be selected based on the actual workload and maintenance requirements.

### UI
- A user-facing UI is part of the initial product goal.
- The implementation technology is intentionally not prescribed here.

## Capability selection principles

- Prefer deterministic code when it is sufficient.
- Use AI only where it adds meaningful value.
- Prefer structured/type-safe interfaces when they reduce ambiguity.
- Prefer official/reliable APIs over scraping or browser automation when available.
- Consider cost, latency, rate limits, reliability, maintainability, and provider lock-in.
- Keep provider-specific integrations replaceable where practical.
- Do not use a capability merely because it is available.
- The orchestrator should ask specialists to evaluate material tradeoffs rather than making technology choices prematurely.
