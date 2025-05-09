FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
WORKDIR /app
EXPOSE 5136

ENV ASPNETCORE_URLS=http://+:5136

USER app
FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG configuration=Release
WORKDIR /src
COPY ["Blazor-WebApp-Docker/Blazor-WebApp-Docker.csproj", "Blazor-WebApp-Docker/"]
RUN dotnet restore "Blazor-WebApp-Docker/Blazor-WebApp-Docker.csproj"
COPY . .
WORKDIR "/src/Blazor-WebApp-Docker"
RUN dotnet build "Blazor-WebApp-Docker.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "Blazor-WebApp-Docker.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Blazor-WebApp-Docker.dll"]
