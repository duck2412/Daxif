DAXIF# export and import of solutions have the option to use extended solution. This features extends the solution with additional data to be exported from source environment and imported into the target environment. We found that the current solution does not cover all cases necessary to ensure that both source and target environment are identically. 

In general, the standard import and export only export some records and will create or update the records import. This means that deleted plugins, workflows, or web resources will still be present in the target environment after a successful import. Leading to unnecessary bloat in all upstream environments. Extended solution makes sure that the state of selected entities are carried over to target, along with cleanup of certain entity records based on what is present in the source.

# What does extended solution handle?
The following entity states are exported and updated:
* Views
* Workflows

The following entity records are cleaned up on import based on what exist in the source environment:
* Plugins
  * Assembly
  * Types
  * Steps
  * Images
* Workflows
* WebResources

These information are stored in the `ExtendedSolution.xml` file which is added the the packaged solution.

# How?
To enable extended solution, simply set `extended=true` exporting and importing solutions.

```fsharp
// Export unmanaged
Solution.Export(Env.dev, SolutionInfo.name, Path.Daxif.crmSolutionsFolder, managed = false, extended = true)

// Import managed
Solution.Export(Env.test, SolutionInfo.name, Path.Daxif.crmSolutionsFolder, managed = false, extended = true)
```

Extended export/import will perform a standard export/import first and all action regarding extended solution is handled after.
This means that an extended import will fail on a non-extended solution but the import of the solution is still completed.