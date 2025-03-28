# Bitbucket

Buildkite integrates with [Bitbucket](https://bitbucket.org/) to provide automated builds based on your source control. You can run a build every time you push code to Bitbucket, and pull requests can have their build status live-updated as builds progress.

This guide shows you how to set up your Bitbucket builds with Buildkite.

## Set up the Bitbucket webhook

Once you've created a pipeline in Buildkite and copied in your Bitbucket repository URL, Buildkite shows you setup instructions for configuring your Bitbucket webhooks.

You can also find these instructions by following the **Bitbucket Setup Instructions** link on your Buildkite pipeline's **Settings** page:

<%= image "setup-instructions.png", width: 592/2, height: 356/2, alt: "Screenshot of Bitbucket setup instructions link" %>

The setup instructions give you:

- A direct link to your Bitbucket repository's **Webhooks** settings
- Instructions
- A custom webhook URL for the pipeline

<%= image "setup.png", width: 1738/2, height: 764/2, alt: "Screenshot of Bitbucket setup instructions on Buildkite" %>

Once you've followed the link you can add a new webhook:

<%= image "bitbucket-webhook-add.png", width: 1396/2, height: 528/2, alt: "Screenshot of a Bitbucket webhook settings" %>

After filling out the webhook details using the instructions from your Buildkite pipeline settings, select **Save**, and you're ready to trigger a build.

## Enable commit status updates

If you want your Bitbucket pull request's build status icons to update as builds progress, you need to connect your Bitbucket account with Buildkite. You only need to do this once, and if you don't need build status updates you can skip this step altogether.

To connect your Bitbucket account:

1. Open Buildkite's **Personal Settings**.
2. Choose **Connected Apps**.
3. Select **Connect** next to **Bitbucket**.

<%= image "personal-settings.png", width: 2322/2, height: 590/2, alt: "Screenshot of the Buildkite Connected Apps screen" %>

Buildkite prompts you to give permission for Buildkite to post status updates, then redirects back to your **Connected Apps** page.

## Branch configuration and settings

<%= render_markdown partial: 'pipelines/source_control/branch_config_settings' %>

## Using one repository in multiple pipelines and organizations

<%= render_markdown partial: 'pipelines/source_control/one_repo_multi_org' %>

## Build skipping

<%= render_markdown partial: 'pipelines/source_control/build_skipping' %>
