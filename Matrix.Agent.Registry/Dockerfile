# prepare final image
FROM mcr.microsoft.com/dotnet/core/aspnet:2.2 AS base
WORKDIR /app
EXPOSE 80

# prepare build image
FROM mcr.microsoft.com/dotnet/core/sdk:2.2 AS build
WORKDIR /src

COPY Matrix.Agent.Registry/Matrix.Agent.Registry.csproj Matrix.Agent.Registry/
COPY Matrix.Agent.Registry.Business/Matrix.Agent.Registry.Business.csproj Matrix.Agent.Registry.Business/
COPY Matrix.Agent.Registry.Database/Matrix.Agent.Registry.Database.csproj Matrix.Agent.Registry.Database/
COPY Matrix.Agent.Registry.Model/Matrix.Agent.Registry.Model.csproj Matrix.Agent.Registry.Model/
COPY . .

# restore
RUN dotnet restore "Matrix.Agent.Registry/Matrix.Agent.Registry.csproj" -s http://nuget.matrix.paramg.com:8888/v3/index.json

# build
WORKDIR /src/Matrix.Agent.Registry
RUN dotnet build "Matrix.Agent.Registry.csproj" -c Release -o /app

# publish
FROM build AS publish
RUN dotnet publish "Matrix.Agent.Registry.csproj" -c Release -o /app

# finalize
FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "Matrix.Agent.Registry.dll"]