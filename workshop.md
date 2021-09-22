# Workshop hands-on session

## Step 1: create a free account on Hasura Cloud and YugabyteDB cloud


- Create a [Yugabyte Cloud Instance](https://www.yugabyte.com/cloud/)
- Createa a [Hasura Cloud Instance](https://cloud.hasura.io/signup?pg=products&plcmt=body&cta=get-started-for-free&tech=default)

## Step 2: Configure Hasura Cloud instance to use YugabyteDB Cloud Instance

1) Deploy GraphQL Engine on Hasura Cloud and setup Yugabyte Cloud Instance:
  
  [![Deploy to Hasura Cloud](https://graphql-engine-cdn.hasura.io/img/deploy_to_hasura.png)](https://cloud.hasura.io/)

2) Get the Hasura app URL (say `hasura-yb-demo.hasura.app`)

## Step 3: Clone the workshop repo

  ```bash
  git clone https://github.com/yugabyte/yugabyte-graphql-apps
  cd realtime-poll
  ```

## Step 4: Run the migrations for setting the database schema

[Install Hasura CLI](https://hasura.io/docs/latest/graphql/core/hasura-cli/install-hasura-cli.html)

- Goto `hasura/` and edit `config.yaml`:

  ```yaml
  endpoint: https://hasura-yb-demo.hasura.app
  ```
- Apply the migrations:

  ```bash
  hasura migrate apply --database-name yugabyte-cloud-instance
  ```

## Step 5: Verify the setup

  - Navigate to Hasura GraphiQL console
  - Run the GraphQL Mutation and Queries present in [GraphQL.js](./src/GraphQL.js) file.

## Step 5: Update the nodejs application to make use of Hasura cloud instance configured with YugabyteDB

  Edit `HASURA_GRAPHQL_ENGINE_HOSTNAME` and `hasura_secret` in `src/apollo.js` and set it to the
  Hasura app URL:

  ```js
  export const HASURA_GRAPHQL_ENGINE_HOSTNAME = 'hasura-yb-demo.hasura.app';
  const hasura_secret = '';
  ```

  Note: Obtain `hasura_secret` from Hasura Cloud Console.

## Step 6: Run the app

go the root of repo:

  ```bash
  npm install
  npm start
  ```

