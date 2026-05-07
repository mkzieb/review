## API management

API management lets you create and manage service accounts that applications, scripts, and integrations can use to access data in your workspace through platform APIs.

Use API management when you want to:
- connect an external application or script to workspace data
- automate data access or ingestion
- give a system access to selected data sets or to the full workspace
- generate credentials for API authentication

## Key concepts

### Service account

A service account is a non-human account used by an application, script, or integration to access APIs. Instead of signing in as a person, the system uses service account credentials to authenticate API requests.

Service accounts are typically used for automated integrations, backend services, scripts, and scheduled jobs.

### Credentials

When you create a service account, the platform generates credentials such as the service account ID, service account secret, and API key. Your application or script uses these values to authenticate and call platform APIs.

Store these credentials securely and do not share them with users who do not need access.

### Service account secret

The service account secret works like a password for the service account. You can see it only once, so copy it immediately and store it securely.

Service account secrets expire 2 years after creation. You will not be notified automatically before a secret expires.

### API key

The API key identifies the API subscription or access context required by the platform APIs. It must be included in API requests where required.

### Base URL

The base URL is the starting address for API calls from your workspace. Your application combines this base URL with specific API endpoints to call platform APIs.

## Platform APIs and authentication

Platform APIs require authentication. For automated integrations, scripts, and backend services, you normally use a service account with the OAuth 2.0 client credentials flow.

The platform also supports the authorization code flow for scenarios where an individual user signs in and grants access.

To use the client credentials flow, you must create a service account.

## Decide the access scope for your service account

When you create a service account, you must decide whether to grant it access to all workspace data or only to specific data sets.

Use **Grant all workspace data** when the integration needs broad workspace access, such as reading multiple data sets, writing data, or calling workspace-level APIs.

Use **Select data sets manually** when the integration should access only specific data sets. This is more restrictive, but it also means the service account cannot call workspace-level or tenant-level APIs.

## To add a new service account

To add a new service account:

1. In your workspace, select the **API management** tab.
2. In the left sidebar, select **Add account**.
3. Under **Account name**, enter the name for the account.
4. Under **Contact email**, enter the contact email for the owner of this service account.
5. Under **Access to data sets**, choose one of the following:
   - **Grant all workspace data** - Grants access to all workspace data. You must also set the **Workspace access level** to either **Reader** or **Admin**.
   - **Select data sets manually** - Lets you share specific data sets with the account.
6. If you selected **Grant all workspace data**, under **Workspace access level**, choose whether the account should have **Reader** or **Admin** access. For details, see [Workspace access level](#workspace-access-level).
7. Select the **Create service account** button.

After the service account is created, you will get the values for the service account secret, service account ID, and API key. You will need them to [authenticate API calls](82827b64-e07a-4fd2-9685-ad3c57f73615).

**Important**:
- You will see the service account secret only once. Copy it and store it securely.
- Service account secrets expire 2 years after creation and you will not be notified automatically. Plan to regenerate the secret before expiry to avoid authentication failures, such as `HTTP 401`.

### Workspace access level

The workspace access level defines what the service account can do in the workspace.

- **Reader** - View-only access to the workspace. The service account can read data but cannot invite users, connect services, or modify content or settings.
- **Admin** - Full access to manage the workspace. The service account can create and modify data sets and files, invite users, manage permissions, and adjust settings and content.

## To copy the base URL endpoint

To copy the base URL endpoint of a service account:

1. In your workspace, select the **API management** tab.
2. In the left sidebar, under **Service accounts**, select an account.
3. In the row under **Endpoints base URL**, select **Copy**.

## To update a service account's name

To update the name of a service account:

1. In your workspace, select the **API management** tab.
2. In the left sidebar, under **Service accounts**, select an account.
3. Under **Account name**, update the name.
4. Select the **Save** button.

## To update a service account's contact email

To update the contact email of a service account:

1. In your workspace, select the **API management** tab.
2. In the left sidebar, under **Service accounts**, select an account.
3. Under **Contact email**, update the email.
4. Select the **Save** button.

## To update shared data sets

For a service account with **Select data sets manually** enabled, you can update the data sets shared with the account.

To update shared data sets:

1. In your workspace, select the **API management** tab.
2. In the left sidebar, under **Service accounts**, select an account.
3. Under **Access to data sets**, do one of the following:
   - To share new data sets with the account, select **Select data sets**.
   - To see the details of a data set, select its name. This opens the data set in a new browser tab.
   - To stop sharing a data set with the service account, select the **X** icon in the row with the data set's name.
4. Select the **Save** button.

## To regenerate a service account secret

Regenerating a service account secret invalidates the old secret immediately.

To regenerate a service account secret:

1. In your workspace, select the **API management** tab.
2. In the left sidebar, select a service account.
3. Next to **Service account secret**, select **Regenerate**.

Remember that service account secrets expire 2 years after creation and must be manually regenerated. You will not be notified before the secret expires.

## To delete a service account

To delete a service account:

1. In your workspace, select the **API management** tab.
2. In the left sidebar, select a service account.
3. In the bottom right corner, select the **Remove service account** button. A pop-up window appears.
4. In the pop-up window, select the **Delete** button.

## Ready-to-use Python script for API management

In API management, you can generate sample Python code that includes your credentials. You can download the script and use it to start making API calls.

To generate the code:

1. Go to **API management** > **Service accounts**.
2. Select a service account.
3. Under **Generate sample Python code**, select the **Generate** button.

## Troubleshooting

### 403 Forbidden error

If you select **Select data sets manually** in **Access to data sets**, the service account can only access APIs related to the specific shared data sets.

Calls to endpoints that require workspace-level or tenant-level access return a `403 Forbidden` error. For details, see [Limitations of dataset-scoped service accounts](#limitations-of-dataset-scoped-service-accounts).

## Limitations of dataset-scoped service accounts

Service accounts created with **Select data sets manually** are restricted to data set-level operations. They cannot call workspace-level or tenant-level API endpoints.

If such an account tries to access unsupported endpoints, the response is `403 Forbidden`.

Examples of unsupported endpoints for accounts with manually selected data set access:

- `GET /workspaces/{workspaceId}`
- `POST /workspaces/{workspaceId}/ingest`
- `GET /tenants/{tenantId}/users/roles`
- `GET /workspaces/{workspaceId}/users/roles`
- `GET /tenants/{tenantId}/workspaces`
- `GET /workspaces/{workspaceId}/schemas`
- `GET /workspaces/{workspaceId}/ledger`

To avoid this, use a service account with **Grant all workspace data** access or an authenticated user token with the required privileges.