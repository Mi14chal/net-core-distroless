# Overview

As we do not have an official **.NET** distroless docker image so I decided to create it because it can be created very easily.

# Note
For example purposes, I used **mcr.microsoft.com/dotnet/runtime:3.1.10-buster-slim** image so keep in mind to change to whatever version of **.NET** you need but remember to choose docker image based on Debian.

# Example 1 - Normal version
```
FROM mcr.microsoft.com/dotnet/runtime:3.1.10-buster-slim as base
USER root
RUN cp -a --parents /usr/lib /opt && \
        cp -a --parents /lib/x86_64-linux-gnu /opt && \
        cp -a --parents /lib64 /opt && \
        cp -a --parents /usr/share/dotnet /opt

FROM gcr.io/distroless/base-debian10
COPY --from=base /opt /
USER 1337:1337
ENTRYPOINT ["/usr/share/dotnet/dotnet", "/app/dll.dll"]
```

# Example 2 - Debug version
```
FROM mcr.microsoft.com/dotnet/runtime:3.1.10-buster-slim as base
USER root
RUN cp -a --parents /usr/lib /opt && \
        cp -a --parents /lib/x86_64-linux-gnu /opt && \
        cp -a --parents /lib64 /opt && \
        cp -a --parents /usr/share/dotnet /opt

FROM gcr.io/distroless/base-debian10:debug
COPY --from=base /opt /
USER 1337:1337
ENTRYPOINT ["/usr/share/dotnet/dotnet", "/app/dll.dll"]
```

# Summary
At this point you should be more secure as your docker image:
* won't have a shell unless you use debug version for debugging purposes
* won't be running as root
