# syntax=docker/dockerfile:1
FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build-env
WORKDIR /App

# Copy everything else
COPY . ./

# Copy csproj and restore as distinct layers
RUN dotnet restore "/App/SpirePdfConvertPdfToJpgPOC_WebApp/SpirePdfConvertPdfToJpgPOC_WebApp.csproj"

# Build
RUN dotnet publish SpirePdfConvertPdfToJpgPOC_WebApp/SpirePdfConvertPdfToJpgPOC_WebApp.csproj -c Release -o out

# Build runtime image
FROM mcr.microsoft.com/dotnet/aspnet:5.0
RUN apt-get update && apt-get install -y apt-utils libgdiplus libc6-dev
RUN groupadd -g 1000 -r noviadmin && useradd --no-log-init -u 1000 -r -g noviadmin noviadmin
WORKDIR /App
COPY --from=build-env /App/out .
RUN chown -R noviadmin:noviadmin /App
EXPOSE 8080
USER noviadmin
ENTRYPOINT ["dotnet", "SpirePdfConvertPdfToJpgPOC_WebApp.dll"]
