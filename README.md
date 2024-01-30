# CODEMASH

Read official AWS SAM documentation in [README.AWS.md](README.AWS.md)

### Init

You can initiate this SAM template with the following command. If you have already done so, please skip this command and go to the build section.

```bash
sam init --location gh:yourusername/my-sam-template
```

## Build

Every time you update the code, you need to rebuild it.

```bash
sam build
```

## Develop

Here are examples how you can call Lambda function from CodeMash

- Write function and call it using any SDK
- Call function from trigger
  - Membership
  - Database
  - Files
  - Payments

## Deploy

Before deploying, it's crucial to get your CodeMash account ID and project ID.
Please refer to our documentation [here](https://docs.codemash.io/api/prerequisites) in order to obtain them.

- **Tags**: You need to pass the account ID and project ID to the tags. That's how CodeMash can find lambda functions dedicated to CodeMash.

- **S3 Bucket**: The deployment bucket facilitates lambda deployments. You can specify the `--s3-bucket` parameter as `cm-{projectId}`. For example, if your project ID is `a81050d8-170a-43bb-8099-a9ac53861b86`, you would specify `--s3-bucket cm-a81050d8-170a-43bb-8099-a9ac53861b86` in the `sam deploy` command. Alternatively, you can set `resolve_s3` to `true` in the `samconfig.toml` file.

- **Region**: Specify the region where the lambda will be deployed. Consider where the CodeMash cluster resides for better performance and lower costs.

Please refer to our documentation [here](https://docs.codemash.io/api/prerequisites) for more information.

Reg

```bash
sam deploy
  --tags "codemash:accountid=090f4408-30ef-4a8d-8f99-9d97b174d952 codemash:projectid=a81050d8-170a-43bb-8099-a9ac53861b86"
  --s3-bucket cm-a81050d8-170a-43bb-8099-a9ac53861b86
  --region eu-central-1
```
