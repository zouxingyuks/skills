# Fallback PR Template

Use this template only when the target repository has no PR or merge template.

````markdown
## Summary

[2-3 sentences describing what changed and why]

## Test Results

- [ ] Ran relevant unit tests
- [ ] Ran relevant integration tests
- [ ] Ran frontend checks or build
- [ ] Completed manual verification
- [ ] Not tested; reason: [reason]

Commands and results:

```text
[commands and outcomes]
```

## Risks and Rollback

- Risks: [compatibility, deployment, user impact, or none]
- Rollback plan: [how to revert safely]

## Related Issues

- Related: #<issue-number>

## Affected Areas

- [ ] API contract or route
- [ ] permission / auth
- [ ] storage / migration
- [ ] deployment / config
- [ ] frontend UI
- [ ] docs only

Impact notes: [scope and boundaries]
````
