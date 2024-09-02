#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["ReactTsWithDotnet.Server/ReactTsWithDotnet.Server.csproj", "ReactTsWithDotnet.Server/"]
RUN dotnet restore "./ReactTsWithDotnet.Server/ReactTsWithDotnet.Server.csproj"
COPY . .
WORKDIR "/src/ReactTsWithDotnet.Server"
RUN dotnet build "./ReactTsWithDotnet.Server.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./ReactTsWithDotnet.Server.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ReactTsWithDotnet.Server.dll"]