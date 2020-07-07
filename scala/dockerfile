FROM openjdk:8u212-jdk-alpine as builder

RUN mkdir -p /home/app/libs

WORKDIR /home/app

COPY ./function/app.jar /home/app/app.jar

FROM openfaas/of-watchdog:0.7.6 as watchdog

COPY --from=watchdog /fwatchdog /usr/bin/fwatchdog
RUN chmod +x /usr/bin/fwatchdog

RUN addgroup -S app && adduser -S -g app app
WORKDIR /home/app
COPY --from=builder /home/app/entrypoint/build/distributions/entrypoint-1.0.zip ./entrypoint-1.0.zip
RUN unzip ./entrypoint-1.0.zip

RUN chown -R app /home/app
USER app

ENV upstream_url="http://127.0.0.1:8082"
ENV mode="http"
ENV CLASSPATH="/home/app/entrypoint-1.0/lib/*"

ENV fprocess="java -XX:+UnlockExperimentalVMOptions -XX:+UseCGroupMemoryLimitForHeap com.openfaas.entrypoint.App"
EXPOSE 8080

HEALTHCHECK --interval=5s CMD [ -e /tmp/.lock ] || exit 1

CMD ["fwatchdog"]
