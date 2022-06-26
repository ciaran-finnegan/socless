# SOCless - serverless security orchestration, automation and response

SOCless is a serverless framework built to help security teams easily automate their incident response and operations workflows.

# Overview

SOCless uses the AWS Step Functions and AWS Lambda services to execute user-defined workflows. The workflows, called Playbooks, are defined as JSON objects and triggered by real-time alerts from data sources or AWS CloudWatch schedules.

![](https://twilio-labs.github.io/socless/imgs/socless-base-architecture.png)    

Features
---
- Responds to real-time or scheduled events
- Orchestrates existing security tools into workflows using AWS Lambda functions written in Python 3
- Interact with humans as part of automated workflows and adapt to their responses
- Static IP address that can be whitelisted to internal resources
- Rapid automation development life-cycle courtesy of reusable, modular and shareable plugins
- Infrastructure and response workflows deploy as code using [The Serverless Framework](https://serverless.com)
- Serverless design has low cost, low operational overhead, and scales effortlessly

Ready? Check out the [docs!](https://twilio-labs.github.io/socless/)

Join our [community Slack workspace](https://join.slack.com/t/socless/shared_invite/enQtODA3ODEzNzcwNDgxLTBiYjVjYjI4ODI4YTY5YzM4OWRlYjQ1Yzg4M2EzMGUzMGMyYThlN2U5NTI5OWIwZWE1ZTcwNjA2MjgyZDRmMjg)


# Development Guide

## Building and Redeploying the Docs

SOCless documentation is contained in the docs folder and is powered by [MkDocs](https://www.mkdocs.org/) and [MkDocs Material](https://squidfunk.github.io/mkdocs-material/). The built docs are hosted on Github pages

**To setup your environment for building the docs**

```
python3 -m venv venv
. venv/bin/activate
pip install -r docs-requirements.txt
```

**To serve the docs locally (after setup)**
```
mkdocs serve
```

**To deploy the docs to Github pages**
```
mkdocs gh-deploy
```

### Installating prerequisites

#### MacOS

- Follow the instructions at the URL below to install the AWS CLI
  - https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
- Follow the instructions at the URL below to install NPM
  - https://nodejs.org/en/
- Follow the instructions at the URL below to install the Serverless Framework
  - https://www.serverless.com/framework/docs/getting-started
  - Install the serverless framework
    ```
    npm install -g serverless.
    ```
- Configure the aws cli profile (using AWS SSO)  
    ```
    aws configure sso
    SSO start URL [https://d-90674c9ee9.awsapps.com/start/]: https://surfcoachguru.awsapps.com/start
    SSO Region [us-east-1]:
    There are 6 AWS accounts available to you.
    Using the account ID 054146564569
    There are 2 roles available to you.
    Using the role name "AWSAdministratorAccess"
    CLI default client Region [us-east-1]: ap-southeast-2
    CLI default output format [json]: json
    CLI profile name [AWSAdministratorAccess-054146564569]: socless-dev-AWSAdministratorAccess

    To use this profile, specify the profile name using --profile, as shown:

    aws s3 ls --profile socless-dev-AWSAdministratorAccess
    ```

    - Serverless framework doesn't support credentials configured by AWS SSO
      - Refer to details here https://github.com/serverless/serverless/issues/7567
      - This was fixed here https://github.com/aws/aws-sdk-js/pull/4047 but isn't working for me on serverless version 2.57.0
      - To work around this use yawsso, https://github.com/victorskl/yawsso
        ```
        yawsso -p socless-dev-AWSAdministratorAccesss
        aws s3 ls --region ap-southeast-2 --profile socless-dev-AWSAdministratorAccess
- Install brew
  - 
- Install pyenv
  - Refer to detailed instructions at the URL below
    - https://opensource.com/article/19/5/python-3-default-mac
    ```
    brew install pyenv
    ```
- Install python
  - https://www.python.org/ftp/python/3.7.9/python-3.7.9-macosx10.9.pkg
- Install pip
  - 
- Install git


### Deployig SOCless

- Update package.json and add in the following
        ```
                ,
        "config":{
            "aws_profile": "socless-dev-AWSAdministratorAccess",
            "dev": "--stage dev --region ap-southeast-2"
        }
        ```
-   Refer to detailed instructions here
    - https://twilio-labs.github.io/socless/deploying-socless/

```
git clone git@github.com:twilio-labs/socless.git
cd socless
npm install
virtualenv --python=python3.7 venv
. venv/bin/activate
serverless deploy --stage dev --region ap-southeast-2 --aws-profile socless-dev-AWSAdministratorAccess 
```


