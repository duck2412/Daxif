### Using different authentication methods
Older versions of DAXIF had a single method of authenticating - the OrganizationServiceProxy
```fsharp
Environment.Create(
      name = "Development",
      url = "https://devenv.crm4.dynamics.com/XRMServices/2011/Organization.svc",
      ap = AuthenticationProviderType.OnlineFederation,
      creds = creds,
      args = fsi.CommandLineArgs
    )
```

The newer versions of DAXIF use the CrmServiceClient instead, which means the config file needs two more references
```fsharp
#r @"bin\Microsoft.IdentityModel.Clients.ActiveDirectory.dll"
#r @"bin\Microsoft.Xrm.Tooling.Connector.dll"
```

It is then possible to use OAuth or ClientSecret as an authentication method. OAuth is easily added by adding `mfaAppId` and `mfaReturnUrl` to your environment.

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

This still uses a user though. In order to purely authenticate using an application user you will need to use the ClientSecret method.

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

All of these examples pass the command line arguments to the authentication. This is done such that a DevOps pipeline can overwrite the values. 