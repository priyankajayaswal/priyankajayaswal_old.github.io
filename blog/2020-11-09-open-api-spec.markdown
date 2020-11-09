---
title: "OpenAPISpec + Productivity"
layout: post
date: 2020-09-10 23:02
image: \assets\images\blog\2020-11-09-open-api-spec.markdown.PNG
headerImage: false
projects: true
hidden: false # don't count this post in blog pagination
tag:
- CSharp
- Swagger
- OpenApiSpecs
- DeveloperProductivity
category: dev_productivity
author: priyankajayaswal
description: Adding swagger creation anf UI support for .NET Code Service
---

# Introduction

This blog talks about how to use [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle.AspNetCore/) to enable using swagger.json creation and swagger UI creation in .NET Core 3.1 services.
NOTE:
 - OpenAPISpec/Swagger can be used interchangibly, though OpenAPISpec is preferred.

## [OpenAPI Specification](https://swagger.io/)

- In 2015, under the OpenAPI Initiative, OpenAPI Specification was donated to Linux Foundation
- With this specification, a RESTful interface for the developed APIs is created which can be easily consumed. This is created by effectively mapping resources and operations.

# Benefits of adding swagger/OpenApiSpecs
- With several tools created to enable swagger creation at development time, this becomes a visual representation of the API development and progress.
- One can have a look at the swagger/openAPISpecs file which is created and have a clear understanding on what's the API structure looks like. This is highly beneficial when collaborating with remote teams wtc., where a frequent review is needed.
- Once the swagger files are created and are validated to be free from invalid syntaxes, this can be used in client generation and eventually create powerful SDKs on top of it across several languages (C#, Python, Java, TypeScript, Go, Ruby and even Powershell) using several tools like
    - [Autorest](https://github.com/Azure/autorest/)
    - [OpenAPI Generator](https://github.com/OpenAPITools/openapi-generator)
    - etc.

# Working demo:

- Repo that contains swagger file creation and UI enabled in a .NET Core 3.1 service - [Refer](https://github.com/priyankajayaswal1/SwaggerDummy)
- Clone the repo.
- Run `dotnet build SwaggerDummy.csproj`

# Steps:
1. Install `Swashbuckle.AspNetCore` (use > 5.0.0 since it supports OpenApi)
2. In `Startup.cs` enable swagger by adding following in `ConfigureServices`
    ```
        services.AddSwaggerGen(setupAction =>
            {
                setupAction.SwaggerDoc("<PathToHostSwagger>", new Microsoft.OpenApi.Models.OpenApiInfo
                {
                    Title = "Your preferred title.",
                    Version = "1.0",
                });
            });

    ```
3. In `Startup.cs` also add following to `Configure` after `app.UseHttpsRedirection()`
    ```
        app.UseSwagger(c =>
            {
                c.SerializeAsV2 = true; // Can use when intention is to create OpenAPISpec 2.0
            });
    ```
3. With above code, you are enabling swagger file creation for your service and to be hosted at '<your-service-url>/swagger/<PathToHostSwagger>/swagger.json`.
4. The title and version is that for the swagger file that would be created.
5. Enable Swagger UI by adding following in `Configure` function after the above added snippet.
    ```

            app.UseSwaggerUI(setupAction=>
            {
                setupAction.SwaggerEndpoint("/swagger/"<PathToHostSwagger>/swagger.json", "<DfinitionName>");
            });
    ```
6. Note: the SwaggerEndpoint defined above is same where the swagger.json file got hosted on creation i.e., `/swagger/"<PathToHostSwagger>/swagger.json`
7. The UI looks like ![UI](..\assets\images\blog\2020-11-09-blog.markdown.PNG)
