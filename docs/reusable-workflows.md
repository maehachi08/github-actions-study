## Reusable workflows

- https://docs.github.com/en/actions/sharing-automations/reusing-workflows
- https://docs.github.com/en/actions/sharing-automations/avoiding-duplication

Github actions workflow often has some cases which reusable workflow already exists.
For example, When providing CICD using github actions in organization, there are some cases that Build, Test, Scan, and Deploy will be used same actions.
In that cases, It's complicated like maintenance and take a lot of maintenance cost when the workflow definition write to each github repo.
If you use `Reusable workflows`, common workflow definitions like Build, Test, Scan, and Deploy can be centrally maintained.
So, common workflow definitions will be managed centrally, and then the app repo will be refered to that repo.
If you want to update common workflow definitions, you would update one repo only.

In github doc, `caller` is workflow of app repo and then `called` is reusable workflow.

> A workflow that uses another workflow is referred to as a "caller" workflow. The reusable workflow is a "called" workflow.

The followings are differences between reusable workflow and composite action.

- The reusable workflow can be run by each jobs and steps as is reusable workflow definitions. The composite action will be run as one step that caller definition.
- Also, there are diffrences that the logging is each jobs and steps as is reusable workflow definitions, or one step that caller definition.
- The reusable workflow can specify different machine type than caller job in each job.
- The composite action will be run as part of caller workflow, however the reusable workflow will be run as completly one job independetly. So can't add the steps before and after in the reusable workflow. And also `GITHUB_ENV` can't pass to sequent job steps.

Also, please check [here](https://docs.github.com/en/actions/sharing-automations/avoiding-duplication#key-differences-between-reusable-workflows-and-composite-actions) about key differences between reusable workflow and composite action.

### How to call the reusable workflow

- https://docs.github.com/en/actions/sharing-automations/reusing-workflows#calling-a-reusable-workflow

#### Using a matrix

- https://docs.github.com/en/actions/sharing-automations/reusing-workflows#using-a-matrix-strategy-with-a-reusable-workflows

### Trigger event

`on.workflow_call` need to define to reusable workflow yaml file.

- https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#onworkflow_call
- https://docs.github.com/en/actions/writing-workflows/choosing-when-your-workflow-runs/events-that-trigger-workflows#workflow_call

```
on:
  workflow_call:
```

### Variable, Environment variable, and Secret

- https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#onworkflow_callinputs
- https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#onworkflow_callsecrets
- https://docs.github.com/en/actions/sharing-automations/reusing-workflows#limitations
    - Variable, Environment variable, and Secret need to define in the caller reusable workflow job for using its in the reusable workflow.
        ```yaml title="caller workflow"
        jobs:
          call-reusable:
            uses: my-org/reusable-workflow/.github/workflows/main.yml@main
            with:
              some_input: value                          # You can pass the Variable to the reusable workflow by defiining the `inputs` in the reusable workflow.
            env:
              MESSAGE: hello                             # This is able to pass to `jobs.<job>.env` in the called reusable workflow.
            secrets:
              SECRET_TOKEN: ${{ secrets.MY_SECRET }}     # You can pass the Secret to the reusable workflow by defiining the `secrets` in the reusable workflow.
        ```
        ```yaml title="called workflow(reusable workflow)"
        on:
          workflow_call:
            inputs:
              some_input:
                required: true
                type: string
            secrets:
              SECRET_TOKEN:
                required: true

        jobs:
          echo-job:
            runs-on: ubuntu-latest
            steps:
              - name: Print message
                run: echo "Message is: $MESSAGE"
        ```
- When the same organization both caller repo and called repo, Secret can use the inherit keyword to implicitly pass the secrets. In this case, you don't need to specify Secrets definitions to reusable workflow. Note: `secrets: inherit` is that Secret can be passed directly called workflow. So in workflow A -> B -> C case, you need to specify `secrets: inherit` in the workflow A and workflow B, if you want to pass to workflow C.
    ```yaml title="caller workflow"
    jobs:
      call-reusable:
        uses: my-org/reusable-workflow/.github/workflows/main.yml@main
        secrets: inherit
    ```

### Reusable workflow runs as a part of caller workflow.

- If `actions/checkout` runs in the reusable workflow, github repo of caller workflow will be checked out.
- The `github` context is always associated with the caller workflow.
- The permission of `github.token` and `secrets.GITHUB_TOKEN` are always associated with the caller workflow.

