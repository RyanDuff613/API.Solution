# Project Roadmap 
## Creating an ASP.NET Core API

## Getting Started...
1. Create a project directory: `$ mkdir CretaceousApi.Solution`
2. Navigate to project directory: `$ cd CretaceousApi.Solution`
3. Use `dotnet new` to scaffold project: `$ dotnet new webapi -o CretaceousApi  --framework net6.0`
4. Create README and .gitignore: `$ touch README.md .gitignore`
5. Complete `.gitignore` and add list of directories and files git should ignore.
    <details><summary><code>CretaceousApi.Solution/.gitignore</code></summary> 

    ```
    bin
    obj
    appsettings.json
    ```
    </details>
6. Initialize Git: `$ git init`

7. Make first commit for `.gitignore` so that Git doesn't track unwanted files.
    - `$ git add .gitignore` 
    - `$ git commit -m "add .gitignore"` 

8. Disable nullable context for the entire project:
    <details><summary><code>CretaceousApi.csproj</code></summary> 

    ```c#
    <Project Sdk="Microsoft.NET.Sdk.Web">

      <PropertyGroup>
        <TargetFramework>net6.0</TargetFramework>
        // comment-out the line below to disable nullable context
        // Note that we can still create a nullable context where 
        // necessary with a directive: #nullable enable
        <!-- <Nullable>enable</Nullable> -->
        <ImplicitUsings>enable</ImplicitUsings>
      </PropertyGroup>

      <ItemGroup>
        <PackageReference Include="Swashbuckle.AspNetCore" Version="6.    2.3" />
      </ItemGroup>

    </Project>
    ```
    </details>

9. Update appsetting.Development.json:
    <details><summary><code>appsettings.Development.json</code></summary> 

    ```json
    {
      "Logging": {
        "LogLevel": {
          "Default": "Information",
          "Microsoft": "Trace",
          "Microsoft.AspNetCore": "Information",
          "Microsoft.Hosting.Lifetime": "Information"
        }
      }
    }
    ```
    </details>

10. Build a Host for the Web API in Program.cs:
    <details><summary><code>Program.cs</code></summary> 

    ```c#
    var builder = WebApplication.CreateBuilder(args);

    // Add services to the container.
    builder.Services.AddControllers();
    builder.Services.AddEndpointsApiExplorer();
    builder.Services.AddSwaggerGen();

    var app = builder.Build();

    // Configure the HTTP request pipeline.
    if (app.Environment.IsDevelopment())
    {
      app.UseSwagger();
      app.UseSwaggerUI();
    }
    else 
    {
      app.UseHttpsRedirection();
    }

    app.UseAuthorization();

    app.MapControllers();

    app.Run();
    ```
    </details>
11. Update launchSettings.json:
    <details><summary><code>Properties/launchSettings.json</code></summary> 

    ```json
    {
      "$schema": "https://json.schemastore.org/launchsettings.json",
      "iisSettings": {
        "windowsAuthentication": false,
        "anonymousAuthentication": true,
        "iisExpress": {
          "applicationUrl": "http://localhost:64697",
          "sslPort": 44346
        }
      },
      "profiles": {
        "CretaceousApi": {
          "commandName": "Project",
          "dotnetRunMessages": true,
          "launchBrowser": true,
          "launchUrl": "swagger",
          "applicationUrl": "https://localhost:5001;http://localhost:5000",
          "environmentVariables": {
            "ASPNETCORE_ENVIRONMENT": "Development"
          }
        }
      },
      "IIS Express": {
        "commandName": "IISExpress",
        "launchBrowser": true,
        "launchUrl": "swagger",
        "environmentVariables": {
          "ASPNETCORE_ENVIRONMENT": "Development"
        }
      }
    }
    ```
    </details>

12. Run the app with `$ dotnet run` and visit `http://localhost:5000/swagger` in a browser to confirm that app is properly configured. The swagger page should indicate that your app is ready to recieve GET requests for the boilerplate WeatherForecast app that was generated by the `dotnet new` command.

