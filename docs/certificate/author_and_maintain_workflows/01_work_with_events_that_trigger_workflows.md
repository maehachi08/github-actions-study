
https://docs.github.com/en/actions/writing-workflows/choosing-when-your-workflow-runs/triggering-a-workflow

When you specify the trigger for running the workflow, use `on:` values.

## Configure workows to run for one or more events

### Using a single event



### Using a multiple event

### Using activity types

- Some events have activity types that what kind of types do the event occured.
- e.g. In the `issue_comment` case, there are activity types like `created`, `edited`, `deleted`.
- Regarding to activity types each event, please refer to [here](https://docs.github.com/en/actions/writing-workflows/choosing-when-your-workflow-runs/events-that-trigger-workflows).
- https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#onevent_nametypes
ing filters
- Some events have filters that you can more controll the workfow should run or not.
- e.g. In the `push` case, there are filters like `branches`, `tag`, `branches-ignore`, `tags-ignore`.
    - https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#onpull_requestpull_request_targetbranchesbranches-ignore
    - https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#onpushbranchestagsbranches-ignoretags-ignore
    - https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#onpushpull_requestpull_request_targetpathspaths-ignore

!!!info
    - https://docs.github.com/en/actions/writing-workflows/choosing-when-your-workflow-runs/triggering-a-workflow#using-activity-types-and-filters-with-multiple-events

        > You can't use branches and branches-ignore to filter the same event in a single workflow. Similarly, you can't use tags and tags-ignore to filter the same event in a single workflow. If you want to both include and exclude branch or tag patterns for a single event, use the branches or tags filter along with the ! character to indicate which branches or tags should be excluded.

        > If you define a branch with the ! character, you must also define at least one branch without the ! character. If you only want to exclude branches, use branches-ignore instead. Similarly, if you define a tag with the ! character, you must also define at least one tag without the ! character. If you only want to exclude tags, use tags-ignore instead.


## Conifigure workows to run for scheduled events

- https://docs.github.com/en/actions/writing-workflows/choosing-when-your-workflow-runs/events-that-trigger-workflows#schedule
- You can schedule a workflow to run at specific UTC times using [POSIX cron syntax](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/crontab.html#tag_20_25_07).

!!!info
    Please refer to https://docs.github.com/en/actions/writing-workflows/choosing-when-your-workflow-runs/events-that-trigger-workflows#schedule

    - The schedule will be delayed during periods of high loads of GitHub Actions workflow runs.
        - If the load is sufficiently high enough, some queued jobs may be dropped.
        - High load times include the start of every hour.
    - The scheduled events will only run on the default branch.
    - In a public repository, scheduled workflows are automatically disabled when no repository activity has occurred in 60 days.
        - For information on re-enabling a disabled workflow, see [Disabling and enabling a workflow](https://docs.github.com/en/enterprise-server@3.12/actions/managing-workflow-runs-and-deployments/managing-workflow-runs/disabling-and-enabling-a-workflow#enabling-a-workflow).
    - When the last user to commit to the cron schedule of a workflow is removed from the organization, the scheduled workflow will be disabled. 
        - If a user with write permissions to the repository makes a commit that changes the cron schedule, the scheduled workflow will be reactivated. 


## Configure workows to run for manual events

- https://docs.github.com/en/actions/writing-workflows/choosing-when-your-workflow-runs/triggering-a-workflow#defining-inputs-for-manually-triggered-workflows
- https://docs.github.com/en/actions/managing-workflow-runs-and-deployments/managing-workflow-runs/manually-running-a-workflow
- The `workflow_dispatch` event can run manually.
    - The `workflow_dispatch` can define the `inputs` parameter for specifying when running workflow manually.
        - The maximum number of top-level properties for inputs is 10.

## Configure workflows to run for webhook events (i.e. check_run, check_suite, deployment, etc.)

- https://docs.github.com/en/webhooks/webhook-events-and-payloads
    - Payloads are capped at 25 MB. If you have many branches, or tags, the push event will be delivered many payloads.
    - Which events do you want to run the webhook, you can select events when the webhook creating.

## Demonstrate a GitHub event to trigger a workow based on a practical use case

