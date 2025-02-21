
# Install with Docker


## Prerequisites

-   This documentation expects you already have a working  [Docker Compose](https://docs.docker.com/compose/)  environment.
    
-   Make sure to have at least 4 GB of RAM to run the containers.
    
-   You’re required to adjust your host’s settings to run Elasticsearch properly:
    
    > sysctl  -w  vm.max_map_count=262144
    


## Deployment with Docker-Compose

Step 1:  **Clone the GitHub Repo**

    git  clone  https://github.com/zammad/zammad-docker-compose.git

Make sure to run  `git  pull`  frequently to fetch updates. Alternatively, you can download the files from  [the releases page](https://github.com/zammad/zammad-docker-compose/releases).

Step 2:  **Adjust Environment as Needed**

In some cases our default environment is not what a docker-compose user is looking for. See  [Docker Environment Variables](https://docs.zammad.org/en/latest/install/docker-compose/environment.html)  for details on which settings can be configured.

Note

If you want to use a  `.env`  file, you can use the provided  `.env.dist`  file and copy it to  `.env`. That way it will be picked up by Docker-Compose automatically and not overwritten during updates.

Zammad runs on port  `8080`  by default. If you want to use another port, you can set it via the variable  `NGINX_EXPOSE_PORT`.

Step 3: Start the stack

```
cd  zammad-docker-compose
docker  compose  up  -d
```

After the stack is ready, you can access Zammad via the configured docker host and port, e.g.  `http://localhost:8080/`.

## Customizing the Zammad Stack

Sometimes it’s necessary to apply local changes to the Zammad docker stack, e.g. to include additional services. If you plan to do so, we recommend that you do not change the  `docker-compose.yml`  file, but instead create a local  `docker-compose.override.yml`  that includes all your modifications. Docker-Compose will  [automatically load this file and merge its changes into your stack](https://docs.docker.com/compose/multiple-compose-files/merge/).

## How to Run Commands in the Stack

The docker entrypoint script sets up environment variables required by Zammad to function properly. That is why calling  `rails`  or  `rake`  on the console should be done via one of the following methods:

# Directly execute a specific command:

    docker  compose  run  --rm  zammad-railsserver  rails  r  '...your rails command here...'

# Run the interactive rails console to manually enter Rails commands:

    docker  compose  run  --rm  zammad-railsserver  rails  c

# Via 'docker exec':

```
docker  exec  zammad-docker-compose-zammad-railsserver-1  /docker-entrypoint.sh  rails  r  '...your rails command here...'
```

If you need to retrieve information from the rails server, you can place for example  `pp`  (pretty print) in front of your rails command. This leads to an output in your terminal.
