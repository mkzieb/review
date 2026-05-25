# API endpoints (Data Platform)
To start calling Data Platform APIs, see **[API: How to index tutorials](https://docs.veracity.com/pages/data-platform/howto-index-tutorials-1/api-how-to-index-tutorials)**. The tutorials walk through a complete, practical flow, including typical API calls, request patterns, and how indexing uses the APIs end to end.

To access Data Platform APIs programmatically, applications and scripts use **service accounts** scoped to a specific workspace. In **Data Workbench**, the **API management** page lets you create and manage service accounts and their access to workspace data.

##  When to use service accounts
Use **service accounts** when an application, script, or integration needs to access Data Platform APIs without a human user context.

In Data Workbench, **API management** lets you create service accounts and scope their access to a workspace and its data.

Typical use cases:
- backend services calling Data Platform APIs
- ingestion or integration scripts
- scheduled or CI/CD jobs

## Platform APIs and authentication
Platform APIs require authentication ([see details](https://docs.veracity.com/pages/data-platform/access-and-security/authentication?utm_source=veracity-docs)). For automated integrations, scripts, and backend services, you normally use a service account with [the OAuth 2.0 client credentials flow](https://docs.veracity.com/pages/data-platform/access-and-security/authentication#user-content-client-credential-flow).

The platform also supports the authorization code flow for scenarios where an individual user signs in and grants access.

To use the client credentials flow, you must [create a service account](https://docs.veracity.com/pages/application-user-guides/veracity-data-workbench/api-management).

## Service account credentials
When you create a service account, the platform generates the credentials your code uses to authenticate API requests:

- **Service account ID**
- **Service account secret**
- **API key**
- **Base URL**

How these are used:
- The **service account secret** is exchanged for an **access token**.
- The **access token** is used to call platform APIs.
- The **API key** must be included in requests where required by the API.
- The **base URL** is combined with specific endpoints.

Security and lifecycle considerations:
- Store credentials securely and share them only with systems that need access.
- The service account secret is shown only once, so copy and store it securely.
- Service account secrets **expire 2 years after creation** and are **not rotated automatically**.

## Terminology mapping (for Microsoft users)
If you are familiar with Microsoft Entra ID (Azure AD) or OAuth 2.0, the following table shows how the concepts in this platform map to standard Microsoft terminology.

| Data Platform term | Microsoft terminology | What it means |
|------------------|----------------------|--------------|
| **Service account** | Confidential client / Application | A non-user identity used by applications or services to authenticate |
| **Service account ID** | Client ID (Application ID) | Unique identifier of the application |
| **Service account secret** | Client secret | A credential used to authenticate the application and obtain tokens |
| **Credentials** | Client credentials | The set of values (client ID + secret/certificate) used for authentication |
| **Service account authentication** | Client credentials flow | OAuth 2.0 flow where the application authenticates as itself (no user) |
| **Platform APIs** | Resource server / Protected API | The API that requires a valid access token |
| **API access** | Application permissions | Permissions granted directly to the application by an admin |
| **Workspace access level (Reader/Admin)** | App roles / RBAC roles | Defines what the application is allowed to do |
| **Access to datasets** | Scoped permissions | Restricts access to specific resources |
| **Authorization code flow** | Delegated flow | Flow where a user signs in and grants access |
| **API key** | *(Not part of OAuth)* | Platform-specific key used for routing, subscription, or context |
| **Base URL** | API endpoint / Resource URL | The base address of the API |

A service account in Data Platform is conceptually equivalent to a confidential client (application) using the OAuth 2.0 client credentials flow. The service account uses its credentials to obtain an access token, which is then used to authorize API requests.

---

## Create and manage service accounts in Data Workbench (UI)
For step-by-step UI instructions (create account, set scope, rotate secret, delete account), see:

- [**Data Workbench: Create and manage service accounts (API management)**](https://docs.veracity.com/pages/application-user-guides/veracity-data-workbench/api-management)
