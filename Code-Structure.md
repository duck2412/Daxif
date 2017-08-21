# Before you begin
Run the build.cmd script located at src/build.cmd in order to build the code structure. After the script is done, you can open the DAXIF solution, which is located at src/Delegate.Daxif.sln. The rest of this section will describe the contents of the solution.

# The solution
There are several folders, which contain general helpful functions and items. These folders are:
* Resources
* Common
* Setup
In general these shouldn't be touched, unless some missing general functionality is missing.


There are 3 folders, which are relevant when adding new features.
* Modules
* API
* ScriptTemplates

The point of each folder will be described below

## API
This folder contains the end-points which the users of DAXIF interact with. These are described as types with static private members. Each end-point calls the Main function located in the respective module. When writing a new end-point, consider whether it makes sense to add it to an existing type of create a new one.

## Modules
This folder contains a folder for each file in the API folder. Each folder has a single Main file, which contains a function for each end-point in the API. It's this function which the API calls, thus this function does the logic. Generally a helper is created for each folder, which the functions in the Main file calls, such that the logic is abstracted away from the controller. The rest of the files in each module folder contains the files necessary to implement the logic for all the functions in the Main file.

## ScriptTemplates
DAXIF declares several sample scripts, each using different features of DAXIF. This way, the end user doesn't need to start from scratch when they try out a feature, but they can simply modify the sample scripts. Thus if a new feature is developed, a sample script needs to be created showing a simple use case of the feature.

# Updating the wiki
This wiki is only useful if it contains definitions of all features. Therefore it must be updated with each new feature.
