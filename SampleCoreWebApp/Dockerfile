

FROM microsoft/dotnet:2.1-aspnetcore-runtime-nanoserver-sac2016 AS base
WORKDIR /app


# copy csproj and restore as distinct layers
FROM microsoft/dotnet:2.1-sdk-nanoserver-sac2016 AS build
WORKDIR /src
COPY ["SampleCoreWebApp/SampleCoreWebApp.csproj", "SampleCoreWebApp/"]
RUN dotnet restore "SampleCoreWebApp/SampleCoreWebApp.csproj"

# copy everything else and build
COPY . .
WORKDIR "/src/SampleCoreWebApp"
RUN dotnet build "SampleCoreWebApp.csproj" -c Release -o /app

# publish 
FROM build AS publish
RUN dotnet publish "SampleCoreWebApp.csproj" -c Release -o /app

# build runtime image
FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "SampleCoreWebApp.dll"]