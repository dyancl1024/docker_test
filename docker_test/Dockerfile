FROM microsoft/dotnet:2.2-aspnetcore-runtime AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM microsoft/dotnet:2.2-sdk AS build
WORKDIR /src
COPY ["docker_test/docker_test.csproj", "docker_test/"]
RUN dotnet restore "docker_test/docker_test.csproj"
COPY . .
WORKDIR "/src/docker_test"
RUN dotnet build "docker_test.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "docker_test.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "docker_test.dll"]