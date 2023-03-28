# Copyright 2021 Google LLC
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# https://mcr.microsoft.com/v2/dotnet/sdk/tags/list
FROM mcr.microsoft.com/dotnet/sdk:7.0.202@sha256:f712881bafadf0e56250ece1da28ba2baedd03fb3dd49a67f209f9d0cf928e81 as builder
WORKDIR /app
COPY cartservice.csproj .
RUN dotnet restore cartservice.csproj -r linux-musl-x64
COPY . .
RUN dotnet publish cartservice.csproj -p:PublishSingleFile=true -r linux-musl-x64 --self-contained true -p:PublishTrimmed=True -p:TrimMode=Link -c release -o /cartservice --no-restore

# Download the latest version of the tracer but don't install yet
RUN TRACER_VERSION=$(curl -s \https://api.github.com/repos/DataDog/dd-trace-dotnet/releases/latest | grep tag_name | cut -d '"' -f 4 | cut -c2-) \
    && curl -Lo /tmp/datadog-dotnet-apm.deb https://github.com/DataDog/dd-trace-dotnet/releases/download/v${TRACER_VERSION}/datadog-dotnet-apm_${TRACER_VERSION}_amd64.deb

# https://mcr.microsoft.com/v2/dotnet/runtime-deps/tags/list
FROM mcr.microsoft.com/dotnet/runtime-deps:7.0.4-alpine3.16-amd64@sha256:8ac1b8d28e208c65891f7309984e422d973752fb88481da2384607e55c178176 as without-grpc-health-probe-bin

WORKDIR /app
COPY --from=builder /cartservice .
EXPOSE 7070
ENV DOTNET_EnableDiagnostics=0 \
    ASPNETCORE_URLS=http://*:7070
USER 1000
ENTRYPOINT ["/app/cartservice"]

FROM without-grpc-health-probe-bin
USER root
# renovate: datasource=github-releases depName=grpc-ecosystem/grpc-health-probe
ENV GRPC_HEALTH_PROBE_VERSION=v0.4.17
RUN wget -qO/bin/grpc_health_probe https://github.com/grpc-ecosystem/grpc-health-probe/releases/download/${GRPC_HEALTH_PROBE_VERSION}/grpc_health_probe-linux-amd64 && \
    chmod +x /bin/grpc_health_probe

# Copy the tracer from build target
COPY --from=builder /tmp/datadog-dotnet-apm.deb /tmp/datadog-dotnet-apm.deb

# Install the tracer
ENV DD_APM_VERSION=1.9.0
RUN mkdir -p /opt/datadog
RUN curl -L https://github.com/DataDog/dd-trace-dotnet/releases/download/v${DD_APM_VERSION}/datadog-dotnet-apm-${DD_APM_VERSION}.tar.gz |  tar xzf - -C /opt/datadog
RUN mkdir -p /var/log/datadog

# Enable the tracer
ENV CORECLR_ENABLE_PROFILING=1
ENV CORECLR_PROFILER={846F5F1C-F9AE-4B07-969E-05C26BC060D8}
ENV CORECLR_PROFILER_PATH=/opt/datadog/Datadog.Trace.ClrProfiler.Native.so
ENV DD_DOTNET_TRACER_HOME=/opt/datadog
ENV DD_INTEGRATIONS=/opt/datadog/integrations.json

USER 1000