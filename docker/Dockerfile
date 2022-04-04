#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:3.1 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:3.1 AS build

WORKDIR /src
COPY ["keycloak/src/Keycloak.Net/Keycloak.Net.csproj", "keycloak/Keycloak.Net/"]
COPY ["src/registration/CatenaX.NetworkServices.Registration.Service/CatenaX.NetworkServices.Registration.Service.csproj", "registration/CatenaX.NetworkServices.Registration.Service/"]
COPY ["src/consent/CatenaX.NetworkServices.Consent.Library.Data/CatenaX.NetworkServices.Consent.Library.Data.csproj", "consent/CatenaX.NetworkServices.Consent.Library.Data/"]
COPY ["src/keycloak/CatenaX.NetworkServices.Keycloak.Authentication/CatenaX.NetworkServices.Keycloak.Authentication.csproj", "keycloak/CatenaX.NetworkServices.Keycloak.Authentication/"]
COPY ["src/keycloak/CatenaX.NetworkServices.Keycloak.Factory/CatenaX.NetworkServices.Keycloak.Factory.csproj", "keycloak/CatenaX.NetworkServices.Keycloak.Factory/"]
COPY ["src/mailing/CatenaX.NetworkServices.Mailing.SendMail/CatenaX.NetworkServices.Mailing.SendMail.csproj", "mailing/CatenaX.NetworkServices.Mailing.SendMail/"]
COPY ["src/mailing/CatenaX.NetworkServices.Mailing.Template/CatenaX.NetworkServices.Mailing.Template.csproj", "mailing/CatenaX.NetworkServices.Mailing.Template/"]
COPY ["src/provisioning/CatenaX.NetworkServices.Provisioning.Library/CatenaX.NetworkServices.Provisioning.Library.csproj", "provisioning/CatenaX.NetworkServices.Provisioning.Library/"]
COPY . .
WORKDIR "/registration/CatenaX.NetworkServices.Registration.Service"
RUN dotnet build "CatenaX.NetworkServices.Registration.Service.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "CatenaX.NetworkServices.Registration.Service.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "CatenaX.NetworkServices.Registration.Service.dll"]