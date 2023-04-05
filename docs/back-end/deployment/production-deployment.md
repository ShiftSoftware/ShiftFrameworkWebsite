# Pushing Staging to Production

## Swapping The Application From Staging to Production

This time if we need to publish the staging project to production, we just need to swap it using this pipeline. First, add the following code to the new pipeline.

``` yaml
trigger: none

pool:
  vmImage: ubuntu-latest

variables:
- group: pipeline
- name: buildConfiguration
  value: 'Release'

steps:
 - checkout: self
   submodules: true
   persistCredentials: true
   fetchTags: true
   fetchDepth: 10
```

Then, add a ``Bash`` task to fetch the latest release tag.

``` yaml hl_lines="18-23"
trigger: none

pool:
  vmImage: ubuntu-latest

variables:
- group: pipeline
- name: buildConfiguration
  value: 'Release'

steps:
 - checkout: self
   submodules: true
   persistCredentials: true
   fetchTags: true
   fetchDepth: 10

 - task: Bash@3
   displayName: 'Get Latest Release Tag' 
   inputs:
    targetType: 'inline'
    script: echo "##vso[task.setvariable variable=tag;]$(git describe --tags --match release-* --abbrev=0)"
```

After that, checkout to the commit that the release tag was pushed.

``` yaml hl_lines="24-25"
trigger: none

pool:
  vmImage: ubuntu-latest

variables:
- group: pipeline
- name: buildConfiguration
  value: 'Release'

steps:
 - checkout: self
   submodules: true
   persistCredentials: true
   fetchTags: true
   fetchDepth: 10

 - task: Bash@3
   displayName: 'Get Latest Release Tag' 
   inputs:
    targetType: 'inline'
    script: echo "##vso[task.setvariable variable=tag;]$(git describe --tags --match release-* --abbrev=0)"

 - script: git checkout $(tag)
   displayName: 'Checkout Latest Release Tag'
```

Now, install ``dotnet-ef`` and update the database.

``` yaml hl_lines="27-28 30-31"
trigger: none

pool:
  vmImage: ubuntu-latest

variables:
- group: pipeline
- name: buildConfiguration
  value: 'Release'

steps:
 - checkout: self
   submodules: true
   persistCredentials: true
   fetchTags: true
   fetchDepth: 10

 - task: Bash@3
   displayName: 'Get Latest Release Tag' 
   inputs:
    targetType: 'inline'
    script: echo "##vso[task.setvariable variable=tag;]$(git describe --tags --match release-* --abbrev=0)"

 - script: git checkout $(tag)
   displayName: 'Checkout Latest Release Tag'

 - script: dotnet tool install --global dotnet-ef
   displayName: 'Install dotnet-ef'

 - script: dotnet ef database update --project <Path to the csproj file> --connection '<Staging database connection string>'
   displayName: 'Update Database'
```

Finally, add a task for swapping the slots to production.

``` yaml hl_lines="33-39"
trigger: none

pool:
  vmImage: ubuntu-latest

variables:
- group: pipeline
- name: buildConfiguration
  value: 'Release'

steps:
 - checkout: self
   submodules: true
   persistCredentials: true
   fetchTags: true
   fetchDepth: 10

 - task: Bash@3
   displayName: 'Get Latest Release Tag' 
   inputs:
    targetType: 'inline'
    script: echo "##vso[task.setvariable variable=tag;]$(git describe --tags --match release-* --abbrev=0)"

 - script: git checkout $(tag)
   displayName: 'Checkout Latest Release Tag'

 - script: dotnet tool install --global dotnet-ef
   displayName: 'Install dotnet-ef'

 - script: dotnet ef database update --project <Path to the csproj file> --connection '<Staging database connection string>'
   displayName: 'Update Database'

 - task: AzureAppServiceManage@0
   inputs:
    azureSubscription: '<Place the Azure subscription here>'
    Action: 'Swap Slots'
    WebAppName: '<Write the name of the application here>'
    ResourceGroupName: '<Write the resouce group name here>'
    SourceSlot: 'staging'
```