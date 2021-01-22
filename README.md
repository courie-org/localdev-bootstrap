# Courie Developer Setup
Welcome! This repository contains various scripts and resources to get Courie up and running for local development.  

## Prerequisites

### Build Tools

| Software | Version |
| -------  | ------- |
| Node     | v10.10.0|
| NPM      | 6.4.1   |
| Docker   | 20.10.0 |
| Maven    | 3.6.3   | 
| JDK      | 1.8.0_231|


### Google API 
You will need to sign up for a Google API key with the following APIs enabled:

* Directions API
* Geocoding API
* Maps JavaScript API
* Places API

## Courie Services
The backend Quarkus services depend on Redis for persistence and Kafka for eventing. The included "*courie-deps-compose.yml*" will start instances of these in a few containers. 

```
docker-compose -f courie-deps-compose.yml up
```

If you would like to run all services via `docker-compose` simply run:
```
docker-compose up
```

Some things to note:
*  Remember to set the `DIRECTIONS_API_KEY` environment variable
*  Driver Web port is `3001` and can be found at [http://localhost:3001](http://localhost:3001)
*  Courie Web port is `3000` and can be found at [http://localhost:3000](http://localhost:3000)


### Driver Service
The driver service requires an environment variable with the Google API key in order to work correctly. This can be a Run Configuration setting in your IDE or just set in your env. 

__DIRECTIONS_API_KEY__ - The API obtained by Google. 

Once this env variable has been setup, you should be able to run the application via:

```
mvn clean compile quarkus:dev
```

once the service is up and running, you can test the API exposed by the application with curl (or postman):

```
curl http://localhost:8080/drivers
[]
```

### Delivery Service
This is the same as the Driver service, however, no environment variables are required at this time. 

By default, the port is configured to start on 9098.

```
curl http://localhost:9098/deliveries
[]
```

### Courie Web Applications (Driver and Customer)

#### Local ENV
Each of the reactjs applications must define a .env.local file in the root directory. You can copy the provided .env file for reference. 

Open .env.local and add the Google API key. 

```
REACT_APP_DIRECTIONS_KEY=*******API KEY GOES HERE*********
REACT_APP_DELIVERIES_HOST=localhost:9098
REACT_APP_DRIVERS_HOST=localhost:8080
```


#### Javascript Deps Install
For each of these, you will need to install the dependences via npm

```
npm install
```

> NOTE: Sometimes you could get an error for gyp not being installed. If this is the case, you may need to install Xcode CLI tools. 

#### Starting the Web Applications
By default, the react applications will start on port 3000. If you plan on running both reactjs apps at the same time, you will need to change the default port on one of them. 

Default port of 3000:
```
npm start
```

Change the port to 3001
```
PORT=3001 npm start
```


