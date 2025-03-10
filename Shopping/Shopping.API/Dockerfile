# Base 이미지
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

# Build 스테이지
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# Shopping.API.csproj만 먼저 복사 후 restore 실행
COPY ./Shopping.API.csproj Shopping.API/
WORKDIR /src/Shopping.API
RUN dotnet restore "Shopping.API.csproj"

# 전체 프로젝트 복사 및 빌드 실행
COPY . .
RUN dotnet build "./Shopping.API/Shopping.API.csproj" -c $BUILD_CONFIGURATION -o /app/build -v d

# Publish 스테이지
FROM build AS publish
RUN dotnet publish "Shopping.API.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Final 스테이지
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Shopping.API.dll"]
