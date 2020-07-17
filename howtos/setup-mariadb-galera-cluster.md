# Create a local MariaDb Galera cluster with docker images

## Preconditions

### Docker installed
https://docs.docker.com/get-docker/

#### Docker composed installed
https://docs.docker.com/compose/install/
For Windows user, it comes by default with Docker Desktop from the above step.

### Authenticate to Google registry
https://confluence.camunda.com/display/camBPM/Pulling+images+locally+from+Google+registry

## Create docker compose file
Create a `docker-compose.yaml` file with the following content:

```
master:
        image: gcr.io/ci-30-162810/mariadb:g25v0.3.2
        container_name: master
        environment:
                - MARIADB_OPTS=--wsrep-new-cluster --wsrep-node-address=master
        ports:
                - "3306:3306"
                - "4567:4567"
                - "4568:4568"
                - "4444:4444"
        mem_limit: 4G
        tty: true

master2:
        image: gcr.io/ci-30-162810/mariadb:g25v0.3.2
        container_name: master2
        environment:
                - MARIADB_OPTS=--wsrep_cluster_address=gcomm://master --wsrep_sst_receive_address=0.0.0.0:4444 --wsrep_provider_options=gmcast.listen_addr=tcp://0.0.0.0:4567;base_port=4567
        ports:
                - "13306:3306"
                - "14567:4567"
                - "14568:4568"
                - "14444:4444"
        mem_limit: 1G
        links:
          - master
        tty: true

master3:
        image: gcr.io/ci-30-162810/mariadb:g25v0.3.2
        container_name: master3
        environment:
                - MARIADB_OPTS=--wsrep_cluster_address=gcomm://master --wsrep-node-address=master3 --wsrep_sst_receive_address=0.0.0.0:4444 --wsrep_provider_options=gmcast.listen_addr=tcp://0.0.0.0:4567;base_port=4567
        ports:
                - "33306:3306"
                - "34567:4567"
                - "34568:4568"
                - "34444:4444"
        mem_limit: 4G
        links:
          - master
        tty: true
```

## Execute the compose
In the directory where the compose file is stored, execute the following:
```docker-compose up -d```

## Adjust jdbcUrl
Set the `jdbcUrl` to:
```
jdbc:mariadb:failover://localhost:3306,localhost:13306,localhost:33306/process-engine
```
