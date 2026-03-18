# TODO

## ClickUp Integration Cleanup

Once the ClickUp integration has been stable and working for a while, remove the extra debugging added to the "Comment on ClickUp task with multidev URL" step in `.github/workflows/reusable-deploy-multidev.yml`:

- Remove `set -x` (line-by-line command tracing)
- Remove `echo "Branch: $BRANCH"` and `echo "PR Title: $PR_TITLE"` debug lines
- Change curl to discard response body (`-o /dev/null`) instead of saving to `/tmp/clickup_response.json`
- Remove the `cat /tmp/clickup_response.json || true` line
