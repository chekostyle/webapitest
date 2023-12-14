FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["WebApiTestLinux/WebApiTestLinux.csproj", "WebApiTestLinux/"]
RUN dotnet restore "WebApiTestLinux/WebApiTestLinux.csproj"
COPY . .
WORKDIR "/src/WebApiTestLinux"
RUN dotnet build "WebApiTestLinux.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "WebApiTestLinux.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WebApiTestLinux.dll"]
