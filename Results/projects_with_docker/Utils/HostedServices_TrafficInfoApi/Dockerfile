FROM mcr.microsoft.com/dotnet/core/aspnet:3.1 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS build
WORKDIR /src
COPY ["HostedServices/TrafficInfoApi/TrafficInfoApi.csproj", "HostedServices/TrafficInfoApi/"]
COPY ["SharedKernel/Contracts/Contracts.csproj", "SharedKernel/Contracts/"]
RUN dotnet restore "HostedServices/TrafficInfoApi/TrafficInfoApi.csproj"
COPY . .
WORKDIR "/src/HostedServices/TrafficInfoApi"
RUN dotnet build "TrafficInfoApi.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "TrafficInfoApi.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "TrafficInfoApi.dll", "--environment=Docker"]