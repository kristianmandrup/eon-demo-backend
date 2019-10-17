# E-ON push notification PWA demo

## Serverless

Install serverless CLI

```sh
$ npm i -g serverless
```

### AWS

Install AWS CLI

```sh
$ pip3 install awscli --upgrade --user
```

Create serverless node starter project:

```js
serverless install --url https://github.com/AnomalyInnovations/serverless-nodejs-starter --name my-project
```

Alternatively use a `serverless` template

```sh
$ serverless create --template aws-nodejs --path serverless-aws-nodejs
```

See [Serverless Framework: Deploy an HTTP endpoint using NodeJS, Lambda on AWS](https://itnext.io/serverless-framework-deploy-an-http-endpoint-using-nodejs-lambda-on-aws-30558422de1b)

See [Deploy a REST API using Serverless, Express and Node.js](https://serverless.com/blog/serverless-express-rest-api/)

## Services

See [Serverless services](https://serverless.com/framework/docs/providers/aws/guide/services/)

## Cognito user

### Create User

First, we will use AWS CLI to sign up a user with their email and password.

```sh
$ aws cognito-idp sign-up \
  --region YOUR_COGNITO_REGION \
  --client-id YOUR_COGNITO_APP_CLIENT_ID \
  --username admin@example.com \
  --password Passw0rd!
```

Now, the user is created in Cognito User Pool.

### Verify user

Before the user can authenticate with the User Pool, the account needs to be verified. Let’s quickly verify the user using an administrator command.

In your terminal, run.

```sh
$ aws cognito-idp admin-confirm-sign-up \
 --region YOUR_COGNITO_REGION \
 --user-pool-id YOUR_COGNITO_USER_POOL_ID \
 --username admin@example.com
```

Now our test user is ready.

## Install and Run

### Installing API dependencies

```shell script
npm i
```

## Test locally

### Unit tests

See [Unit Tests in Serverless](https://serverless-stack.com/chapters/unit-tests-in-serverless.html)

#### Create a New AWS Profile

See [Multiple AWS profiles](https://serverless-stack.com/chapters/configure-multiple-aws-profiles.html)

Follow the steps outlined in the Create an IAM User chapter to create an IAM user in another AWS account and take a note of the Access key ID and Secret access key.

To configure the new profile in your AWS CLI use:

```sh
$ aws configure --profile newAccount
```

### Create note

Invoke locally

```sh
$ serverless invoke local --function create --path mocks/create-event.json
```

If you have multiple profiles for your AWS SDK credentials, you will need to explicitly pick one.

```sh
$ AWS_PROFILE=myProfile serverless invoke local --function create --path mocks/create-event.json
```

### Get note

Invoke locally

```sh
$ serverless invoke local --function get --path mocks/get-event.json
```

### Update note

Invoke locally

```sh
$ serverless invoke local --function update --path mocks/update-event.json
```

## Deployment

This is using the serverless framework. Deploys to dev stage.

```shell script
serverless deploy --aws-profile <AWS_PROFILE>
```

Serverless will auto-generate endpoints on an API Gateway following REST conventions

```sh
Service Information
service: notes-app-api
stage: prod
region: us-east-1
api keys:
  None
endpoints:
  POST - https://ly55wbovq4.execute-api.us-east-1.amazonaws.com/prod/notes
  GET - https://ly55wbovq4.execute-api.us-east-1.amazonaws.com/prod/notes/{id}
  GET - https://ly55wbovq4.execute-api.us-east-1.amazonaws.com/prod/notes
  PUT - https://ly55wbovq4.execute-api.us-east-1.amazonaws.com/prod/notes/{id}
  DELETE - https://ly55wbovq4.execute-api.us-east-1.amazonaws.com/prod/notes/{id}
  POST - https://ly55wbovq4.execute-api.us-east-1.amazonaws.com/prod/billing
functions:
  create: notes-app-api-prod-create
  get: notes-app-api-prod-get
  list: notes-app-api-prod-list
  update: notes-app-api-prod-update
```

In our case, `us-east-1` is our API Gateway Region and `ly55wbovq4` is our API Gateway ID.

## Environments

Serverless doesn’t change how you setup long lived stages. You still have the usual `dev` stage, `prod` stage.
You also have the intermediate stages such as `staging`, `qa`, `preprod`, etc.
