FROM java:8-jre-alpine
# Use any image as your base image, or "scratch"
# Then set fprocess to the process you want to invoke per request - i.e. "cat" or "my_binary"

RUN apk add --no-cache curl && \
    curl -sfL \
    https://github.com/openfaas-incubator/of-watchdog/releases/download/0.7.6/of-watchdog > /usr/bin/fwatchdog && \
    chmod +x /usr/bin/fwatchdog

COPY ./function/app.jar /app.jar

ENV fprocess="java -jar /app.jar"
ENV upstream_url="http://127.0.0.1:8082"
ENV mode="http"
RUN touch /tmp/.lock

HEALTHCHECK --interval=5s CMD [ -e /tmp/.lock ] || exit 1
EXPOSE 8080
CMD ["fwatchdog"]
