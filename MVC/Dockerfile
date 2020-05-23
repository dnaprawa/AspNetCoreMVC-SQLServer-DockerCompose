FROM mcr.microsoft.com/dotnet/core/sdk:2.2 AS build-env
WORKDIR /app

# Copy csproj and restore as distinct layers
COPY *.csproj ./
RUN dotnet restore

# Copy everything else and build
COPY . ./
RUN dotnet publish -c Release -o out

# Build runtime image
FROM mcr.microsoft.com/dotnet/core/aspnet:2.2
WORKDIR /app
COPY --from=build-env /app/out .

RUN groupadd -g 500 dotnet && \
    useradd -u 500 -g dotnet dotnet \
    && chown -R dotnet:dotnet /app

USER dotnet:dotnet

ENV ASPNETCORE_URLS http://*:5000

ENTRYPOINT ["dotnet", "MVC.dll"]