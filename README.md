# Minimalist Devops Infra

This is a repository with some buzzwords in title that shows I do some cool staff.

# Prequsites

* A Linux host or VM
    * Minimum 1 CPU core and 512MB memory tested
    * Or a raspberry pi
* Docker installed [follow official docs for ubuntu](https://docs.docker.com/engine/install/ubuntu/#install-using-the-convenience-script).
* docker-compose installed (pip install docker-compose)

# Setting Up Gitea & Drone Server

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