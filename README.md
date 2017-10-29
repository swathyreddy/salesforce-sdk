# IBM Watson Salesforce SDK

[![Build Status](https://travis-ci.com/germanattanasio/salesforce-sdk.svg?token=KTpTGKpqxmhtwWo4UVVC&branch=master)](https://travis-ci.com/germanattanasio/salesforce-sdk)

The IBM Watson Salesforce SDK uses the [Watson Developer Cloud](http://www.ibm.com/watson/developercloud/) services to help you solve complex problems using Apex in your Salesforce environment. Currently, this SDK supports two Watson services: Conversation and Discovery. More are planned to be added in the future.

## Installation using Salesforce DX

### Automatic deployment

You can automatically deploy the SDK to a new scratch environment using the _Deploy to SFDX_ button.

[![Deploy](https://deploy-to-sfdx.com/dist/assets/images/DeployToSFDX.svg)](https://deploy-to-sfdx.com/)


### Manual deployment


1. Clone this repository from GitHub using the following command:
    ```bash
    git clone git@github.com:germanattanasio/salesforce-sdk.git
    ```

2. Create a new scratch environment (optional if you don't want to re-use an existing one):

    ```bash
    sfdx force:org:create -a watsonSdk -s -f config/project-scratch-def.json
    ```

3. Push the source to the scratch environment:

    ```bash
    sfdx force:source:push
    ```

If you want to use the Watson SDK within a non-scratch environment you can deploy it using the Salesforce DX CLI.


1. Authenticate the Salesforce DX CLI to the target environment:

    ```bash
    sfdx force:auth:web:login -a TargetOrg
    ```

2. Create an output directory:

    ```bash
    mkdir mdapioutput
    ```

3. Convert the source code:

    ```bash
    sfdx force:source:convert -d mdapioutput/
    ```

4. Deploy the source code:

    ```bash
    sfdx force:mdapi:deploy -d mdapipackage/ -u TargetOrg -w 100
    ```

## Installation using the Ant Build Tool

You can install or update the SDK using the Ant Build Tool by following these steps:

1. Clone this repository from GitHub using the following command:
    ```bash
    git clone git@github.com:germanattanasio/salesforce-sdk.git
    ```

2. Edit `install/build.properties` to insert your Salesforce username and password.  Since you will be using the API to access Salesforce, remember to [append your Security Token](http://www.salesforce.com/us/developer/docs/api/Content/sforce_api_concepts_security.htm#topic-title_login_token) to your password.

3. Open your command line to the `install` folder, then deploy using Ant:

    ```bash
    $ ant deployWatson
    ```

## Getting Started

### Getting Service Credentials

Using the Watson services require service credentials in [Bluemix](https://console.bluemix.net), meaning you will need to create a Bluemix account if you do not have one already.

To get your service-specific credentials, follow these steps:

1. Log in to [Bluemix](https://console.bluemix.net)

2. Create an instance of the desired service:
    1. In the Bluemix **Catalog**, select the service you want to use.
    2. Click **Create**.

3. Copy your credentials:
    1. On the left side of the page, click **Service Credentials**, and then **View credentials** to view your service credentials.
    2. Copy `url`, `username` and `password`.

### Using the Services

Getting started using a service is very simple! All services follow the same pattern of service instantiation, option building, and requesting. To get an idea, below is an example of using the Discovery service to get a list of your current environments:

```java
// instantiating service with Bluemix credentials
DiscoveryV1 discovery = new DiscoveryV1(DiscoveryV1.VERSION_DATE_2017_09_01);
discovery.setEndPoint(<url>);
discovery.setUsernameAndPassword(<username>, <password>);

// configuring options for listing environments
DiscoveryV1Models.ListEnvironmentsOptions options = new DiscoveryV1Models
  .ListEnvironmentsOptionsBuilder()
  .build();

// making request
DiscoveryV1Models.ListEnvironmentsResponse environmentList 
  = service.listEnvironments(options);
```

Similarly, here is an example of creating an intent in the Conversation service:

```java
// instantiating service with Bluemix credentials
ConversationV1 conversation = new ConversationV1(ConversationV1.VERSION_DATE_2017_05_26);
conversation.setEndPoint(<url>);
conversation.setUsernameAndPassword(<username>, <password>);

// configuring options for creating intent
ConversationV1Models.CreateIntentOptions options = new ConversationV1Models.CreateIntentOptionsBuilder()
    .workspaceId(<workspace_id>)
    .intent('MyIntent')
    .description('This is an example of creating an intent!')
    .build();

// making request
ConversationV1Models.Intent intent = conversation.createIntent(options);
```

The manner of instantiating and using services should be consistent no matter which you decide to use, which should make it easy to explore the many capabilities Watson services have to offer.

## Contributing

If you're interested in helping to make this project better, see [Contributing.md](.github/Contributing.md).

## License

This library is licensed under the MIT license. Full license text is
available in [LICENSE](LICENSE).
