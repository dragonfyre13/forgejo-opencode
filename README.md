Usage should look something like the below, just create it as a file in `.forgejo/workflows/opencode.yaml` within a repository. The PR_CREATION_PAT secret is optional, but highly recommended. Without it, only branches are created vs auto-creating PRs for  change requests.

```yaml
name: opencode

on:
  issue_comment:
    types: [created]
  pull_request_review_comment:
    types: [created]
  pull_request_review:
    types: [submitted]

jobs:
  opencode:
    if: |
      contains(forgejo.event.comment.body, ' /oc') ||
      startsWith(forgejo.event.comment.body, '/oc') ||
      contains(forgejo.event.comment.body, ' /opencode') ||
      startsWith(forgejo.event.comment.body, '/opencode') ||
      contains(forgejo.event.review.content, ' /oc') ||
      startsWith(forgejo.event.review.content, '/oc') ||
      contains(forgejo.event.review.content, ' /opencode') ||
      startsWith(forgejo.event.review.content, '/opencode')
    runs-on: node24
    steps:
      - name: Run opencode
        uses: https://github.com/dragonfyre13/forgejo-opencode@main
        with:
          model: opencode-go/kimi-k2.6
          pr-creation-pat: ${{ secrets.PR_CREATION_PAT }}
          agent: build
          variant: ""
          mentions: "/oc,/opencode"
        env:
          OPENCODE_API_KEY: ${{ secrets.OPENCODE_API_KEY }}
```
