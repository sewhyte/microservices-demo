# Forked Notes
This repo has src changes to build each container with Datadog Instrumentation.

Each service directory contains a Makefile for building the container and pushing to samew/<image_name> on docker hub.

# Service Changes
## adservice (java)
1. Deploy dd-agent into container image.
1. Modify `JAVA_OPTS` environment variable to include the datadog agent.

## cartservice (.net)
Not currently working.

## checkoutservice (go)
1. Remove GCP-native tracing and profiling.
1. Create unary and stream inteceptors using the datadog grpctrace library.
1. Change the `grpc.NewServer` invocation to use the datadog interceptors instead of the GCP ones.
1. Change `DialContext` to use grpc client interceptors instead of the GCP ones.

## currencyservice (nodejs)
1. Add `dd-trace` package as a requirement.
1. Update ENTRYPOINT to contain `--require dd-trace/init`.

## emailservice (python)
1. Add `ddtrace` package via pip (in Dockerfile)
1. Change ENTRYPOINT to use `ddtrace-run` wrapper to invoke python script.

## frontend
Not instrumented.

## loadgenerator
Not instrumented.

## paymentservice (nodejs)
1. Add `dd-trace` package as a requirement.
1. Update ENTRYPOINT to contain `--require dd-trace/init`.

## productcatalogservice (go)
1. Remove GCP-native tracing and profiling.
1. Create unary and stream inteceptors using the datadog grpctrace library.
1. Change the `grpc.NewServer` invocation to use the datadog interceptors instead of the GCP ones.

## recommentationservice (python)
1. Add `ddtrace` package via pip (in Dockerfile)
1. Change ENTRYPOINT to use `ddtrace-run` wrapper to invoke python script.

## shippingservice (go)
1. Remove GCP-native tracing and profiling.
1. Create unary and stream inteceptors using the datadog grpctrace library.
1. Change the `grpc.NewServer` invocation to use the datadog interceptors instead of the GCP ones.