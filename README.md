# Installation Guide
## Index

- [Introduction](#1-introduction)
- [System Requirements and Dependencies](#2-system-requirements-and-dependencies)
- [Installation Procedure](#3-installation-procedure)
- [Deploy wg-infra Repository](#step-1-deploy-wg-infra-repository)
- [Authentication Microservice](#step-2-authentication-microservice)
- [Notification Microservice](#step-3-notification-microservice)
- [Frontend Microservice](#step-4-frontend-microservice)
- [Wallet Microservice](#step-5-wallet-microservice)
- [CountriesNow API](#step-6-countriesnow-api)
- [Custom Codes Microservice](#step-7-custom-codes-microservice)
- [Configuration Details (local.tfvars)](#configuration-details-localtfvars-variables)
- [Microservices List](#microservices-list)

## 1. Introduction 

This document provides a comprehensive guide for installing and setting up Wallet Guru Wallet. It includes system requirements, dependencies, installation procedures, and troubleshooting steps.

## 2. System Requirements and dependencies

### Dependencies:

- JavaScript runtime
- Progressive Node.js framework
- NoSQL database
- ORM for DynamoDB
- AWS integration
- Secure password hashing
- Deploy services with Terraform
- Deploy services rafiki in local for connect with open payments
- Terraform
- AWS account
- AWS CLI
- This application depends on AWS Cognito for user authentication and authorization.
- Know Your Customer (KYC) verification is mandatory for all users. The system exclusively integrates with SUMSUB as the KYC provider. No other KYC services have been implemented or validated.
- Uptime Kuma: Used for uptime monitoring and alerting. This is an optional monitoring tool and not required for core application functionality.
- AWS SES (Simple Email Service) Required for sending transactional emails (e.g., verification codes, password resets, notifications).

## 3. Installation Procedure

### Step 1: Deploy wg-infra Repository

#### Clone the Repository
```sh
    git clone https://github.com/WalletGuruLLC/wg-infra.git
```
    Create or update a file called local.tfvars in the root directory of the project and add the change the values of the 

#### Create the secrets in the secret manager of each Microservice.

#### Run the following commands:
```sh
    terraform init
    terraform plan
    terraform apply
```
* You can also run all services with docker compose go to the root of the project replace all. env\_ files with the credentials of AWS and run the following command:
```sh
    docker-compose up
```
### Step 2 Authentication Microservice

The Authentication Microservice handles secure user login and authorization using Node.js and NestJS. It integrates DynamoDB as the NoSQL database with Dynamoose as the ORM.

#### Dependencies

This microservice uses the following key dependencies:

- Node.js - JavaScript runtime
- NestJS - Progressive Node.js framework
- DynamoDB - NoSQL database
- Dynamoose - ORM for DynamoDB
- AWS SDK - AWS integration
- bcrypt - Secure password hashing
- Wg-infra - Deploy services with Terraform

#### Install

##### Clone the Repository:
```sh
    git clone https://github.com/WalletGuruLLC/wg-backend-auth.git
    cd wg-backend-auth
```
##### Install Dependencies:
```sh
    npm install
```
##### Create envs in AWS Secrets Manager

In AWS Secrets Manager, create a new secret named walletguru-auth-local with the following key-value pairs:
```json
    {
    "AWS\_ACCESS\_KEY\_ID":"", # AWS Access Key ID to access DynamoDB and Cognito
    "AWS\_SECRET\_ACCESS\_KEY":"", # AWS Secret Access Key to access DynamoDB and Cognito
    "AWS\_REGION":"", # AWS Region
    "COGNITO\_USER\_POOL\_ID":"", # Cognito User Pool ID
    "COGNITO\_CLIENT\_ID":"", # Cognito Client ID
    "COGNITO\_CLIENT\_SECRET\_ID":"", # Cognito Client Secret ID
    "SQS\_QUEUE\_URL":"", # SQS Queue URL for sending email notifications
    "SENTRY\_DSN":"", # Sentry DSN for error tracking
    "AWS\_S3\_BUCKET\_NAME":"", # AWS S3 Bucket Name
    "WALLET\_URL":"", # Wallet URL for public access
    "APP\_SECRET":"", # App Secret for JWT token
    "NODE\_ENV":"development", # Node Environment
    "SUMSUB\_APP\_TOKEN":"", # Sumsub App Token
    "SUMSUB\_SECRET\_TOKEN":"", # Sumsub Secret Token
    "URL\_UPTIME":"", # URL for uptime monitoring
    "UPTIME\_PASSWORD":"", # Password for uptime monitoring
    "UPTIME\_USERNAME":"", # Username for uptime monitoring
    "SUMSUB\_DIGEST\_SECRET\_TOKEN":"" # Sumsub Digest Secret Token
    }
```
##### Set Up Environment Variables

Create a .env file in the root directory and add:
```sh
    AWS\_ACCESS\_KEY\_ID=""
    AWS\_SECRET\_ACCESS\_KEY=""
    SECRET\_NAME="walletguru-auth-local"
```
##### Run the Application using Docker Compose:
```sh
    docker-compose up
```
### Step 3 Notification Microservice

This microservice is responsible for sending notifications to users using Node.js and NestJS as the development framework and AWS SES for sending emails. It provides functionalities such as sending welcome emails and other user notifications.

#### Dependencies

This microservice uses the following key dependencies:

- JavaScript runtime
- Progressive Node.js framework
- NoSQL database
- ORM for DynamoDB
- AWS integration (or another SMTP server) and email verified from the STMP server
- Secure password hashing
- Deploy services with Terraform

#### Install

##### 1. Clone the Repository
```sh
    git clone https://github.com/ErgonStreamGH/wg-backend-notification.git
    cd wg-backend-notification
```
##### 2. Install Dependencies
```sh
    npm install
```
##### 3. Create envs in AWS Secrets Manager

Create a secret in AWS Secrets Manager with the name walletguru-notification-local and the following key-value pairs:
```json
    {
    "AWS\_REGION":"",
    "AWS\_ACCESS\_KEY\_ID":"",
    "AWS\_SECRET\_ACCESS\_KEY":"",
    "SQS\_QUEUE\_URL":"",
    "MAIL\_HOST":"",
    "MAIL\_PORT":"",
    "MAIL\_FROM":"",
    "MAIL\_USER":"",
    "MAIL\_PASS":"",
    "SENTRY\_DSN":"",
    "NODE\_ENV":"",
    "FRONTEND\_ACTIVE\_ACCOUNT\_URL":""
    }
```
##### 4. Set Up Environment Variables

Create a .env file in the root directory and add:
```sh
    AWS\_ACCESS\_KEY\_ID=""
    AWS\_SECRET\_ACCESS\_KEY=""
    SECRET\_NAME="walletguru-notification-local"
```
##### 5. Run the Application

Using Docker Compose:
```sh
    docker-compose up
```
### Step 4 Frontend Microservice

#### Dependencies

- Node.js v20.15
- pnpm v9.1.1
- (Optional) Docker v27 or higher
- Create resources of aws Wg infra
- Turborepo
- Eslint
- (Optional)  v27 or higher
- Wg infra
- Eslint
    • The project uses a shared eslint configuration located in the tooling/eslint directory.
    • The configuration is basically the default eslint config with some additional rules specific to react.
    • The configuration is shared across all packages and apps.
- Prettier
    • The project uses a shared prettier configuration located in the tooling/prettier directory.
    • The configuration is basically the default prettier config with some additional rules specific to tailwind.
    • The configuration is shared across all packages and apps.

#### Install

##### Code Structure:

```plaintext
.vscode/
└── Recommended extensions and settings for VSCode users

apps/
├── admin/
│   └── Next.js app that serves the admin webapp
└── more...

packages/
├── ui/
│   └── Start of a UI package for the webapp using Tailwind
├── hooks/
│   └── Start of a hooks package for shared logic
└── more...

tooling/
├── eslint/
│   └── Shared, fine-grained ESLint presets
├── prettier/
│   └── Shared Prettier configuration
├── tailwind/
│   └── Shared Tailwind configuration
└── typescript/
    └── Shared tsconfig you can extend from
```


##### Install dependencies
```sh
    pnpm i
```
##### Configure environment variables

There is an `.env.example` in the root directory you can use for reference
```sh
    cp .env.example .env
```
Start the development server for all apps
```sh
    pnpm dev
```
Note: If you want to run a specific app, you can use pnpm -F  dev where  is the name of the app you want to run.

##### Deployment

The default deployment is done using Docker. You can use the already configured Dockerfiles in each app to build and run the app on any platform that supports Docker. Replace for the name of the app you want to deploy.

Important: Some apps validate the environment variables at runtime AND at build time, so make sure to provide the required environment variables when running the Docker image AND when building the image.

##### Build the Docker image
```sh
    docker build -f apps//Dockerfile . --no-cache --build-arg = --build-arg = -t 
```
##### Run the Docker image
```sh
    docker run -p 3000:3000 --env-file ./.env 
```
#### Envs for pipeline
```json
    BITBUCKET\_CLONE\_DIR: Directory where the repository is cloned for pnpm install
    NODE\_ENV: Environment of the application (development, qa, staging, production)
    NEXT\_PUBLIC\_ADMIN\_BASE\_URL: Base URL of the admin webapp
    NEXT\_PUBLIC\_AUTH\_MICROSERVICE\_URL: URL of the auth microservice
    NEXT\_PUBLIC\_NOTIFICATION\_MICROSERVICE\_URL: URL of the notification microservice
    NEXT\_PUBLIC\_COUNTRIES\_MICROSERVICE\_URL: URL of the countries microservice
    NEXT\_PUBLIC\_WALLET\_MICROSERVICE\_URL: URL of the wallet microservice
    AWS\_KEY: Key of the AWS account for deploy image of docker in ECR
    AWS\_SECRET: Secret of the AWS account for deploy image of docker in ECR
    IMAGE: Name of the image for deploy in ECR
    CLUSTER\_NAME: Name of the cluster in ECS
    AWS\_ACCESS\_KEY\_ID\_TERRAFORM: Key ID of the AWS account for Terraform
    AWS\_SECRET\_ACCESS\_KEY\_TERRAFORM: Secret key of the AWS account for Terraform
```
### Step 5 Wallet Microservice

This Wallet Microservice provides secure user authentication and authorization using Node.js and * NestJS*. It integrates DynamoDB as the NoSQL database with Dynamoose as the ORM.

#### Dependencies

This microservice uses the following key dependencies:

- JavaScript runtime
- Progressive Node.js framework
- NoSQL database
- ORM for DynamoDB
- AWS integration
- Secure password hashing
- Deploy services with Terraform
- Deploy services rafiki in local for connect with open payments

#### Install

##### 1. Clone the Repository
```sh
    git clone https://github.com/ErgonStreamGH/wg-backend-wallet.git
    cd wg-backend-wallet
```
###### 2. Install Dependencies
```sh
    npm install
```
###### 3. Create envs in AWS Secrets Manager

Create a secret in AWS Secrets Manager with the name walletguru-wallet-local and the following key-value pairs:
```json
    {
    "AWS\_REGION": "", # AWS Region for the application
    "AWS\_ACCESS\_KEY\_ID": "", # AWS Access Key ID to access DynamoDB and Cognito
    "AWS\_SECRET\_ACCESS\_KEY": "", # AWS Secret Access Key to access DynamoDB and Cognito
    "NODE\_ENV": "", # Node Environment
    "COGNITO\_USER\_POOL\_ID": "", # Cognito User Pool ID
    "COGNITO\_CLIENT\_ID": "", # Cognito Client ID
    "SENTRY\_DSN": "", # Sentry DSN for error tracking
    "AUTH\_URL": "", # Authentication URL for public access
    "RAFIKI\_GRAPHQL\_URL": "", # Rafiki GraphQL URL
    "DOMAIN\_WALLET\_URL": "", # Wallet URL for public access
    "BACKEND\_API\_SIGNATURE\_VERSION": "1", # Backend API Signature Version
    "BACKEND\_API\_SIGNATURE\_SECRET": "", # Backend API Signature
    "SQS\_QUEUE\_URL": "", # SQS Queue URL for sending email notifications
    "WALLET\_WG\_URL": "", # Wallet WG URL
    "CRON\_TIME\_EXPRESSION": "1 0 1 * *", # Cron Time
    "API\_SECRET\_SERVICES": "", # API Secret Services
    "WS\_URL": "", # WS URL for public access
    "SIGNATURE\_URL": "https://kxu5d4mr4blcthphxomjlc4xk40rvdsx.lambda-url.eu-central-1.on.aws/" # Signature URL for public access
    }
```
###### 4. Set Up Environment Variables

Create a .env file in the root directory and add:
```sh
    AWS\_ACCESS\_KEY\_ID=""
    AWS\_SECRET\_ACCESS\_KEY=""
    SECRET\_NAME="walletguru-wallet-local"
```
### Step 6 CountriesNow API

A curation of Countries data including (dial codes, states, cities, currencies, capitals etc) served over a REST API so you don't have to have them locally in your applications. This means lighter application sizes as you wouldn't have to install another package to use geo data. Please note that this doesn't guarantee complete or correct data, feel free to raise issues where necessary.

#### Usage

The API does not require any form of Authentication or token.
```javascript
    const BASE\_URL = 'https://countriesnow.space/api/v0.1/countries'
    let getCountries = async () => {
    const response = await fetch(`${BASE\_URL}`).then(response => response.json())
    const { data } = response
    data.forEach((country) => {
        console.log(country) // {"country": "Afghanistan", "cities": [ "Herat", "Kabul", "Kandahar", "Molah", ...]}
        })
    }
```

The API does not require any form of Authentication or token.

- Read Docs on [Postman](https://documenter.getpostman.com/view/1134062/T1LJjU52?version=latest)
- Try it out on [Swagger](https://countriesnow.space/swagger-docs)
- Check out the [Example Applications](https://github.com/MartinsOnuoha/countriesNow-Demo-Apps)

#### Local Setup
```sh
    git clone https://github.com/YourName/countriesNowAPI.git
    cd countriesNowAPI
```
##### Install packages
```sh
    npm i
```
##### Start Project
```sh
    npm start
```
##### Run test
```sh
    npm run integration:test
```
### Step 7 Custom codes Microservice

This microservice is responsible for user custom codes using Node.js and NestJS as the development framework, DynamoDB as the NoSQL database, and Dynamoose as the ORM for interaction with DynamoDB.

#### Dependencies

- Node.js (v14 or higher)
- NestJS (v7 or higher)
- AWS DynamoDB
- Dynamoose (v2 or higher)
- AWS SDK for Node.js
- Deploy services with Terraform

#### Install

##### 1. Clone the Repository
```sh
    git clone https://github.com/WalletGuruLLC/wg-backend-codes.git
    cd wg-backend-codes
```
##### 2. Install Dependencies
```sh
    npm install
```
##### 3. Set Up Environment Variables

Create a .env file in the root directory and add:
```sh
    AWS\_KEY\_ID=
    AWS\_SECRET\_ACCESS\_KEY=
    AWS\_REGION=
```
###### 4. Run the Application

Using Docker Compose:
```sh
docker-compose up
```

### Step 7 Cron Service
#### Dependencies
- Node.js (v14 or higher)
- NestJS (v7 or higher)
- AWS DynamoDB
- Dynamoose (v2 or higher)
- AWS SDK for Node.js

#### Installation
```sh
npm install
```

#### Configuration
##### Set up the environment variables
    Create a .env file in the root of the project following the content of .env.example.
##### Running the Application
```sh
    npm run start
```
##### Envs for pipeline
- AWS_KEY_ID: Key ID of the AWS account
- AWS_SECRET_ACCESS_KEY: Secret key of the AWS account
- SECRET_NAME: Name of the secret in AWS Secrets Manager

## Configuration Details (local.tfvars Variables)

| Name of env                    | Description                                                                                                                                                                                                 | Instrucction                                                                                                                                                                                                                                | Required | Default                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|--------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| aws_access_key                 | AWS Access Key                                                                                                                                                                                              | Go to aws iam and create a new user with name **terraform** and set permission **AdministratorAccess**, in security credentials crea a new access key with Command Line Interface (CLI) option, now you have this env **Access key**        | Yes      | AKIA6GB...                                                                                                                                                                                                                                                                                                                                                                                                                              |
| aws_secret_key                 | AWS Secret Key                                                                                                                                                                                              | Go to aws iam and create a new user with name **terraform** and set permission **AdministratorAccess**, in security credentials crea a new access key with Command Line Interface (CLI) option, now you have this env **Secret access key** | Yes      | GeAqIRagq9...                                                                                                                                                                                                                                                                                                                                                                                                                           |
| aws_region                     | AWS Region to deploy resources                                                                                                                                                                              | Set region to deploy resources                                                                                                                                                                                                              | Yes      | us-east-2                                                                                                                                                                                                                                                                                                                                                                                                                               |
| aws_account_id                 | AWS Account ID                                                                                                                                                                                              | Get account id in the next link https://us-east-1.console.aws.amazon.com/billing/home?region=us-east-2#/account                                                                                                                             | Yes      | 975050359999                                                                                                                                                                                                                                                                                                                                                                                                                            |
| aws_access_key_shared          | AWS Access Key its used only for access to secret manager, can use **aws_access_key** if you use the same account of aws                                                                                    | Go to aws iam and create a new user with name **terraform** and set permission **AdministratorAccess**, in security credentials crea a new access key with Command Line Interface (CLI) option, now you have this env **Access key**        | Yes      | AKIA6GB...                                                                                                                                                                                                                                                                                                                                                                                                                              |
| aws_secret_key_shared          | AWS Secret Key its used only for access to secret manager, can use **aws_secret_key** if you use the same account of aws                                                                                    | Go to aws iam and create a new user with name **terraform** and set permission **AdministratorAccess**, in security credentials crea a new access key with Command Line Interface (CLI) option, now you have this env **Secret access key** | Yes      | GeAqIRagq9...                                                                                                                                                                                                                                                                                                                                                                                                                           |
| aws_region_shared              | AWS Region to deploy secret manager                                                                                                                                                                         | Set region to deploy resources                                                                                                                                                                                                              | Yes      | us-east-2                                                                                                                                                                                                                                                                                                                                                                                                                               |
| public_key                     | Public key for entry in any instance of ec2                                                                                                                                                                 | Paste your ssh public key                                                                                                                                                                                                                   | Yes      | ssh-rsa AAAAB3NzaC1y                                                                                                                                                                                                                                                                                                                                                                                                                    |
| repos_list                     | List of repositories create in ECR (elastic container registry)                                                                                                                                             | Set a list with name of repositories for create in ECR                                                                                                                                                                                      | No       | ["backend-auth", "backend-notification", "frontend-admin", "backend-wallet","ws]                                                                                                                                                                                                                                                                                                                                                        |
| dynamo_tables                  | List of tables created in DynamoDb service of aws                                                                                                                                                           | Set a list with object configuration of tables in DynamoDb, name is the name of table, billing_mode is the type of billing of the table, read_capacity and write_capacity is the capacity of the table for read and write objects           | No       | ```[ { name                        = "Attempts" billing_mode                = "PROVISIONED" read_capacity               = 5 write_capacity              = 5 hash_key                    = "Id" range_key                   = "" deletion_protection_enabled = true, attributes = [   {     name = "Id"     type = "S"   } ], ttl = [], global_secondary_index = [], tags = {   Name        = "Attempts"   Environment = "dev" } }, ]``` |
| name_queue                     | Name of the queue used in SQS service of aws                                                                                                                                                                | Set a name of SQS used for notification                                                                                                                                                                                                     | Yes      | paystreme-notifications-local                                                                                                                                                                                                                                                                                                                                                                                                           |
| s3_buckets                     | List of names of buckets created for save files                                                                                                                                                             | Set a list with names of s3 buckets                                                                                                                                                                                                         | Yes      | ```{ name = "bucket-dev"}]```                                                                                                                                                                                                                                                                                                                                                                                                           |
| create_domain                  | Bool for create aws_route53_zone in route53 service                                                                                                                                                         | Set true if you need create aws_route53_zone                                                                                                                                                                                                | Yes      | False                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| zone_id                        | Zone id of aws_route53_zone create [How create a hosted zone](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/CreatingHostedZone.html)                                                            | Set id of aws_route53_zone for send emails                                                                                                                                                                                                  | Yes      | Z00522293EO3PH12345                                                                                                                                                                                                                                                                                                                                                                                                                     |
| email_configuration_source_arn | Id of ses for send emails [How create a configuration ses](https://docs.aws.amazon.com/ses/latest/dg/creating-configuration-sets.html#:~:text=To%20create%20a%20configuration%20set,Choose%20Create%20set.) | Set id of ses configuration for send emails                                                                                                                                                                                                 | Yes      | arn:aws:ses:us-east-2:975050359999:identity/YOUR_DOMAIN                                                                                                                                                                                        

## Microservices List

| **Microservice**                                | **Repository URL**                                               |
|-------------------------------------------------|------------------------------------------------------------------|
| Authentication Service (`backend-auth`)         | [GitHub Repo](https://github.com/WalletGuruLLC/wg-backend-auth)     |
| Notification Service (`backend-notification`)   | [GitHub Repo](https://github.com/WalletGuruLLC/wg-backend-notification)  |
| Admin Frontend (`frontend-admin`)               | [GitHub Repo](https://github.com/WalletGuruLLC/wg-frontend)   |
| Wallet Service (`backend-wallet`)               | [GitHub Repo](https://github.com/WalletGuruLLC/wg-backend-wallet)   |
| Countries Now Service (`backend-countries-now`) | [GitHub Repo](https://github.com/WalletGuruLLC/wg-countries-now) |
| Codes Service (`backend-codes`)                 | [GitHub Repo](https://github.com/WalletGuruLLC/wg-backend-codes) |
| Cron Service (`backend-codes`)                  | [GitHub Repo](https://github.com/WalletGuruLLC/wg-cron) |
