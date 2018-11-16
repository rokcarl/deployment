# Infrastructure Overview

We have two types of apps that we deploy:
- _[a static site](infra-static.md):_ a static HTML site is compiled during build time, deployed to S3 and served through Cloudfront.
- _[a node app](infra-node.md):_ a docker container is built and deployed to an EC2 server in an Auto Scaling Group, served through the AWS Application Load Balancer.
