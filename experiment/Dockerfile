FROM registry.redhat.io/rhbk/keycloak-rhel9:22 as builder

# Enable health and metrics support
ENV KC_HEALTH_ENABLED=true
ENV KC_METRICS_ENABLED=true

# Configure a database vendor
ENV KC_DB=postgres

WORKDIR /opt/keycloak
# for demonstration purposes only, please make sure to use proper certificates in production instead
RUN keytool -genkeypair -storepass password -storetype PKCS12 -keyalg RSA -keysize 2048 -dname "CN=server" -alias server -ext "SAN:c=DNS:localhost,IP:127.0.0.1" -keystore conf/server.keystore
RUN /opt/keycloak/bin/kc.sh build

FROM registry.redhat.io/rhbk/keycloak-rhel9:22
COPY --from=builder /opt/keycloak/ /opt/keycloak/

# change these values to point to a running postgres instance
ENV KC_DB=database1
ENV KC_DB_URL=jdbc:postgresql://mbk-postgres:5432/public
ENV KC_DB_USERNAME=user1
ENV KC_DB_PASSWORD=password1
ENV KC_HOSTNAME=mbk-postgres
ENTRYPOINT ["/opt/keycloak/bin/kc.psh"]