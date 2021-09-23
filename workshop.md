# Workshop hands-on session

## Step 1: create a free account on Hasura Cloud and YugabyteDB cloud


- Create a [Yugabyte Cloud Instance](https://www.yugabyte.com/cloud/)
- Create a [Hasura Cloud Instance](https://cloud.hasura.io/signup?pg=products&plcmt=body&cta=get-started-for-free&tech=default)
- Add `Hasura Cloud IP` in whitelist ip-address of Yuagbyte Cloud instance for enabling access to respective ip-addresses.

## Step 2: Configure Hasura Cloud instance to use YugabyteDB Cloud Instance

  a) Deploy GraphQL Engine on Hasura Cloud and connect to Yugabyte Cloud Instance using `Connect Exisiting Database`

   - create database connection with the display name `yugabyte-cloud-instance`
   - Retrive the hostname of Yugabyte cloud instance
   - Retrive the credentials of Yugabyte cloud instance
   - Build the connection string for yugabytedb 

  Connection String Format:

    ```yaml
    postgresql://admin:password@hostname:5433/yugabyte?ssl=true&sslmode=require
    ```

  Connection String Example:

     ```
     postgresql://admin:xxxxxxx%21%23YJCRp9%403@c0aef75b-6889-4c86-8aa7-xxxxxxxxx.aws.ybdb.io:5433/yugabyte?ssl=true&sslmode=require
     
     ```

     Note: Special charecters in the username/password needs to be encoded

  b) Get the Hasura app URL (say `hasura-yb-demo.hasura.app`)

## Step 3: Clone the workshop repo

  ```bash
  git clone https://github.com/yugabyte/yugabyte-hasura-cloud-workshop
  cd yugabyte-hasura-cloud-workshop
  ```

## Step 4: Run the migrations for setting the database schema

- [Install Hasura CLI](https://hasura.io/docs/latest/graphql/core/hasura-cli/install-hasura-cli.html)

- Goto `hasura/` directory in the project and edit `config.yaml`:

  Update the `endpoint` and `admin_secret` values from the respective Hasura cloud instance.

  ```yaml
  endpoint: https://hasura-yb-demo.hasura.app
  admin_secret: hasura_instance_secret
  ```
- Apply the migrations:

  ```bash
  hasura migrate apply --database-name yugabyte-cloud-instance
  ```


## Step 5: Track the all the tables and relationships

   a. Track all the tables and relationship from `data` tab of Hasura console

   b. Additionaly we need to manually add a Array relationship for `poll_results` 

   Relationalship name `option` on `poll_results` table referencing `poll_id` to `option.poll_id`

   ```
   (option - poll_results . poll_id  → option . poll_id)
   ```
   
## Step 6: Verify the setup

  - Navigate to Hasura GraphiQL console
  - Run the GraphQL Mutation and Queries present in [GraphQL.js](./src/GraphQL.js) file.

## Step 7: Update the nodejs application to make use of Hasura cloud instance configured with YugabyteDB

  Edit `HASURA_GRAPHQL_ENGINE_HOSTNAME` and `hasura_secret` in `src/apollo.js` and set it to the
  Hasura app URL:

  ```js
  export const HASURA_GRAPHQL_ENGINE_HOSTNAME = 'hasura-yb-demo.hasura.app';
  const hasura_secret = '';
  ```

  Note: Obtain `hasura_secret` from Hasura Cloud Console.

## Step 8: Run the app

go the root of repo:

  ```bash
  npm install
  npm start
  ```

