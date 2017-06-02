# Kong setup for local lumlife testing

## Prerequisite 

Need to have docker and postman installed

## Set up Cassandra and Kong

Run the following command to spin up cassandra database and then kong. Must wait 20 second for cassandra then start kong

    docker run -d --rm --name kong-database -p 9042:9042 cassandra:3
    -----------------wait 20 seconds---------------------------
    docker run -d --rm --name kong \
              --link kong-database:kong-database \
              -e "KONG_DATABASE=cassandra" \
              -e "KONG_CASSANDRA_CONTACT_POINTS=kong-database" \
              -e "KONG_PG_HOST=kong-database" \
              -p 8000:8000 \
              -p 8443:8443 \
              -p 8001:8001 \
              -p 7946:7946 \
              -p 7946:7946/udp \
              kong:latest

## Setup Kong API

Download and import LumLife.postman_collection.json file.

Edit global variable kong_host to your kong container IP (kong_port should be the same as 8001).

### Register API

Go to Postman/Lumlife/Kong Setup and register the first four POST requests.

NOTE: FOR EACH POST REQUEST, GO TO BODY AND FIX THE UPSTREAM_URL TO YOUR DESKTOP IP_ADDRESS, SO THAT IT CAN ACCESS YOUR SPRING LOCALHOST SERVICE

PATCH request is for modifying if you make any mistake

### Register Plugin for key-auth

Go to Postman/Lumlife/Kong Setup and register the last three POST request

## Testing

### Sign Up, Sign In

Edit global variable kong_gateway_host to your kong container IP (kong_gateway_port should be the same as 8000).

Go to Postman/Lumlife/User to test these urls. 

NOTE: LUMLIFE_USER_ID AND LUMLIFE_USER_TOKEN WILL BE CAPTURED AND STORED AUTOMATICALLY.

### Protected resource + LogOut

Edit global variable kong_gateway_host to your kong container IP (kong_gateway_port should be the same as 8000).

Go to Postman/Lumlife/Protected to test.



