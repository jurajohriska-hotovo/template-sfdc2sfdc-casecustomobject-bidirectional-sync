
# Anypoint Template: Salesforce Org to Org Case/Custom Object Bi-directional Sync

+ [License Agreement](#licenseagreement)
+ [Use Case](#usecase)
+ [Considerations](#considerations)
	* [Salesforce Considerations](#salesforceconsiderations)
+ [Run it!](#runit)
	* [Running on premise](#runonopremise)
	* [Running on Studio](#runonstudio)
	* [Running on Mule ESB stand alone](#runonmuleesbstandalone)
	* [Running on CloudHub](#runoncloudhub)
	* [Deploying your Anypoint Template on CloudHub](#deployingyouranypointtemplateoncloudhub)
	* [Properties to be configured (With examples)](#propertiestobeconfigured)
+ [API Calls](#apicalls)
+ [Customize It!](#customizeit)
	* [config.xml](#configxml)
	* [businessLogic.xml](#businesslogicxml)
	* [endpoints.xml](#endpointsxml)
	* [errorHandling.xml](#errorhandlingxml)


# License Agreement <a name="licenseagreement"/>
Note that using this template is subject to the conditions of this [License Agreement](AnypointTemplateLicense.pdf).
Please review the terms of the license before downloading and using this template. In short, you are allowed to use the template for free with Mule ESB Enterprise Edition, CloudHub, or as a trial in Anypoint Studio.

# Use Case <a name="usecase"/>
As a Salesforce admin I want to have my Cases from orgA synchronized with Custom objects from Salesforce orgB.

This Template should serve as a foundation for setting an online bi-directional sync of Cases and Custom Objects between two 
Salesforce instances with ability to specify filtering criteria. 

The main behavior of this template is fetching data for changes (new or modified Cases/Custom objects) that have occurred in any of the Salesforce 
instances during a certain defined period of time. For those Cases/Custom objects, that were identified not present in the target instance, the integration triggers an upsert operation (update or create depending on the existence of the object in the target instance) taking the last modification of the object as the one that should be applied.

Additionally, there are two sub-flows which synchronize Accounts and Contacts if the Case in orgA or Case__c custom object 
in orgB in manner of one time integration.
If the Account or Contact field in Case object is specified and the Account or Contact does not exist in the other org, it will be created there for the first time.
The matching criteria for Account is field 'Account Name'. And matching criteria for Contact is field 'Email'.

Requirements have been set not only to be used as examples, but also to establish starting point to adapt the integration to any given requirements.

# Considerations <a name="considerations"/>

To make this Anypoint Template run, there are certain preconditions that must be considered. All of them deal with the preparations in both, 
that must be made in order for all to run smoothly. 
**Failling to do so could lead to unexpected behavior of the template.**



## Salesforce Considerations <a name="salesforceconsiderations"/>

There may be a few things that you need to know regarding Salesforce, in order for this template to work.

In order to have this template working as expected, you should be aware of your own Salesforce field configuration.

### FAQ

 - Where can I check that the field configuration for my Salesforce instance is the right one?

    [Salesforce: Checking Field Accessibility for a Particular Field][1]

- Can I modify the Field Access Settings? How?

    [Salesforce: Modifying Field Access Settings][2]


[1]: https://help.salesforce.com/HTViewHelpDoc?id=checking_field_accessibility_for_a_particular_field.htm&language=en_US
[2]: https://help.salesforce.com/HTViewHelpDoc?id=modifying_field_access_settings.htm&language=en_US

### As source of data

If the user configured in the template for the source system does not have at least *read only* permissions for the fields that are fetched, then a *InvalidFieldFault* API fault will show up.

```
java.lang.RuntimeException: [InvalidFieldFault [ApiQueryFault [ApiFault  exceptionCode='INVALID_FIELD'
exceptionMessage='
Account.Phone, Account.Rating, Account.RecordTypeId, Account.ShippingCity
^
ERROR at Row:1:Column:486
No such column 'RecordTypeId' on entity 'Account'. If you are attempting to use a custom field, be sure to append the '__c' after the custom field name. Please reference your WSDL or the describe call for the appropriate names.'
]
row='1'
column='486'
]
]
```

### As destination of data

There are no particular considerations for this Anypoint Template regarding Salesforce as data destination.









# Run it! <a name="runit"/>
Simple steps to get Salesforce Org to Org Case/Custom Object Bi-directional Sync running.
In order to have your application up and running you just need to complete two simple steps:

 1. [Configure the application properties](#propertiestobeconfigured)
 2. Run it! ([on premise](#runonopremise) or [in Cloudhub](#runoncloudhub))

## Running on premise <a name="runonopremise"/>
In this section we detail the way you should run your Anypoint Template on your computer.


### Where to Download Mule Studio and Mule ESB
First thing to know if you are a newcomer to Mule is where to get the tools.

+ You can download Mule Studio from this [Location](http://www.mulesoft.com/platform/mule-studio)
+ You can download Mule ESB from this [Location](http://www.mulesoft.com/platform/soa/mule-esb-open-source-esb)


### Importing an Anypoint Template into Studio
Mule Studio offers several ways to import a project into the workspace, for instance: 

+ Anypoint Studio Project from File System
+ Packaged mule application (.jar)

You can find a detailed description on how to do so in this [Documentation Page](http://www.mulesoft.org/documentation/display/current/Importing+and+Exporting+in+Studio).


### Running on Studio <a name="runonstudio"/>
Once you have imported you Anypoint Template into Anypoint Studio you need to follow these steps to run it:

+ Locate the properties file `mule.dev.properties`, in src/main/resources
+ Complete all the properties required as per the examples in the section [Properties to be configured](#propertiestobeconfigured)
+ Once that is done, right click on you Anypoint Template project folder 
+ Hover you mouse over `"Run as"`
+ Click on  `"Mule Application (configure)"`
+ Inside the dialog, select Environment and set the variable `"mule.env"` to the value `"dev"`
+ Click `"Run"`


### Running on Mule ESB stand alone <a name="runonmuleesbstandalone"/>
Fill in all properties in one of the property files, for example in [mule.dev.properties] (./src/main/resources/mule.dev.properties) and run your app 
with the corresponding environment variable to use it. To follow the example, this will be `mule.env=dev`. 


## Running on CloudHub <a name="runoncloudhub"/>
While [creating your application on CloudHub](http://www.mulesoft.org/documentation/display/current/Hello+World+on+CloudHub) (Or you can do it later as a next step), you need to go to Deployment > Advanced to set all environment variables detailed in **Properties to be configured** as well as the **mule.env**.
In order to [create your application on CloudHub](http://www.mulesoft.org/documentation/display/current/Hello+World+on+CloudHub) 
you should to go to Deployment > Advanced to set all environment variables detailed in **Properties to be configured** as well as the **mule.env**.

### Deploying your Anypoint Template on CloudHub <a name="deployingyouranypointtemplateoncloudhub"/>
Mule Studio provides you with really easy way to deploy your Template directly to CloudHub, for the specific steps to do so please check this [link](http://www.mulesoft.org/documentation/display/current/Deploying+Mule+Applications#DeployingMuleApplications-DeploytoCloudHub)


## Properties to be configured (With examples) <a name="propertiestobeconfigured"/>
In order to use this Mule Anypoint Template you need to configure properties (Credentials, configurations, etc.) either in properties file or in CloudHub as Environment Variables. Detail list with examples:
### Application configuration
+ scheduler.frequency `10000`  
This are the milliseconds that will run between two different checks for updates in either Salesforce instance

+ scheduler.startDelay `15000`

+ watermark.default.expression `2018-02-25T11:00:00.000Z`  
This property is an important one, as it configures what should be the start point of the synchronization. If the use case includes synchronization of every 
case created from the begining of the times, you should use a date previous to any case creation (perhaphs `1900-01-01T08:00:00.000Z` is a good choice). 
If you want to synchronize the contacts created from now on, then you should use a default value according to that requirement (for example, 
if today is April 21st of 2018 and it's eleven o'clock in London, then you could use the following value `2018-04-21T11:00:00.000Z`).

#### Salesforce Connector configuration for company A
+ sfdc.a.username `bob.dylan@orga`
+ sfdc.a.password `DylanPassword123`
+ sfdc.a.securityToken `avsfwCUl7apQs56Xq2AKi3X`
+ sfdc.a.integration.user.id `005d0000000yYC7AAM`

	**Note:** To find the correct *sfdc.a.integration.user.id* value, please, refer to example project **Salesforce Data Retrieval** in [Anypoint Exchange](http://www.mulesoft.org/documentation/display/current/Anypoint+Exchange).
 

#### Salesforce Connector configuration for company B
+ sfdc.b.username `joan.baez@orgb`
+ sfdc.b.password `JoanBaez456`
+ sfdc.b.securityToken `ces56arl7apQs56XTddf34X`
+ sfdc.b.integration.user.id `005i0000002qZ75AAE`

	**Note:** To find out the correct *sfdc.b.integration.user.id* value, please, refer to example project **Salesforce Data Retrieval** in [Anypoint Exchange](http://www.mulesoft.org/documentation/display/current/Anypoint+Exchange).


### Some points to consider about configuration properties

Scheduler Frequency is expressed in milliseconds (different time units can be used) and the Watermark Default Expression defines the date to be used to query the first time the integration runs.

The date format accepted in SFDC Query Language is either YYYY-MM-DDThh:mm:ss+hh:mm or you can use Constants (Like YESTERDAY in the example). [More information about Dates in Salesforce.](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql_select_dateformats.htm)

The query fields list must include both 'Email' and 'LastModifiedDate' fields, as those fields are embedded in the integration business logic

#### Structure of the Case object

To be able to match pair Case object in orgA with Case custom object in orgB we needed to extend Case object in orgA with the following field. 

+ ExtId__c `Text(50) External ID` external id which is referencing ID of Case\_\_c in orgB

Feel free to customize the name of this field, but keep in mind that you will need to update all occurrences in flows for this particular field reference.


#### Structure of the Case Custom object

In this particular template we named the Custom object `Case`, that is the API name of the Custom object in orgB is `Case__c`, so all osql queries, DataWeave conversions are issued to this particular object. 
Case\_\_c custom object inherits all required fields from Case in orgA together with Description\_\_c, Subject\_\_c text fields and
Contact\_\_c and Account\_\_c lookup fields.

+ Account__c `Lookup(Account)`
+ CaseId__c `Text(50) External ID` external id which is referencing ID of Case in orgA
+ Contact__c `Lookup(Contact)`
+ Description__c `Text(50)`
+ Origin__c `Text(50) Required`
+ Priority__c `Text(50) Required`
+ Status__c `Text(50) Required`
+ Subject__c `Text(50)`

This structure may be customized but keep in mind that you will then need to 

+ update query `fetch case__c objects from B` in `endpoints.xml` to fetch all extra fields
+ update DataWeave transformer `transform Case to Case__c`
+ update DataWeave transformer `transform Case__c to Case`

# API Calls <a name="apicalls"/>
Salesforce imposes limits on the number of API Calls that can be made. Therefore calculating this amount may be an important factor to consider. 
Case/Case custom object Anypoint Template calls to the API can be calculated using the formula:

***1 + CasesToSync + 2  / CommitSize***

Where ***CasesToSync*** is the number of Cases to be synchronized on each run. 

The division by ***CommitSize*** is because by default, for each Upsert API Call, Cases are gathered in groups of a number defined by the Commit Size property. 
Also consider that these calls are executed repeatedly every polling cycle.	

For instance if 10 records are fetched from origin instance, then at least 13 api calls will be made (1 + 10 + 2).


# Customize It!<a name="customizeit"/>
This brief guide intends to give a high level idea of how this Anypoint Template is built and how you can change it according to your needs.
As mule applications are based on XML files, this page will be organized by describing all the XML that conform the Anypoint Template.
Of course more files will be found such as Test Classes and [Mule Application Files](http://www.mulesoft.org/documentation/display/current/Application+Format), but to keep it simple we will focus on the XMLs.

Here is a list of the main XML files you'll find in this application:

* [config.xml](#configxml)
* [endpoints.xml](#endpointsxml)
* [businessLogic.xml](#businesslogicxml)
* [errorHandling.xml](#errorhandlingxml)


## config.xml<a name="configxml"/>
Configuration for Connectors and [Configuration Properties](http://www.mulesoft.org/documentation/display/current/Configuring+Properties) are set in this file. **Even you can change the configuration here, all parameters that can be modified here are in properties file, and this is the recommended place to do it so.** Of course if you want to do core changes to the logic you will probably need to modify this file.

In the visual editor they can be found on the *Global Element* tab.


## businessLogic.xml<a name="businesslogicxml"/>
This file holds the functional aspect of the template, directed by one flow responsible of conducting the business logic.



## endpoints.xml<a name="endpointsxml"/>
This is the file where you will find the inbound and outbound sides of your integration app. It is intented to define the application API.



## errorHandling.xml<a name="errorhandlingxml"/>
This is the right place to handle how your integration will react depending on the different exceptions. 
This file holds a [Error Handling](http://www.mulesoft.org/documentation/display/current/Error+Handling) that is referenced by the main flow in the business logic.



