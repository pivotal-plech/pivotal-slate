---
title: PCF Ops Manager API Reference

language_tabs:
  - shell

toc_footers:

includes:

search: true
---

#TOPICS

Welcome to the Ops Manager API! You can use our API to access endpoints, which can create, read, update, and delete resources in Ops Manager.

We have language bindings in cURL, Ruby, and Python! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

## Authentication

You must pass a token to each API endpoint. To get a token, and curl an API endpoint using that token, follow these instructions:

> From a command line with Ruby installed, install the cf-uaac gem:

```shell
gem install cf-uaac
```
> Target your Ops Manager IP:

```shell
uaac target https://YOUR_OPSMAN_IP/uaa
```
> Log in to your Ops Manager with the Client name "opsman":

```shell
uaac token owner get

Client name: opsman
Client secret:
User name: YOUR_USERNAME_HERE
Password: YOUR_PASSWORD_HERE
```
> Retrieve your Ops Manager access token:

```shell
uaac context
```

Ops Manager uses authorization tokens to allow access to the API. You can get an authorization token from the [settings page](https://10.85.43.43/settings/edit) or by using the uaac command line tool (instructions to the right).

Ops Manager expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: Bearer YOUR_ACCESS_TOKEN`

<aside class="warning">
You must replace <code>YOUR_ACCESS_TOKEN</code> with your personal API authorization token.
</aside>


## The Ops Manager workflow

Available ---> Staged ---> Deployed

The Staged and Deployed namespaces refer to the desired and actual states of the Ops Manager installation. Most changes to an Ops Manager installation require triggering an "Apply Changes" action in order to take effect. Applying changes effectively makes the staged and deployed states identical until the next round of changes is made.

For example, when changes are made to a product, these changes are associated with the product in the Staged namespace, and triggering an "Apply Changes" action deploys these changes.

## Status Codes

Ops Manager uses conventional HTTP response codes to indicate the success or failure of an API request. Generally, codes in the 2xx range indicate success, codes in the 4xx range indicate an error that failed given the information provided (e.g., a required parameter was omitted), and codes in the 5xx range indicate an error with the Ops Manager server.

Code | Description
--------- | -------
200 - OK | Everything worked as expected
404 - not found | The route requested does not exist
400 - bad request | The request is syntactically incorrect
401 - unauthorized | The access token has expired or is invalid
409 - conflict | Another user is logged in
422 - unprocessable entity | The request is syntactically correct but the supplied values do not work
500 - internal server error | Something went wrong with our server
503 - service unavailable | The authentication service is not available yet

#COMMON TASKS
##Setting up Ops Manager
##Configuring BOSH director
##Importing and installing products
##Viewing logs and credentials
##Changing configuration settings


#CORE CONCEPTS
##Setup

## Viewing/Triggering Events
### Applying changes

Any pending changes can be triggered with the following request:

```shell
curl "https://example.com/api/v0/installations" -d -X POST
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

> The above command returns JSON structured like this:

```json
{
  "error": "undefined method `fetch' for nil:NilClass"
}
```

#### HTTP Request

POST /api/v0/installations

A successful request returns an installation id which can be used to track the status of the install process.



### Checking the status of an in-progress installation

Get the status of a running install process:

```shell
GET /api/v0/installations/:id
```

> The above command returns JSON structured like this:

```json
{
  "status": "running"
}
```

#### HTTP Request

GET /api/v0/installations/:id

### Getting a list of recent install events

Recent install events can be accessed as follows:

```shell
GET /api/v0/installations/:id
```

> The above command returns JSON structured like this:

```json
{
  "status": "running"
}
```

#### HTTP Request

GET /api/v0/installations

Recent install events can be accessed as follows:

```shell
GET /api/v0/installations/:id
```

> The above command returns JSON structured like this:

```json
{
  "status": "running"
}
```

#### HTTP Request

GET /api/v0/installations

##Staged BOSH Director
##Deployed BOSH Director
###Get credentials for a deployed director
###Get status for a deployed director
###Get logs for a deployed director
###Get manifest for a deployed director

Use the following request to retrieve the currently deployed bosh director manifest

```shell
curl "https://YOUR_OPSMANAGER_IP/api/v0/staged/director/manifest" -X GET -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

> The above command returns JSON structured like this:

This endpoint retrieves the bosh director manifest.

#### HTTP Request

GET /api/v0/staged/director/manifest



<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>


##Available Products

###Uploading products

Products (.pivotal files) can be uploaded to Ops Manager via the API

```shell
curl "https://example.com/api/v0/products" -F 'product[file]=@PRODUCT_FILENAME.pivotal' -X POST
  -H "Authorization: Bearer UAA_ACCESS_TOKEN"
```

#### HTTP Request

POST /api/v0/products

###Deleting unused products

Products (.pivotal files) can be uploaded to Ops Manager via the API

```shell
curl "https://example.com/api/v0/products" -F 'product[file]=@PRODUCT_FILENAME.pivotal' -X POST
  -H "Authorization: Bearer UAA_ACCESS_TOKEN"
```

#### HTTP Request

POST /api/v0/products


##Staged Products
###Move products to the staged namespace
###Get current resource configuration for a staged product
###Configure resources for a staged product
###Get networks and azs for a staged product
###Configure networks and azs for a staged product
###Get currently selected errands for a staged product
###Configure errands for a staged product
###Configure other properties for a staged product
###Get manifest for a staged product

#### HTTP Request

POST /api/v0/products

##Deployed Products
###Get credentials for a deployed product
###Get status for a deployed product
###Get logs for a deployed product
###Get manifest for a deployed product
###Mark a product for deletion

##Stemcells
###Uploading stemcells
###Deleting stemcells


##Disk & VM types
###Disk types
###VM types

##Export or Retire an Installation
### Export your installation
### Delete your installation

#ADVANCED
##Settings
###UAA
###Pivotal Network
##Metadata
###Migrating metadata
###Uploading metadata
##Generating manifests
##Uploading releases
