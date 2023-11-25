<p align="center">
  <img src="./CoolScrumGames/wwwroot/images/diamond.png" width="500">
</p>

# CoolScrumGames
CoolScrumGames Repository for SE2

"Games made by ETSU students, played by ETSU students."

---

## Docker Support

This project is designed to be run on a persistant instance, preferably a Docker container.
The project can be containerized by hand, but must have the HTTP and HTTPS ports opened.
It can also be containerized using Visual Studio's Docker Support
<details>
  <summary markdown="span">Creating a Dockerfile</summary>
  
1. Open the project in Visual Studio
2. Right click the project
4. Highlight Add
5. Select Docker Support

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
