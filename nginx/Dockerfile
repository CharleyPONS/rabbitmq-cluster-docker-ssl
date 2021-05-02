FROM nginx:alpine

#Set labels to record information
LABEL maintainer="Artly Production"
LABEL org.label-schema.version=$BUILD_VERSION
LABEL org.label-schema.build-date=$BUILD_DATE

# Create a system group and user to be used by HAProxy.
ARG FILE_PATH

COPY --chown=nginx:nginx  $FILE_PATH /etc/nginx/nginx.conf
CMD ["nginx" , "-g", "daemon off;"]

