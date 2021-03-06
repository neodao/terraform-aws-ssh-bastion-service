# 3.0

With version 3 series (backward compatible with version 2) the ability to assume a role in another account has now been integrated with conditional logic. If you supply the ARN for a role for the bastion service to assume in another account ${var.assume_role_arn} then this plan will create an instance profile, role and policy along with each bastion to make use of it. A matching sample policy and trust relationship is given as an output from the plan to assist with application in the other account. If you do not supply this arn then this plan presumes IAM lookups in the same account and creates an appropriate instance profile, role and policies for each bastion in the same AWS account. 'Each bastion' here refers to a combination of environment, AWS account, AWS region and VPCID determined by deployment. Since this is a high availabilty service, it is not envisaged that there would be reason for more than one independent deployment within such a combination. 

Also with version 3 the IAM policy generation and user data have been moved from modules back into the main plan. User data is no longer displayed. 

If you are seeking a solution for ECS hosts then you are recommended to either the [Widdix project]((https://github.com/widdix/aws-ec2-ssh)) directly or my [Ansible-galaxy respin of it](https://galaxy.ansible.com/joshuamkite/aws-ecs-iam-users-tags/). This offers a range of features, suitable for a long-lived stateful host built.

# 2.0 

Breaking Changes from version 1.x series
In version 1.0 (download this release if you want it!) this plan deployed a simple static host. With the version 2 branch a move has been made to make this a high availability service with an autoscaling group, health checks and a load balancer. This has necessitated the removal of the feature in version 1.x of creating and attaching to the container host an Elastic Network Interface for each additional subnet specified. With the new branch additional subnets are supplied instead to the autoscaling group and load balancer. The expectation is that separation will be managed by vpc rather than segregated subnet. The VPC-id is also integrated into the DNS entry to permit multiple deployments to different vpc's within a single region.

# 1.1

Thanks to Piotr Jaromin for implementing these features

* S3 bucket is no longer necessary, golang script for iam-authorized-command is stored inside this repository.
* IAM roles are generated based on region and environment role, so there should be no more conflicts.
* Added additional user-data file to output variables, it can be used to populate ecs/k8s nodes(based on amazon linux image), to allow sshing from bastion into nodes.
* Added tags variable ( it will attach additional tags to aws resources)