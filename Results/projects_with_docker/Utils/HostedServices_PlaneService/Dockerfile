FROM mcr.microsoft.com/dotnet/core/runtime:3.1 AS base
WORKDIR /app
EXPOSE 81

FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS build
WORKDIR /src
COPY ["HostedServices/PlaneService/PlaneService.csproj", "HostedServices/PlaneService/"]
COPY ["SharedKernel/Utils/Utils.csproj", "SharedKernel/Utils/"]
COPY ["SharedKernel/Contracts/Contracts.csproj", "SharedKernel/Contracts/"]
RUN dotnet restore "HostedServices/PlaneService/PlaneService.csproj"
COPY . .
WORKDIR "/src/HostedServices/PlaneService"
RUN dotnet build "PlaneService.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "PlaneService.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "PlaneService.dll", "--environment=Docker"]