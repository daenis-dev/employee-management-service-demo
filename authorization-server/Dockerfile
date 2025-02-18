FROM debian:bullseye-slim AS builder

RUN apt-get update && apt-get install -y \
    ca-certificates \
    openjdk-17-jre-headless \
    && rm -rf /var/lib/apt/lists/*

FROM quay.io/keycloak/keycloak:26.0.4

COPY certs/server.p12 /opt/keycloak/conf/server.p12
COPY conf/keycloak.conf /opt/keycloak/conf/keycloak.conf
COPY conf/Employee-Management-Service-realm.json /opt/keycloak/data/import/realm.json
COPY certs/employee-service.crt /app/employee-service.crt

USER root
RUN chown keycloak:keycloak /opt/keycloak/conf/server.p12
RUN chown keycloak:keycloak /app/employee-service.crt
RUN keytool -importcert -file /app/employee-service.crt -alias employee-service -cacerts -storepass changeit -noprompt -v
USER keycloak

ENV KC_BOOTSTRAP_ADMIN_USERNAME=auth-admin
ENV KC_BOOTSTRAP_ADMIN_PASSWORD=changeit
ENV KC_IMPORT=/opt/keycloak/data/import/realm.json

EXPOSE 9880

ENTRYPOINT ["/opt/keycloak/bin/kc.sh", "start", "--https-key-store-file=/opt/keycloak/conf/server.p12", "--https-key-store-password=changeit", "--db-url=jdbc:postgresql://auth-db:5432/authorization-db", "--db-password=changeitdb", "--import-realm"]
