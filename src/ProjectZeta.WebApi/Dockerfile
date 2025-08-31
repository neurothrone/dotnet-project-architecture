FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["ProjectZeta.WebApi/ProjectZeta.WebApi.csproj", "ProjectZeta.WebApi/"]
RUN dotnet restore "ProjectZeta.WebApi/ProjectZeta.WebApi.csproj"
COPY . .
WORKDIR "/src/ProjectZeta.WebApi"
RUN dotnet build "./ProjectZeta.WebApi.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./ProjectZeta.WebApi.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ProjectZeta.WebApi.dll"]
