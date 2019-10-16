![titulo](/docs/titulo.JPG)

# NetCore3-Heroku

Deploying a .Net Core 3 web API to Heroku by using Docker.

## Technologies

- [.Net Core 3](https://docs.microsoft.com/pt-br/dotnet/core/whats-new/dotnet-core-3-0)
- [Docker](https://www.docker.com/get-started)
- [Heroku](https://www.heroku.com/)

## Before start

This guide is directed for those who have a mininum experience with Docker and a proper environment to run it.

## Topics

- [Heroku](#heroku)
- [.Net Core 3](#net-core-3)
- [Docker](#docker)

#3# Heroku

Heroku is a cloud platform that allows applications be hosted at will. It's mostly used for web APIs.
Go to [Heroku website](https://signup.heroku.com/login) and sign up or sign in.

In the Dashboard, click on "Create new app".

![heroku01](/images/uploads/netcore3_heroku_heroku01.JPG)

Or

![heroku01](/images/uploads/netcore3_heroku_heroku01_2.JPG)

Name the app and click on "Create app"

![heroku02](/images/uploads/netcore3_heroku_heroku02.JPG)

In your machine, install the latest version of Heroku CLI [here](https://devcenter.heroku.com/articles/heroku-cli)

### .Net Core 3

Download the web API from this GitHub repository: [NetCore3-MySQL](https://github.com/lucianopereira86/NetCore3-MySQL).  
Follow the instructions from this [article](https://dev.to/lucianopereira86/net-core-web-api-part-2-mysql-3bje) to make the required database connection.

In the _api_ folder, create manually a file called "Dockerfile" without extension and edit it with this lines:

```batch
FROM mcr.microsoft.com/dotnet/core/sdk:3.0-buster AS build
WORKDIR /app
COPY ./ ./

RUN dotnet publish -c Release -o out

FROM mcr.microsoft.com/dotnet/core/aspnet:3.0-buster-slim
WORKDIR /app
COPY --from=build /app/out .
CMD ASPNETCORE_URLS=http://*:$PORT dotnet NetCore3WebAPI.dll
```

### Docker

In the following commands, replace [MYAPP] by the app's name created on Heroku.
Inside the publish folder, run in the terminal:

```batch
docker build -t [MYAPP] .
```

This will create a image for the .Net Core 3 project with the same name of your app.
To authenticate with Heroku, run these commands and follow the instructions:

```batch
heroku login
heroku container:login
```

Tag the image:

```batch
docker tag [MYAPP] registry.heroku.com/[MYAPP]/web
```

Build the Dockerfile and push the image:

```batch
heroku container:push web -a [MYAPP]
```

Release the image to deploy the app:

```batch
heroku container:release web -a [MYAPP]
```

Access the URL of your app with Swagger:

```batch
https://[MYAPP].herokuapp.com/swagger/index.html
```

![heroku03](/images/uploads/netcore3_heroku_heroku03.JPG)

## Conclusion

It was very simple to publish on Heroku. Docker was used only to build the image.
