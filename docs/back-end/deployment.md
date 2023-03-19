# Deploying The Application

We will build two different pipelines, one for deploying the application on staging and one for swapping the application from staging to production.

## Deploying The Application on Staging

First, We add the following code to the YAML file.

``` yaml
# Write the name of the branch that you want to be triggered by.
trigger:
- master

# Define the virtual machine you want to run this pipeline on.
pool:
  vmImage: ubuntu-latest

# Define any variable groups or variables in this section.
# For variables just write:
# - group: <variable group name>
#
# for variables:
# - name: <variable name>
#   value: <variable value>
variables:
- group: pipeline
```

After that, we have to define the steps of the pipeline. First step, add the following code.

``` yaml hl_lines="19-22"
# Write the name of the branch that you want to be triggered by.
trigger:
- master

# Define the virtual machine you want to run this pipeline on.
pool:
  vmImage: ubuntu-latest

# Define any variable groups or variables in this section.
# For variables just write:
# - group: <variable group name>
#
# for variables:
# - name: <variable name>
#   value: <variable value>
variables:
- group: pipeline

steps:
 - checkout: self
   submodules: true
   persistCredentials: true
```

Second step is to check .NET version.

``` yaml hl_lines="24-26"
# Write the name of the branch that you want to be triggered by.
trigger:
- master

# Define the virtual machine you want to run this pipeline on.
pool:
  vmImage: ubuntu-latest

# Define any variable groups or variables in this section.
# For variables just write:
# - group: <variable group name>
#
# for variables:
# - name: <variable name>
#   value: <variable value>
variables:
- group: pipeline

steps:
 - checkout: self
   submodules: true
   persistCredentials: true

 - script: dotnet --version
   displayName: 'Check dotnet Version'
```

Third step, we will install ``dotnet-ef`` globally.

``` yaml hl_lines="27-28"
# Write the name of the branch that you want to be triggered by.
trigger:
- master

# Define the virtual machine you want to run this pipeline on.
pool:
  vmImage: ubuntu-latest

# Define any variable groups or variables in this section.
# For variables just write:
# - group: <variable group name>
#
# for variables:
# - name: <variable name>
#   value: <variable value>
variables:
- group: pipeline

steps:
 - checkout: self
   submodules: true
   persistCredentials: true

 - script: dotnet --version
   displayName: 'Check dotnet Version'

 - script: dotnet tool install --global dotnet-ef
   displayName: 'Install dotnet-ef'
```

Fourth step, update the staging database, by providing the path to the ``csproj`` file and connection string for staging database like the following code.

``` yaml hl_lines="30-31"
# Write the name of the branch that you want to be triggered by.
trigger:
- master

# Define the virtual machine you want to run this pipeline on.
pool:
  vmImage: ubuntu-latest

# Define any variable groups or variables in this section.
# For variables just write:
# - group: <variable group name>
#
# for variables:
# - name: <variable name>
#   value: <variable value>
variables:
- group: pipeline

steps:
 - checkout: self
   submodules: true
   persistCredentials: true

 - script: dotnet --version
   displayName: 'Check dotnet Version'

 - script: dotnet tool install --global dotnet-ef
   displayName: 'Install dotnet-ef'

 - script: dotnet ef database update --project <Path to the csproj file> --connection '<Staging database connection string>'
   displayName: 'Update Database'
```

Fifth step is publishing it to artifacts. Add the following code.

``` yaml hl_lines="33-34"
# Write the name of the branch that you want to be triggered by.
trigger:
- master

# Define the virtual machine you want to run this pipeline on.
pool:
  vmImage: ubuntu-latest

# Define any variable groups or variables in this section.
# For variables just write:
# - group: <variable group name>
#
# for variables:
# - name: <variable name>
#   value: <variable value>
variables:
- group: pipeline

steps:
 - checkout: self
   submodules: true
   persistCredentials: true

 - script: dotnet --version
   displayName: 'Check dotnet Version'

 - script: dotnet tool install --global dotnet-ef
   displayName: 'Install dotnet-ef'

 - script: dotnet ef database update --project <Path to the csproj file> --connection '<Staging database connection string>'
   displayName: 'Update Database'

 - script: dotnet publish <Path to the csproj file> --output $(Build.ArtifactStagingDirectory)
   displayName: 'Publish to Artifacts'
```

After that, add a task for archiving the project.

