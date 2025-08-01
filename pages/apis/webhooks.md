---
description: Receive real-time Buildkite events in your systems
---

# Webhooks overview

Buildkite webhooks send JSON payloads through HTTP requests to specific URL endpoints of third-party applications, which let these applications react to activities on the Buildkite platform as they happen.

Common use cases for implementing Buildkite webhooks include:

- Generating chat alerts in Slack that aren't covered by the [Slack Workspace](/docs/platform/integrations/slack-workspace) and [Slack](/docs/pipelines/integrations/notifications/slack) notification service integrations, as well as in other chat applications like Microsoft Teams.
- Automating infrastructure, such as scaling agents.
- Allowing your third party applications to:
    * Ingest analytics or data on specific activities from the Buildkite platform.
    * Display custom dashboards on data from the Buildkite platform.

Buildkite currently provides webhook support for both [Pipelines](/docs/apis/webhooks/pipelines) and [Test Engine](/docs/apis/webhooks/test-engine).

## Creating webhooks

Learn more about how to add Buildkite webhooks from the **Add a webhook** procedures for both [Pipelines](/docs/apis/webhooks/pipelines#add-a-webhook) and [Test Engine](/docs/apis/webhooks/test-engine#add-a-webhook).

Pipelines webhooks also provide [request headers](/docs/apis/webhooks/pipelines#http-headers) to allow the authenticity of these webhook events to be verified.

## Event families

### Pipelines

Buildkite Pipelines supports the following categories of webhook events.

| Event family | Description |
|--------------|-------------|
| [Build events](/docs/apis/webhooks/pipelines/build-events) | A pipeline build starts, fails, finishes, is scheduled, or is skipped. |
| [Job events](/docs/apis/webhooks/pipelines/job-events) | A pipeline's job runs, finishes, is in a scheduled state, or is activated. |
| [Agent events](/docs/apis/webhooks/pipelines/agent-events) | A Buildkite Agent connects, disconnects, stops, is lost, or gets blocked. |
| [Ping events](/docs/apis/webhooks/pipelines/ping-events) | A webhook's notification configuration has changed. |
| [Agent-token events](/docs/apis/webhooks/pipelines/agent-token-events) | An agent token's registration has failed. |
| [Integrations](/docs/apis/webhooks/pipelines/integrations) | Buildkite Pipeline events related to third-party application integrations. |

### Test Engine

Buildkite Test Engine supports webhook events relating to [changes in test states and labels](/docs/apis/webhooks/test-engine).

## Security best practices

When configuring your third party applications to receive Buildkite webhook events, ensure the following security measures are implemented:

- Serve your endpoint over TLS.
- Restrict accepted IP ranges for Buildkite webhooks to Buildkite's outgoing addresses.
- Ensure your endpoints only accept JSON payloads from Buildkite webhooks.

## See also

- [REST API overview](/docs/apis/rest-api)
- [GraphQL API overview](/docs/apis/graphql-api)
- [Amazon EventBridge integration](/docs/pipelines/integrations/observability/amazon-eventbridge)
