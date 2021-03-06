# Docker network ready to start coding your Symfony app

After following the tutorial by  Gary Clarke https://github.com/GaryClarke on how to set a working envoriment of docker containers to run a Symfony app I thought it would be useful to save a copy I can use as a template for my future projects. Moreover, I could create a repo so everyone can have a working symfony env out of the box.

I'm not an expert on docker so I strongly recommend you to watch the tutorial series by Gary. I found it really educational. 
https://www.youtube.com/watch?v=ITOnpzkzlYM&list=PLQH1-k79HB39jeuhSFRwAg2i-BqDeArP-&ab_channel=GaryClarke

Anyways, for those who don't have time to watch a 30 minute tutorial here is a quick guide, let's get down to it.


# Guide

## 0. Requirements: install docker

## 1. Build the php container

Once the repo is downloaded create de php container
```
docker-compose up -d --build
```

## 2. install composer (inside the php container!)
First check out the php container name. It has the same name of the service with the porject name as a sufix,
```
docker ps
```
It should be named something like **projectName_php74-service_1**.
Then enter to the container using the bash console:

```
docker exec -it **projectName_php74-service_1** bash

composer install
```
exit the container

## 3. Create the databse
```
docker-compose run --rm php74-service php bin/console doctrine:database:create
```
## 4. Install yarn / npm

```
docker-compose run --rm node-service yarn install
docker-compose run --rm node-service yarn dev
```

```
docker-compose run --rm node-service npm install
docker-compose run --rm node-service npm run dev
```

## 5. Create a repo for your symfony app.

Now you have started a docker project which happens to contain some symofny directories, you don't need to (and shouldn't) push and pull all the docker related items every time, create a proper symfony repo inside de /app directory.

The working tree goes as follows:
```
docker-symfony
├── app  (this is your symfony app, start your git project here)
├── mysql
├── nginx
└── php
```

 
 # Run and stop the envoriment
 
 Go to the root of this project and run:
 to start:
 ```
 docker-compose up -d
 ```
 
 To stop:
 ```
 docker-compose down
 ```
 
 When it is running to interact with some container use docker exec
 ```
 docker exec -it [container name] [command to run]
 ```
 
 example, run bash inside the php container:
 ```
 docker exec -it php74-container bash
```

# Permissions of files created inside a container

Files and directories created by composer or yarn inside the containers are created by the root user since this is the user inside the container.
By now I came up with changing the owner once the files are created
```
chown 1000:1000 -R .
```

Any other more definitive solution would be most appreciated, let me know if you have one. Thanks

# Final note and greetings

First of all, a big shoutout for Gary Clarke for his amazing tutorial.

Finally, feel free to share any recommendations or issues you encounter, I probably won't know the answer but it would be mutualy benefitial for all symfony users.


