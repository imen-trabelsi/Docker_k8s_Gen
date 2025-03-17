FROM mcr.microsoft.com/dotnet/core/runtime:3.1 AS base
WORKDIR /app
EXPOSE 81

FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS build
WORKDIR /src
COPY ["HostedServices/AirportService/AirportService.csproj", "HostedServices/AirportService/"]
COPY ["SharedKernel/Utils/Utils.csproj", "SharedKernel/Utils/"]
COPY ["SharedKernel/Contracts/Contracts.csproj", "SharedKernel/Contracts/"]
RUN dotnet restore "HostedServices/AirportService/AirportService.csproj"
COPY . .
WORKDIR "/src/HostedServices/AirportService"
RUN dotnet build "AirportService.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "AirportService.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "AirportService.dll", "--environment=Docker"]