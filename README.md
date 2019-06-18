# cf-client3

[![Build Status](https://travis-ci.org/IBM-Bluemix/cf-nodejs-client.svg)](https://travis-ci.org/IBM-Bluemix/cf-nodejs-client)
[![Build status](https://ci.appveyor.com/api/projects/status/adxrubgykqys7pp9?svg=true)](https://ci.appveyor.com/project/jsloyer/cf-nodejs-client)
[![Dependency Status](https://david-dm.org/IBM-Bluemix/cf-nodejs-client.svg)](https://david-dm.org/IBM-Bluemix/cf-nodejs-client)
[![devDependency Status](https://david-dm.org/IBM-Bluemix/cf-nodejs-client/dev-status.svg)](https://david-dm.org/IBM-Bluemix/cf-nodejs-client#info=devDependencies)
[![Changelog](https://img.shields.io/badge/see-CHANGELOG-red.svg?style=flat-square)](https://github.com/IBM-Bluemix/cf-nodejs-client/blob/master/CHANGELOG.md)
[![API Doc](https://doclets.io/IBM-Bluemix/cf-nodejs-client/master.svg)](https://doclets.io/IBM-Bluemix/cf-nodejs-client/master)

# Table of Contents

- [Overview](#overview)
- [Focus](#focus)
- [Getting Started](#getting-started)
- [JSDoc](https://doclets.io/IBM-Bluemix/cf-nodejs-client/master)
- [Changelog](https://github.com/IBM-Bluemix/cf-nodejs-client/blob/master/CHANGELOG.md)
- [Testing](#testing)
- [Issues](#issues)
- [References](#references)

# Overview

This project provides a simple client library to interact with the [Cloud Foundry Architecture](https://docs.pivotal.io/pivotalcf/concepts/architecture/):

![ScreenShot](https://raw.githubusercontent.com/IBM-Bluemix/cf-nodejs-client/master/docs/cf_architecture_block.png)

Using this library, you could interact with the following platforms: [PWS](https://console.run.pivotal.io)
, [Bluemix](https://console.ng.bluemix.net/) or a [Local Cloud Foundry instance](https://github.com/yudai/cf_nise_installer):

| **[Cloud Controller](http://apidocs.cloudfoundry.org/)**  	| **[UAA](https://github.com/cloudfoundry/uaa)**   	| **Logging & Metrics** 	|
|------------------------ |-----------------------	|------------------------	|
| [Apps](https://doclets.io/IBM-Bluemix/cf-nodejs-client/master#dl-Apps)                    | [Users](https://doclets.io/IBM-Bluemix/cf-nodejs-client/master#dl-UsersUAA)             	    | [Logs](https://doclets.io/IBM-Bluemix/cf-nodejs-client/master#dl-Logs)                   	|
| [Buildpacks](https://doclets.io/IBM-Bluemix/cf-nodejs-client/master#dl-BuildPacks)              |                    	    |                       	|
| [Domains](https://doclets.io/IBM-Bluemix/cf-nodejs-client/master#dl-Domains)                 |                    	    |                       	|
| [Jobs](https://doclets.io/IBM-Bluemix/cf-nodejs-client/master#dl-Jobs)                    |                    	    |                       	|
| [Organizations](https://doclets.io/IBM-Bluemix/cf-nodejs-client/master#dl-Organizations)           |                    	    |                       	|
| [Organizations Quotas](https://doclets.io/IBM-Bluemix/cf-nodejs-client/master#dl-OrganizationsQuota)     |                    	    |                       	|
| [Routes](https://doclets.io/IBM-Bluemix/cf-nodejs-client/master#dl-Routes)                  |                    	    |                       	|
| [Services](https://doclets.io/IBM-Bluemix/cf-nodejs-client/master#dl-Services) | | |
| [Service Bindings](https://doclets.io/IBM-Bluemix/cf-nodejs-client/master#dl-ServiceBindings)        |                    	    |                       	|
| [Service Instances](https://doclets.io/IBM-Bluemix/cf-nodejs-client/master#dl-ServiceInstances) | | |
| [Service Plans](https://doclets.io/IBM-Bluemix/cf-nodejs-client/master#dl-ServicePlans) | | |
| [Spaces](https://doclets.io/IBM-Bluemix/cf-nodejs-client/master#dl-Spaces)                  |                    	    |                       	|
| [Spaces Quotas](https://doclets.io/IBM-Bluemix/cf-nodejs-client/master#dl-SpacesQuota)            |                    	    |                       	|
| [Stacks](https://doclets.io/IBM-Bluemix/cf-nodejs-client/master#dl-Stacks)                  |                    	    |                       	|
| [User provided Services](https://doclets.io/IBM-Bluemix/cf-nodejs-client/master#dl-UserProvidedServices)  |                    	    |                       	|
| [Users](https://doclets.io/IBM-Bluemix/cf-nodejs-client/master#dl-Users)                   |                    	    |                       	|

# Focus

The development doesn't cover the whole CloudController API. Main areas of development are:

**App life cycle:**

* Create an App
* Upload source code in .zip or .war (Support for Static, Python, PHP, Node.js & JEE)
* Create an User Provided Services
* Associate Apps with an User Provided Services
* Start | Stop an App
* Restage Apps
* Scale Apps
* Simple Logs management
* Remove Apps
* Remove User Provided Services

**PaaS Management:**

* Organization quota
* Organization
* Space
* Services, Service Instances, Service Plans, User provided Services & Service Binding
* UAA Users
* Users

# Getting Started

If you need to interact with a Cloud Foundry platform try this [online tool](https://tonicdev.com/npm/cf-client) and use this example:

``` Javascript
"use-strict";

const endpoint = "https://api.ng.bluemix.net";
const username = "BLUEMIX_USERNAME";
const password = "BLUEMIX_PASSWORD";

const CloudController = new (require("cf-client")).CloudController(endpoint);
const UsersUAA = new (require("cf-client")).UsersUAA;
const Apps = new (require("cf-client")).Apps(endpoint);

CloudController.getInfo().then( (result) => {
    UsersUAA.setEndPoint(result.authorization_endpoint);
    return UsersUAA.login(username, password);
}).then( (result) => {
	Apps.setToken(result);
    return Apps.getApps();
}).then( (result) => {
    console.log(result);
}).catch( (reason) => {
    console.error("Error: " + reason);
});

```

Explore the library and if you like the features, use it on your App:

``` Javascript

npm install cf-client --save

```

# Testing

This project has a test suite to ensure the readability of this project. Take a look the [Tests cases](https://github.com/jabrena/cf-nodejs-client/tree/master/test/) developed with [Mocha](https://mochajs.org/) & [Chai](http://chaijs.com/api/bdd/) to understand some stuff about [Cloud Foundry](https://www.cloudfoundry.org/)  and the usage of this client. Besides, the project has invested some amount of time in testing phase to be the code with a nice coverage level.

The development has been tested with:

| [Local Instance](https://github.com/yudai/cf_nise_installer) | [PWS](https://console.run.pivotal.io)           | [Bluemix](https://console.ng.bluemix.net/) |
| -------------- |:-------------:| -------:|
| 2.25.0         | 2.60.0        | 2.54.0  |

**Last test:** 2016/01/26

**Testing against Bluemix:**

``` shell
export BLUEMIX_CF_API_URL=https://api.ng.bluemix.net && export BLUEMIX_username=$USERNAME && export BLUEMIX_password=$PASSWORD && npm run test:bluemix
```

**Test suite:**

``` shell
npm test

```

**Code coverage:**

``` shell
istanbul cover node_modules/mocha/bin/_mocha -- -R spec

```

# Issues

If you have any question or doubt, please [create an issue](https://github.com/IBM-Bluemix/cf-nodejs-client/issues).

# License

Licensed under the [Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0).

# References

* API Docs: http://apidocs.cloudfoundry.org/
* Developer list: https://lists.cloudfoundry.org/archives/list/cf-dev@lists.cloudfoundry.org/
* PWS Console: https://console.run.pivotal.io
* Bluemix Console: https://console.ng.bluemix.net/
* PWS Forum: https://support.run.pivotal.io/forums
* Bluemix Forum: https://developer.ibm.com/answers/
* CF for Beginners: From Zero to Hero http://slides.cf-hero.cloudcredo.io/
