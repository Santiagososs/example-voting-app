FROM --platform=${BUILDPLATFORM} mcr.microsoft.com/dotnet/sdk:7.0-alpine AS build
ARG TARGETPLATFORM
ARG TARGETARCH
ARG BUILDPLATFORM
RUN echo "I am running on $BUILDPLATFORM, building for $TARGETPLATFORM"

RUN apk update && apk upgrade --no-cache \
    && apk add --no-cache \
       busybox=1.36.1-r19 \
       libcrypto3=3.1.6-r0 \
       libssl3=3.1.6-r0

WORKDIR /source
COPY *.csproj .
RUN dotnet restore -a $TARGETARCH

COPY . .
RUN dotnet publish -c release -o /app -a $TARGETARCH --self-contained false --no-restore

FROM mcr.microsoft.com/dotnet/aspnet:7.0-alpine AS base

RUN apk update && apk upgrade --no-cache \
    && apk add --no-cache \
       busybox=1.36.1-r19 \
       libcrypto3=3.1.6-r0 \
       libssl3=3.1.6-r0

WORKDIR /app
COPY --from=build /app .
ENTRYPOINT ["dotnet", "Worker.dll"]
