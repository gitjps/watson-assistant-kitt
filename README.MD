# Introduction
Graph databases can be used to control chatbots, transfer knowledge between chatbot solutions, bridge semantic domains etc.
This repo contains a simple technical integration between a chatbot based on Watson Assistant service and the [KITT](https://www.ibm.com/blogs/internet-of-things/iot-digital-twins-foster-innovation/) solution that is the foundation of the Digital Twin solution of IBM Research, Dublin.
It uses the pre-installed *Retail_Store* graph, that contains some sample data (store, aisle, sensors, etc).
Currently a basic use case is implemented traversing the graph:
- list all stores
- select one of the stores
- list all aisles of that store

More complex use cases with a tighter integration of the KITT knowledge graph and the Watson Assistant skill are conceivable.

# Architecture
![Architecture](WA-KITT-Architecture.jpg)

# KITT
- it is assumed that a KITT instance is deployed (not covered here)
- note down the url, user, password, and graph name
The KITT instance comes with a Retail_Store graph, we'll be using the *store* instance *Dublin01* and *aisle* instance *Grocerie*.
![KITT](kitt.jpg)

# Node-RED Application
- create a Node-RED instance and import the [Node-RED flow](node-red-flow.json)
- deploy the Node-RED flow
- adapt the **Initialize Data** node (parameters of the KITT instance)
![Node-RED flow](node-red-flow.jpg)

## Flow
1. initialize variables
2. receive request from the Watson Assistant service and do request specific initialization (e.g. store the name of the request); throw an error if the request name is unknown
3. get a bearer token 
4. query against KITT using the bearer token
5. create the result/response for the Watson Assistant service depending on the request name and return to the WA service

# Watson Assistant
- create a WA instance
- create an English skill and import the [JSON file](skill-Retail-Store.json)
- adapt the webhook: http://node-red-address/wahandler

![wa.jpg](wa.jpg)

If an utterance like "show me all stores" is detected the Node-RED flow is called via a Webhook.
The child node prompts for the store name to be selected.

