# NLP4Sales Service Broker

Service Broker for Sales Natural Language Processing Reuse Service

## Setup and Deploy Service Broker on CF

### Prerequisites:
1. create xsuaa instance with broker plan, e.g. nlp4sales-uaa
2. [NLP4Sales](https://github.wdf.sap.corp/ML-innovation-SG/nlp4sales.git) deployed in your space and use the nlp4sales-uaa. Make sure
3. Node.js v6 or later
4. Cloud Foundry CLI
5. Access to a Cloud Foundry installation where you can login via CLI and push applications


### Setup

#### Add the Service Broker Framework


Download the @sap/sbf package and add it to your service broker by executing the following command:

```
npm install
```

#### Generate Catalog Ids
Execute the following command to generate unique IDs for the services and their plans in the catalog.json file:
```
node_modules/.bin/gen-catalog-ids
```

#### Service Broker Hashed Credentials
Generate password in hashed format

```
node_modules/.bin/hash-broker-password
```

Copy the generated password and replace **<hashed-password>** in [**manifest.yml**](https://github.wdf.sap.corp/ML-innovation-SG/nlp4sales-service-broker/blob/master/manifest.yml)

#### Set URL of Photo wall reuse Service ([Repo](https://github.wdf.sap.corp/sapit-cf-training/photo-wall))

Replace **https://nlp4sales-backend.internal.cfapps.sap.hana.ondemand.com** in [**manifest.yml**](https://github.wdf.sap.corp/ML-innovation-SG/nlp4sales-service-broker/blob/master/manifest.yml) with the URL of service deployed in your space.



#### Create Audit log service instance
The service broker is configured by default to audit log every operation. It needs information to connect to the Audit log service.

Create Audit log service with the following command:
```
cf create-service auditlog standard nlp4sales-auditlog
```

#### Deploy the Service Broker
```
cf push
```

### Register the Service Broker
In order to consume the re-usable in a space (create a service instance), the service broker must first be registered:
```
cf target -s {space-name}
cf create-service-broker nlp4sales-{space-name} {sb-user} {sb-password} {sb-url} --space-scoped
```

### Useful commands

check available service brokers
```
cf service-brokers
```

delete a service broker
```
cf delete-service-broker {service-broker-name}
```