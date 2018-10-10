# Mongo Docker Shell
A simple shell application that uses Docker to run a container-ized MongoDB

### Requirements
* Docker
* NPM
* Node >= 8

> **Note for Windows users** - you may only have the option to install Docker Toolkit which is fine. During setup be sure `Docker Compose` is checked in the first step (others can be unchecked), then accept the defaults for the remainder if the installation. **After installation** run the Docker Terminal App - all Docker activity is performed within this command prompt utility VM.

## Starting Mongo with NPM
An NPM script, `start` has been included in `package.json` that runs the Docker command to start the mongo server:

`npm start`

> Note: This runs the Docker container in the background. To run in the foreground and see the output, remove the `-d` option in `package.json`.

## Stopping Mongo with NPM
A `stop` script has also been included in `package.json` to make stopping the environment easier:

`npm run stop`

## Connecting to Mongo with GUI Client (Robo 3T)
* host: `localhost` or `127.0.0.1`
* port: default `27017`
* auth database: `admin`
* user: `root`
* pass: `root`

> Note: `user`, `pass`, and `port` can be changed in `mongo.yml` if desired though likely uncessary for local development

## Connecting with Mongo Terminal Client
Docker can be used to connect directly to the Mongo database server if access to the terminal is needed. This is a two step process that first requires obtaining the **container id** that is currently running Mongo.

* **Step 1** Start the Mongo container with `npm start`
* **Step 2** Locate the container id matching the `Mongo` image with `docker ps`
* **Step 3** Copy the container id and paste it into this terminal command: 

`docker exec -it [container_id] mongo --username root --password root --authenticationDatabase admin`

example:

`docker exec -it b90b5c439315 mongo --username root --password root --authenticationDatabase admin`

## Starting Over
Did something come off the rails along the way? Want a fresh Database? You can delete the `Mongo` Docker Image, and the next time the evironment is started it will be pulled fresh from the Docker Repository. This is done in two steps.

* **Step 1** find the `image id` of the Mongo image using `docker image ls`
* **Step 2** paste this id in to the following command: `docker rmi image_id --force`, e.g., `docker rmi 052ca8f03af8 --force`

## Configuring Mongo
To set additional config options, or change the defaults, edit the `mongo.yml` file

### Troubleshooting
* Port 27017 already in use
    * A Mongo container is probably already running
    * Try another `npm run stop`
* `npm run stop` didn't stop the Mongo container
    * Happens occasionally. Usually another `npm run stop` will kill it
    * If problem persists, kill the container manually:
        * Check running processes with `docker ps`, copy container id
        * If another Mongo container is running, kill it with `docker kill [containerid]`