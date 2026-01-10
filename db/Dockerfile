FROM supabase/postgres:15.8.1.085

# Install postgres_exporter
ARG POSTGRES_EXPORTER_VERSION=0.15.0
ADD https://github.com/prometheus-community/postgres_exporter/releases/download/v${POSTGRES_EXPORTER_VERSION}/postgres_exporter-${POSTGRES_EXPORTER_VERSION}.linux-amd64.tar.gz /tmp/
RUN tar xzf /tmp/postgres_exporter-${POSTGRES_EXPORTER_VERSION}.linux-amd64.tar.gz -C /tmp \
    && mv /tmp/postgres_exporter-${POSTGRES_EXPORTER_VERSION}.linux-amd64/postgres_exporter /usr/local/bin/ \
    && rm -rf /tmp/postgres_exporter*

COPY ./scripts/realtime.sql /docker-entrypoint-initdb.d/migrations/99-realtime.sql
COPY ./scripts/webhooks.sql /docker-entrypoint-initdb.d/init-scripts/98-webhooks.sql
COPY ./scripts/roles.sql /docker-entrypoint-initdb.d/init-scripts/99-roles.sql
COPY ./scripts/jwt.sql /docker-entrypoint-initdb.d/init-scripts/99-jwt.sql
COPY ./scripts/_supabase.sql /docker-entrypoint-initdb.d/migrations/97-_supabase.sql
COPY ./scripts/logs.sql /docker-entrypoint-initdb.d/migrations/99-logs.sql

COPY --chmod=755 wrapper.sh /usr/local/bin/wrapper.sh

EXPOSE 5432 9187

ENTRYPOINT ["wrapper.sh"]
CMD ["postgres", "-c", "config_file=/etc/postgresql/postgresql.conf", "-c", "log_min_messages=fatal", "--port=5432"]