13. Remove boilerplate WeatherForecast files: 
    ```
    $ rm WeatherForecast.cs 
    $ rm Controllers/WeatherForecastController.cs
    ```

14. Add the necessary packages for Entity Framework Core and migrations
    ```
    $ dotnet add package Microsoft.EntityFrameworkCore -v 6.0.0
    $ dotnet add package Pomelo.EntityFrameworkCore.MySql -v 6.0.0
    $ dotnet add package Microsoft.EntityFrameworkCore.Design -v 6.0.0
    ```

    - After adding the packages above your `.csproj` should look like this:

      <details><summary><code>Models/Animal.cs</code></summary> 

      ```c#
      <Project Sdk="Microsoft.NET.Sdk.Web">

        <PropertyGroup>
          <TargetFramework>net6.0</TargetFramework>
          <!-- <Nullable>enable</Nullable> -->
          <ImplicitUsings>enable</ImplicitUsings>
        </PropertyGroup>

        <ItemGroup>
          <PackageReference Include="Microsoft.EntityFrameworkCore" Version="6.0.0" />
          <PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="6.0.0">
            <IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>
            <PrivateAssets>all</PrivateAssets>
          </PackageReference>
          <PackageReference Include="Pomelo.EntityFrameworkCore.MySql" Version="6.0.0" />
          <PackageReference Include="Swashbuckle.AspNetCore" Version="6.2.3" />
        </ItemGroup>

      </Project>

      ```
      </details>

## Creating Models and Controller Actions to enable CRUD functionality.
1. Add Application Model and Entity Context Model: 

    ```
    $ mkdir Models
    $ touch Models/Animal.cs Models/CretaceousApiContext.cs
    ```

    <details><summary><code>Models/Animal.cs</code></summary> 

    ```c#
    namespace CretaceousApi.Models
    {
      public class Animal
      {
        public int AnimalId { get; set; }
        public string Name { get; set; }
        public string Species { get; set; }
        public int Age { get; set; }
      }
    }
    ```
    </details>

    <details><summary><code>Models/CretaceousApiContext.cs</code></summary> 
    
    ```c#
    using Microsoft.EntityFrameworkCore;

    namespace CretaceousApi.Models
    {
      public class CretaceousApiContext : DbContext
      {
        public DbSet<Animal> Animals { get; set; }

        public CretaceousApiContext(DbContextOptions<CretaceousApiContext> options) : base(options)
        {
        }
      }
    }
    ```
    </details>

2. Upgrade Program.cs to use MySQL and Context Model

    <details><summary><code>CretaceousApi/Program.cs</code></summary> 

    ```c#
    using CretaceousApi.Models;
    using Microsoft.EntityFrameworkCore;

    var builder = WebApplication.CreateBuilder(args);

    builder.Services.AddControllers();

    builder.Services.AddDbContext<CretaceousApiContext>(
                      dbContextOptions => dbContextOptions
                        .UseMySql(
                          builder.Configuration["ConnectionStrings:DefaultConnection"], 
                          ServerVersion.AutoDetect(builder.Configuration["ConnectionStrings:DefaultConnection"]
                        )
                      )
                    );

    builder.Services.AddEndpointsApiExplorer();
    builder.Services.AddSwaggerGen();

    var app = builder.Build();

    if (app.Environment.IsDevelopment())
    {
        app.UseSwagger();
        app.UseSwaggerUI();
    }
    else 
    {
      app.UseHttpsRedirection();
    }

    app.UseAuthorization();

    app.MapControllers();

    app.Run();
    ```
    </details>

3. Update appsettings.json with database connection string.
    <details><summary><code>appsettings.json</code></summary> 

    ```json
    {
      "Logging": {
        "LogLevel": {
          "Default": "Information",
          "Microsoft.AspNetCore": "Warning"
        }
      },
      "AllowedHosts": "*",
      "ConnectionStrings": {
        "DefaultConnection": "Server=localhost;Port=3306; database=cretaceous_api;uid=[YOUR-USERNAME-HERE];pwd=  [YOUR-PASSWORD-HERE];"
      }
    }
    ```
    </details>

