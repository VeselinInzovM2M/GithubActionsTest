FROM mcr.microsoft.com/dotnet/aspnet:6.0-nanoserver-1809 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0-nanoserver-1809 AS build
WORKDIR /src
COPY ["TestProject/TestProject/TestProject.csproj", "TestProject/"]
RUN dotnet restore "./TestProject/TestProject/TestProject.csproj"
COPY . .
WORKDIR "/src/TestProject"
RUN dotnet build "./TestProject.csproj" -c Release -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./TestProject.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "TestProject.dll"]
