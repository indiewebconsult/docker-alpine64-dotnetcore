### ASP.NET Core SDK 2.2.202
FROM indiewebconsult/dotnetcore-runtime:2.2.3-alpine
# Disable the invariant mode (set in base image)
RUN apk add --no-cache icu-libs libintl

ENV DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=false \
    LC_ALL=en_US.UTF-8 \
    LANG=en_US.UTF-8

# Install .NET Core SDK
ENV DOTNET_SDK_VERSION 2.2.202

RUN apk add --no-cache --virtual .build-deps \
        openssl \
    && curl https://dotnetcli.blob.core.windows.net/dotnet/Sdk/$DOTNET_SDK_VERSION/dotnet-sdk-$DOTNET_SDK_VERSION-linux-musl-x64.tar.gz -o dotnet.tar.gz \
    && dotnet_sha512='62ff6177e91a28b994ccdd90c33643c768840ac881a8b47ca2ebb3b546134c21917e6b9e9321db02855e9c3c84c3ed810969e82a3ab21cb4b5ddd3acce285beb' \
    && rm -rf /usr/bin/dotnet \
    && echo "$dotnet_sha512  dotnet.tar.gz" | sha512sum -c - \
    && tar -C /usr/share/dotnet -xzf dotnet.tar.gz \
    && ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet \
    && rm dotnet.tar.gz \
    && apk del .build-deps

# Enable correct mode for dotnet watch (only mode supported in a container)
ENV DOTNET_USE_POLLING_FILE_WATCHER=true \ 
    # Skip extraction of XML docs - generally not useful within an image/container - helps performance
    NUGET_XMLDOC_MODE=skip

# Trigger first run experience by running arbitrary cmd to populate local package cache
RUN dotnet help