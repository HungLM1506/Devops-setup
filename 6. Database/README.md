# DevOps Setup

This guide will help you set up Elastic Search, Kibana, Minio, and Postgres using Docker Compose.

## Prerequisites

- Docker
- Docker Compose

## Setup

1. Create a `docker-compose.yml` file in your project directory with the following content:

    ```yaml
    services:
        elasticsearch:
            image: elasticsearch:7.17.0
            container_name: elasticsearch
            environment:
            - discovery.type=single-node
            - "ES_JAVA_OPTS=-Xms2g -Xmx2g"
            - xpack.security.enabled=true
            - ELASTIC_PASSWORD=password
            mem_limit: 3g
            volumes:
            - ./esdata:/usr/share/elasticsearch/data
            ports:
            - "9200:9200"
            restart: always

        kibana:
            image: kibana:7.17.0
            container_name: kibana
            environment:
            - ELASTICSEARCH_HOSTS=http://elasticsearch:9200
            - ELASTICSEARCH_USERNAME=elastic
            - ELASTICSEARCH_PASSWORD=password
            ports:
            - "5601:5601"
            depends_on:
            - elasticsearch
            volumes:
            - ./kibanadata:/usr/share/kibana/data
            restart: always

        minio:
            image: minio/minio:latest
            container_name: minio
            environment:
            MINIO_ROOT_USER: admin
            MINIO_ROOT_PASSWORD: password
            ports:
            - "9000:9000"
            - "9001:9001"
            volumes:
            - ./minio-data:/data
            command: server /data --console-address ":9001"
            restart: always

        postgres:
            image: pgvector/pgvector:pg16
            container_name: pgsql_pgvector
            restart: always
            environment:
            POSTGRES_USER: ${POSTGRES_USER:-admin}     
            POSTGRES_PASSWORD: ${POSTGRES_PASSWORD:-password} 
            POSTGRES_DB: postgres         
            PGDATA: /data/postgres
            ports:
            - "5432:5432"                
            volumes:
            - ./postgres:/var/lib/postgresql/data 
            command: >
            bash -c "chown -R postgres:postgres /var/lib/postgresql/data &&
                    chmod -R 700 /var/lib/postgresql/data &&
                    docker-entrypoint.sh postgres"
    ```

2. Run the following command to start the services:

    ```sh
    docker-compose up -d
    ```

3. Access the services using the following URLs:
    - Elastic Search: [http://localhost:9200](http://localhost:9200)
    - Kibana: [http://localhost:5601](http://localhost:5601)
    - Minio: [http://localhost:9000](http://localhost:9000)
    - Postgres: `localhost:5432` (You can use a database client to connect)

## Stopping the Services

To stop the services, run:

```sh
docker-compose down
```

## Additional Information

- For more details on Elastic Search, visit the [official documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html).
- For more details on Kibana, visit the [official documentation](https://www.elastic.co/guide/en/kibana/current/index.html).
- For more details on Minio, visit the [official documentation](https://docs.min.io/).
- For more details on Postgres, visit the [official documentation](https://www.postgresql.org/docs/).
