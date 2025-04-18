# Archiving and deleting pipelines

You can archive and delete pipelines from the dashboard.

## Archiving pipelines

You can archive/unarchive a pipeline if you're an administrator of the Buildkite organization or in a team that has Full Access to the pipeline.

Archiving a pipeline preserves all builds, job logs, artifacts, and history for the pipeline. Archived Pipelines are hidden on the Pipelines page and won't run new builds.

To archive or unarchive a pipeline:

1. Navigate to the pipeline.
1. Select the pipeline's **Settings** > **General** page.
1. In the **Pipeline Management** section, select **Archive Pipeline**/**Unarchive Pipeline**.
1. Read the warnings.
1. Type in the slug of the pipeline.
1. Select **Archive Pipeline**/**Unarchive Pipeline**.

You can view archived pipelines using the team selector on the Pipelines dashboard.

## Deleting pipelines

You can delete a pipeline if you're an administrator of the Buildkite organization or in a team that has Full Access to the pipeline.

Deleting a pipeline deletes all associated builds, job logs, artifacts, and history for this pipeline.

To delete a pipeline:

1. Navigate to the pipeline.
1. Select the pipeline's **Settings** > **General** page.
1. In the **Pipeline Management** section, select **Delete Pipeline**.
1. Read the warnings.
1. Type in the slug of the pipeline.
1. Select **Delete Pipeline**.

> 🚧 Builds from deleted pipelines are not exported
> When a pipeline is deleted, all of its associated builds are also deleted and will _not_ be exported as part of the [build export](/docs/pipelines/governance/build-exports) process.
> If you need to [retain builds](/docs/pipelines/configure/build-retention) to preserve their data and be able to export them, [archive the pipeline](/docs/pipelines/configure/workflows/archiving-and-deleting-pipelines#archiving-pipelines) instead.
