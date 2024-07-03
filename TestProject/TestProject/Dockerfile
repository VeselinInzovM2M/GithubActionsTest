# Use an official .NET runtime as a parent image
FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

# Use an official .NET SDK image as a parent image
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["TestProject/TestProject/TestProject.csproj", "TestProject/"]
RUN dotnet restore "TestProject/TestProject.csproj"
COPY . .
WORKDIR "/src/TestProject"
RUN dotnet build "TestProject.csproj" -c Release -o /app/build

# Publish the project
FROM build AS publish
RUN dotnet publish "TestProject.csproj" -c Release -o /app/publish /p:UseAppHost=false

# Use the base image to run the application
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "TestProject.dll"]
