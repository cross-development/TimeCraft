FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["TimeCraft.Server/TimeCraft.Server.csproj", "TimeCraft.Server/"]
RUN dotnet restore "./TimeCraft.Server/TimeCraft.Server.csproj"
COPY . .
WORKDIR "/src/TimeCraft.Server"
RUN dotnet build "./TimeCraft.Server.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./TimeCraft.Server.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "TimeCraft.Server.dll"]