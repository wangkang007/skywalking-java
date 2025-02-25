# Table of Agent Configuration Properties
This is the properties list supported in `agent/config/agent.config`.

property key | Description | Default |
----------- | ---------- | --------- | 
`agent.namespace` | Namespace isolates headers in cross process propagation. The HEADER name will be `HeaderName:Namespace`. | Not set | 
`agent.service_name` | The service name to represent a logic group providing the same capabilities/logic. Suggestion: set a unique name for every logic service group, service instance nodes share the same code, Max length is 50(UTF-8 char). Optional, once `service_name` follows `<group name>::<logic name>` format, OAP server assigns the group name to the service metadata.| `Your_ApplicationName` |
`agent.sample_n_per_3_secs`|Negative or zero means off, by default.SAMPLE_N_PER_3_SECS means sampling N TraceSegment in 3 seconds tops.|Not set|
`agent.authentication`|Authentication active is based on backend setting, see application.yml for more details.For most scenarios, this needs backend extensions, only basic match auth provided in default implementation.|Not set|
`agent.trace_segment_ref_limit_per_span`|The max number of TraceSegmentRef in a single span to keep memory cost estimatable.|500 |
`agent.span_limit_per_segment`|The max number of spans in a single segment. Through this config item, SkyWalking keep your application memory cost estimated.|300 |
`agent.ignore_suffix`|If the operation name of the first span is included in this set, this segment should be ignored.|Not set|
`agent.is_open_debugging_class`|If true, skywalking agent will save all instrumented classes files in `/debugging` folder. SkyWalking team may ask for these files in order to resolve compatible problem.|Not set|
`agent.is_cache_enhanced_class`|If true, SkyWalking agent will cache all instrumented classes files to memory or disk files (decided by class cache mode), allow another java agent to enhance those classes that enhanced by SkyWalking agent. To use some Java diagnostic tools (such as BTrace, Arthas) to diagnose applications or add a custom java agent to enhance classes, you need to enable this feature. |`false`|
`agent.class_cache_mode`|The instrumented classes cache mode: `MEMORY` or `FILE`. `MEMORY`: cache class bytes to memory, if instrumented classes is too many or too large, it may take up more memory. `FILE`: cache class bytes in `/class-cache` folder, automatically clean up cached class files when the application exits.|`MEMORY`|
`agent.instance_name` |Instance name is the identity of an instance, should be unique in the service. If empty, SkyWalking agent will generate an 32-bit uuid. Default, use `UUID`@`hostname` as the instance name. Max length is 50(UTF-8 char)|`""`|
`agent.instance_properties[key]=value` | Add service instance custom properties. Notice it could be overridden by `agent.instance_properties_json `, if the key duplication. | Not set|
`agent.instance_properties_json={"key":"value"}` | Add service instance custom properties in json format.  | Not set|
`agent.cause_exception_depth`|How depth the agent goes, when log all cause exceptions.|`5`|
`agent.force_reconnection_period `|Force reconnection period of grpc, based on grpc_channel_check_interval.|`1`|
`agent.operation_name_threshold `|The operationName max length, setting this value > 190 is not recommended.|`150`|
`agent.keep_tracing`|Keep tracing even the backend is not available if this value is `true`.|`false`|
`agent.force_tls`|Force open TLS for gRPC channel if this value is `true`.|`false`|
`agent.ssl_trusted_ca_path` | gRPC SSL trusted ca file. | `/ca/ca.crt` |
`agent.ssl_key_path`| The private key file. Enable mTLS when ssl_key_path and ssl_cert_chain_path exist. | `""` |
`agent.ssl_cert_chain_path`| The certificate file. Enable mTLS when ssl_key_path and ssl_cert_chain_path exist. | `""` |
`osinfo.ipv4_list_size`| Limit the length of the ipv4 list size. |`10`|
`collector.grpc_channel_check_interval`|grpc channel status check interval.|`30`|
`collector.heartbeat_period`|agent heartbeat report period. Unit, second.|`30`|
`collector.properties_report_period_factor`|The agent sends the instance properties to the backend every `collector.heartbeat_period * collector.properties_report_period_factor` seconds |`10`|
`collector.backend_service`|Collector SkyWalking trace receiver service addresses.|`127.0.0.1:11800`|
`collector.grpc_upstream_timeout`|How long grpc client will timeout in sending data to upstream. Unit is second.|`30` seconds|
`collector.get_profile_task_interval`|Sniffer get profile task list interval.|`20`|
`collector.get_agent_dynamic_config_interval`|Sniffer get agent dynamic config interval|`20`|
`collector.is_resolve_dns_periodically`|If true, skywalking agent will enable periodically resolving DNS to update receiver service addresses.|`false`|
`logging.level`|Log level: TRACE, DEBUG, INFO, WARN, ERROR, OFF. Default is info.|`INFO`|
`logging.file_name`|Log file name.|`skywalking-api.log`|
`logging.output`| Log output. Default is FILE. Use CONSOLE means output to stdout. |`FILE`|
`logging.dir`|Log files directory. Default is blank string, means, use "{theSkywalkingAgentJarDir}/logs  " to output logs. {theSkywalkingAgentJarDir} is the directory where the skywalking agent jar file is located |`""`|
`logging.resolver`|Logger resolver: `PATTERN` or `JSON`. The default is `PATTERN`, which uses `logging.pattern` to print traditional text logs. `JSON` resolver prints logs in JSON format. |`PATTERN`|
`logging.pattern `|Logging format. There are all conversion specifiers: <br>&nbsp;&nbsp;* `%level` means log level. <br>&nbsp;&nbsp;*  `%timestamp` means now of time with format `yyyy-MM-dd HH:mm:ss:SSS`.<br>&nbsp;&nbsp;*   `%thread` means name of current thread.<br>&nbsp;&nbsp;*   `%msg` means some message which user logged. <br>&nbsp;&nbsp;*  `%class` means SimpleName of TargetClass. <br>&nbsp;&nbsp;*  `%throwable` means a throwable which user called. <br>&nbsp;&nbsp;*  `%agent_name` means `agent.service_name`. Only apply to the `PatternLogger`. |`%level %timestamp %thread %class : %msg %throwable`|
`logging.max_file_size`|The max size of log file. If the size is bigger than this, archive the current file, and write into a new file.|`300 * 1024 * 1024`|
`logging.max_history_files`|The max history log files. When rollover happened, if log files exceed this number,then the oldest file will be delete. Negative or zero means off, by default.|`-1`|
`statuscheck.ignored_exceptions`|Listed exceptions would not be treated as an error. Because in some codes, the exception is being used as a way of controlling business flow.|`""`|
`statuscheck.max_recursive_depth`|The max recursive depth when checking the exception traced by the agent. Typically, we don't recommend setting this more than 10, which could cause a performance issue. Negative value and 0 would be ignored, which means all exceptions would make the span tagged in error status.|`1`|
`correlation.element_max_number`|Max element count in the correlation context.|3|
`correlation.value_max_length`|Max value length of each element.|`128`|
`correlation.auto_tag_keys`|Tag the span by the key/value in the correlation context, when the keys listed here exist.|`""`|
`jvm.buffer_size`|The buffer size of collected JVM info.|`60 * 10`|
`buffer.channel_size`|The buffer channel size.|`5`|
`buffer.buffer_size`|The buffer size.|`300`|
`profile.active`|If true, skywalking agent will enable profile when user create a new profile task. Otherwise disable profile.|`true`|
`profile.max_parallel`|Parallel monitor segment count|`5`|
`profile.duration`|Max monitor segment time(minutes), if current segment monitor time out of limit, then stop it.|`10`|
`profile.dump_max_stack_depth`|Max dump thread stack depth|`500`|
`profile.snapshot_transport_buffer_size`|Snapshot transport to backend buffer size|`50`|
`meter.active`|If true, the agent collects and reports metrics to the backend.|`true`|
`meter.report_interval`|Report meters interval. The unit is second|`20`|
`meter.max_meter_size`| Max size of the meter pool |`500`|
`log.max_message_size`| The max size of message to send to server.Default is 10 MB. |`10485760`|
`plugin.mount` | Mount the specific folders of the plugins. Plugins in mounted folders would work. | `plugins,activations` |
`plugin.peer_max_length `|Peer maximum description limit.|`200`|
`plugin.exclude_plugins `|Exclude some plugins define in plugins dir.Plugin names is defined in [Agent plugin list](Plugin-list.md)|`""`|
`plugin.mongodb.trace_param`|If true, trace all the parameters in MongoDB access, default is false. Only trace the operation, not include parameters.|`false`|
`plugin.mongodb.filter_length_limit`|If set to positive number, the `WriteRequest.params` would be truncated to this length, otherwise it would be completely saved, which may cause performance problem.|`256`|
`plugin.elasticsearch.trace_dsl`|If true, trace all the DSL(Domain Specific Language) in ElasticSearch access, default is false.|`false`|
`plugin.springmvc.use_qualified_name_as_endpoint_name`|If true, the fully qualified method name will be used as the endpoint name instead of the request URL, default is false.|`false`|
`plugin.toolit.use_qualified_name_as_operation_name`|If true, the fully qualified method name will be used as the operation name instead of the given operation name, default is false.|`false`|
`plugin.jdbc.trace_sql_parameters`|If set to true, the parameters of the sql (typically `java.sql.PreparedStatement`) would be collected.|`false`|
`plugin.jdbc.sql_parameters_max_length`|If set to positive number, the `db.sql.parameters` would be truncated to this length, otherwise it would be completely saved, which may cause performance problem.|`512`|
`plugin.jdbc.sql_body_max_length`|If set to positive number, the `db.statement` would be truncated to this length, otherwise it would be completely saved, which may cause performance problem.|`2048`|
`plugin.solrj.trace_statement`|If true, trace all the query parameters(include deleteByIds and deleteByQuery) in Solr query request, default is false.|`false`|
`plugin.solrj.trace_ops_params`|If true, trace all the operation parameters in Solr request, default is false.|`false`|
`plugin.light4j.trace_handler_chain`|If true, trace all middleware/business handlers that are part of the Light4J handler chain for a request.|false|
`plugin.springtransaction.simplify_transaction_definition_name`|If true, the transaction definition name will be simplified.|false|
`plugin.jdkthreading.threading_class_prefixes` | Threading classes (`java.lang.Runnable` and `java.util.concurrent.Callable`) and their subclasses, including anonymous inner classes whose name match any one of the `THREADING_CLASS_PREFIXES` (splitted by `,`) will be instrumented, make sure to only specify as narrow prefixes as what you're expecting to instrument, (`java.` and `javax.` will be ignored due to safety issues) | Not set |
`plugin.tomcat.collect_http_params`| This config item controls that whether the Tomcat plugin should collect the parameters of the request. Also, activate implicitly in the profiled trace. | `false` |
`plugin.springmvc.collect_http_params`| This config item controls that whether the SpringMVC plugin should collect the parameters of the request, when your Spring application is based on Tomcat, consider only setting either `plugin.tomcat.collect_http_params` or `plugin.springmvc.collect_http_params`. Also, activate implicitly in the profiled trace. | `false` |
`plugin.httpclient.collect_http_params`| This config item controls that whether the HttpClient plugin should collect the parameters of the request | `false` |
`plugin.http.http_params_length_threshold`| When `COLLECT_HTTP_PARAMS` is enabled, how many characters to keep and send to the OAP backend, use negative values to keep and send the complete parameters, NB. this config item is added for the sake of performance.  | `1024` |
`plugin.http.http_headers_length_threshold`| When `include_http_headers` declares header names, this threshold controls the length limitation of all header values. use negative values to keep and send the complete headers. Note. this config item is added for the sake of performance. | `2048` |
`plugin.http.include_http_headers`| Set the header names, which should be collected by the plugin. Header name must follow `javax.servlet.http` definition. Multiple names should be split by comma.  | ``(No header would be collected) |
`plugin.feign.collect_request_body`| This config item controls that whether the Feign plugin should collect the http body of the request. | `false` |
`plugin.feign.filter_length_limit`| When `COLLECT_REQUEST_BODY` is enabled, how many characters to keep and send to the OAP backend, use negative values to keep and send the complete body.  | `1024` |
`plugin.feign.supported_content_types_prefix`| When `COLLECT_REQUEST_BODY` is enabled and content-type start with SUPPORTED_CONTENT_TYPES_PREFIX, collect the body of the request , multiple paths should be separated by `,`  | `application/json,text/` |
`plugin.influxdb.trace_influxql`|If true, trace all the influxql(query and write) in InfluxDB access, default is true.|`true`|
`plugin.dubbo.collect_consumer_arguments`| Apache Dubbo consumer collect `arguments` in RPC call, use `Object#toString` to collect `arguments`. |`false`| 
`plugin.dubbo.consumer_arguments_length_threshold`| When `plugin.dubbo.collect_consumer_arguments` is `true`, Arguments of length from the front will to the OAP backend |`256`| 
`plugin.dubbo.collect_provider_arguments`| Apache Dubbo provider collect `arguments` in RPC call, use `Object#toString` to collect `arguments`. |`false`| 
`plugin.dubbo.provider_arguments_length_threshold`| When `plugin.dubbo.collect_provider_arguments` is `true`, Arguments of length from the front will to the OAP backend |`256`| 
`plugin.kafka.bootstrap_servers`| A list of host/port pairs to use for establishing the initial connection to the Kafka cluster. | `localhost:9092`|
`plugin.kafka.get_topic_timeout`| Timeout period of reading topics from the Kafka server, the unit is second. |`10`|
`plugin.kafka.producer_config`| Kafka producer configuration. Read [producer configure](http://kafka.apache.org/24/documentation.html#producerconfigs) to get more details. Check [Kafka report doc](advanced-reporters.md#kafka-reporter) for more details and examples. | |
`plugin.kafka.producer_config_json` | Configure Kafka Producer configuration in JSON format. Notice it will be overridden by `plugin.kafka.producer_config[key]`, if the key duplication. | |
`plugin.kafka.topic_meter` | Specify which Kafka topic name for Meter System data to report to. | `skywalking-meters` |
`plugin.kafka.topic_metrics` | Specify which Kafka topic name for JVM metrics data to report to. | `skywalking-metrics` |
`plugin.kafka.topic_segment` | Specify which Kafka topic name for traces data to report to. | `skywalking-segments` |
`plugin.kafka.topic_profiling` | Specify which Kafka topic name for Thread Profiling snapshot to report to. | `skywalking-profilings` |
`plugin.kafka.topic_management` | Specify which Kafka topic name for the register or heartbeat data of Service Instance to report to. | `skywalking-managements` |
`plugin.kafka.topic_logging` | Specify which Kafka topic name for the logging data to report to. | `skywalking-logging` |
`plugin.kafka.namespace` | isolate multi OAP server when using same Kafka cluster (final topic name will append namespace before Kafka topics with `-` ).  | `` |
`plugin.springannotation.classname_match_regex` |  Match spring beans with regular expression for the class name. Multiple expressions could be separated by a comma. This only works when `Spring annotation plugin` has been activated. | `All the spring beans tagged with @Bean,@Service,@Dao, or @Repository.` |
`plugin.toolkit.log.transmit_formatted` | Whether or not to transmit logged data as formatted or un-formatted. | `true` |
`plugin.lettuce.trace_redis_parameters` | If set to true, the parameters of Redis commands would be collected by Lettuce agent.| `false` |
`plugin.lettuce.redis_parameter_max_length` | If set to positive number and `plugin.lettuce.trace_redis_parameters` is set to `true`, Redis command parameters would be collected and truncated to this length.| `128` |
`plugin.neo4j.trace_cypher_parameters`|If set to true, the parameters of the cypher would be collected.|`false`|
`plugin.neo4j.cypher_parameters_max_length`|If set to positive number, the `db.cypher.parameters` would be truncated to this length, otherwise it would be completely saved, which may cause performance problem.|`512`|
`plugin.neo4j.cypher_body_max_length`|If set to positive number, the `db.statement` would be truncated to this length, otherwise it would be completely saved, which may cause performance problem.|`2048`|

# Dynamic Configurations
All configurations above are static, if you need to change some agent settings at runtime, please read [CDS - Configuration Discovery Service document](configuration-discovery.md) for more details.
