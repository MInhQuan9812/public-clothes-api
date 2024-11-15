FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

COPY ./certificate.pfx /https/certificate.pfx

# Image build
FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["clothes.api/clothes.api.csproj", "clothes.api/"]
RUN dotnet restore "./clothes.api/clothes.api.csproj"
COPY . .
WORKDIR "/src/clothes.api"
RUN dotnet build "./clothes.api.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Publish image
FROM build AS publish
RUN dotnet publish "./clothes.api.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppH>

# Runtime image
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "clothes.api.dll"]

