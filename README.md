# How to create the project

## Intructions to create a new project with layers architecture (similar to DDD).
First, you need to put these commands on your terminal, replacing 'projectName.' by your project's name.
```
dotnet new sln -n projectName
dotnet new webapi -n projectName.Api
dotnet new classlib -n projectName.Application
dotnet new classlib -n projectName.Domain
dotnet new classlib -n projectName.Infrastructure

dotnet sln add projectName.Api projectName.Application projectName.Domain projectName.Infrastructure 

dotnet add projectName.Api reference projectName.Application
dotnet add projectName.Application reference projectName.Domain
dotnet add projectName.Infrastructure reference projectName.Application

rider .

```
