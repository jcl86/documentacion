# Tips varios Blazor

## Data binding

##### One-way Binding

Simplemente, escribir el campo o la propiedad con @
   
```` csharp
<h1>Details for @person.Name</h1>

<input type="text" readonly class="form-control" value="@person.Name" />

@code {
  private Person person;
}
````

##### Two-way Binding

```` csharp
<input @bind="@person.Name" placeholder="Type your name" />

<input @bind-value="person.Name" @bind-value:event="oninput" placeholder="Type your name" /

@code {
  private Person person = new person();
}
````

##### Forms

```` csharp
<EditForm Model="@dto" OnValidSubmit="@HandleValidSubmit" OnInvalidSubmit="@HandleInvalidSubmit">
  <InputText id="lastName" @bind-Value="@dto.Name" placeholder="Type your name" />
  <button type="submit">Create</button>
</EditForm>
@code {
  private CreatePerson dto;
}
````
