# Imagem base do Trino
FROM trinodb/trino:latest

# Download do JMX Exporter
ADD https://repo1.maven.org/maven2/io/prometheus/jmx/jmx_prometheus_javaagent/0.16.1/jmx_prometheus_javaagent-0.16.1.jar /usr/lib/trino/lib/

# Alterando o usuário para root para corrigir permissões
USER root

# Alterando o owner do jar para trino e o grupo para trino
RUN chown trino:trino /usr/lib/trino/lib/jmx_prometheus_javaagent-0.16.1.jar

# Alterando as permissões do jar
RUN chmod 644 /usr/lib/trino/lib/jmx_prometheus_javaagent-0.16.1.jar

# Voltando o usuário para trino
USER trino

# Criando uma arquivo de configuração do JMX Exporter vazio
RUN touch /etc/trino/jmx_config.yml

# Adicionando a flag javaagent para execução do JMX Exporter junto ao Trino
RUN echo "-javaagent:/usr/lib/trino/lib/jmx_prometheus_javaagent-0.16.1.jar=9483:/etc/trino/jmx_config.yml" >> /etc/trino/jvm.config