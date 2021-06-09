## How the pipeline works

1. Developers make changes to the main branch on Github.
2. Github Actions [workflow](https://github.com/quantum-ohtu/WebMark2/blob/main/.github/workflows/docker-publish.yml) starts which builds and uploads Docker images to [Docker Hub](https://hub.docker.com/).
3. [Watchtower](https://github.com/containrrr/watchtower) container running on staging server listens for new Docker images. It will pull the uploaded images when it detects changes and restarts containers.
4. The changes are now visible on the staging server.

## Setting up the pipeline

1. Create a [Docker Hub](https://hub.docker.com/) account and create an access token. An access token can be created by logging in to Docker Hub and clicking on your username in the top right corner and selecting **Account Settings** > **Security** > **New Access Token**.
2. Provide DOCKERHUB_USERNAME and DOCKERHUB_TOKEN as Github Secrets. The secrets cannot be read after they have been set so they are safe from your fellow developers!
    - DOCKERHUB_USERNAME is your Docker Hub username
    - DOCKERHUB_TOKEN is the access token you created
3. Ask instructors for permissions to access the staging server. 
4. Login to the staging server according to the instructions you should have been provided with. This includes running a login script.
5. Create a file `docker-compose.yml` to `/qleader`.
6. Create a file `qleader.subfolder.conf` to `/nginx/config/nginx/proxy-confs` with the following contents:
```nginx
location /qleader {
	proxy_pass http://quantmark-web:8000/;
        proxy_set_header Host $host;
}
```
7. All done!

## Known issues

* The staging server is very close to the development environment when it should reflect the production environment.
* The staging server does not serve static files at all. So far this has not been an issue but this might change in the future.
* The staging server runs in a path `/quantmark` so hard-coded links between pages will not work. For example, this is **NOT** going to work:
    ```python
    return redirect('/newMolecule/')  # WILL NOT WORK ON STAGING!
    ```
    You should use a view name instead (defined in urls.py) so this is correct:
    ```python
    return redirect('newMolecule')  # using view name defined in urls.py
    ```
