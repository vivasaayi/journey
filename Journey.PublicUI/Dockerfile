FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["Journey.PublicUI/Journey.PublicUI.csproj", "Journey.PublicUI/"]
RUN dotnet restore "Journey.PublicUI/Journey.PublicUI.csproj"
COPY . .
WORKDIR "/src/Journey.PublicUI"
RUN dotnet build "Journey.PublicUI.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "Journey.PublicUI.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Journey.PublicUI.dll"]
