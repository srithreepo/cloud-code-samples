FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /src
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY . .
RUN dotnet restore helloworld.csproj
RUN dotnet build "./helloworld.csproj" -c Debug -o /out

FROM build AS publish
RUN dotnet publish helloworld.csproj -c Debug -o /out


# Building final image used in running container
FROM base AS final
# Installing procps on the container to enable debugging of .NET Core
RUN apt-get update \
    && apt-get install -y unzip procps
WORKDIR /src
COPY --from=publish /out .
ENV ASPNETCORE_URLS=http://*:8080

ENTRYPOINT ["dotnet", "helloworld.dll"]
