# VPC

The entire application is configured to be deployed to its own custom VPC. VPCs gives you an isolated environment from the rest of your existing network.

## Default VPC

Should you choose to use the default VPC, follow these steps:

1. Comment out `vpc.yml` and `lambda.yml` resources in `serverless.yml`

```yaml
# - ${file(${self:custom.path.resources}/vpc.yml)}
# - ${file(${self:custom.path.resources}/lambda.yml)}
```

2. Comment out `vpc` in `provider` in `serverless.yml`

```yaml
# vpc:
#   securityGroupIds:
#     - Ref: LambdaSecurityGroup
#   subnetIds:
#     - Ref: PrivateSubnet1
#     - Ref: PrivateSubnet2
```

This will now set your lambda and required resources within your own AWS default VPC.

!!! info "ElastiCache VPC Requirement"

    Note that ElastiCache requires you to set a VPC and will no longer work once you use a default VPC.

## Connecting to existing VPC

Coming soon...
