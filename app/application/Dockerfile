# Use .NET Core runtime as base image
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 80

# Use .NET SDK to build the app
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Copy the .csproj file first
COPY ["application.csproj", "."]
RUN dotnet restore "application.csproj"

# Copy everything else and build the project
COPY . .
RUN dotnet build "application.csproj" -c Release -o /app/build
RUN dotnet publish "application.csproj" -c Release -o /app/publish

# Use a smaller runtime image for the final stage
FROM base AS final
WORKDIR /app
COPY --from=build /app/publish .
ENTRYPOINT ["dotnet", "application.dll"]

