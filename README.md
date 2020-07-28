# notes-app-serverless

Identity pool ID 
eu-central-1:02be0678-bca9-4424-8d48-ad6d01c4553b

## Deploy

Run the following in your working directory.

```serverless deploy```

If you have multiple profiles for your AWS SDK credentials, you will need to explicitly pick one. Use the following command instead:

```serverless deploy --aws-profile myProfile```

Where myProfile is the name of the AWS profile you want to use. If you need more info on how to work with AWS profiles in Serverless, refer to our Configure multiple AWS profiles chapter.

```
Service Information
service: notes-app-api
stage: prod
region: eu-central-1
stack: notes-app-api-prod
resources: 39
api keys:
  None
endpoints:
  POST - https://kgazkvyv5h.execute-api.eu-central-1.amazonaws.com/prod/notes
  PUT - https://kgazkvyv5h.execute-api.eu-central-1.amazonaws.com/prod/notes/{id}
  DELETE - https://kgazkvyv5h.execute-api.eu-central-1.amazonaws.com/prod/notes/{id}
  POST - https://kgazkvyv5h.execute-api.eu-central-1.amazonaws.com/prod/billing
functions:
  create: notes-app-api-prod-create
  get: notes-app-api-prod-get
  list: notes-app-api-prod-list
  update: notes-app-api-prod-update
  delete: notes-app-api-prod-delete
  billing: notes-app-api-prod-billing
layers:
  None

**************************************************************************************************************************************
Serverless: Announcing Metrics, CI/CD, Secrets and more built into Serverless Framework. Run "serverless login" to activate for free..
**************************************************************************************************************************************

```

## Deploy a single function

```
There are going to be cases where you might want to deploy just a single API endpoint as opposed to all of them. The serverless deploy function command deploys an individual function without going through the entire deployment cycle. This is a much faster way of deploying the changes we make.

For example, to deploy the list function again, we can run the following.

```serverless deploy function -f list```

Now before we test our APIs we have one final thing to set up. We need to ensure that our users can securely access the AWS resources we have created so far. Let’s look at setting up a Cognito Identity Pool.
```

## Test auth https://serverless-stack.com/chapters/test-the-apis.html

```
$ npx aws-api-gateway-cli-test \
--username='admin@example.com' \
--password='Passw0rd!' \
--user-pool-id='eu-central-1_qLIMKoQp8' \
--app-client-id='79es7gqs03r271c4rph688emu9' \
--cognito-region='eu-central-1' \
--identity-pool-id='eu-central-1:02be0678-bca9-4424-8d48-ad6d01c4553b' \
--invoke-url='https://kgazkvyv5h.execute-api.eu-central-1.amazonaws.com/prod/' \
--api-gateway-region='eu-central-1' \
--path-template='notes' \
--method='POST' \
--body='{"content":"hello world","attachment":"hello.jpg"}'
```