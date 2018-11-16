# Static sites

## Overview

Static sites are deployed to S3 and served through Cloudfront. S3 only supports static sites, so the contents are compiled from a source project (e.g. Nuxt.js) and uploaded to S3. Cloudfront acts as a CDN to serve the files to users.

To get a project running, we first create the infrastructure using Cloudformation. This includes things like S3 bucket, DNS entries, CloudFront distributions for the site and www redirect.

The deployment is a simple script that compiles the new code (for Nuxt.js projects that's usually `npm run generate`), deploys it to S3 and invalidates the CDN cache.

## Deployment

Run `npm run deploy:staging` and `npm run deploy:production` to deploy to staging and production, respectively. This will checkout a fresh version of either the `dev` or `master` branch from Github and run whatever `npm` magic is needed to compile a new version of the code. E.g. for swapmarket.com this does:

```bash
npm cache verify
npm install
npm run generate
```

Then it syncs code with S3 (`aws s3 sync`) and invalidates the CDN cache (`aws cloudfront create-invalidation`).

## Infrastructure

The following are specific instructions for swapmarket.com (DEX), but will be very similar to other static sites at 0xcert.

### Create Infra

#### Set up secrets

1. Copy the example secrets file: `cp infra/cf/infra-params-secrets-example.json infra/cf/infra-params-secrets.json`.
2. Edit the file `infra/cf/infra-params-secrets.json` and put in your secrets where needed.

#### Provision infrastructure

```bash
STACK_NAME=dex-ui
aws cloudformation deploy --template-file infra/cf/infra.yml --stack-name $STACK_NAME --parameter-overrides file://./infra/cf/infra-params-secrets.json --capabilities CAPABILITY_NAMED_IAM
```

#### Delete the entire infrastructure

**Warning:** this will wipe out the entire infrastructure for this app,
so be sure this is what you wat to do.

```bash
STACK_NAME=dex-ui
aws cloudformation delete-stack --stack-name $STACK_NAME
```
