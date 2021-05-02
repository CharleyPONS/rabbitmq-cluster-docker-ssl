FROM rabbitmq:3-management-alpine as build_base

#Set labels to record information
LABEL maintainer="Artly Production"
LABEL org.label-schema.version=$BUILD_VERSION
LABEL org.label-schema.build-date=$BUILD_DATE

RUN addgroup -S rabbit && adduser -S rabbit -u 1001 -G rabbit

COPY --chown=rabbit:rabbit definitions.json /etc/rabbitmq/definitions.json
COPY --chown=rabbit:rabbit $FILE_PATH_CONFIG_RABBIT /etc/rabbitmq/$FILE_PATH_CONFIG_RABBIT
