# Custom API
[This is a preview feature. There are some limitations.]

Creations of custom apis can be done using DAXIF#. The registrations are similar to plugin registrations. 
## How to use 
You need to extend your api using [CustomAPI.cs][customapi-gist]: 
 ```csharp
    public class MyCustomAPI : CustomAPI {
        public MyCustomAPI()
            : base(typeof(MyCustomAPI))
        {
            ...
        }
        ...
    }
```

Then you can register a new api using the functions defined in the base class: 
 ```csharp
    RegisterCustomAPI("MyCustomAPI", MyAction);

    protected void MyAction(LocalPluginContext localContext)
        {
            ...
        }
```

There is a range of configuration possibilities of the api. These can be defined fluently. For a full overview of the different configurations see the <a href="https://docs.microsoft.com/en-us/powerapps/developer/data-platform/reference/entities/customapi" title="Custom API entity reference">Custom API entity reference</a>: 

 ```csharp
    RegisterCustomAPI("MyCustomAPI", MyAction)
                .AllowCustomProcessingStep(AllowedCustomProcessingStepType.AsyncOnly)
                .Bind<Account>(BindingType.Entity)
                .EnableCustomization()
                .MakeFunction()
                .MakePrivate()
                .EnableForWorkFlow();
```

<a href="https://docs.microsoft.com/en-us/powerapps/developer/data-platform/reference/entities/customapirequestparameter" title="Request Parameters">Request Parameters</a> and <a href="https://docs.microsoft.com/en-us/powerapps/developer/data-platform/reference/entities/customapiresponseproperty" title="Request Parameters">Response Properties</a> can be defined: 

 ```csharp
    RegisterCustomAPI("MyCustomAPI", MyAction)
                .EnableCustomization()
                .AddRequestParameter(new CustomAPIConfig.CustomAPIRequestParameter("myRequestParameter", RequestParameterType.Integer))
                .AddResponseProperty(new CustomAPIConfig.CustomAPIResponseProperty("myResponseProperty", RequestParameterType.Integer));

    protected void MyAction(LocalPluginContext localContext)
        {
            ...
            var pluginContext = localContext.PluginExecutionContext;
            var input = (int)pluginContext.InputParameters["myRequestParameter"];
            ...
            pluginContext.OutputParameters["myResponseProperty"] = 1337;
        }
```

## Deployment
The creation of Custom APIs has been included in the PluginSyncDev.fs script. Meaning if you want to introduce your implemented APIs to dev, then you just need to execute the script. 

The APIs and its dependencies is then a part of your solution, and will be deployed as usual across environments. 

## Limitations
There is some limitations to the current setup: 
- [ ] There might be some properties that are not relevant for users to define. 
- [ ] There might lack some properties that the user want to define but can not. 
- [ ] Names of customapis, requestparameters and responseproperties have to be unique. 
- [ ] Certain combination of configurations should not be possible according to the documentation. These should result in failures when trying to sync. 
- [ ] Name/Displayname/unique of customapi/properties/parameters need to be alligned.  