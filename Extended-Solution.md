DAXIF# export and import of solutions have the option to use export and import extended features. This extends the solution with additional data to be exported from the source environment and imported into the target environment. We found that the standard solution does not cover all cases necessary to ensure that the source and target environments are identical. 

Exporting and importing a solution only handles additions and updates but not deletions. This means that deleted plugins, workflows, and web resources will not be delete form the target environment after a successful import. This can lead to unnecessary bloat in all upstream environments. Using the Extended solution functionality the state of selected entities are carried over to target, along with cleanup of certain entity records based on what is present in the source environment.

# What does extended solution handle?
The following entity states are exported and updated:
* Views
* Workflows

The following entity records are deleted during import if they have been deleted from the solution:
* Plugins
  * Assembly
  * Types
  * Steps
  * Images
* Workflows
* WebResources

The information is stored in the `ExtendedSolution.xml` file which is added to the packaged solution.

# How?
To use extended solution, simply set `extended=true` when both exporting and importing solutions.

```fsharp
// Export unmanaged
Solution.Export(Env.dev, SolutionInfo.name, Path.Daxif.crmSolutionsFolder, managed = false, extended = true)

// Import managed
Solution.Export(Env.test, SolutionInfo.name, Path.Daxif.crmSolutionsFolder, managed = false, extended = true)
```

Extended export/import will perform a standard export/import first and all action regarding extended solution is handled after. 
This means that an extended import will fail on a non-extended solution but the import of the solution is still completed.