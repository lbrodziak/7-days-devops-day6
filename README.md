# Infrastructure as Code with CloudFormation

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-devops-cloudformation-updated)

**Author:** Łukasz Brodziak  
**Email:** lukasz.brodziak@gmail.com

---

![Image](http://learn.nextwork.org/surprised_maroon_fierce_chinese_gooseberry/uploads/aws-devops-cloudformation-updated_bd8b836b)

---

## Introducing Today's Project!

In this project, I will work on moving the CI/CD pipeline into CloudFormation. I'm doing this project to learn Infrastructure as Code (IaC).

### Key tools and concepts

Services I used were CloudFormation and IaC generator. Key concepts I learnt include creating CloudFormation templates, using templates to create stack and deploy resources. Also I have explored manual creation of code for resources that IaC generator was not able to scan for.

### Project reflection

This project took me approximately 2 hours to complete. The most challenging part was dealing with errors that were not included in the project like the problem with deploying connections. It was most rewarding to have this error handled.

This project is part six of a series of DevOps projects where I'm building a CI/CD pipeline!

---

## Generating a CloudFormation Template

The IaC Generator is a tool in AWS CloudFormation that scans all AWS resources which then can be placed in a template. It works in a three-step process:
1. A scan of resources is performed
2. A template file is created
3. Template is imported into CloudFormation and can be later deployed.

A CloudFormation template contains code definitions for all the resources we want to create. The resources that I could add to my template include IAM roles and policies, EC2 instance profiles, CodeArtifact repos and more.

The resources I couldn’t add to my template were CodeBuild and CodeDeploy because they require specific configuration details and and security permissions that our generator isn't able to handle automatically.

![Image](http://learn.nextwork.org/surprised_maroon_fierce_chinese_gooseberry/uploads/aws-devops-cloudformation-updated_0495b046)

---

## Template Testing

Before testing my template, I deleted existing resources from my account because they will be now recreated by the CloudFormation template. If they were not removed it would cause conflicts and the template deployment failure.

I tested my template by creating a new stack and submitting it. The result of my first test was a failure due to the policies being created before the role they depend on.

![Image](http://learn.nextwork.org/surprised_maroon_fierce_chinese_gooseberry/uploads/aws-devops-cloudformation-updated_f56730fd)

---

## DependsOn

To resolve the error, I by adding the DependsOn attribute to failing policies. The DependsOn attribute means that the resource it was defined in will not be created until the resource mentioned in DependsOn attribute is created.

The DependsOn line was added to for different parts of my template which are four CodeBuild policies, which allow access to CodeArtifact, CodeConnection, CloudWatch and EC2 instances which is underCodeBuild base policy.

![Image](http://learn.nextwork.org/surprised_maroon_fierce_chinese_gooseberry/uploads/aws-devops-cloudformation-updated_f0df8018)

---

## Circular Dependencies

I gave my CloudFormation template another test! But this time I encountered a circular dependency error. This means that two(or more) resources defined in the template refer to each other.

To fix this error, I the references to the IAM policies from the IAM role definition.

![Image](http://learn.nextwork.org/surprised_maroon_fierce_chinese_gooseberry/uploads/aws-devops-cloudformation-updated_e6fd85ed)

---

## Manual Additions

In a project extension, I manually defined two more resources:
1. CodeBuild
2. CodeDeploy

I also had to make sure the references were consistent in this template, so I edited CodeBuild role id, S3 Bucket id, codedeploy role id and codedeploy application id.

I also introduced Parameters, which are a way of making code more flexible. 

![Image](http://learn.nextwork.org/surprised_maroon_fierce_chinese_gooseberry/uploads/aws-devops-cloudformation-updated_1cee0428)

---

## Success!

I could verify all the deployed resources by visiting Resources tab that contains the list of created resources.

![Image](http://learn.nextwork.org/surprised_maroon_fierce_chinese_gooseberry/uploads/aws-devops-cloudformation-updated_bd8b836b)

---
## Here is link to day 7 where I create a fully automated CI/CD pipeline in AWS
[7 days DevOps challenge day 7](https://github.com/lbrodziak/7-days-devops-day7)
