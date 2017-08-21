The ViewExtender works by declaring several type declaration files, which helps you declare your view extension. This is done by the first part of the script.

```fsharp
#load @"_Config.fsx"
open _Config
open DG.Daxif

View.GenerateFiles(Env.dev, Path.daxifRoot,
  solutions = [|
    SolutionInfo.name
    |],
  entities = [|
    // eg. "systemuser"
    |])
```

Then the declaration files are loaded.

```fsharp
#load @"viewExtenderData\_ViewGuids.fsx" 
#load @"viewExtenderData\_EntityRelationships.fsx" 
#load @"viewExtenderData\_EntityAttributes.fsx"

open ViewGuids
open EntityAttributes
open EntityRelationships
```

The rest of the script should contain your declarations. The Views module contains definitions for each view grouped by entity. So to get a view structure write the following.

```fsharp
Views.Account.MyParentView
|> View.Parse Env.dev
```
The "MyParentView" is know parsed and ready to be used. In order to copy this to another view, use the changeId function.

```fsharp
Views.Account.MyParentView
|> View.Parse Env.dev
|> View.ChangeId Views.Account.FirstChildView
```
This view is now an exact copy of "MyParentView", but the id of the view is "FirstChildView". If the views should simply be copied to "FirstChildView", then call the UpdateView function.

```fsharp
Views.Account.MyParentView
|> View.Parse Env.dev
|> View.ChangeId Views.Account.FirstChildView
|> View.UpdateView Env.dev
```

Whenever this script is run, the view "FirstChildView" will become an exact copy of "MyParentView". The View module contains several functions, which help extend the view, such that "FirstChildView" becomes more than just a copy.
* AddColumn
* AddColumnFirst
* AddColumnLast
* RemoveColumn
* Add/RemoveOrdering
* AddRelatedColumn
* AddRelatedColumnFirst
* AddRelatedColumnLast
* RemoveRelatedColumn
* InitFilter
* AndFilters
* OrFilters
* SetFilter
* AddFilter
* AddCondition
* AddCondition2
* AddConditionMany

All of these functions can be used before calling UpdateView, such that "FirstChildView" gets the desired extension to "MyParentView".

