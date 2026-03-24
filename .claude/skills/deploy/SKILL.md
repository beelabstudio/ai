# Deploy Skill

## Description

Guide and validate deployment processes.

## Activation

This skill is auto-invoked when:
- Deployment configuration files are modified
- CI/CD pipeline changes
- Release tags are created
- User asks about deployment

## Pre-Deploy Checklist

- [ ] All tests passing locally
- [ ] No TypeScript errors (`tsc --noEmit`)
- [ ] Linting passes
- [ ] Environment variables documented
- [ ] Database migrations prepared (if applicable)
- [ ] Feature flags configured (if applicable)
- [ ] Rollback plan ready

## Deployment Steps

1. **Preparation**
   - Merge PR to main
   - Tag release: `git tag -a v1.2.0 -m "Release 1.2.0"`
   - Push tag: `git push origin v1.2.0`

2. **Build**
   - Run build: `npm run build`
   - Verify build output
   - Check bundle size

3. **Deploy**
   - Deploy to staging first
   - Run smoke tests
   - Deploy to production
   - Monitor metrics

4. **Verification**
   - Check application health endpoints
   - Verify critical user flows
   - Monitor error rates
   - Confirm database migrations ran

## Rollback

If issues detected:
1. Stop deployment
2. Run rollback procedure
3. Notify team
4. Document incident

## Post-Deploy

- Update changelog
- Announce release
- Monitor for 24 hours
