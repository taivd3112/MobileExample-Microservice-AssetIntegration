# MobileExample-Microservice-AssetIntegration
Here is a sample microservice that communicates with a Predix Assets service instance and pushes data to Predix Mobile service instance. The data gets synced on devices running a Predix Mobile app.  

## Before You Begin:
It is assumed that you have running instances of [_Predix Mobile service_](https://www.predix.io/docs#rae4EfJ6) and [_Predix Asset service_](https://www.predix.io/docs/#aRPNr2R9) , and you have installed the _Predix Mobile pm command line tool_.  

## Configuration

When running on a development system, use the configuration found in the `config/default.json` directory. When pushing to Cloud Foundry, use the `manifest.yml`.
Here is a simple configuration document:

```
{
  "PORT": 8086,

  "ASSET": {
    "PA_PREDIX_ZONE_ID"   : "43bf717f-86a5-45da-9053-6651c00d1a99", //Asset zone/tenant id
    "PA_URL_ASSET"        : "https://predix_asset.run.aws-usw02-pr.ice.predix.io/locomotives", //query url for Asset
    "PA_URL_UAA"          : "https://predix-uaa.run.aws-usw02-pr.ice.predix.io/oauth/token", // UAA url for Asset service
    "PA_USERNAME"         : "asset_user", //UAA user who is allowed to access Asset service
    "PA_PASSWORD"         : "********",
    "DATA_REFRESH_TIME"   : 5 //used to refresh data
  },

  "MOBILE_UP": {
    "PM_EP_URL"           : "https://Mobile_up.run.aws-usw02-pr.ice.predix.io/", // Mobile service `api_gateway_short_route`
    "PM_USERNAME"         : "mobileup_user@ge.com",
    "PM_PASSWORD"         : "********",
    "PM_UAA_URL"          : "https://predix-uaa.run.aws-usw02-pr.ice.predix.io", // UAA url for Mobile services
    "WEB-APP-NAME"        : "pa-basic-app", //App name that can access the Asset data generated by this microservice: name and version will be used to create a channel on which this data will be assigned.
    "WEB-APP-VERSION"     : "0.0.1"
  }
}
```

## Installation

Checkout this repository, open the folder in a terminal window, and execute:

```
npm install
```

## Running the example

Run either of these commands:

```
node index.js
OR
node index.js | ./node_modules/.bin/bunyan # outputs nice looking logs in terminal window
```

The command will:

* Get oauth-token for Asset service.
* Get oauth-token for Mobile service.
* Fetch data from Asset service every 5 seconds.
* Create (update) a Mobile service document (`docid = PM_USERNAME+'_predix_asset_'+PA_PREDIX_ZONE_ID`) and assign it to a channel, based on WEB-APP-NAME & WEB-APP-VERSION. 
* Push this document into Mobile service.
* Start an Express server {TODO: accept commands to make changes in asset service data}  


## [Sample Asset WebApp]
[Sample Asset WebApp]:https://github.com/PredixDev/MobileExample-WebApp-AssetIntegration
