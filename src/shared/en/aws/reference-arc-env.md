# `npm run env`

Read and write environment variables that are made immediately available to all deployed Lambda functions. Sensitive configuration data, such as API keys, needs to happen _outside_ of the codebase in revision control and you can use this tool to ensure an entire team and the deployment targets are in sync. 

To use `npm run env`, add the following to your project's `package.json` scripts:

```json
{
  "scripts": {
    "env": "AWS_PROFILE={profile} AWS_REGION={region} arc-env"
  }
}
```

> Reminder: All `arc` npm run scripts require `AWS_PROFILE` and `AWS_REGION` environment variables set. Learn more in the [Prerequisites guide](/quickstart).

## Example Usage

- `npm run env` displays environment variables for the current `.arc`
- `npm run env staging FOOBAZ somevalue` writes env variable `FOOBAZ=somevalue` to staging Lambdas
- `npm run env remove testing FOOBAZ` removes a `testing` env var
- `npm run env verify` display a report of Lambdas and their env variables

Things to note

- Adding and removing variables automatically syncs all lambdas and the current working directory `.arc-env`
- `NODE_ENV`, `ARC_APP_NAME` and `SESSION_TABLE_NAME` are reserved
- There is no performance impact to your app; these variables are synchronized every write and immediately available to all Lambdas defined by the current `.arc` file

> Currently `.arc` uses AWS Systems Manager Parameter Store as a centralized backing storage mechanism for app environment variables. [Read more about AWS Systems Manager Parameter Store.](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-paramstore.html)

## The `.arc-env` File

This file **should not** be commited into your project git repository.

- Generated by adding or removing env vars using `npm run env`
- Automatically read by `arc-sandbox` and populates `process.env` when `npm start` runs locally
- Lists env vars for `testing` or `staging` for local dev

This is an example file:

```arc
# example .arc-env
@testing 
GLOBAL asdfasdf

@staging
GLOBAL_KEY val
```