4. Create initial migration and update database
    ```
    $ dotnet ef migrations add Initial
    $ dotnet ef database update
    ```

5. Use the database context model to seed the database:
    <details><summary><code>Models/CretaceousApiContext.cs</code></summary> 

    ```c#
    using Microsoft.EntityFrameworkCore;

    namespace CretaceousApi.Models
    {
      public class CretaceousApiContext : DbContext
      {
        public DbSet<Animal> Animals { get; set; }

        public CretaceousApiContext(DbContextOptions<CretaceousApiContext> options) : base (options)
        {
        }

        protected override void OnModelCreating(ModelBuilder builder)
        {
          builder.Entity<Animal>()
            .HasData(
              new Animal { AnimalId = 1, Name = "Matilda", Species = "Woolly Mammoth", Age = 7 },
              new Animal { AnimalId = 2, Name = "Rexie", Species = "Dinosaur", Age = 10 },
              new Animal { AnimalId = 3, Name = "Matilda", Species = "Dinosaur", Age = 2 },
              new Animal { AnimalId = 4, Name = "Pip", Species = "Shark", Age = 4 },
              new Animal { AnimalId = 5, Name = "Bartholomew", Species = "Dinosaur", Age = 22 }
            );
        }
      }
    }
    
    ```
    </details>

    - `$ dotnet ef migrations add SeedData`
    - `$ dotnet ef database update`

6. Add a Controller for your model and complete controller actions for Reading all instances and individual instances of your Model.
  - `$ touch Controllers/AnimalsController.cs`

    <details><summary><code>Controllers/AnimalsController.cs</code></summary>  
    
    ```c#
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.EntityFrameworkCore;
    using CretaceousApi.Models;

    namespace CretaceousApi.Controllers
    {
      [Route("api/[controller]")]
      [ApiController]
      public class AnimalsController : ControllerBase
      {
        private readonly CretaceousApiContext _db;

        public AnimalsController(CretaceousApiContext db)
        {
          _db = db;
        }

        // GET api/animals
        [HttpGet]
        public async Task<ActionResult<IEnumerable<Animal>>> Get()
        {
          return await _db.Animals.ToListAsync();
        }

        // GET: api/Animals/{id}
        [HttpGet("{id}")]
        public async Task<ActionResult<Animal>> GetAnimal(int id)
        {
          Animal animal = await _db.Animals.FindAsync(id);

          if (animal == null)
          {
            return NotFound();
          }

          return animal;
        }
      }
    }
    ```
    </details>

7. Test app and confirm new endpoints are working. Run app with `$ dotnet run`. In the browser, visit `http://localhost:5000/swagger/index.html` to view Swagger pages. If using Postman, send GET requests to: http://localhost:5000/api/animals and http://localhost:5000/api/animals/1

8. Add a Create action in the controller:
    <details><summary><code>Controllers/AnimalsController.cs</code></summary> 

    ```c#
    ...
    // POST api/animals
    [HttpPost]
    public async Task<ActionResult<Animal>> Post(Animal animal)
    {
      _db.Animals.Add(animal);
      await _db.SaveChangesAsync();
      return CreatedAtAction(nameof(GetAnimal), new { id = animal.AnimalId }, animal);
    }
    ...
    ```
    </details>

9. Add an Update action in the controller:
    <details><summary><code>Controllers/AnimalsController.cs</code></summary> 

    ```c#
    ...
    // PUT: api/Animals/{id}
    [HttpPut("{id}")]
    public async Task<IActionResult> Put(int id, Animal animal)
    {
      if (id != animal.AnimalId)
      {
        return BadRequest();
      }

      _db.Animals.Update(animal);

      try
      {
        await _db.SaveChangesAsync();
      }
      catch (DbUpdateConcurrencyException)
      {
        if (!AnimalExists(id))
        {
          return NotFound();
        }
        else
        {
          throw;
        }
      }

      return NoContent();
    }

    // We create this private method so that our controller actions
    // can easily determine whether or not an "animal" exists in the
    // database. Our new Update/Put method needs this!
    private bool AnimalExists(int id)
    {
      return _db.Animals.Any(e => e.AnimalId == id);
    }
    ...
    ```
    </details>

