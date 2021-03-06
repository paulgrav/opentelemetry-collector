# Changelog

## Unreleased

## 🛑 Breaking changes 🛑

- Use consumer for sender interface, remove unnecessary receiver address from Runner #1941
- Enable sending queue by default in all exporters configured to use it #1924

## 🧰 Bug fixes 🧰

- Fix bug where the service does not correctly start/stop the log exporters (#1943)
- Fix Queued Retry Unusable without Batch Processor (#1813) - #1930

## v0.12.0 Beta

## 🚀 New components 🚀

- `configauth` package with the auth settings that can be used by receivers (#1807, #1808, #1809, #1810)
- `perfcounters` package that uses perflib for host metrics receiver (#1835, #1836, #1868, #1869, #1870)

## 💡 Enhancements 💡

- Remove `queued_retry` and enable `otlp` metrics receiver in default config (#1823, #1838)
- Add `limit_percentage` and `spike_limit_percentage` options to `memorylimiter` processor (#1622)
- `hostmetrics` receiver:
  - Collect additional labels from partitions in the filesystems scraper (#1858)
  - Add filters for mount point and filesystem type (#1866)
- Add cloud.provider semantic conventions (#1865)
- `attribute` processor: Add log support (#1783)
- Deprecate OpenCensus-based internal data structures (#1843)
- Introduce SpanID data type, not yet used in Protobuf messages ($1854, #1855)
- Enable `otlp` trace by default in the released docker image (#1883)
- `tailsampling` processor: Combine batches of spans into a single batch (#1864)
- `filter` processor: Update to use pdata (#1885)
- Allow MSI upgrades (#1914)

## 🧰 Bug fixes 🧰

- `prometheus` receiver: Print a more informative message about 'up' metric value (#1826)
- Use custom data type and custom JSON serialization for traceid (#1840)
- Skip creation of redundant nil resource in translation from OC if there are no combined metrics (#1803)
- `tailsampling` processor: Only send to next consumer once (#1735)
- Report Windows pagefile usage in bytes (#1837)
- Fix issue where Prometheus SD config cannot be parsed (#1877)

## v0.11.0 Beta

## 🛑 Breaking changes 🛑

- Rename service.Start() to Run() since it's a blocking call
- Fix slice Append to accept by value the element in pdata
- Change CreateTraceProcessor and CreateMetricsProcessor to use the same parameter order as receivers/logs processor and exporters.
- Prevent accidental use of LogsToOtlp and LogsFromOtlp and the OTLP data structs (#1703)
- Remove SetType from configmodels, ensure all registered factories set the type in config (#1798)
- Move process telemetry to service/internal (#1794)

## 💡 Enhancements 💡

- Add map and array attribute value type support (#1656)
- Add authentication support to kafka (#1632)
- Implement InstrumentationLibrary translation to jaeger (#1645)
- Add public functions to export pdata to ExportXServicesRequest Protobuf bytes (#1741)
- Expose telemetry level in the configtelemetry (#1796)
- Add configauth package (#1807)
- Add config to docker image (#1792)

## 🧰 Bug fixes 🧰

- Use zap int argument for int values instead of conversion (#1779)
- Add support for gzip encoded payload in OTLP/HTTP receiver (#1581)
- Return proto status for OTLP receiver when failed (#1788)

## v0.10.0 Beta

## 🛑 Breaking changes 🛑

- **Update OTLP to v0.5.0, incompatible metrics protocol.**
- Remove support for propagating summary metrics in OtelCollector.
  - This is a temporary change, and will affect mostly OpenCensus users who use metrics.

## 💡 Enhancements 💡
- Support zipkin proto in `kafka` receiver (#1646)
- Prometheus Remote Write Exporter supporting Cortex (#1577, #1643)
- Add deployment environment semantic convention (#1722)
- Add logs support to `batch` and `resource` processors (#1723, #1729)

## 🧰 Bug fixes 🧰
- Identify config error when expected map is other value type (#1641)
- Fix Kafka receiver closing ready channel multiple times (#1696)
- Fix a panic issue while processing Zipkin spans with an empty service name (#1742)
- Zipkin Receiver: Always set the endtime (#1750)

## v0.9.0 Beta

## 🛑 Breaking changes 🛑

- **Remove old base factories**:
  - `ReceiverFactoryBase` (#1583)
  - `ProcessorFactoryBase` (#1596)
  - `ExporterFactoryBase` (#1630)
- Remove logs factories and merge with normal factories (#1569)
- Remove `reconnection_delay` from OpenCensus exporter (#1516)
- Remove `ConsumerOld` interfaces (#1631)

## 🚀 New components 🚀
- `prometheusremotewrite` exporter: Send metrics data in Prometheus TimeSeries format to Cortex or any Prometheus (#1544)
- `kafka` receiver: Receive traces from Kafka (#1410)

## 💡 Enhancements 💡
- `kafka` exporter: Enable queueing, retry, timeout (#1455)
- Add `Headers` field in HTTPClientSettings (#1552)
- Change OpenCensus receiver (#1556) and exporter (#1571) to the new interfaces
- Add semantic attribute for `telemetry.auto.version` (#1578)
- Add uptime and RSS memory self-observability metrics (#1549)
- Support conversion for OpenCensus `SameProcessAsParentSpan` (#1629)
- Access application version in components (#1559)
- Make Kafka payload encoding configurable (#1584)

## 🧰 Bug fixes 🧰
- Stop further processing if `filterprocessor` filters all data (#1500)
- `processscraper`: Use same scrape time for all data points coming from same process (#1539)
- Ensure that time conversion for 0 returns nil timestamps or Time where IsZero returns true (#1550)
- Fix multiple exporters panic (#1563)
- Allow `attribute` processor for external use (#1574)
- Do not duplicate filesystem metrics for devices with many mount points (#1617)

## v0.8.0 Beta

## 🚀 New components 🚀

- `groupbytrace` processor that waits for a trace to be completed (#1362)

## 💡 Enhancements 💡

- Migrate `zipkin` receiver/exporter to the new interfaces (#1484)
- Migrate `prometheus` receiver/exporter to the new interfaces (#1477, #1515)
- Add new FactoryUnmarshaler support to all components, deprecate old way (#1468)
- Update `fileexporter` to write data in OTLP (#1488)
- Add extension factory helper (#1485)
- Host scrapers: Use same scrape time for all data points coming from same source (#1473)
- Make logs SeverityNumber publicly available (#1496)
- Add recently included conventions for k8s and container resources (#1519)
- Add new config StartTimeMetricRegex to `prometheus` receiver (#1511)
- Convert Zipkin receiver and exporter to use OTLP (#1446)

## 🧰 Bug fixes 🧰

- Infer OpenCensus resource type based on OpenTelemetry's semantic conventions (#1462)
- Fix log adapter in `prometheus` receiver (#1493)
- Avoid frequent errors for process telemetry on Windows (#1487)

## v0.7.0 Beta

## 🚀 New components 🚀

- Receivers
  - `fluentfoward` runs a TCP server that accepts events via the [Fluent Forward protocol](https://github.com/fluent/fluentd/wiki/Forward-Protocol-Specification-v1) (#1173)
- Exporters
  - `kafka` exports traces to Kafka (#1439)
- Extensions
  - **Experimental** `fluentbit` facilitates running a FluentBit subprocess of the collector (#1381)

## 💡 Enhancements 💡

- Updated `golang/protobuf` from v1.3.5 to v1.4.2 (#1308)
- Updated `opencensus-proto` from v0.2.1 to v0.3.0 (#1308)
- Added round_robin `balancer_name` as an option to gRPC client settings (#1353)
- `hostmetrics` receiver
  - Switch to using perf counters to get disk io metrics on Windows (#1340)
  - Add device filter for file system (#1379) and disk (#1378) scrapers
  - Record process physical & virtual memory stats separately (#1403)
  - Scrape system.disk.time on Windows (#1408)
  - Add disk.pending_operations metric (#1428)
  - Add network interface label to network metrics (#1377)
- Add `exporterhelper` (#1351) and `processorhelper` (#1359) factories
- Update OTLP to latest version (#1384)
- Disable timeout, retry on failure and sending queue for `logging` exporter (#1400)
- Add support for retry and sending queue for `jaeger` exporter (#1401)
- Add batch size bytes metric to `batch` processor (#1270)
- `otlp` receiver: Add Log Support (#1444)
- Allow to configure read/write buffer sizes for http Client (#1447)
- Update DB conventions to latest and add exception conventions (#1452)

## 🧰 Bug fixes 🧰

- Fix `resource` processor for old metrics (#1412)
- `jaeger` receiver: Do not try to stop if failed to start. Collector service will do that (#1434)

## v0.6.0 Beta

## 🛑 Breaking changes 🛑

- Renamed the metrics generated by `hostmetrics` receiver to match the (currently still pending) OpenTelemetry system metric conventions (#1261) (#1269)
- Removed `vmmetrics` receiver (#1282)
- Removed `cpu` scraper `report_per_cpu` config option (#1326)

## 💡 Enhancements 💡

- Added disk merged (#1267) and process count (#1268) metrics to `hostmetrics`
- Log metric data points in `logging` exporter (#1258)
- Changed the `batch` processor to not ignore the errors returned by the exporters (#1259)
- Build and publish MSI (#1153) and DEB/RPM packages (#1278, #1335)
- Added batch size metric to `batch` processor (#1241)
- Added log support for `memorylimiter` processor (#1291) and `logging` exporter (#1298)
- Always add tags for `observability`, other metrics may use them (#1312)
- Added metrics support (#1313) and allow partial retries in `queued_retry` processor (#1297)
- Update `resource` processor: introduce `attributes` config parameter to specify actions on attributes similar to `attributes` processor, old config interface is deprecated (#1315)
- Update memory state labels for non-Linux OSs (#1325)
- Ensure tcp connection value is provided for all states, even when count is 0 (#1329)
- Set `batch` processor channel size to num cpus (#1330)
- Add `send_batch_max_size` config parameter to `batch` processor enforcing hard limit on batch size (#1310)
- Add support for including a per-RPC authentication to gRPC settings (#1250)

## 🧰 Bug fixes 🧰

- Fixed OTLP waitForReady, not set from config (#1254)
- Fixed all translation diffs between OTLP and Jaeger (#1222)
- Disabled `process` scraper for any non Linux/Windows OS (#1328)

## v0.5.0 Beta

## 🛑 Breaking changes 🛑

- **Update OTLP to v0.4.0 (#1142)**: Collector will be incompatible with any other sender or receiver of OTLP protocol
of different versions
- Make "--new-metrics" command line flag the default (#1148)
- Change `endpoint` to `url` in Zipkin exporter config (#1186)
- Change `tls_credentials` to `tls_settings` in Jaegar receiver config (#1233)
- OTLP receiver config change for `protocols` to support mTLS (#1223)
- Remove `export_resource_labels` flag from Zipkin exporter (#1163)

## 🚀 New components 🚀

- Receivers
  - Added process scraper to the `hostmetrics` receiver (#1047)

## 💡 Enhancements 💡

- otlpexporter: send configured headers in request (#1130)
- Enable Collector to be run as a Windows service (#1120)
- Add config for HttpServer (#1196)
- Allow cors in HTTPServerSettings (#1211)
- Add a generic grpc server settings config, cleanup client config (#1183)
- Rely on gRPC to batch and loadbalance between connections instead of custom logic (#1212)
- Allow to tune the read/write buffers for gRPC clients (#1213)
- Allow to tune the read/write buffers for gRPC server (#1218)

## 🧰 Bug fixes 🧰

- Handle overlapping metrics from different jobs in prometheus exporter (#1096)
- Fix handling of SpanKind INTERNAL in OTLP OC translation (#1143)
- Unify zipkin v1 and v2 annotation/tag parsing logic (#1002)
- mTLS: Add support to configure client CA and enforce ClientAuth (#1185)
- Fixed untyped Prometheus receiver bug (#1194)
- Do not embed ProtocolServerSettings in gRPC (#1210)
- Add Context to the missing CreateMetricsReceiver method (#1216)

## v0.4.0 Beta

Released 2020-06-16

## 🛑 Breaking changes 🛑

- `isEnabled` configuration option removed (#909) 
- `thrift_tchannel` protocol moved from `jaeger` receiver to `jaeger_legacy` in contrib (#636) 

## ⚠️ Major changes ⚠️

- Switch from `localhost` to `0.0.0.0` by default for all receivers (#1006)
- Internal API Changes (only impacts contributors)
  - Add context to `Start` and `Stop` methods in the component (#790)
  - Rename `AttributeValue` and `AttributeMap` method names (#781)
(other breaking changes in the internal trace data types)
  - Change entire repo to use the new vanityurl go.opentelemetry.io/collector (#977)

## 🚀 New components 🚀

- Receivers
  - `hostmetrics` receiver with CPU (#862), disk (#921), load (#974), filesystem (#926), memory (#911), network (#930), and virtual memory (#989) support
- Processors
  - `batch` for batching received metrics (#1060) 
  - `filter` for filtering (dropping) received metrics (#1001) 

## 💡 Enhancements 💡

- `otlp` receiver implement HTTP X-Protobuf (#1021)
- Exporters: Support mTLS in gRPC exporters (#927) 
- Extensions: Add `zpages` for service (servicez, pipelinez, extensions) (#894) 

## 🧰 Bug fixes 🧰

- Add missing logging for metrics at `debug` level (#1108) 
- Fix setting internal status code in `jaeger` receivers (#1105) 
- `zipkin` export fails on span without timestamp when used with `queued_retry` (#1068) 
- Fix `zipkin` receiver status code conversion (#996) 
- Remove extra send/receive annotations with using `zipkin` v1 (#960)
- Fix resource attribute mutation bug when exporting in `jaeger` proto (#907) 
- Fix metric/spans count, add tests for nil entries in the slices (#787) 


## 🧩 Components 🧩

### Traces

| Receivers | Processors | Exporters |
|:----------:|:-----------:|:----------:|
| Jaeger | Attributes | File |
| OpenCensus | Batch | Jaeger |
| OTLP | Memory Limiter | Logging |
| Zipkin | Queued Retry | OpenCensus |
| | Resource | OTLP |
| | Sampling | Zipkin |
| | Span ||

### Metrics

| Receivers | Processors | Exporters |
|:----------:|:-----------:|:----------:|
| HostMetrics | Batch | File |
| OpenCensus | Filter | Logging |
| OTLP | Memory Limiter | OpenCensus |
| Prometheus || OTLP |
| VM Metrics || Prometheus |

### Extensions

- Health Check
- Performance Profiler
- zPages


## v0.3.0 Beta

Released 2020-03-30

### Breaking changes

-  Make prometheus receiver config loading strict. #697 
Prometheus receiver will now fail fast if the config contains unused keys in it.

### Changes and fixes

- Enable best effort serve by default of Prometheus Exporter (https://github.com/orijtech/prometheus-go-metrics-exporter/pull/6)
- Fix null pointer exception in the logging exporter #743 
- Remove unnecessary condition to have at least one processor #744 

### Components

| Receivers / Exporters | Processors | Extensions |
|:---------------------:|:-----------:|:-----------:|
| Jaeger | Attributes | Health Check |
| OpenCensus | Batch | Performance Profiler |
| OpenTelemetry | Memory Limiter | zPages |
| Zipkin | Queued Retry | |
| | Resource | |
| | Sampling | |
| | Span | |


## v0.2.8 Alpha

Alpha v0.2.8 of OpenTelemetry Collector

- Implemented OTLP receiver and exporter.
- Added ability to pass config to the service programmatically (useful for custom builds).
- Improved own metrics / observability.
- Refactored component and factory interface definitions (breaking change #683) 


## v0.2.7 Alpha

Alpha v0.2.7 of OpenTelemetry Collector

- Improved error handling on shutdown
- Partial implementation of new metrics (new obsreport package)
- Include resource labels for Zipkin exporter
- New `HASH` action to attribute processor



## v0.2.6 Alpha

Alpha v0.2.6 of OpenTelemetry Collector.
- Update metrics prefix to `otelcol` and expose command line argument to modify the prefix value.
- Extend Span processor to have include/exclude span logic.
- Batch dropped span now emits zero when no spans are dropped.


## v0.2.5 Alpha

Alpha v0.2.5 of OpenTelemetry Collector.

- Regexp-based filtering of spans based on service names.
- Ability to choose strict or regexp matching for include/exclude filters.


## v0.2.4 Alpha

Alpha v0.2.4 of OpenTelemetry Collector.

- Regexp-based filtering of span names.
- Ability to extract attributes from span names and rename span.
- File exporter for debugging.
- Span processor is now enabled by default.


## v0.2.3 Alpha

Alpha v0.2.3 of OpenTelemetry Collector.

Changes:
21a70d6 Add a memory limiter processor (#498)
9778b16 Refactor Jaeger Receiver config (#490)
ec4ad0c Remove workers from OpenCensus receiver implementation (#497)
4e01fa3 Update k8s config to use opentelemetry docker image and configuration (#459)


## v0.2.2 Alpha

Alpha v0.2.2 of OpenTelemetry Collector.

Main changes visible to users since previous release:

- Improved Testbed and added more E2E tests.
- Made component interfaces more uniform (this is a breaking change).

Note: v0.2.1 never existed and is skipped since it was tainted in some dependencies.


## v0.2.0 Alpha

Alpha v0.2 of OpenTelemetry Collector.

Docker image: omnition/opentelemetry-collector:v0.2.0 (we are working on getting this under an OpenTelemetry org)

Main changes visible to users since previous release:

* Rename from `service` to `collector`, the binary is now named `otelcol`

* Configuration reorganized and using strict mode

* Concurrency issues for pipelines transforming data addressed

Commits:

```terminal
0e505d5 Refactor config: pipelines now under service (#376)
402b80c Add Capabilities to Processor and use for Fanout cloning decision (#374)
b27d824 Use strict mode to read config (#375)
d769eb5 Fix concurrency handling when data is fanned out (#367)
dc6b290 Rename all github paths from opentelemtry-service to opentelemetry-collector (#371)
d038801 Rename otelsvc to otelcol (#365)
c264e0e Add Include/Exclude logic for Attributes Processor (#363)
8ce427a Pin a commit for Prometheus dependency in go.mod (#364)
2393774 Bump Jaeger version to 1.14.0 (latest) (#349)
63362d5 Update testbed modules (#360)
c0e2a27 Change dashes to underscores to separate words in config files (#357)
7609eaa Rename OpenTelemetry Service to Collector in docs and comments (#354)
bc5b299 Add common gRPC configuration settings (#340)
b38505c Remove network access popups on macos (#348)
f7727d1 Fixed loop variable pointer bug in jaeger translator (#341)
958beed Ensure that ConsumeMetricsData() is not passed empty metrics in the Prometheus receiver (#345)
0be295f Change log statement in Prometheus receiver from info to debug. (#344)
d205393 Add Owais to codeowners (#339)
8fa6afe Translate OC resource labels to Jaeger process tags (#325)
```


## v0.0.2 Alpha

Alpha release of OpenTelemetry Service.

Docker image: omnition/opentelemetry-service:v0.0.2 (we are working on getting this under an OpenTelemetry org)

Main changes visible to users since previous release:

```terminal
8fa6afe Translate OC resource labels to Jaeger process tags (#325)
047b0f3 Allow environment variables in config (#334)
96c24a3 Add exclude/include spans option to attributes processor (#311)
4db0414 Allow metric processors to be specified in pipelines (#332)
c277569 Add observability instrumentation for Prometheus receiver (#327)
f47aa79 Add common configuration for receiver tls (#288)
a493765 Refactor extensions to new config format (#310)
41a7afa Add Span Processor logic
97a71b3 Use full name for the metrics and spans created for observability (#316)
fed4ed2 Add support to record metrics for metricsexporter (#315)
5edca32 Add include_filter configuration to prometheus receiver (#298)
0068d0a Passthrough CORS allowed origins (#260)
```


## v0.0.1 Alpha

This is the first alpha release of OpenTelemetry Service.

Docker image: omnition/opentelemetry-service:v0.0.1


[v0.3.0]: https://github.com/open-telemetry/opentelemetry-collector/compare/v0.2.10...v0.3.0
[v0.2.10]: https://github.com/open-telemetry/opentelemetry-collector/compare/v0.2.8...v0.2.10
[v0.2.8]: https://github.com/open-telemetry/opentelemetry-collector/compare/v0.2.7...v0.2.8
[v0.2.7]: https://github.com/open-telemetry/opentelemetry-collector/compare/v0.2.6...v0.2.7
[v0.2.6]: https://github.com/open-telemetry/opentelemetry-collector/compare/v0.2.5...v0.2.6
[v0.2.5]: https://github.com/open-telemetry/opentelemetry-collector/compare/v0.2.4...v0.2.5
[v0.2.4]: https://github.com/open-telemetry/opentelemetry-collector/compare/v0.2.3...v0.2.4
[v0.2.3]: https://github.com/open-telemetry/opentelemetry-collector/compare/v0.2.2...v0.2.3
[v0.2.2]: https://github.com/open-telemetry/opentelemetry-collector/compare/v0.2.0...v0.2.2
[v0.2.0]: https://github.com/open-telemetry/opentelemetry-collector/compare/v0.0.2...v0.2.0
[v0.0.2]: https://github.com/open-telemetry/opentelemetry-collector/compare/v0.0.1...v0.0.2
[v0.0.1]: https://github.com/open-telemetry/opentelemetry-collector/tree/v0.0.1
