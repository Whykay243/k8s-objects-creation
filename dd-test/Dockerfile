FROM openjdk:17-jdk AS builder
WORKDIR application
#ARG JAR_FILE=build/libs/MY_APP.jar
#COPY ${JAR_FILE} application.jar
#RUN java -Djarmode=layertools -jar application.jar extract

FROM openjdk:17-jdk
WORKDIR application
RUN  apt-get update \
  && apt-get install -y wget \
  && rm -rf /var/lib/apt/lists/*
RUN wget -O dd-java-agent.jar 'https://dtdg.co/latest-java-tracer'

COPY --from=builder application/application.jar ./application.jar
RUN true
COPY --from=builder application/dependencies/ ./
RUN true
COPY --from=builder application/spring-boot-loader/ ./
RUN true
COPY --from=builder application/snapshot-dependencies/ ./
RUN true
COPY --from=builder application/application/ ./
RUN true

ENV TZ Asia/Seoul

ENTRYPOINT ["java", "-javaagent:dd-java-agent.jar", "-Ddd.profiling.enabled=true", "-XX:FlightRecorderOptions=stackdepth=256", "org.springframework.boot.loader.JarLauncher"]