10. Add an Delete action in the controller:
    <details><summary><code>Controllers/AnimalsController.cs</code></summary> 

    ```c#
    ...
    // DELETE: api/Animals/{id}
    [HttpDelete("{id}")]
    public async Task<IActionResult> DeleteAnimal(int id)
    {
      Animal animal = await _db.Animals.FindAsync(id);
      if (animal == null)
      {
        return NotFound();
      }

      _db.Animals.Remove(animal);
      await _db.SaveChangesAsync();

      return NoContent();
    }
    ...
    ```
    </details>

### Adding Parameters to a GET Request to Support Query Strings

- We can add optional parameters for GET rquests so that users can filter results. In this example, we'll give users the ability to filter by `species`. The URL for a GET request for all animals of the species `dinosaur` would look like this: `http://localhost:5000/api/animals?species=dinosaur`.

  - We just need to update our controller's `Get()` action like this:
    <details><summary><code>Controllers/AnimalsController.cs</code></summary> 

    ```c#
    ...
    // GET: api/Animals
    [HttpGet]
    public async Task<ActionResult<IEnumerable<Animal>>> Get(string species)
    {
      IQueryable<Animal> query = _db.Animals.AsQueryable();

      if (species != null)
      {
        query = query.Where(entry => entry.Species == species);
      }

      return await query.ToListAsync();
    }
    ...
    ```
    </details>

  - If we want to handle multiple search parameters we can do so. In this case we add a `name` parameter so users can filter by `species` and `name`:
    <details><summary><code>Controllers/AnimalsController.cs</code></summary> 

    ```c#
    ...
    // GET api/animals
    [HttpGet]
    public async Task<ActionResult<IEnumerable<Animal>>> Get(string species, string name)
    {
      IQueryable<Animal> query = _db.Animals.AsQueryable();

      if (species != null)
      {
        query = query.Where(entry => entry.Species == species);
      }

      if (name != null)
      {
        query = query.Where(entry => entry.Name == name);
      }

      return await query.ToListAsync();
    }
    ...
    ```
    </details>

  - In this case we create a parameter for the `minimumAge` of an animal. If we want to include a non-string parameter we need to remember that the default value will be 0 if the user does not provide a value. That means we need to check `if (minimumAge > 0)` to determine if a user has included the parameter in their request.
    <details><summary><code>Controllers/AnimalsController.cs</code></summary> 

    ```c#
    ...
    // GET api/animals
    [HttpGet]
    public async Task<ActionResult<IEnumerable<Animal>>> Get(string species, string name)
    {
      IQueryable<Animal> query = _db.Animals.AsQueryable();

      if (species != null)
      {
        query = query.Where(entry => entry.Species == species);
      }

      if (name != null)
      {
        query = query.Where(entry => entry.Name == name);
      }

      return await query.ToListAsync();
    }
    ...
    ```
    </details>

### API Model Validations:
-  Model validations in a .NET API work much the same way they do in .NET MVC apps. 
    - `[Required]`: Makes a field required.
    - `[StringLength]`: Determines a maximum length for a string.
    - `[Range]`: Determines a maximum and minimum for a numeric field.
    - You can include custom `ErrorMessages` that will be sent back to the user when they send an invalid POST/PUT/PATCH request. If you don't include a custom `ErrorMessage` the API will provide it's own.
    - Always be sure to create a new migration and update the database after every change you make to your models.
    - Here's how we might include them in our Animal model:
      <details><summary><code>Controllers/AnimalsController.cs</code></summary> 

      ```c#
      using System.ComponentModel.DataAnnotations;

      namespace CretaceousApi.Models
      {
        public class Animal
        {
          public int AnimalId { get; set; }
          [Required]
          [StringLength(20)]
          public string Name { get; set; }
          [Required]
          public string Species { get; set; }
          [Required]
          [Range(0, 200, ErrorMessage = "Age must be between 0 and 200.")]
          public int Age { get; set; }
        }
      }
      ```
      </details>




