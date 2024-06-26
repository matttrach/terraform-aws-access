# Policies

Specifically talking about IAM policies necessary for enabling OIDC between GitHub runners and AWS.
There is an excellent tutorial on how to get started with this in the AWS credential action's repo:
https://github.com/aws-actions/configure-aws-credentials?tab=readme-ov-file#oidc.

One thing they can't cover there is what permissions to give, because they are application specific.
I have been meaning to add the policies you should set here somewhere, so I am dumping it in this note.

I split my policies into two parts: access and server.
I am not sure if these are exactly least priviledge, but pretty close,
  I suggest monitoring the access and reducing as seems possible.

Please remember that these are just my notes, make good decisions and evaluate your security concerns.

## Access

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Tags",
            "Effect": "Allow",
            "Action": [
                "ec2:CreateTags",
                "ec2:DeleteTags",
                "ec2:DescribeTags"
            ],
            "Resource": "*"
        },
        {
            "Sid": "VPC",
            "Effect": "Allow",
            "Action": [
                "ec2:CreateVpc",
                "ec2:DeleteVpc",
                "ec2:DescribeVpcs",
                "ec2:DescribeVpcAttribute",
                "ec2:ModifyVpcAttribute",
                "ec2:MoveAddressToVpc",
                "ec2:DisassociateVpcCidrBlock",
                "ec2:AssociateVpcCidrBlock",
                "ec2:CreateDefaultVpc"
            ],
            "Resource": "*"
        },
        {
            "Sid": "InternetGateway",
            "Effect": "Allow",
            "Action": [
                "ec2:CreateInternetGateway",
                "ec2:DeleteInternetGateway",
                "ec2:AttachInternetGateway",
                "ec2:DetachInternetGateway",
                "ec2:DescribeInternetGateways"
            ],
            "Resource": "*"
        },
        {
            "Sid": "Route",
            "Effect": "Allow",
            "Action": [
                "ec2:CreateRoute",
                "ec2:DeleteRoute",
                "ec2:CreateRouteTable",
                "ec2:DeleteRouteTable",
                "ec2:DescribeRouteTables",
                "ec2:DisassociateRouteTable",
                "ec2:ReplaceRoute",
                "ec2:ReplaceRouteTableAssociation",
                "ec2:AssociateRouteTable"
            ],
            "Resource": "*"
        },
        {
            "Sid": "Subnet",
            "Effect": "Allow",
            "Action": [
                "ec2:CreateSubnet",
                "ec2:AssociateSubnetCidrBlock",
                "ec2:CreateSubnetCidrReservation",
                "ec2:DeleteSubnetCidrReservation",
                "ec2:DisassociateSubnetCidrBlock",
                "ec2:GetSubnetCidrReservations",
                "ec2:ModifySubnetAttribute",
                "ec2:CreateDefaultSubnet",
                "ec2:DeleteSubnet",
                "ec2:DescribeSubnets",
                "ec2:DescribeAvailabilityZones",
                "ec2:DescribeAddresses",
                "ec2:DescribeAddressesAttribute",
                "ec2:DescribeAddressTransfers",
                "ec2:DescribeMovingAddresses",
                "ec2:AllocateAddress",
                "ec2:AssociateAddress",
                "ec2:DisassociateAddress",
                "ec2:ModifyAddressAttribute",
                "ec2:ReleaseAddress"
            ],
            "Resource": "*"
        },
        {
            "Sid": "SecurityGroup",
            "Effect": "Allow",
            "Action": [
                "ec2:CreateSecurityGroup",
                "ec2:DeleteSecurityGroup",
                "ec2:DescribeSecurityGroups",
                "ec2:AuthorizeSecurityGroupEgress",
                "ec2:AuthorizeSecurityGroupIngress",
                "ec2:DescribeSecurityGroupReferences",
                "ec2:ModifySecurityGroupRules",
                "ec2:DescribeSecurityGroupRules",
                "ec2:RevokeSecurityGroupIngress",
                "ec2:RevokeSecurityGroupEgress",
                "ec2:UpdateSecurityGroupRuleDescriptionsEgress",
                "ec2:UpdateSecurityGroupRuleDescriptionsIngress"
            ],
            "Resource": "*"
        },
        {
            "Sid": "SshKeyPair",
            "Effect": "Allow",
            "Action": [
                "ec2:CreateKeyPair",
                "ec2:DeleteKeyPair",
                "ec2:DescribeKeyPairs",
                "ec2:ImportKeyPair"
            ],
            "Resource": "*"
        },
        {
            "Sid": "Domain",
            "Effect": "Allow",
            "Action": [
                "route53:GetChange",
                "route53:GetHostedZone",
                "route53:ListCidrBlocks",
                "route53:ListHostedZones",
                "route53:ListHostedZonesByName",
                "route53:ListHostedZonesByVPC",
                "route53:ListResourceRecordSets",
                "route53:GetHostedZoneLimit",
                "route53:ListTagsForResource",
                "route53:ListTagsForResources",
                "route53:TestDNSAnswer",
                "route53:AssociateVPCWithHostedZone",
                "route53:ChangeResourceRecordSets",
                "route53:ChangeCidrCollection",
                "route53:CreateCidrCollection",
                "route53:CreateHostedZone",
                "route53:DeleteHostedZone",
                "route53:DeleteCidrCollection",
                "route53:CreateVPCAssociationAuthorization",
                "route53:DeleteVPCAssociationAuthorization",
                "route53:DisassociateVPCFromHostedZone",
                "route53:UpdateHostedZoneComment",
                "route53:UpdateHealthCheck",
                "route53:ChangeTagsForResource",
                "route53:ListHealthChecks",
                "route53:GetHealthCheck",
                "route53:GetAccountLimit",
                "route53:ListCidrLocations",
                "route53:ListCidrCollections",
                "route53:GetHostedZoneCount",
                "route53:CreateHealthCheck",
                "route53:DeleteHealthCheck",
                "iam:UploadServerCertificate",
                "iam:ListServerCertificates",
                "iam:ListServerCertificateTags",
                "iam:DeleteServerCertificate",
                "iam:TagServerCertificate",
                "iam:GetServerCertificate",
                "iam:UntagServerCertificate"
            ],
            "Resource": "*"
        },
        {
            "Sid": "Loadbalancing",
            "Effect": "Allow",
            "Action": [
                "elasticloadbalancing:CreateLoadBalancer",
                "ec2:CreateSecurityGroup",
                "ec2:DescribeAccountAttributes",
                "ec2:DescribeInternetGateways",
                "ec2:DescribeSecurityGroups",
                "ec2:DescribeSubnets",
                "ec2:DescribeVpcs",
                "elasticloadbalancing:AttachLoadBalancerToSubnets",
                "elasticloadbalancing:ApplySecurityGroupsToLoadBalancer",
                "elasticloadbalancing:DeregisterInstancesFromLoadBalancer",
                "ec2:DescribeClassicLinkInstances",
                "ec2:DescribeInstances",
                "elasticloadbalancing:DescribeInstanceHealth",
                "elasticloadbalancing:DescribeLoadBalancers",
                "elasticloadbalancing:DisableAvailabilityZonesForLoadBalancer",
                "elasticloadbalancing:EnableAvailabilityZonesForLoadBalancer",
                "elasticloadbalancing:RegisterInstancesWithLoadBalancer",
                "ec2:DescribeVpcClassicLink",
                "elasticloadbalancing:AddTags",
                "elasticloadbalancing:ModifyLoadBalancerAttributes",
                "elasticloadbalancing:DescribeLoadBalancerAttributes",
                "elasticloadbalancing:DescribeListeners",
                "elasticloadbalancing:DescribeTags",
                "elasticloadbalancing:DescribeRules",
                "elasticloadbalancing:DescribeTargetGroupAttributes",
                "elasticloadbalancing:DescribeTargetGroups",
                "elasticloadbalancing:DescribeTargetHealth",
                "elasticloadbalancing:DescribeSSLPolicies",
                "elasticloadbalancing:DescribeListenerCertificates",
                "elasticloadbalancing:AddListenerCertificates",
                "elasticloadbalancing:CreateListener",
                "elasticloadbalancing:CreateRule",
                "elasticloadbalancing:CreateTargetGroup",
                "elasticloadbalancing:DeleteListener",
                "elasticloadbalancing:DeleteLoadBalancer",
                "elasticloadbalancing:DeleteRule",
                "elasticloadbalancing:DeleteTargetGroup",
                "elasticloadbalancing:DeregisterTargets",
                "elasticloadbalancing:ModifyListener",
                "elasticloadbalancing:ModifyRule",
                "elasticloadbalancing:ModifyTargetGroup",
                "elasticloadbalancing:ModifyTargetGroupAttributes",
                "elasticloadbalancing:RegisterTargets",
                "elasticloadbalancing:RemoveListenerCertificates",
                "elasticloadbalancing:SetIpAddressType",
                "elasticloadbalancing:SetRulePriorities",
                "elasticloadbalancing:SetSecurityGroups",
                "elasticloadbalancing:SetSubnets",
                "elasticloadbalancing:RemoveTags",
                "elasticloadbalancing:ConfigureHealthCheck",
                "elasticloadbalancing:DescribeLoadBalancerPolicies",
                "elasticloadbalancing:DescribeLoadBalancerPolicyTypes",
                "elasticloadbalancing:CreateLoadBalancerListeners",
                "elasticloadbalancing:CreateLoadBalancerPolicy",
                "elasticloadbalancing:DeleteLoadBalancerListeners",
                "elasticloadbalancing:DeleteLoadBalancerPolicy",
                "elasticloadbalancing:DetachLoadBalancerFromSubnets",
                "elasticloadbalancing:SetLoadBalancerListenerSSLCertificate",
                "elasticloadbalancing:SetLoadBalancerPoliciesForBackendServer",
                "elasticloadbalancing:SetLoadBalancerPoliciesOfListener"
            ],
            "Resource": "*"
        }
    ]
}
```

## Server

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ManageEC2Instances",
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeInstanceAttribute",
                "ec2:DescribeInstances",
                "ec2:DescribeInstanceStatus",
                "ec2:DescribeInstanceTypeOfferings",
                "ec2:DescribeInstanceCreditSpecifications",
                "ec2:DescribeInstanceTypes",
                "ec2:GetInstanceTypesFromInstanceRequirements",
                "ec2:ImportInstance",
                "ec2:ModifyInstanceAttribute",
                "ec2:ModifyInstanceMetadataOptions",
                "ec2:RebootInstances",
                "ec2:ReportInstanceStatus",
                "ec2:ResetInstanceAttribute",
                "ec2:RunInstances",
                "ec2:StartInstances",
                "ec2:StopInstances",
                "ec2:TerminateInstances",
                "ec2:CreateTags",
                "ec2:DeleteTags",
                "ec2:DescribeTags"
            ],
            "Resource": "*"
        },
        {
            "Sid": "AddressAndInterface",
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeAddresses",
                "ec2:DescribeAddressesAttribute",
                "ec2:DescribeAddressTransfers",
                "ec2:DescribeMovingAddresses",
                "ec2:DisableAddressTransfer",
                "ec2:DisassociateAddress",
                "ec2:EnableAddressTransfer",
                "ec2:ModifyAddressAttribute",
                "ec2:MoveAddressToVpc",
                "ec2:ReleaseAddress",
                "ec2:ResetAddressAttribute",
                "ec2:UnassignIpv6Addresses",
                "ec2:UnassignPrivateIpAddresses",
                "ec2:AcceptAddressTransfer",
                "ec2:AllocateAddress",
                "ec2:AssignIpv6Addresses",
                "ec2:AssignPrivateIpAddresses",
                "ec2:AssociateAddress",
                "ec2:AttachNetworkInterface",
                "ec2:CreateNetworkInterface",
                "ec2:CreateNetworkInterfacePermission",
                "ec2:DeleteNetworkInterface",
                "ec2:DeleteNetworkInterfacePermission",
                "ec2:DescribeNetworkInterfaceAttribute",
                "ec2:DescribeNetworkInterfacePermissions",
                "ec2:DescribeNetworkInterfaces",
                "ec2:DetachNetworkInterface",
                "ec2:ModifyNetworkInterfaceAttribute",
                "ec2:ResetNetworkInterfaceAttribute",
                "ec2:CreateTags",
                "ec2:DeleteTags",
                "ec2:DescribeTags"
            ],
            "Resource": "*"
        },
        {
            "Sid": "Image",
            "Effect": "Allow",
            "Action": [
                "ec2:CopyImage",
                "ec2:CreateImage",
                "ec2:CreateRestoreImageTask",
                "ec2:CreateStoreImageTask",
                "ec2:DeregisterImage",
                "ec2:DescribeExportImageTasks",
                "ec2:DescribeImageAttribute",
                "ec2:DescribeImages",
                "ec2:DescribeImportImageTasks",
                "ec2:DescribeStoreImageTasks",
                "ec2:DisableImageDeprecation",
                "ec2:EnableImageDeprecation",
                "ec2:ExportImage",
                "ec2:ImportImage",
                "ec2:ListImagesInRecycleBin",
                "ec2:ModifyImageAttribute",
                "ec2:RegisterImage",
                "ec2:ResetImageAttribute",
                "ec2:RestoreImageFromRecycleBin",
                "ec2:CancelImageLaunchPermission",
                "ec2:CreateTags",
                "ec2:DeleteTags",
                "ec2:DescribeTags"
            ],
            "Resource": "*"
        },
        {
            "Sid": "ManageVolumesAndSnapshots",
            "Effect": "Allow",
            "Action": [
                "ec2:AttachVolume",
                "ec2:CreateReplaceRootVolumeTask",
                "ec2:CreateVolume",
                "ec2:DeleteVolume",
                "ec2:DescribeReplaceRootVolumeTasks",
                "ec2:DescribeVolumeAttribute",
                "ec2:DescribeVolumes",
                "ec2:DescribeVolumesModifications",
                "ec2:DescribeVolumeStatus",
                "ec2:DetachVolume",
                "ec2:EnableVolumeIO",
                "ec2:ImportVolume",
                "ec2:ModifyVolume",
                "ec2:ModifyVolumeAttribute",
                "ec2:CopySnapshot",
                "ec2:CreateSnapshot",
                "ec2:CreateSnapshots",
                "ec2:DeleteSnapshot",
                "ec2:DescribeImportSnapshotTasks",
                "ec2:DescribeSnapshotAttribute",
                "ec2:DescribeSnapshots",
                "ec2:DescribeSnapshotTierStatus",
                "ec2:ImportSnapshot",
                "ec2:ListSnapshotsInRecycleBin",
                "ec2:ModifySnapshotAttribute",
                "ec2:ModifySnapshotTier",
                "ec2:ResetSnapshotAttribute",
                "ec2:RestoreSnapshotFromRecycleBin",
                "ec2:RestoreSnapshotTier",
                "ec2:CreateTags",
                "ec2:DeleteTags",
                "ec2:DescribeTags"
            ],
            "Resource": "*"
        },
        {
            "Sid": "ManageHostnames",
            "Effect": "Allow",
            "Action": [
                "route53Domains:EnableDomainAutoRenew",
                "route53Domains:GetDomainDetail",
                "route53Domains:ListDomains",
                "route53Domains:ListOperations",
                "route53Domains:PushDomain",
                "route53Domains:RegisterDomain",
                "route53Domains:RenewDomain",
                "route53Domains:UpdateDomainContact",
                "route53Domains:UpdateDomainContactPrivacy",
                "route53Domains:UpdateDomainNameservers",
                "route53Resolver:AssociateResolverEndpointIpAddress",
                "route53Resolver:UpdateResolverConfig",
                "route53:AssociateVPCWithHostedZone",
                "route53:CreateHostedZone",
                "route53:DeleteHostedZone",
                "route53:DisassociateVPCFromHostedZone",
                "route53:UpdateHostedZoneComment",
                "route53:GetHostedZone",
                "route53:GetHostedZoneCount",
                "route53:GetHostedZoneLimit",
                "route53:ListHostedZones",
                "route53:ListHostedZonesByName",
                "route53:ListHostedZonesByVPC",
                "route53:ListResourceRecordSets",
                "route53:ChangeResourceRecordSets",
                "route53Resolver:TagResource",
                "route53Resolver:UntagResource",
                "route53Domains:UpdateTagsForDomain",
                "route53Domains:ListTagsForDomain",
                "route53Domains:DeleteTagsForDomain",
                "route53:ListTagsForResource",
                "route53:ChangeTagsForResource",
                "route53:ListTagsForResource",
                "route53:ListTagsForResources"
            ],
            "Resource": "*"
        },
        {
            "Sid": "S3ForBackup",
            "Effect": "Allow",
            "Action": [
                "s3:CreateBucket",
                "s3:DeleteBucket",
                "s3:ListBucket",
                "s3:ListBucketVersions",
                "s3:PutBucketTagging",
                "s3:PutBucketVersioning",
                "s3:DeleteObject",
                "s3:DeleteObjectTagging",
                "s3:GetObject",
                "s3:GetObjectAcl",
                "s3:GetObjectAttributes",
                "s3:GetObjectRetention",
                "s3:PutObject",
                "s3:PutObjectAcl",
                "s3:PutObjectRetention",
                "s3:PutObjectTagging",
                "s3:RestoreObject",
                "s3:GetBucketTagging"
            ],
            "Resource": "*"
        }
    ]
}
```
