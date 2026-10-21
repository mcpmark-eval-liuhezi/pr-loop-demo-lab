# Notes: What This Demo Proves

This demo exercises the complete GitHub pull-request automation loop, end to end:

1. A feature branch (`feature/smoke-checks`) is created from the default branch (`main`).
2. A GitHub Actions workflow (`.github/workflows/smoke.yml`) is added on that branch. It triggers on both `push` and `pull_request`, so checks run immediately on the branch push and again when the pull request is opened.
3. A pull request is opened from the feature branch into `main`, and its automated checks are watched until they finish green.
4. The job logs are inspected to confirm the exact marker line was printed:

   ```
   SMOKE_LOOP_OK
   ```

5. The pull request is then squash-merged into `main`, collapsing the branch commits into one verified commit.

## What it proves

- Branch-based development works with no server or local git client required: everything was done through the GitHub REST API / automation tooling.
- The `on: push` and `on: pull_request` triggers both fire, so a change is validated twice: once on the feature branch and once against the merge target.
- Check runs (status checks) gate the pull request: the PR shows the workflow result, and we can retrieve the job logs programmatically and see the expected output.
- A squash merge produces exactly one commit on `main`, keeping the default branch history clean.

## What attendees should look for in the logs

Open the workflow run attached to the pull request, then the `smoke` job, then the `Run smoke check` step. The step output contains exactly:

```
SMOKE_LOOP_OK
```

Seeing that line in the logs is the proof that the full loop — branch, push trigger, pull-request trigger, check run, log retrieval — completed successfully.
