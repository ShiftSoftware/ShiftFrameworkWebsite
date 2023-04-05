# Deploying The Application to Staging

We will build two different pipelines, one for deploying the application on staging and one for swapping the application from staging to production.

## Setting Up The Pipeline

First, We add the following code to the YAML file.

``` yaml
# Write the name of the branch that you want to be triggered by, and to triggered when there is a release tag on the commit.
trigger:
 branches:
    include:
    - master
 tags:
    include:
    - release*

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
- name: buildConfiguration
  value: 'Release'
```

After that, we have to define the steps of the pipeline. First step, add the following code.

``` yaml hl_lines="26-29"
# Write the name of the branch that you want to be triggered by, and to triggered when there is a release tag on the commit.
trigger:
 branches:
    include:
    - master
 tags:
    include:
    - release*

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
- name: buildConfiguration
  value: 'Release'

steps:
 - checkout: self
   submodules: true
   persistCredentials: true
```

Second step is to remove the reference of the other projects with the **ToDo.API** project.

``` yaml hl_lines="31-33"
# Write the name of the branch that you want to be triggered by, and to triggered when there is a release tag on the commit.
trigger:
 branches:
    include:
    - master
 tags:
    include:
    - release*

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
- name: buildConfiguration
  value: 'Release'

steps:
 - checkout: self
   submodules: true
   persistCredentials: true

 - script: dotnet remove <Path to the ToDo.API csproj file> reference <Path to the referenced csproj file>
   displayName: 'Remove <referenced project> from ToDo.API'
```

Third step, removing the referenced project and its folder.

``` yaml hl_lines="34-35 37-38"
# Write the name of the branch that you want to be triggered by, and to triggered when there is a release tag on the commit.
trigger:
 branches:
    include:
    - master
 tags:
    include:
    - release*

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
- name: buildConfiguration
  value: 'Release'

steps:
 - checkout: self
   submodules: true
   persistCredentials: true

 - script: dotnet remove <Path to the ToDo.API csproj file> reference <Path to the referenced csproj file>
   displayName: 'Remove <referenced project> from ToDo.API'
  
 - script: dotnet sln remove <Path to the referenced csproj file>
   displayName: 'Remove <Referenced project>'

 - script: rm -r <Path to the referenced folder> 
   displayName: 'Remove <Referenced> folder'
```

Fourth step, we will install ``dotnet-ef`` globally.

``` yaml hl_lines="40-42"
# Write the name of the branch that you want to be triggered by, and to triggered when there is a release tag on the commit.
trigger:
 branches:
    include:
    - master
 tags:
    include:
    - release*

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
- name: buildConfiguration
  value: 'Release'

steps:
 - checkout: self
   submodules: true
   persistCredentials: true

 - script: dotnet remove <Path to the ToDo.API csproj file> reference <Path to the referenced csproj file>
   displayName: 'Remove <referenced project> from ToDo.API'
  
 - script: dotnet sln remove <Path to the referenced csproj file>
   displayName: 'Remove <Referenced project>'

 - script: rm -r <Path to the referenced folder> 
   displayName: 'Remove <Referenced> folder'

 - script: dotnet tool install --global dotnet-ef
   displayName: 'Install dotnet-ef'
   condition: contains(variables['Build.SourceBranch'], 'refs/tags/release')
```

Fifth step, update the staging database, by providing the path to the **csproj** file and connection string for staging database like the following code.

``` yaml hl_lines="44-46"
# Write the name of the branch that you want to be triggered by, and to triggered when there is a release tag on the commit.
trigger:
 branches:
    include:
    - master
 tags:
    include:
    - release*

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
- name: buildConfiguration
  value: 'Release'

steps:
 - checkout: self
   submodules: true
   persistCredentials: true

 - script: dotnet remove <Path to the ToDo.API csproj file> reference <Path to the referenced csproj file>
   displayName: 'Remove <referenced project> from ToDo.API'
  
 - script: dotnet sln remove <Path to the referenced csproj file>
   displayName: 'Remove <Referenced project>'

 - script: rm -r <Path to the referenced folder> 
   displayName: 'Remove <Referenced> folder'

 - script: dotnet tool install --global dotnet-ef
   displayName: 'Install dotnet-ef'
   condition: contains(variables['Build.SourceBranch'], 'refs/tags/release')

 - script: dotnet ef database update --project <Path to the csproj file> --connection '<Staging database connection string>'
   displayName: 'Update Database'
   condition: contains(variables['Build.SourceBranch'], 'refs/tags/release')
```

Sixth step is publishing it to artifacts. Add the following code.

