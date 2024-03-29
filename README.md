# Example Repo for Section #6 - Building an API

#### By: Ryan Duff

## Description
This project is called the "Cretaceous Park API". It serves as an example project for creating an API with ASP.NET Core. 

## Technologies Used
- C#
- Entity Framework Core
- ASP.NET Core

## Setup/Installation Requirements
### Install Tools

Install the tools that are introduced in [this series of lessons on LearnHowToProgram.com](https://www.learnhowtoprogram.com/c-and-net/getting-started-with-c).

If you have not already, install the `dotnet-ef` tool by running the following command in your terminal:

```
dotnet tool install --global dotnet-ef --version 6.0.0
```

### Set Up and Run Project

1. Clone this repo.
2. Open the terminal and navigate to this project's production directory called "CretaceousApi".
3. Within the production directory "CretaceousApi", create two new files: `appsettings.json` and `appsettings.Development.json`.
4. Within `appsettings.json`, put in the following code. Make sure to replacing the `uid` and `pwd` values in the MySQL database connection string with your own username and password for MySQL. For the LearnHowToProgram.com lessons, we always assume the `uid` is `root` and the `pwd` is `epicodus`.

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
    "DefaultConnection": "Server=localhost;Port=3306;database=cretaceous_api;uid=[YOUR_USERNAME];pwd=[YOUR_MYSQL_PASSWORD];"
  }
}
```

5. Within `appsettings.Development.json`, add the following code:

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

6. Create the database using the migrations in the Cretaceous Park API project. Open your shell (e.g., Terminal or GitBash) to the production directory "CretaceousApi", and run `dotnet ef database update`. You may need to run this command for each of the branches in this repo. 
7. Within the production directory "CretaceousApi", run `dotnet run` in the command line to start the project server. 
9. Use your program of choice to make API calls. CORS policy is set in `Program.cs` and is configured to allow calls from _http://localhost:3000_. If you're developing a client side application to consume data from this API be sure to update the CORS policy to allow calls from the host location of the server your app is running on. 

## Project Roadmap, Notes and Documentation
- [Project Roadmap and Notes](https://github.com/RyanDuff613/API.Solution/blob/main/Notes.md)
- [API Documentation](https://github.com/RyanDuff613/API.Solution/blob/main/ApiDocumentationExample.md)

## Known Bugs
- Please visit this projects [GitHub repository](https://github.com/RyanDuff613/API.Solution.git) to submit Issues and Pull Requests.

## License
MIT License

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the “Software”), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

Copyright (c) Ryan Duff 2023 
