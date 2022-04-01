# Authentication methods

Authentication with DAXIF uses the CrmServiceClient, which means that the following references must be included:
```fsharp
#r @"bin\Microsoft.IdentityModel.Clients.ActiveDirectory.dll"
#r @"bin\Microsoft.Xrm.Tooling.Connector.dll"
```

NB: All examples pass the command line arguments to the authentication. This is done so that a DevOps pipeline can overwrite the values.

## OAuth
Authentication with OAuth is recommended when running scripts locally. This authenticates through an app registration but using your credentials, i.e. all actions performed  by the scripts are executed by the user belonging to the credentials. OAuth authentication requires `mfaAppId` and `mfaReturnUrl` to be included in the environment.

```fsharp
Environment.Create(
      name = "Development",
      url = "https://devenv.crm4.dynamics.com",
      ap = AuthenticationProviderType.OnlineFederation,
      creds = creds,
      method = ConnectionType.OAuth,
      mfaAppId = "xxxxx",
      mfaReturnUrl = "xxxx",
      args = fsi.CommandLineArgs
    )
```

The `creds` is an optional reference which when omitted defaults to using credentials from a local file using the environment name ("Development" in the example above). If no file exists for the given name, the user will be asked to provide credentials and the file is saved afterwards. The files uses the ".daxif" file type.

As an alternative to using the file approach the credentials can be set using `Credentials.create("MyUserName", "MyPassword")`

A sample app id and return url are supplied by [Microsoft](https://docs.microsoft.com/en-us/powerapps/developer/data-platform/xrm-tooling/use-connection-strings-xrm-tooling-connect#connection-string-parameters) but is not intended for production use:
![image alt text](https://user-images.githubusercontent.com/7383967/161254948-e5bb8b6d-92b5-4948-ae68-4c074024b284.png)

## ClientSecret
Authentication using an app registration and a corresponding application user is recommended when executing from DevOps pipelines, where the ClientSecret can be safely stored. This authenticates through an application user meaning all actions performed  by the scripts are executed by the application user. ClientSecret authentication requires `mfaAppId` and `mfaClientSecret` to be included in the environment.

```fsharp
Environment.Create(
      name = "Development",
      url = "https://devenv.crm4.dynamics.com",
      method = ConnectionType.ClientSecret,
      mfaAppId = "xxx",
      mfaClientSecret = "xxx",
      args = fsi.CommandLineArgs
    )
```

### Creating App registration and Application user
If you have never made an App Registration you're probably wondering what an "App id" is. We recommend following [this guide](https://www.powerobjects.com/blog/2018/05/18/authentication-dynamics-365-using-azure-apps/) to create an App Registration in Azure and a corresponding Application User in CDS

## ConnectionString
If you have a custom connection string that is not a constructor on the CrmServiceClient you can pass a connection string directly
```fsharp
Environment.Create(
      name = "Development",
      method = ConnectionType.ConnectionString,
      connectionString = "xxx",
      args = fsi.CommandLineArgs
    )
```

Further details on connection string parameters can be found [here](https://docs.microsoft.com/en-us/powerapps/developer/data-platform/xrm-tooling/use-connection-strings-xrm-tooling-connect).

## On-prem authentication
Older versions of DAXIF had a single method of authenticating - the OrganizationServiceProxy. This was deprecated ([link](https://docs.microsoft.com/en-us/power-platform/important-changes-coming#deprecation-of-office365-authentication-type-and-organizationserviceproxy-class-for-connecting-to-dataverse)) and no longer works in online environments. Attempting to use it will give a Ws-Trust error.

```fsharp
Environment.Create(
      name = "Development",
      url = "https://devenv.crm4.dynamics.com/XRMServices/2011/Organization.svc",
      ap = AuthenticationProviderType.OnlineFederation,
      creds = creds,
      args = fsi.CommandLineArgs
    )
```