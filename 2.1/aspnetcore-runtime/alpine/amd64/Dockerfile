FROM microsoft/dotnet-nightly:2.1-runtime-deps-alpine

# Workaround https://github.com/aspnet/libuv-package/issues/23
# Install libuv globally and link it so coreclr can laod it
RUN apk add --no-cache libuv \
    && ln -s /usr/lib/libuv.so.1 /usr/lib/libuv.so

# Install ASP.NET Core
ENV ASPNETCORE_VERSION 2.1.0-preview2-30552

RUN apk add --no-cache --virtual .build-deps \
        openssl \
    && wget -O aspnetcore.tar.gz https://dotnetcli.blob.core.windows.net/dotnet/aspnetcore/Runtime/$ASPNETCORE_VERSION/aspnetcore-runtime-$ASPNETCORE_VERSION-alpine.3.6-x64.tar.gz \
    && aspnetcore_sha512='e3dcf356aed8a816d574858ff751044d659b76fedb391c7d86ebc82056a0a898d3f331553d687e330702daaf204589c1f601b6a5678d7c45e55550a958874d29' \
    && echo "$aspnetcore_sha512  aspnetcore.tar.gz" | sha512sum -c - \
    && mkdir -p /usr/share/dotnet \
    && tar -zxf aspnetcore.tar.gz -C /usr/share/dotnet \
    && rm aspnetcore.tar.gz \
    && ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet \
    && apk del .build-deps