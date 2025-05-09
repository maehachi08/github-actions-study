## Action

### setup-{language} action

The setup-{language} actions like [action/setup-java](https://github.com/actions/setup-java), [action/setup-python](https://github.com/actions/setup-python), you can cache packages dependencies.These setup-{language} actions use [toolkit/cache](https://github.com/actions/toolkit/tree/main/packages/cache) (e.g. [setup-java@v4](https://github.com/actions/setup-java/blob/v4.6.0/src/cache.ts#L149)).

Dependency files is saved to the cache storage by the setup-{language} action. Save files can be downloaded on the next workflow.

e.g.
in the action/setup-java, dependency files under the `~/.m2/repository` directory will be saved into the cache storage.<br>
https://github.com/actions/setup-java/blob/v4.6.0/src/cache.ts#L23-L64

- [action/setup-java](https://github.com/actions/setup-java)

    ```yaml title="sample"
    steps:
    - uses: actions/checkout@v4
    - uses: actions/setup-java@v4
      with:
        distribution: 'corretto'
        java-version: '21'
        cache: 'maven'
    ```

### cache action

The [action/cache](https://github.com/actions/cache) 

## Cache Storage

!!! info
    - When using github.com and github hosted runner, cache will be stored on GitHub-owned cloud storage.
    - When using github.com and self hosted runner, cache will be stored on GitHub-owned cloud storage.
    - When using GitHub Enterprise Server and self hosted runner, cache will be stored on customer-owned storage.

- Caching dependencies to speed up workflows
    - https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/caching-dependencies-to-speed-up-workflows

        > When using self-hosted runners, caches from workflow runs are stored on GitHub-owned cloud storage. A customer-owned storage solution is only available with GitHub Enterprise Server.

- Enabling GitHub Actions with Amazon S3 storage
    - https://docs.github.com/en/enterprise-server@3.15/admin/managing-github-actions-for-your-enterprise/enabling-github-actions-for-github-enterprise-server/enabling-github-actions-with-amazon-s3-storage

        > GitHub Actions uses external blob storage to store data generated by workflow runs. Stored data includes workflow logs, caches, and user-uploaded build artifacts.

