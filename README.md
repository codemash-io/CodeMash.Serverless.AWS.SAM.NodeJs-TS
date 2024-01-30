# CODEMASH

Read official AWS SAM documentation in [README.AWS.md](README.AWS.md)

### Init

You can initiate this SAM template with the following command. If you have already done so, please skip this command and go to the build section.

```bash
sam init --location gh:codemash-io/CodeMash.Serverless.AWS.SAM.NodeJs-TS
```

### CodeMash setup

Change CodeMash environment variables in [template.yaml](template.yaml) like `CODEMASH_API_URL`, `CODEMASH_PROJECT_ID`, and `CODEMASH_API_KEY` with your personal credentials that you obtain from the CodeMash Cloud dashboard. Refer to [this link](https://docs.codemash.io/api/prerequisites) for more information.

```yaml
Globals:
  Function:
    Runtime: nodejs20.x
    MemorySize: 128
    Timeout: 180
    Architectures:
      - arm64
    AutoPublishAlias: live
    CodeUri: src/
    Environment:
      Variables:
        STAGE: Production
        CODEMASH_API_URL: https://api.codemash.io/v2 # change to yours personal url
        CODEMASH_PROJECT_ID: _your_project_id_ # change to yours personal project id
        CODEMASH_API_KEY: _your_api_key_ # change to yours personal api key
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
sam deploy \
  --tags "codemash:accountid=090f4408-30ef-4a8d-8f99-9d97b174d952 codemash:projectid=a81050d8-170a-43bb-8099-a9ac53861b86" \
  --s3-bucket cm-a81050d8-170a-43bb-8099-a9ac53861b86 \
  --region eu-central-1
```
