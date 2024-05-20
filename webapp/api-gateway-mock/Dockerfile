# build stage
FROM alpine:latest as build-stage

WORKDIR /mock

# copy project files and folders to the current working directory 
COPY . ./config


# -------------------------------


# production stage
FROM mockserver/mockserver:5.13.0 as production-stage

COPY --from=build-stage /mock/config /config

# env
ENV MOCKSERVER_INITIALIZATION_JSON_PATH /config/coordinates.json
ENV MOCKSERVER_PROPERTY_FILE /config/mockserver.properties

EXPOSE 1080
