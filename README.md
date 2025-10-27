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

// Finally, to run the project:
dotnet run --project projectName.Api


```
🧩 1. Singleton

🔹 Se crea una única instancia de la clase para toda la aplicación.
Esa misma instancia se reutiliza para todas las solicitudes (requests) y todos los usuarios.

📘 Ejemplo:
```
builder.Services.AddSingleton<CustomerService>();
```

🔹 Cuándo usarlo:

Cuando el servicio no mantiene datos específicos de un usuario.

Ideal para servicios que solo leen información o manejan configuraciones globales.

🔸 Ejemplo típico:

Servicio de logging

Servicio de configuración global

Cache compartido

⚠️ Cuidado:
Si la clase guarda datos en propiedades internas (como listas o contadores), esos datos serán compartidos entre todos los usuarios, lo cual puede ser un problema si no es intencional.

🧭 2. Scoped

🔹 Crea una nueva instancia del servicio por cada solicitud HTTP (request).

📘 Ejemplo:
```
builder.Services.AddScoped<CustomerService>();
```

🔹 Cuándo usarlo:

Cuando el servicio debe mantener datos durante toda una petición, pero no entre diferentes peticiones.

Muy común para manejar el DbContext en Entity Framework Core.

🔸 Ejemplo típico:
```
builder.Services.AddDbContext<AppDbContext>(options => ...);
```

✅ Cada request obtiene su propio contexto de base de datos, lo que evita conflictos de concurrencia.

⚙️ 3. Transient

🔹 Crea una nueva instancia cada vez que se solicita el servicio, incluso dentro de la misma petición.

📘 Ejemplo:
```
builder.Services.AddTransient<CustomerService>();
```

🔹 Cuándo usarlo:

Cuando el servicio no guarda estado (stateless).

Ideal para operaciones livianas, rápidas o desechables.

🔸 Ejemplo típico:

Servicios que procesan datos o validaciones temporales.

Servicios que solo ejecutan una lógica de negocio corta.

⚠️ Cuidado:
Demasiados servicios Transient pueden generar más consumo de memoria si se crean muchas instancias repetidamente.

🧮 🔍 Resumen visual

|TIPO| Tipo de ciclo de vida | Cuándo se crea | Cuándo se destruye | 
|---------|-----------------------|----------------|--------------------|
|Sinlgeton| Uso recomendado Singleton |Una sola vez (al iniciar la app) | Al apagar la app 
|Scoped| Configuraciones globales, logging Scoped | Una vez por request HTTP | Al terminar el request |
|Transient| Servicios con datos por petición, DbContext Transient | Cada vez que se inyecta | Cuando se libera el objeto | 
