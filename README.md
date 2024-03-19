# soci-index-ecr-buildkite-plugin

Genrate a SOCI index for a docker image and push it to ECR.

SOCI indexes can reduce the start time of ECS/Fargate tasks by allowing the
task to lazy load the images and start before all data is available.

More info: 
https://aws.amazon.com/blogs/aws/aws-fargate-enables-faster-container-startup-using-seekable-oci/

## Usage

Add the plugin to your steps like this:

```
steps:
  - name: ":docker: SOCI"
    plugins:
      - buildkite/soci-index-ecr:
          image: "445615400570.dkr.ecr.us-east-1.amazonaws.com/foo:123"
```

The image will be downloaded, and an SOCI index pushed back to the same repository.

The agent or job must have permission to read and push the ECR repository. If
the EC2 instance role doesn't have the permission, it's possible to combine it
with the aws-assume-role-with-web-identity plugin.

```
steps:
  - name: ":docker: SOCI"
    plugins:
      - aws-assume-role-with-web-identity:
          role-arn: arn:aws:iam::445615400570:role/role-name
      - buildkite/soci-index-ecr:
          image: "445615400570.dkr.ecr.us-east-1.amazonaws.com/foo:123"
```
