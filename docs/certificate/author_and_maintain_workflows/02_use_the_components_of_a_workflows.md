## Identify the correct syntax for workflow jobs (i.e. indentation and encapsulation of parts of the workflow)

- This section is the basical grammar, how to configure.
- https://docs.github.com/en/actions/writing-workflows

## Use job steps for actions and shell commands

- The github actions workflow is consisted one or more jobs. Also, one job is consisted one or more steps.
- One or more jobs, each of which will execute on a runner machine and run a series of one or more steps.
    - https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#jobsjob_idsteps
- In the step section, you need to specify either the `uses` for specifying the action or the `run` for specifying the command-line programs.
    - https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#jobsjob_idstepsuses
    - https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#jobsjob_idstepsrun

## Use conditional keywords for steps

- https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#jobsjob_idif
- https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#jobsjob_idstepsif
- https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/accessing-contextual-information-about-workflow-runs
- https://docs.github.com/en/actions/writing-workflows/choosing-when-your-workflow-runs/using-conditions-to-control-job-execution

## Describe how actions, workflows, jobs, steps, runs, and the marketplace work together

- Workflow pipelines are defined in the `.github/workflows/` dir with yaml file.
- Workflow pipeline has one or more jobs.
- Job has one or more steps.
- Step has `runs` key for using other actions.
- There are public actions, commnity actions, and selh hosted actions(local repo or public reo) in actions.

### Marketplace

- https://github.com/marketplace
- You can find actions that you want to try. And then you can create custom action and publish to the marketplace if there aren't it.
- There are `Verified creators` or not in creators of action.
    - https://github.com/marketplace?verification=verified_creator&type=actions
    - e.g.
        - https://github.com/actions
        - https://github.com/github
        - https://github.com/aws-actions

#### Caution when using github enterprise server

- https://docs.github.com/en/enterprise-cloud@latest/admin/enforcing-policies/enforcing-policies-for-your-enterprise/enforcing-policies-for-github-actions-in-your-enterprise

## Identify scenarios suited for using GitHub-hosted and self-hosted runners

- https://docs.github.com/en/actions/using-github-hosted-runners/using-github-hosted-runners/about-github-hosted-runners
- https://docs.github.com/en/actions/writing-workflows/choosing-where-your-workflow-runs/choosing-the-runner-for-a-job#choosing-self-hosted-runners

## Implement workflow commands as a run step to communicate with the runner

- https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/workflow-commands-for-github-actions
    - https://github.com/actions/toolkit/blob/main/packages/core/README.md
    - - https://github.com/actions/toolkit/blob/master/docs/commands.md
    - `workflow commands` are special function on the github actions to communicate to runner.
    - `workflow commands` can use in two usecases.
        - Using workflow commands to access toolkit functions
        - Using workflow commands as annotations in workflow `run` step.

### Problem Matchers

- https://github.com/actions/toolkit/blob/master/docs/problem-matchers.md
- https://github.com/actions/toolkit/blob/master/docs/commands.md#problem-matchers
- You can regist problem matchers for error output, when printing on annotations.
- sample
    - multiline matcher
        - refer to https://github.com/actions/toolkit/blob/master/docs/problem-matchers.md#multiline-matching
    - single line matcher

        ```
        badFile.js: line 50, col 11, Error - 'myVar' is defined but never used. (no-unused-vars)
        ```

    - sample matcher

        ```
        {
            "problemMatcher": [
                {
                    "owner": "eslint-compact",
                    "pattern": [
                        {
                            "regexp": "^(.+):\\sline\\s(\\d+),\\scol\\s(\\d+),\\s(Error|Warning|Info)\\s-\\s(.+)\\s\\((.+)\\)$",
                            "file": 1,
                            "line": 2,
                            "column": 3,
                            "severity": 4,
                            "message": 5,
                            "code": 6
                        }
                    ]
                }
            ]
        }
        ```

- Some setup-{language} actions are already supported with problem matcher.
    - e.g.
        - 


## Demonstrate the use of dependent jobs

