## How the pipeline works

1. Developers make changes to the main branch on Github.
2. Github Actions [workflow](https://github.com/quantum-ohtu/WebMark2/blob/main/.github/workflows/docker-publish.yml) starts which builds and uploads Docker images to [Docker Hub](https://hub.docker.com/).
3. [Watchtower](https://github.com/containrrr/watchtower) container running on staging server listens for new Docker images. It will pull the uploaded images when it detects changes and restarts containers.
4. The changes are now visible on the staging server (see that some changes like changes to the database might need more than just restarting. In other words, manual actions on the server. See _Useful and needed docker commands_)

## Setting up the pipeline

1. Create a [Docker Hub](https://hub.docker.com/) account and create an access token. An access token can be created by logging in to Docker Hub and clicking on your username in the top right corner and selecting **Account Settings** > **Security** > **New Access Token**.
2. Provide DOCKERHUB_USERNAME and DOCKERHUB_TOKEN as Github Secrets. The secrets cannot be read after they have been set so they are safe from your fellow developers!
    - DOCKERHUB_USERNAME is your Docker Hub username
    - DOCKERHUB_TOKEN is the access token you created
3. Ask instructors for permissions to access the staging server. 
4. Login to the staging server according to the instructions you should have been provided with. This includes running a login script.
5. Create a file `docker-compose.yml` to `/qleader`. Here is an example [docker-compose.yml](https://github.com/quantum-ohtu/QuantMark/wiki/docker-compose.yml-in-the-HY-server) based on the current file in the staging.
6. Create a file `qleader.subfolder.conf` to `/nginx/config/nginx/proxy-confs` with the following contents:
```nginx
location /qleader/ {
	proxy_pass http://qleader-web:8000/;
        proxy_set_header Host $host;
	proxy_set_header PathPrefix /qleader;
}

```

## Useful and needed docker commands

If there is changes that require more than Watchtower's restarting actions, do the following in the given order:

1. Stopping the project:

       docker-compose down

2. Listing running containers (perhaps want to see what's the names of the containers):

       docker container ls -a
      
3. Removing container (removing your project's old containers might be needed when creating new ones):

       docker container rm qleader-db # Here is an example so replace the container name "qleader-db" with correct one!
      
    * See that there is more options like _stop_ and _kill_ if those work better for the situation than the _rm_.


4. Building project (in the beginning or if there are some bigger changes, for example, changes in the database)
    
       docker-compose up --build up -d
 
5. Restarting (might be needed if some changes has been made)

       docker-compose restart
      
    * See that restarting is useful in the project folder _/qleader_ and in the _/nginx_
 

## Known issues

* The staging server does not serve static files at all. So far this has not been an issue but this might change in the future.
* The staging server runs in a path `/qleader` so hard-coded links between pages will not work. For example, this is **NOT** going to work:
    ```python
    return redirect('/newMolecule/')  # WILL NOT WORK ON STAGING!
    ```
    You should use a view name instead (defined in urls.py) so this is correct:
    ```python
    return redirect('newMolecule')  # using view name defined in urls.py
    ```
