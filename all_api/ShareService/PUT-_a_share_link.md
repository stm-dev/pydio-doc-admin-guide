






 
Put or Create a share room  


### Body Parameters

Name | Description | Type | Required
---|---|---|---
**CreatePassword** | Set if switching from no password to password | _string_ |   
**PasswordEnabled** | Whether it has Password enabled | _boolean_ |   
**ShareLink** | Content of the link to create | _#/definitions/restShareLink_ |   
**UpdateCustomHash** | Change the ShareLink Hash with a custom value | _string_ |   
**UpdatePassword** | Set if updating an existing password | _string_ |   


### Body Example
```
{
  "CreatePassword": "string",
  "PasswordEnabled": true,
  "ShareLink": {
    "AccessEnd": "string",
    "AccessStart": "string",
    "CurrentDownloads": "string",
    "Description": "string",
    "Label": "string",
    "LinkHash": "string",
    "LinkUrl": "string",
    "MaxDownloads": "string",
    "PasswordRequired": true,
    "Permissions": [
      "string"
    ],
    "Policies": [
      {
        "Action": "string",
        "Effect": "string",
        "JsonConditions": "string",
        "Resource": "string",
        "Subject": "string",
        "id": "string"
      }
    ],
    "PoliciesContextEditable": true,
    "RestrictToTargetUsers": true,
    "RootNodes": [
      {
        "AppearsIn": [
          {
            "Path": "string",
            "WsLabel": "string",
            "WsUuid": "string"
          }
        ],
        "Commits": [
          {
            "Data": "string",
            "Description": "string",
            "Event": {
              "Metadata": {},
              "Optimistic": true,
              "Silent": true,
              "Source": "[Recursive structure]",
              "Target": "[Recursive structure]",
              "Type": "string"
            },
            "MTime": "string",
            "OwnerUuid": "string",
            "Size": "string",
            "Uuid": "string"
          }
        ],
        "Etag": "string",
        "MTime": "string",
        "MetaStore": {},
        "Mode": 10,
        "Path": "string",
        "Size": "string",
        "Type": "string",
        "Uuid": "string"
      }
    ],
    "TargetUsers": {},
    "UserLogin": "string",
    "UserUuid": "string",
    "Uuid": "string",
    "ViewTemplateName": "string"
  },
  "UpdateCustomHash": "string",
  "UpdatePassword": "string"
}
```






### Response Example (200)
Response Type /definitions/restShareLink

```
{
  "AccessEnd": "string",
  "AccessStart": "string",
  "CurrentDownloads": "string",
  "Description": "string",
  "Label": "string",
  "LinkHash": "string",
  "LinkUrl": "string",
  "MaxDownloads": "string",
  "PasswordRequired": true,
  "Permissions": [
    "string"
  ],
  "Policies": [
    {
      "Action": "string",
      "Effect": "string",
      "JsonConditions": "string",
      "Resource": "string",
      "Subject": "string",
      "id": "string"
    }
  ],
  "PoliciesContextEditable": true,
  "RestrictToTargetUsers": true,
  "RootNodes": [
    {
      "AppearsIn": [
        {
          "Path": "string",
          "WsLabel": "string",
          "WsUuid": "string"
        }
      ],
      "Commits": [
        {
          "Data": "string",
          "Description": "string",
          "Event": {
            "Metadata": {},
            "Optimistic": true,
            "Silent": true,
            "Source": "[Recursive structure]",
            "Target": "[Recursive structure]",
            "Type": "string"
          },
          "MTime": "string",
          "OwnerUuid": "string",
          "Size": "string",
          "Uuid": "string"
        }
      ],
      "Etag": "string",
      "MTime": "string",
      "MetaStore": {},
      "Mode": 10,
      "Path": "string",
      "Size": "string",
      "Type": "string",
      "Uuid": "string"
    }
  ],
  "TargetUsers": {},
  "UserLogin": "string",
  "UserUuid": "string",
  "Uuid": "string",
  "ViewTemplateName": "string"
}
```

