<h1 align="center" style="border-bottom: none;">🚀 CodeMash</h1>
<h3 align="center">Back end as a service</h3>

[CodeMash](https://codemash.io) is a vital toolset for each developer who wants to achieve daily development tasks rapidly. CodeMash provides many common back-end services for you so you can focus on your front-end. Services such as database, email and push notifications, authentication, file storage, and [many others](https://docs.codemash.io/dashboard/register-at-codemash) are already implemented and can be easily accessed through the CodeMash dashboard or API.

> Please refer to our technical [documentation](https://docs.codemash.io) page for more information.

## CodeMash + AWS SAM (NodeJs + TypeScript)

![node-current](https://img.shields.io/badge/Node-%3E=20-success?style=for-the-badge&logo=node)

For other programming languages, please refer to our other templates

1. [Node.js](https://github.com/codemash-io/CodeMash.Serverless.AWS.SAM.NodeJs-TS)
2. [Node.js + TypeScript](https://github.com/codemash-io/CodeMash.Serverless.AWS.SAM.NodeJs-TS)
3. [Python](https://github.com/codemash-io/CodeMash.Serverless.AWS.SAM.Python)
4. [.NET](https://github.com/codemash-io/CodeMash.Serverless.AWS.SAM.NET)
5. [Go](https://github.com/codemash-io/CodeMash.Serverless.AWS.SAM.Go)
6. [Ruby](https://github.com/codemash-io/CodeMash.Serverless.AWS.SAM.Ruby)
7. [Java](https://github.com/codemash-io/CodeMash.Serverless.AWS.SAM.Java)

### Installation

You can initiate this SAM template with the following command. If you have already done so, please skip this command and go to the configuration section.

```bash
sam init --location gh:codemash-io/CodeMash.Serverless.AWS.SAM.NodeJs-TS
```

### Configuration

In order to avoid having to remember the deploy command ...

```bash
sam deploy \
  --tags "codemash:account-id=090f4408-30ef-4a8d-8f99-9d97b174d952 codemash:project-id=a81050d8-170a-43bb-8099-a9ac53861b86" \
  --s3-bucket cm-a81050d8-170a-43bb-8099-a9ac53861b86 \
  --region eu-central-1
```

... and its parameters every time you deploy, you can set settings to the samconfig.toml file.

#### samconfig.toml

You can obtain CodeMash project id and account id from [here](https://docs.codemash.io/api/prerequisites)

```bash
[default.deploy.parameters]
stack_name = "codemash" # change if such stack has already been set before
s3_bucket = "cm-{_your_project_id_}" # change to yours
s3_prefix = "cm-lambda-deployments"
region = "us-west-2"
tags = "codemash:account-id={_your_account_id_},codemash:project-id={_your_project_id_}" # change to yours
capabilities = "CAPABILITY_IAM"
resolve_s3 = false
confirm_changeset = true
```

If you have specific environments defined like `dev`, `prod` you can use e.g.: `[prod.default.deploy.parameters]` or `[dev.default.deploy.parameters]`

- **Tags**: You need to pass the account ID and project ID to the tags. That's how CodeMash can find lambda functions dedicated to CodeMash.

- **S3 Bucket**: The deployment bucket facilitates lambda deployments. You can specify the `s3-bucket` parameter as `cm-{projectId}`. For example, if your project ID is `a81050d8-170a-43bb-8099-a9ac53861b86`, you would specify `s3-bucket = cm-a81050d8-170a-43bb-8099-a9ac53861b86`. Alternatively, you can set `resolve_s3` to `true` to have dynamically picked S3 bucket for your deployments.

- **Region**: Specify the region where the lambda will be deployed. Consider where the CodeMash cluster resides for better performance and lower costs.

#### template.yaml

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

## Deploy

```bash
sam deploy
```

## Examples

Examples are provided using the [TypeScript](https://docs.codemash.io/sdk/typescript) SDK, but you can call it from any other supported SDK or just make a regular API call. See the documentation [here](https://docs.codemash.io/sdk).

#### Call function from code:

Let's say you have deployed a Lambda function that calculates the final price of the service or product you are selling.
Assume your Lambda function is integrated with CodeMash and has the ID A3282587-1E69-4CF1-9610-201CC0640BF8.
The Lambda code looks something like this:

```ts
import { code } from 'codemash';

async function calculatePrice(request) {
  const response = await code.executeFunction({
      functionId: "A3282587-1E69-4CF1-9610-201CC0640BF8",
      data: { ...request },
  });
  return response;
});

const priceResponse = await calculatePrice(
  {
    basePrice: 20.15,
    calculateVat: true,
    includeFee: false,
    country: "lt"
  });
```

This function examines the CodeMash taxonomies and retrieves the VAT rate based on the country code you provide.
If a fee needs to be calculated, it retrieves it from the environment variables defined for the function, or sets it to a default of 10%.
The function then returns the final price calculated.

Let's call that function from your app (mobile app, web app, script, etc.).

```ts
import { code } from 'codemash';

async function calculatePrice(request) {
  const response = await code.executeFunction({
      functionId: "A3282587-1E69-4CF1-9610-201CC0640BF8",
      data: { ...request },
  });
  return response;
});

const priceResponse = await calculatePrice(
  {
    basePrice: 20.15,
    calculateVat: true,
    includeFee: false,
    country: "lt"
  });
```

#### Call function from Membership trigger:

TODO: //

#### Call function from Database trigger:

TODO: //

#### Call function from Files trigger:

TODO: //

#### Call function from Payments trigger:

TODO: //

## 👉🏼 Hands-On

Run [tests](./CONTRIBUTING.md#tests) and see how it works

## Get help

- [GitHub Discussions](https://github.com/codemash-io/CodeMash.Js/discussions)
- [Twitter](https://twitter.com/codemash_io)

## License

CodeMash.Js is licensed under [Apache 2.0 License.](LICENSE). CodeMash.Js comes with an explicit [NOTICE](notice) file containing additional legal notices and information.
