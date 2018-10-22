# User Authentication in Amazon Pinpoint Apps<a name="permissions-authentication"></a>

To integrate with Amazon Pinpoint, your app must authenticate users to register endpoints and report usage data\. When you add your app to Amazon Pinpoint by creating a project in AWS Mobile Hub, Mobile Hub automatically provisions the following AWS resources to help you implement user authentication:

**Amazon Cognito identity pool**  
Amazon Cognito creates unique identities for your users and provides credentials that grant temporary access to the backend AWS resources for your app\. An identity pool is a store of user identity data for your app users\.  
Amazon Cognito provides credentials for authenticated and unauthenticated users\. Authenticated users include those who sign in to your app through a public identity provider, such as Facebook, Amazon, or Google\. Unauthenticated users are those who do not sign in to your app, such as guest users\.  
You control your users' access to AWS resources with separate AWS Identity and Access Management \(IAM\) roles for authenticated and unauthenticated users\. These roles must be assigned to the identity pool\.

**IAM role for unauthenticated users**  
Includes permissions policies that delegate limited access to AWS resources for unauthenticated users\. You can customize the role as needed\. By default, this role is assigned to the Amazon Cognito identity pool\.

If your app requires users to authenticate with a public identity provider, you must create an IAM role for authenticated users and assign this role to the identity pool\. To support Amazon Pinpoint, the permissions in your authenticated role must include the same permissions as those in the unauthenticated role created by Mobile Hub\.

For more information about IAM roles, see [IAM Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) in the *IAM User Guide*\.

Your app code uses Amazon Cognito and IAM to authenticate users as follows:

1. Your app code constructs an Amazon Cognito credentials provider\.

1. Your app code passes the provider as a parameter when it initializes the Amazon Pinpoint client\.

1. The Amazon Pinpoint client uses the provider to get credentials for the user's identity in the identity pool\. New users are assigned a new identity\. 

1. The user gains the permissions granted by the IAM roles that are associated with the identity pool\.

For more information about how Amazon Cognito supports user authentication, see [Amazon Cognito Identity: Using Federated Identities](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-identity.html) in the *Amazon Cognito Developer Guide*\.

## Unauthenticated Role<a name="permissions-authentication-unauthenticatedrole"></a>

The unauthenticated role created by Mobile Hub allows your app users to send data to Amazon Pinpoint\. The role name includes "`unauth_MOBILEHUB`"; for example, in the IAM console, you will see a role with a name similar to `MySampleApp_unauth_MOBILEHUB_1234567890`\.

IAM roles delegate permissions with two types of policies:
+ Permissions policy – Grants the user of the role permission to take the specified actions on the specified resources\.
+ Trust policy – Specifies which entities are allowed to assume the role and gain its permissions\.

### Permissions Policies<a name="permissions-authentication-unauthenticatedrole-permissionspolicies"></a>

The unauthenticated role includes two permissions policies\. The following permissions policy allows your app users to register with Amazon Pinpoint and report app usage events\. Mobile Hub assigns the policy a name that includes "`mobileanalytics_MOBILEHUB`"\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "mobileanalytics:PutEvents"
      ],
      "Resource": [
        "*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "mobiletargeting:UpdateEndpoint"
      ],
      "Resource": [
        "arn:aws:mobiletargeting:*:*:apps/*"
      ]
    }
  ]
}
```

After your app is integrated with Amazon Pinpoint, your app registers an endpoint with Amazon Pinpoint when a new user starts an app session\. Your app sends updated endpoint data to Amazon Pinpoint each time the user starts a new session\.

The following permissions policy allows your app users to establish an identity with the Amazon Cognito identity pool for your app\. Mobile Hub assigns the policy a name that includes "`signin_MOBILEHUB`"\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "cognito-identity:GetId"
      ],
      "Resource": [
        "arn:aws:cognito-identity:*:*:identityPool/us-east-1:1a2b3c4d-5e6f-7g8h-9i0j-1k2l3m4n5o6p"
      ]
    }
  ]
}
```

### Trust Policy<a name="permissions-authentication-unauthenticatedrole-trustpolicy"></a>

To allow Amazon Cognito to assume the role for unauthenticated users in your identity pool, Mobile Hub adds the following trust policy to the role:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "",
      "Effect": "Allow",
      "Principal": {
        "Federated": "cognito-identity.amazonaws.com"
      },
      "Action": "sts:AssumeRoleWithWebIdentity",
      "Condition": {
        "StringEquals": {
          "cognito-identity.amazonaws.com:aud": "us-east-1:1a2b3c4d-5e6f-7g8h-9i0j-1k2l3m4n5o6p"
        },
        "ForAnyValue:StringLike": {
          "cognito-identity.amazonaws.com:amr": "unauthenticated"
        }
      }
    }
  ]
}
```

For an example of a trust policy assigned to an authenticated role, see [Role\-Based Access Control](https://docs.aws.amazon.com/cognito/latest/developerguide/role-based-access-control.html) in the *Amazon Cognito Developer Guide*\.