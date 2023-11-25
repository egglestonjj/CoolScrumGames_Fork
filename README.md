<img src="./CoolScrumGames/wwwroot/images/diamond.png" align="right"
     alt="Size Limit logo by Anton Lovchikov" width="200" height="200">

# CoolScrumGames
CoolScrumGames Repository for SE2

"Games made by ETSU students, played by ETSU students."

## Docker Support

This project is designed to be run on a persistant instance, preferably a Docker container.

The project can be containerized by hand, but must have the HTTP and HTTPS ports opened.

[It can also be containerized using Visual Studio's Docker Support](https://learn.microsoft.com/en-us/visualstudio/containers/container-build?view=vs-2022).
<details>
  <summary markdown="span">Example Dockerfile</summary>

```sh
FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["CoolScrumGames/CoolScrumGames.csproj", "CoolScrumGames/"]
RUN dotnet restore "CoolScrumGames/CoolScrumGames.csproj"
COPY . .
WORKDIR "/src/CoolScrumGames"
RUN dotnet build "CoolScrumGames.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "CoolScrumGames.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "CoolScrumGames.dll"]
```
</details>
