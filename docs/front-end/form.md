# Creating a Form For Working With The Data

Create a new Razor component inside ``Pages`` folder and name it ``ToDoForm``. Inside the Razor component, add the route like this:

``` razor
@attribute [Route($"/{nameof(ToDoForm)}/{{Key?}}")]
```

Then, we need to inherit ``ShiftForm`` like the following example:

``` razor hl_lines="2"
@attribute [Route($"/{nameof(ToDoForm)}/{{Key?}}")]
@inherits ShiftForm<ToDoForm, ToDoViewDTO>
```

After that, we will create ``ShiftEntityForm`` element with the these parameters:

``` razor hl_lines="4-7"
@attribute [Route($"/{nameof(ToDoForm)}/{{Key?}}")]
@inherits ShiftForm<ToDoForm, ToDoViewDTO>

<ShiftEntityForm @bind-Value="TheItem"
                 @bind-Mode="Mode"
                 @ref="FormContainer"
                 @bind-Key="Key">

</ShiftEntityForm>
```

In order the form to conduct the CRUD operations written in the ``ToDoAPI`` project. We need to pass the following parameter:

``` razor hl_lines="8"
@attribute [Route($"/{nameof(ToDoForm)}/{{Key?}}")]
@inherits ShiftForm<ToDoForm, ToDoViewDTO>

<ShiftEntityForm @bind-Value="TheItem"
                 @bind-Mode="Mode"
                 @ref="FormContainer"
                 @bind-Key="Key"
                 Action="/api/ToDo">

</ShiftEntityForm>
```

## Adding The Fields

We need three fields inside this form, two text fields for ``Title`` and ``Description``, and one checkbox for the ``Done``.

Inside ``ShiftEntityForm`` element, create a ``MudTextField`` for the ``Title``field and add the following parameters.

``` razor hl_lines="10-14"
@attribute [Route($"/{nameof(ToDoForm)}/{{Key?}}")]
@inherits ShiftForm<ToDoForm, ToDoViewDTO>

<ShiftEntityForm @bind-Value="TheItem"
                 @bind-Mode="Mode"
                 @ref="FormContainer"
                 @bind-Key="Key"
                 Action="/api/ToDo">

    <MudTextField @bind-Value="TheItem.Title"
                  Label="Title"
                  For="@(() => TheItem.Title)"
                  Disabled="@Disabled"
                  ReadOnly="@ReadOnly"/>

</ShiftEntityForm>
```

Similarly to the previous text field, we will create one for the ``Description``.

``` razor hl_lines="16-20"
@attribute [Route($"/{nameof(ToDoForm)}/{{Key?}}")]
@inherits ShiftForm<ToDoForm, ToDoViewDTO>

<ShiftEntityForm @bind-Value="TheItem"
                 @bind-Mode="Mode"
                 @ref="FormContainer"
                 @bind-Key="Key"
                 Action="/api/ToDo">

    <MudTextField @bind-Value="TheItem.Title"
                  Label="Title"
                  For="@(() => TheItem.Title)"
                  Disabled="@Disabled"
                  ReadOnly="@ReadOnly"/>
    
    <MudTextField @bind-Value="TheItem.Description"
                  Label="Description"
                  For="@(() => TheItem.Description)"
                  Disabled="@Disabled"
                  ReadOnly="@ReadOnly"/>

</ShiftEntityForm>
```

Now, we will create a checkbox element for ``Done``.

``` razor hl_lines="22-26"
@attribute [Route($"/{nameof(ToDoForm)}/{{Key?}}")]
@inherits ShiftForm<ToDoForm, ToDoViewDTO>

<ShiftEntityForm @bind-Value="TheItem"
                 @bind-Mode="Mode"
                 @ref="FormContainer"
                 @bind-Key="Key"
                 Action="/api/ToDo">

    <MudTextField @bind-Value="TheItem.Title"
                  Label="Title"
                  For="@(() => TheItem.Title)"
                  Disabled="@Disabled"
                  ReadOnly="@ReadOnly"/>
    
    <MudTextField @bind-Value="TheItem.Description"
                  Label="Description"
                  For="@(() => TheItem.Description)"
                  Disabled="@Disabled"
                  ReadOnly="@ReadOnly"/>

    <MudCheckBox @bind-Checked="TheItem.Done"
                 Label="Done"
                 For="@(() => TheItem.Done)"
                 Disabled="@Disabled"
                 ReadOnly="@ReadOnly"/>

</ShiftEntityForm>
```

Finally, we have a form that can edit, add, and remove to do items. But before we run the app, we need to connect ``ToDoForm.razor`` with ``ToDoList.razor``.

Inside ``ToDoList.razor`` file, add the following parameter to the ``ShiftList`` element.

``` razor hl_lines="3"
@attribute [Route($"/{nameof(ToDoList)}")]

<ShiftList T="@ToDoListDTO" Action="/odata/ToDo" ComponentType="typeof(ToDoForm)"></ShiftList>
```