``` yaml hl_lines="48-50"
# Write the name of the branch that you want to be triggered by, and to triggered when there is a release tag on the commit.
trigger:
 branches:
    include:
    - master
 tags:
    include:
    - release*

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
- name: buildConfiguration
  value: 'Release'

steps:
 - checkout: self
   submodules: true
   persistCredentials: true

 - script: dotnet remove <Path to the ToDo.API csproj file> reference <Path to the referenced csproj file>
   displayName: 'Remove <referenced project> from ToDo.API'
  
 - script: dotnet sln remove <Path to the referenced csproj file>
   displayName: 'Remove <Referenced project>'

 - script: rm -r <Path to the referenced folder> 
   displayName: 'Remove <Referenced> folder'

 - script: dotnet tool install --global dotnet-ef
   displayName: 'Install dotnet-ef'
   condition: contains(variables['Build.SourceBranch'], 'refs/tags/release')

 - script: dotnet ef database update --project <Path to the csproj file> --connection '<Staging database connection string>'
   displayName: 'Update Database'
   condition: contains(variables['Build.SourceBranch'], 'refs/tags/release')

 - script: dotnet publish <Path to the csproj file> --output $(Build.ArtifactStagingDirectory)
   displayName: 'Publish to Artifacts'
   condition: contains(variables['Build.SourceBranch'], 'refs/tags/release')
```

After that, add a task for archiving the project.

``` yaml hl_lines="52-59"
# Write the name of the branch that you want to be triggered by, and to triggered when there is a release tag on the commit.
trigger:
 branches:
    include:
    - master
 tags:
    include:
    - release*

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
- name: buildConfiguration
  value: 'Release'

steps:
 - checkout: self
   submodules: true
   persistCredentials: true

 - script: dotnet remove <Path to the ToDo.API csproj file> reference <Path to the referenced csproj file>
   displayName: 'Remove <referenced project> from ToDo.API'
  
 - script: dotnet sln remove <Path to the referenced csproj file>
   displayName: 'Remove <Referenced project>'

 - script: rm -r <Path to the referenced folder> 
   displayName: 'Remove <Referenced> folder'

 - script: dotnet tool install --global dotnet-ef
   displayName: 'Install dotnet-ef'
   condition: contains(variables['Build.SourceBranch'], 'refs/tags/release')

 - script: dotnet ef database update --project <Path to the csproj file> --connection '<Staging database connection string>'
   displayName: 'Update Database'
   condition: contains(variables['Build.SourceBranch'], 'refs/tags/release')

 - script: dotnet publish <Path to the csproj file> --output $(Build.ArtifactStagingDirectory)
   displayName: 'Publish to Artifacts'
   condition: contains(variables['Build.SourceBranch'], 'refs/tags/release')

 - task: ArchiveFiles@2
   condition: contains(variables['Build.SourceBranch'], 'refs/tags/release') 
   inputs:
    rootFolderOrFile: '$(Build.ArtifactStagingDirectory)'
    includeRootFolder: false
    archiveType: 'zip'
    archiveFile: '$(Build.ArtifactStagingDirectory)/to-be-deployed.zip'
    replaceExistingArchive: true
```

Lastly, add another task for deploying the application on Azure Web App.

``` yaml hl_lines="61-71"
# Write the name of the branch that you want to be triggered by, and to triggered when there is a release tag on the commit.
trigger:
 branches:
    include:
    - master
 tags:
    include:
    - release*

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
- name: buildConfiguration
  value: 'Release'

steps:
 - checkout: self
   submodules: true
   persistCredentials: true

 - script: dotnet remove <Path to the ToDo.API csproj file> reference <Path to the referenced csproj file>
   displayName: 'Remove <referenced project> from ToDo.API'
  
 - script: dotnet sln remove <Path to the referenced csproj file>
   displayName: 'Remove <Referenced project>'

 - script: rm -r <Path to the referenced folder> 
   displayName: 'Remove <Referenced> folder'

 - script: dotnet tool install --global dotnet-ef
   displayName: 'Install dotnet-ef'
   condition: contains(variables['Build.SourceBranch'], 'refs/tags/release')

 - script: dotnet ef database update --project <Path to the csproj file> --connection '<Staging database connection string>'
   displayName: 'Update Database'
   condition: contains(variables['Build.SourceBranch'], 'refs/tags/release')

 - script: dotnet publish <Path to the csproj file> --output $(Build.ArtifactStagingDirectory)
   displayName: 'Publish to Artifacts'
   condition: contains(variables['Build.SourceBranch'], 'refs/tags/release')

 - task: ArchiveFiles@2
   condition: contains(variables['Build.SourceBranch'], 'refs/tags/release') 
   inputs:
    rootFolderOrFile: '$(Build.ArtifactStagingDirectory)'
    includeRootFolder: false
    archiveType: 'zip'
    archiveFile: '$(Build.ArtifactStagingDirectory)/to-be-deployed.zip'
    replaceExistingArchive: true

 - task: AzureWebApp@1
   condition: contains(variables['Build.SourceBranch'], 'refs/tags/release')
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