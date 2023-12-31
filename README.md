# PowerSync + Node.js + Railway Backend

## Overview
This repo contains a starter Node.js server application which has HTTP endpoints to authorize a [PowerSync](https://www.powersync.com/) enabled application to sync data between a SQLite client and a Postgres database. This repo is intended for use as a starter template on [Railway](https://railway.app/).

[![Deploy on Railway](https://railway.app/button.svg)](https://railway.app/template/RfZi6y?referralCode=2vw_V9)

For an overview of PowerSync, see [here](https://docs.powersync.com/overview/powersync-overview).

Once you have deployed this template on Railway, you'll need to:

- Implement endpoints to accept client writes
- Implement an endpoint that generates a JWT for clients to use
- Implement a JWKS endpoint, used by PowerSync to authenticate JWTs

# Accepting Client Writes

The relevant endpoints are:

1. PUT `/api/data`

   - PowerSync uses this endpoint to sync upsert events that occurred on the client application.

2. PATCH `/api/data`

   - PowerSync uses this endpoint to sync update events that occurred on the client application.

3. DELETE `/api/data`

    - PowerSync uses this endpoint to sync delete events that occurred on the client application.

# Generating Client JWT

See our [docs](https://docs.powersync.com/usage/installation/authentication-setup/custom) for additional details.

1. GET `/api/auth/token`
   -  PowerSync uses this endpoint to retrieve an JWT token that is generated by the server and is used to authenticate requests.

# JWKS endpoint
   
1. GET `api/auth/keys` 
   - PowerSync uses this endpoint to retrieve the JWKS to validate the token provided in the response above.  

## Packages Used
- [node-postgres](https://github.com/brianc/node-postgres)  is used to interact with the Postgres database when a PowerSync enabled client performs requests to the `/api/data` endpoint.
- [jose](https://github.com/panva/jose) is used to sign the JWT which PowerSync uses for authorization.

## Setup

1. Clone the repository and `nvm use && yarn install`

2. Generate a new public/private key pair by running the following command:
```shell
yarn keys
```
This will output two new keys in the terminal window. Set these as environment variables on the Railway app:
```shell
POWERSYNC_JWT_PUBLICKEY
POWERSYNC_JWT_PRIVATEKEY
```
## Local Development
1. Create a new `.env` file in the root project directory and add the variables as defined in the `.env` file:
```shell
cp .env.template .env
```
2. Run the following to start the application
```shell
yarn start
```
This will start the app on `http://127.0.0.1:PORT`, where PORT is what you specify in your `.env` file.

3. Test if the app is working by opening `http://127.0.0.1:PORT/api/auth/keys/` in the browser

4. You should get a JSON object as the response to that request
