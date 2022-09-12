FROM trinodb/trino:latest
ADD https://repo1.maven.org/maven2/io/prometheus/jmx/jmx_prometheus_javaagent/0.16.1/jmx_prometheus_javaagent-0.16.1.jar /usr/lib/trino/lib/
USER root
RUN chown trino:trino /usr/lib/trino/lib/jmx_prometheus_javaagent-0.16.1.jar
RUN chmod 644 /usr/lib/trino/lib/jmx_prometheus_javaagent-0.16.1.jar
USER trino
RUN touch /etc/trino/jmx_config.yml
RUN echo "-javaagent:/usr/lib/trino/lib/jmx_prometheus_javaagent-0.16.1.jar=9483:/etc/trino/jmx_config.yml" >> /etc/trino/jvm.config
