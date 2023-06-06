# actions-bug-repros

### `pull_request.labeled` events sometimes missing from PR check summary view

I'm attempting to create a workflow that runs once in each of these cases:

- opening a PR containing a label
- adding a label to an existing PR
- pushing to a PR that contains a label

In many cases, when _opening a PR containing the label in question_, the action created from the `pull_request.labeled` event in the workflow runs _but does not show its status in the mergability details view that lists all checks_.

Here are a few examples:

#### With concurrency

Workflow: https://github.com/urcomputeringpal/actions-bug-repros/blob/main/.github/workflows/pr-labels.yml

To accomplish the required concurrency limiting and not run the action _twice_ when opening a PR that contains the appropriate label at creation time, I'm using the following workflow concurrency settings:

```yaml
concurrency:
    group: ${{ github.workflow }}-${{ github.event_name }}-${{ github.ref }}
```

Affected PR: https://github.com/urcomputeringpal/actions-bug-repros/pull/2 

In the above PR, you can see that a comment was made but no Action status is present in the mergability detail view. **Please note that this PR was run before the README.md conflicts existed.**

#### Without concurrency

To rule out any impact of the `concurrency` key on this behavior, I duplicated the workflow without the concurrency limiting settings:

Workflow: https://github.com/urcomputeringpal/actions-bug-repros/blob/main/.github/workflows/pr-labels-no-concurrency.yml

Affected PR: https://github.com/urcomputeringpal/actions-bug-repros/pull/2

In the above PR, you can see that two comments were made but only one Actions status is present in the mergability detail view below. Why are the checks created by the `pull_request.labeled` events not being associated with the PR? **Please note that this PR was run before the README.md conflicts existed.**

