# Frequently used

## Project setup

**Examples:**

### Create a new solution

`$ dotnet new sln -o Example`

### Create a new web api

`$ dotnet new webapi -o Example.Api`

### Create a new class library

`$ dotnet new classlib -o Example.Contacts`

### Add project to solution

_note: does not work with gitbash_

`$ dotnet sln add (ls -r **\*.csproj)`

### Build project

dotnet build

### Add reference to projects

`$ dotnet add .\BuberDinner.api\ reference .\BuberDinner.Contracts\ .\BuberDinner.Application\`

### create git ignore

`$ dotnet new gitignore`

### Full manual reference

https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet
