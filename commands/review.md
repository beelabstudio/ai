# /project:review

Review the current changes before submitting a PR.

## Steps

1. Run `git diff` to see what changed
2. Check for:
   - Code style consistency
   - Test coverage
   - Security issues
   - Documentation updates
3. Run `npm run lint` and `npm run test`
4. Summarize findings

## Report Format

```
## Review Summary

### Files Changed
- [list files]

### Code Quality
- ✅/⚠️ Style consistency
- ✅/⚠️ Test coverage
- ✅/⚠️ Documentation

### Recommendations
- [any suggested changes]
```
