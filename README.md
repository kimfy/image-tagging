# README.md

Just some practical experimenting with optimizing Docker builds between CI and CD to multiple environments. With this workflow you are only building your image once and re-tagging it for production releases.

The GitHub workflow in this repository does the following:

1. Trigger on commits to `refs/heads/trunk` - which is my imaginary test/dev environment.
2. Pulls alpine:latest
3. Tags it with `kimiversen.azurecr.io/alpine:${{ env.github_sha }}` 
4. Push the image
5. End - dev build is now built and pushed, ready for CD to deploy.

A second job is ran for production

1. Trigger on tags with the rule `*.*.*`
2. Pull the already built image from the commit sha `kimiversen.azurecr.io/alpine:${{ github.sha }}`
3. Re-tag to `kimiversen.azurecr.io/alpine:${{ github.ref_name }}`
4. Push the image

```bash
# Push to development
git push origin trunk

# Push to production (where $COMMIT_SHA is the commit sha of the dev build you want to lift to production)
git tag -a 0.0.1 -m "Canon" $COMMIT_SHA
git push origin 0.0.1
```

# License: MIT

# Contribute: Feel free to

# Contact: N/A

