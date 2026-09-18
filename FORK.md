# OmniRoute AI Control Plane Fork

[Polski](docs/i18n/pl/FORK.md)

## Purpose

This repository is a maintained fork of [diegosouzapw/OmniRoute](https://github.com/diegosouzapw/OmniRoute). It preserves OmniRoute's role as an AI gateway while developing and validating an additional use case: a controlled **AI Control Plane** between autonomous AI agents and a changing multi-provider model ecosystem.

The goal is to give agent runtimes a stable, auditable and provider-independent interface for AI execution.

## Problem statement

AI agents should not need to integrate independently with every model provider, authentication scheme, quota system, endpoint convention or provider-specific failure mode.

The target abstraction is:

```text
AI Agents / Orchestrators
          |
          v
   OmniRoute Control Plane
          |
          +-- routing and model selection
          +-- provider health and availability
          +-- quota and limit awareness
          +-- fallback and resilience
          +-- cost and usage controls
          +-- telemetry and observability
          +-- policy and governance
          |
          v
   AI Providers / Local Models
```

The agent decides **what should be done**. The control plane determines **which eligible model/provider should execute it and under which routing policy**.

## Focus of this fork

This fork is intended to explore, validate and maintain capabilities useful for controlled agent infrastructure, including:

- deterministic and policy-aware routing;
- resilient multi-provider execution and fallback;
- provider authentication and connection management;
- quota, rate-limit and availability awareness;
- model capability and context-window constraints;
- observability, auditability and operational evidence;
- cost-aware routing and token-efficiency mechanisms;
- integration with AI agents and orchestration environments;
- isolated runtime patterns for environments requiring stronger operational boundaries.

Hermes Agent is one integration use case, not a requirement for using this fork. Private Hermes governance, credentials and environment-specific operational artifacts are intentionally kept outside this public repository.

## Relationship with upstream

This repository is not intended to obscure or replace the original OmniRoute project.

Upstream remains:

- <https://github.com/diegosouzapw/OmniRoute>

The fork follows a **track and selectively integrate** model. Upstream changes may be evaluated and incorporated when they fit the fork's validated baseline. Changes developed here that are broadly useful to OmniRoute should, where practical, remain suitable for contribution upstream.

## Maintenance and release model

This fork is maintained as a controlled derivative of OmniRoute.

Upstream changes are reviewed and selectively integrated when they fit the
fork's validated baseline. Independent releases may be published when local
divergence or fork-specific capabilities require them. Upstream version
alignment is preferred where practical.

This does not establish a mandatory independent release schedule or a formal
independent lifecycle by default.

## Branch and change model

The fork uses `main` as its stable integration baseline.

```text
upstream OmniRoute
       |
       | evaluate / selectively integrate
       v
      main
       |
       +-- feature/*
       +-- fix/*
       +-- docs/*
       +-- integration/*
              |
              v
             PR
              |
        verification / review
              |
              v
             main
```

Direct mutation of the stable baseline should be avoided for normal development. Runtime, provider, governance and documentation changes should be reviewable and attributable through focused pull requests.

## Languages and translations

English is the canonical language for fork-specific documentation when resolving semantic differences between translations. Polish is maintained as the first reference translation.

Additional translations are welcome.

If you would like to help make the fork documentation available in another language, contributions are encouraged. Technical identifiers, API names, environment variables, model identifiers and code examples should remain unchanged unless localization is technically required.

## Contributions

Contributions are welcome, especially in:

- AI routing and model selection;
- provider integrations;
- agent infrastructure;
- resilience and fallback;
- observability and telemetry;
- security and governance;
- quota and cost controls;
- documentation and localization.

Please keep contributions focused and clearly separate generally useful OmniRoute improvements from environment-specific or private operational material.

## Attribution

OmniRoute is developed by its upstream maintainers and community and is distributed under the MIT License. This fork retains upstream attribution and licensing.