# Postgrest on Render

## Deploy

[![Deploy to Render](https://render.com/images/deploy-to-render-button.svg)](https://render.com/deploy?repo=https://github.com/render-examples/postgrest)

Click this button to deploy [Postgrest](https://postgrest.org/en/stable/) to Render's free web service and database plans.

## Example Configuration

_The following steps are derived from [Step 4 of the Postgrest's Tutorial 0](https://postgrest.org/en/stable/tutorials/tut0.html#step-4-create-database-for-api)_

1. Grab the external connection string for the database you just created on Render.
1. Locally, run `psql <connectionstring>`, where `<connectionstring>` is the connection string from the previous step.
1. Create an example table:

    ```sql
    create table todos (
      id serial primary key,
      done boolean not null default false,
      task text not null,
      due timestamptz
    );
    ```

1. Insert some rows into the table:

    ```sql
    insert into todos (task) values
      ('finish tutorial 0'), ('pat self on back');
    ```

1. Exit psql with `\q`
1. Open the URL of the Postgrest web service you just deployed. It is on the top of the Render dashboard page for the service. You should see the OpenAPI documentation for your new API.
2. Now add `/todos` to the end of the URL to use the API endpoint that was automatically created from the `todos` table. It should return the two rows of data you added to the table.

## Caveats

- This deploy makes the PostgreSQL database accessible to the public internet. This can be changed by updating the `ipAllowList` in the repo's `render.yaml` or by making [similar changes](https://render.com/docs/databases#access-control) in the Render Dashboard. Deleting all access control rules will make the database accessible only from other services in your Render account, including the Postgrest web service you just deployed.
- This deploy also uses the PostgreSQL admin user for all database queries. The Postgrest project recommends creating a new named schema and role for database objects which will be exposed in the API. See [Step 4. Create Database for API](https://postgrest.org/en/stable/tutorials/tut0.html#step-4-create-database-for-api) for an example of how to do this. Review [Postgrest's documentation on auth](https://postgrest.org/en/stable/auth.html) to understand how it manages authentication and authorization.