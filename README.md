# Minimalist Devops Infra

Self-hosted source code management and continious integration solution.
This can run on any docker host. This is intented mostly for experimentation,
please improve on backup and security if used for mission critical deployments.

## Prequsites

* A Linux host or VM
    * Minimum 1 CPU core and 512MB memory tested
    * Or a raspberry pi
* Docker installed [follow official docs for ubuntu](https://docs.docker.com/engine/install/ubuntu/#install-using-the-convenience-script).
* docker-compose installed (pip install docker-compose)

## Setting Up Gitea & Drone Server

```bash
# Clone this repo
git clone http://github.com/tyfncn/minimal-devops-infra.git infra
cd infra
# Put docker host ip adress into .env file
nano .env
# Start gitea service
docker-compose up -d gitea
```
and go to `http://<your ip address>`

* Site Title
* Admin account settings
    * username
    * password
    * an email
* Top right avatar -> Settings -> Applications -> Manage Oauth2 Applications
    * Application Name: `droneio`
    * Redirect URI: `http://<your ip address>:81/login`
    * Save
    * Copy `Client ID` to `DRONE_GITEA_CLIENT_ID` in docker-compose.yml
    * Copy `Client Secret` to `DRONE_GITEA_CLIENT_SECRET` in docker-compose.yml
    * Put a random string in `DRONE_RPC_SECRET`

```bash
# Start drone service
docker-compose up -d drone
```
and go to `http://<your ip address>:81` and finish drone setup

```bash
# Start drone docker runner service
docker-compose up -d drone-runner
```

**Congrats !! Your SCM and CI is now installed**

## Next Steps

* import your application repo to gitea
* Add `.drone.yml` for automated build & tests (examples: [python](https://docs.drone.io/pipeline/docker/examples/languages/python/), [go](https://docs.drone.io/pipeline/docker/examples/languages/golang/), )
* Add a promotion pipeline to deploy same docker host
* Add traefik to use domain names instead of ip adress