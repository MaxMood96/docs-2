---
keywords: oidc, authentication, IAM, roles
---

# OIDC in Buildkite Pipelines

<%= render_markdown partial: 'platform/buildkite_agent_oidc_token_overview' %>

Third-party products and services, such as [AWS](https://aws.amazon.com/), [GCP](https://cloud.google.com/), [Azure](https://azure.microsoft.com/) and many others, as well as Buildkite products, such as [Packages](/docs/packages/security/oidc), can be configured with OIDC-compatible policies that only permit agent interactions from specific Buildkite organizations, pipelines, jobs, and agents, associated with a pipeline's job.

A Buildkite OIDC token, representing such an agent interaction containing this metadata, can be used by these third-party services and Buildkite Packages, to allow the service to authenticate the Buildkite interaction. If one of these interactions does not match or comply with the service's policy, the OIDC token and hence, subsequent interactions are rejected.

The [Buildkite Agent's `oidc` command](/docs/agent/v3/cli-oidc) allows you to request an OIDC token from Buildkite for the pipeline's current job. These tokens are then exchanged on federated systems like AWS for authenticated role-based access with specific permissions to interact with your cloud environments.

This section of the Buildkite Docs covers Buildkite's OIDC implementation with other federated systems, such as [AWS](/docs/pipelines/security/oidc/aws).
