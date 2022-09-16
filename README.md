# RDS Backup/Restore in AWS

## Introduction

This repository contains information on the process as well as the CloudFormation template that is required when restoring a relational database from a backup or a snapshot.
This template can also be used for the initial RDS setup.
Please note that this would be independent of your application CI/CD pipeline and would only be used for either creating a new DB instance or restoring an RDS instance from snapshot.

## Innovation Environment Note

This was written to be deployed to the MD IDS Innovation Environment. Unlike Landing Zone accounts, it does not have VPC-related CloudFormation exports, such as `DefaultVPC` and `PrivateSubnets`. Since Lilly developers will need to use these exports in real life, we have a template, `simulate-lz-exports.yml` to create these exports in the Innovation Environment. We set this up manually before running our pipeline for the first time so that our main CloudFormation template would see these exports.
Please ignore this when working in actual Landing Zone.

## Code File Information that would be needed for the restore/DB creation process
Branch - `master`<br />
`buildspec.yml` - Use for any initial component installation and linting<br />
`params.xxx.json` - Per-environment parameter files, where *xxx* specifies environment. Used as input to CloudFormation template.<br />
`post-deploy-buildspec.yml` - Use for any commands that cannot be run until after the CloudFormation resources are created<br />
`simulate-lz-exports.yml` - Use for creating VPC related Cloudformation export. Not needed in Landing Zone<br />
`template-ui.yml` - Contain the required code to spin up a UI for data entry into RDS. This     template was used for co-create project to build some test data. In real world, the application team will not need this template file<br />
`template.yml` - Contain the required code for creating a new DB instance or restore from a snapshot. There are conditions within the template that lets the app team choose if they want to create an initial DB instance or restore a DB instance from snapshot. It will also let you configure certain parameters during the stack creation

## Post pipeline deployment process
The CloudFormation template will only create/restore DB instance.
The app team will have to manually delete the old DB instance in case of restore and rename the new one with the old instance name.