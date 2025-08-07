FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /app

COPY WebAdminPresensi/WebAdminPresensi.csproj ./WebAdminPresensi/
COPY WebAdminPresensi.sln ./
RUN dotnet restore WebAdminPresensi.sln

COPY . .
RUN dotnet publish WebAdminPresensi/WebAdminPresensi.csproj -c Release -o /app/out

FROM mcr.microsoft.com/dotnet/aspnet:8.0
WORKDIR /app
COPY --from=build /app/out .

ENV ASPNETCORE_URLS=http://+:8080
EXPOSE 8080

ENTRYPOINT ["dotnet", "WebAdminPresensi.dll"]
