FROM openjdk:8-jre-alpine
VOLUME /tmp

# Download dockerize and cache that layer
ARG DOCKERIZE_VERSION
RUN wget -O dockerize.tar.gz https://github.com/jwilder/dockerize/releases/download/${DOCKERIZE_VERSION}/dockerize-alpine-linux-amd64-${DOCKERIZE_VERSION}.tar.gz
RUN tar xzf dockerize.tar.gz
RUN chmod +x dockerize
RUN wget -O /opt/signalfx-tracing.jar https://github.com/signalfx/signalfx-java-tracing/releases/latest/download/signalfx-tracing.jar

# This is the first layer that won't be cached
ARG ARTIFACT_NAME
ADD ${ARTIFACT_NAME}.jar /app.jar
RUN wget -O /opt/signalfx-tracing.jar https://github.com/signalfx/signalfx-java-tracing/releases/latest/download/signalfx-tracing.jar

ARG EXPOSED_PORT
EXPOSE ${EXPOSED_PORT}

ENV SPRING_PROFILES_ACTIVE docker

ENTRYPOINT ["java", \
            "-javaagent:/opt/signalfx-tracing.jar", \
            "-Dsignalfx.endpoint.url=http://tracing-server:9411/api/v2/spans", \
            "-XX:+UnlockExperimentalVMOptions", \
            "-XX:+UseCGroupMemoryLimitForHeap", \
            "-Djava.security.egd=file:/dev/./urandom", \
            "-jar", \
            "/app.jar" \
]
