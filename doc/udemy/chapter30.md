# Chapter 30. Other Services

<!-- TOC -->

- [Chapter 30. Other Services](#chapter-30-other-services)
    - [CloudFormation StackSets](#cloudformation-stacksets)
        - [Administrator and Target Accounts](#administrator-and-target-accounts)
        - [Permission Models](#permission-models)
        - [Stack Instances](#stack-instances)
        - [Stack Set Operation Options](#stack-set-operation-options)

<!-- /TOC -->

An administrator account is the AWS account in which you create and manage stack sets. A target account is the account into which you create, update, or delete one or more stacks in your stack set.

Before you can use a stack set to create stacks in a target account, set up a trust relationship between the administrator and target accounts.

### Permission Models

You can create stack sets using either self-managed permissions or service-managed permissions.

With self-managed permissions, you create the IAM roles required by StackSets to deploy across accounts and regions. Using this permission model, StackSets can deploy to any AWS account in which you have permissions to create an IAM role.

With service-managed permissions, you can deploy stack instances to accounts managed by AWS Organizations. Using this model, StackSets creates the IAM roles on your behalf, and you can turn on automatic deployments to accounts that you add to your Organizations in the future.

### Stack Instances

A stack instance associates with only one stack set. When you update a stack set, all associated stack instances update throughout all accounts and regions.

### Stack Set Operation Options

The options below help to control the time and number of failures allowed to perform successful stack set operations, and prevent you from losing stack resources.

* Maximum concurrent accounts - specify the maximum number of target accounts that an operation performs at one time.

* Failure tolerance - specify the maximum number of failed target accounts before StackSet stops the operation.

* Retain stacks - specify whether to retain a stack after removing it from a stack set.

* Region concurrency - specify whether to start an operation sequentially or in parallel.