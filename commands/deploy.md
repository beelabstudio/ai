# /project:deploy

Deploy the application.

## Prerequisites

- All tests passing
- No uncommitted changes in main branch
- Environment variables configured

## Steps

1. Run pre-deploy checks:
   - `npm run test`
   - `npm run build`
2. Check deployment environment status
3. Execute deployment
4. Verify deployment success
5. Monitor for errors

## Post-Deploy

- Verify application health
- Check logs for errors
- Update deployment documentation if needed