``` yaml hl_lines="36-42"
# Write the name of the branch that you want to be triggered by.
trigger:
- master

# Define the virtual machine you want to run this pipeline on.
pool:
  vmImage: ubuntu-latest

# Define any variable groups or variables in this section.
# For variables just write:
# - group: <variable group name>
#
# for variables:
# - name: <variable name>
#   value: <variable value>
variables:
- group: pipeline

steps:
 - checkout: self
   submodules: true
   persistCredentials: true

 - script: dotnet --version
   displayName: 'Check dotnet Version'

 - script: dotnet tool install --global dotnet-ef
   displayName: 'Install dotnet-ef'

 - script: dotnet ef database update --project <Path to the csproj file> --connection '<Staging database connection string>'
   displayName: 'Update Database'

 - script: dotnet publish <Path to the csproj file> --output $(Build.ArtifactStagingDirectory)
   displayName: 'Publish to Artifacts'

 - task: ArchiveFiles@2
   inputs:
    rootFolderOrFile: '$(Build.ArtifactStagingDirectory)'
    includeRootFolder: false
    archiveType: 'zip'
    archiveFile: '$(Build.ArtifactStagingDirectory)/to-be-deployed.zip'
    replaceExistingArchive: true
```

Lastly, add another task for deploying the application on Azure Web App.

``` yaml hl_lines="44-53"
# Write the name of the branch that you want to be triggered by.
trigger:
- master

# Define the virtual machine you want to run this pipeline on.
pool:
  vmImage: ubuntu-latest

# Define any variable groups or variables in this section.
# For variables just write:
# - group: <variable group name>
#
# for variables:
# - name: <variable name>
#   value: <variable value>
variables:
- group: pipeline

steps:
 - checkout: self
   submodules: true
   persistCredentials: true

 - script: dotnet --version
   displayName: 'Check dotnet Version'

 - script: dotnet tool install --global dotnet-ef
   displayName: 'Install dotnet-ef'

 - script: dotnet ef database update --project <Path to the csproj file> --connection '<Staging database connection string>'
   displayName: 'Update Database'

 - script: dotnet publish <Path to the csproj file> --output $(Build.ArtifactStagingDirectory)
   displayName: 'Publish to Artifacts'

 - task: ArchiveFiles@2
   inputs:
    rootFolderOrFile: '$(Build.ArtifactStagingDirectory)'
    includeRootFolder: false
    archiveType: 'zip'
    archiveFile: '$(Build.ArtifactStagingDirectory)/to-be-deployed.zip'
    replaceExistingArchive: true

 - task: AzureWebApp@1
   inputs:
     azureSubscription: '<Place the Azure subscription here>'
     appType: 'webAppLinux'
     appName: '<Write the name of the application here>'
     deployToSlotOrASE: true
     resourceGroupName: '<Write the resouce group name here>'
     slotName: 'staging'
     package: '$(Build.ArtifactStagingDirectory)/to-be-deployed.zip'
     runtimeStack: 'DOTNETCORE|7.0'
```

## Swapping The Application From Staging to Production

This time if we need to publish the staging project to production, we just need to swap it using this pipeline. First, add the following code like the previous YAML file.

``` yaml
# For this example, this project is going to be triggered manually on Azure DevOps if you don't specify the trigger branch.
trigger: none

pool:
  vmImage: ubuntu-latest

variables:
- group: pipeline

steps:
 - checkout: self
   submodules: true
   persistCredentials: true

 - script: dotnet --version
   displayName: 'Check dotnet Version'

 - script: dotnet tool install --global dotnet-ef
   displayName: 'Install dotnet-ef'
```

Then, add the following step for updating the production database.

``` yaml hl_lines="21-22"
# For this example, this project is going to be triggered manually on Azure DevOps if you don't specify the trigger branch.
trigger: none

pool:
  vmImage: ubuntu-latest

variables:
- group: pipeline

steps:
 - checkout: self
   submodules: true
   persistCredentials: true

 - script: dotnet --version
   displayName: 'Check dotnet Version'

 - script: dotnet tool install --global dotnet-ef
   displayName: 'Install dotnet-ef'

 - script: dotnet ef database update --project --project <Path to the csproj file> --connection '<Production database connection string>'
   displayName: 'Update Database'
```

At the end of the YAML file add this task for swapping the slots from staging to production.

``` yaml hl_lines="24-30"
# For this example, this project is going to be triggered manually on Azure DevOps if you don't specify the trigger branch.
trigger: none

pool:
  vmImage: ubuntu-latest

variables:
- group: pipeline

steps:
 - checkout: self
   submodules: true
   persistCredentials: true

 - script: dotnet --version
   displayName: 'Check dotnet Version'

 - script: dotnet tool install --global dotnet-ef
   displayName: 'Install dotnet-ef'

 - script: dotnet ef database update --project <Path to the csproj file> --connection '<Production database connection string>'
   displayName: 'Update Database'

 - task: AzureAppServiceManage@0
   inputs:
    azureSubscription: '<Place the Azure subscription here>'
    Action: 'Swap Slots'
    WebAppName: '<Write the name of the application here>'
    ResourceGroupName: '<Write the resouce group name here>'
    SourceSlot: 'staging'
```