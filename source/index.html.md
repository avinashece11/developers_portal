---
title: Pasarpolis Developer Guide

language_tabs:
  - ruby
  - python
  - java
  - go
  - javascript

includes:
  - errors

search: true
---

# Introduction

Welcome to Pasarpolis developer guide. You can use our API to access API endpoints for policy.

We have language bindings in Shell, Ruby, and Python! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# Authentication

We use HMAC based authetication behind the scenes.

You just need PartnerCode and PartnerSecret to get started. (details given in the Initialization section)

To know more about HMAC authentication click [here](https://en.wikipedia.org/wiki/HMAC)

# Initialization

> To initialize, use this code:

```ruby
require 'pasarpolis_sdk'

PasarpolisSdk.init(<PartnerCode>, <PartnerSecret>, <Host>)
```

```python
from pasapolis import Client

Client.init(<PartnerCode>, <PartnerSecret>, <Host>)
```

```java
import com.pasarpolis.sdk.PasarpolisClient;

PasarpolisClient.init(<PartnerCode>, <PartnerSecret>, <Host>);

```

```go
import(
  "gitlab.com/pasarpolis/go-sdk"
)

pasarpolis.InitClient(<PartnerCode>, <PartnerSecret>, <Host>);

```


```javascript

const sdk = require('pasarpolis-sdk-nodejs')
const client = sdk.init(<PartnerCode>, <PartnerSecret>, <Host>)

```



For Initilization you need PartnerCode, PartnerSecret and Host.

  **PartnerCode** (string)    : Provided by Pasarpolis for Authentication.
  
  **PartnerSecret** (string)  : Secret Key provided by Pasarpolis.
  
  **Host** (string)           : Fully qualified domain name as specified below.

  &nbsp;&nbsp;&nbsp;&nbsp;Testing:    https://integrations-testing.pasarpolis.com

  &nbsp;&nbsp;&nbsp;&nbsp;Sandbox:    https://integrations-sandbox.pasarpolis.com
    
  &nbsp;&nbsp;&nbsp;&nbsp;Production: https://integrations.pasarpolis.com

<aside class="notice">
Initialize the client before calling any functionality of SDK else it will raise error.<br/>
Please make sure you initialize client only once i.e. at start of your app else it will raise error.
</aside>

# Guide

This guide will help you use our SDK for Travel Protection.

## Create Policy

> Example code:

```ruby
require "pasarpolis_sdk"

params = {}
params["name"]="Jacob"
params["last_property"]=true
policy = Pasarpolis::Policy.create_policy("travel-protection", params);
puts policy.reference_number
puts policy.application_number

```

```python

from pasarpolis import Policy

params = {}
params["name"]="Jacob"
params["last_property"]=true
policy = Policy.create_policy("travel-protection", params);
print(policy.reference_number,"\n")
print(policy.application_number)


```

```java
import java.util.HashMap;
import com.pasarpolis.sdk.models.Policy;

try {
  HashMap<String, Object> params = new HashMap<String, Object>();
  params.put("name","Jacob");
  params.put("last_property",true);
  Policy policy = Policy.createPolicy("travel-protection", params);  
  System.out.println(policy.referenceNumber);
  System.out.println(policy.applicationNumber);
}catch(Exception  e) {
  // Error Handling part
}

```

```go
import(
  pasarpolis_models "gitlab.com/pasarpolis/go-sdk/models"
  "fmt"
)


params := map[string]interface{}{};
params["name"]="Jacob"
params["last_property"]=true
policy := pasarpolis_models.CreatePolicy("travel-protection", params);
fmt.Println(policy.referenceNumber);
fmt.Println(policy.applicationNumber);

```

```javascript
const sdk = require('pasarpolis-sdk-nodejs')
const client = sdk.init(<PartnerCode>, <PartnerSecret>, <Host>)

const packageName = 'travel-protection'
const params = {
    "note": "",
    "travel_destination": 1
}
const createPolicy = client.createPolicy("travel-protection", params)

/** It will give response as promise */ 
createPolicy
    .then(response => console.log(response))
    .catch(err => console.log(err))

```

> The above code snippet returns would return:

```
d77d436ad9485b86b8a0c18c3d2f70f77a10c43d
APP-000000224

```

The above action will create a new policy

For policy creation you will need Product and Params as parameters

where:

  &nbsp;&nbsp;&nbsp;&nbsp;**Product** : Type of product for which policy is being created.

  &nbsp;&nbsp;&nbsp;&nbsp;**Params** : Product specific data to be sent.

<aside class="notice">
Signature of every product specific data is provided <a href="https://gitlab.com/pasarpolis/pasarpolis-sdk-java/tree/master/product_type_doc"> here </a>
</aside>

It will return a Policy object which contains:

  &nbsp;&nbsp;&nbsp;&nbsp;**reference_number** (string): Hexadecimal number provided by Pasarpolis for future reference.

  &nbsp;&nbsp;&nbsp;&nbsp;**application_number** (string): Number provided for future reference.




## Get Policy Status

> Example code

```ruby
require 'pasarpolis_sdk'


refs = ["d77d436ad9485b86b8a0c18c3d2f70f77a10c43d"]
policies = Pasarpolis::Policy.get_policy_status(refs)
puts policies

```

```python
from pasarpolis import Policy


refs = ["d77d436ad9485b86b8a0c18c3d2f70f77a10c43d"]
policies = Policy.get_policy_status(refs)
print(policies)

```

```java
import com.pasarpolis.sdk.models.Policy;
try {  
  String[] refs = {"d77d436ad9485b86b8a0c18c3d2f70f77a10c43d"};
  Policy[] policies = Policy.getPolicyStatus(refs);
  System.out.println(policies);
}catch(Exception  e) {
  // Error Handling part
}

```

```go
import(
  pasarpolis_models "gitlab.com/pasarpolis/go-sdk/models"
  "fmt"
)

String[] refs = {"d77d436ad9485b86b8a0c18c3d2f70f77a10c43d"};
policies = pasarpolis_models.GetPolicyStatus(refs);
fmt.Println(policies);

```

```javascript
const sdk = require('pasarpolis-sdk-nodejs')
const client = sdk.init("DummyPartnerCode", "DummyPartnerSecret", "https://integrations.pasarpolis.com")

const getPolicyStatus = client.getPolicyStatus("48fc2801e487af79ef311621f63e4133c")

/** It will give response as promise */ 
getPolicyStatus
    .then(response => console.log(response))
    .catch(err => console.log(err))


```

> The above will return:


```
[ 
    { 
        ref: '48fc2801e487af79ef311621f63e4133c',
        application_no: 'APP-000000XXX',
        document_url: 'url',
        policy_no: 'XXX',
        status_code: 2,
        status: 'COMPLETED',
        issue_date: '2018-12-10 07:34:12' 
    } 
]
```

Get policy status will give status of the policy created by providing policy reference number. 

It will return array of policies object being queried for.

where:

  &nbsp;&nbsp;&nbsp;&nbsp;**ReferenceNumbers** : An array of string containing reference numbers.

It will return an array of Policy objects. Each Policy object contains:

  &nbsp;&nbsp;&nbsp;&nbsp;**reference_number** (string): Hexadecimal number provided by Pasarpolis for future reference.

  &nbsp;&nbsp;&nbsp;&nbsp;**application_number** (string): Number provided for future reference.

  &nbsp;&nbsp;&nbsp;&nbsp;**document_url** (string): Url for policy document (if any).

  &nbsp;&nbsp;&nbsp;&nbsp;**policy_number** (string): Number for policy.

  &nbsp;&nbsp;&nbsp;&nbsp;**status** (string): Status of a given policy.

  &nbsp;&nbsp;&nbsp;&nbsp;**status_code** (integer): Status code of a given policy.

# API References

### URLs

You can access our API using these URLS :

Environment | URL
--------- | -------
Testing | https://integrations-testing.pasarpolis.com
Sandbox | https://integrations-sandbox.pasarpolis.com
Production | https://integrations.pasarpolis.com

### API Routes

Endpoint | HTTP Method |  | Link
--------- | ------- | ------- | -------
/api/v2/product/travel-protection/packages/  | GET | Get available travel protection packages | click [here](#travel-protection-packages-api)
/api/v2/createpolicy/ | POST | Send travel protection application data to generate policy | click [here](#travel-protection-registration-api)
/api/v2/policystatus/ | POST | Check Statuses of given policies | click [here](#travel-protection-status-api)


## Travel Protection Packages API

> The above code snippet returns would return:

```json
[  
   {  
      "package_name":"PLAN B DOMESTIK ROUNDTRIP",
      "package_no":4,
      "premium":45000,
      "accidental_death_and_disablement":300000000,
      "loss_or_damage_baggage_and_personal_effect":7500000,
      "baggage_delay":1500000,
      "travel_delay":1500000,
      "loss_of_travel_document":0,
      "flight_misconnection":0,
      "medical_expenses_accident_sickness":0,
      "travel_destination":1,
      "benefits":[  
         {  
            "TSI":"300,000,000",
            "Description":"Meninggal Dunia &amp; Cacat Tetap akibat kecelakaan"
         },
         {  
            "TSI":"7,500,000",
            "Description":"Kehilangan atau kerusakan Bagasi &amp; harta benda Pribadi"
         },
         {  
            "TSI":"1,500,000",
            "Description":"Keterlambatan Bagasi, min 4 jam"
         },
         {  
            "TSI":"1,500,000",
            "Description":"Keterlambatan Perjalanan, min 4 jam"
         }
      ]
   },
   {  
      "package_name":"PLAN A OVERSEAS ONE WAY",
      "package_no":5,
      "premium":90000,
      "accidental_death_and_disablement":500000000,
      "loss_or_damage_baggage_and_personal_effect":12500000,
      "baggage_delay":2500000,
      "travel_delay":2500000,
      "loss_of_travel_document":3500000,
      "flight_misconnection":1500000,
      "medical_expenses_accident_sickness":500000000,
      "travel_destination":2,
      "benefits":[  
         {  
            "TSI":"500,000,000",
            "Description":"Meninggal Dunia &amp; Cacat Tetap akibat kecelakaan"
         },
         {  
            "TSI":"500,000,000",
            "Description":"Medical Expenses due to accident and sickness"
         },
         {  
            "TSI":"12,500,000",
            "Description":"Kehilangan atau kerusakan Bagasi &amp; harta benda Pribadi"
         },
         {  
            "TSI":"2,500,000",
            "Description":"Keterlambatan Bagasi, min 4 jam"
         },
         {  
            "TSI":"2,500,000",
            "Description":"Keterlambatan Perjalanan, min 4 jam"
         },
         {  
            "TSI":"3,500,000",
            "Description":"Kehilangan Dokumen Perjalanan"
         },
         {  
            "TSI":"1,500,000",
            "Description":"Ketidaksesuaian Penerbangan Lanjutan"
         }
      ]
   }
]

```

Get all travel protection packages from insurance company

Endpoint | HTTP Method | Definition
--------- | ------- | -------
/api/v2/product/travel-protection/packages/  | GET | Get available travel protection packages

## Travel Protection Registration API

> Sample JSON Request Body

```json
{
  "product":"travel-protection",
  "reference_id": "A259",
  "policy_insured": [
    {
      "name": "John Doe",
      "gender": "Male",
      "address": "Jl Soekarno Hatta",
      "date_of_birth": "1987-09-09",
      "city": "Jakarta",
      "zip_code": "123456",
      "phone_no": "08231986121",
      "handphone_no": "081238921321",
      "email": "johndoe@mail.com",
      "policy_holder_status": 1,
      "insured_status": 1
} ],
  "policy_beneficiary": [
    {
      "name": "Bob",
      "relationship": 1
    }
  ],
  "flight_information": [{
      "type": 0,
      "airport_code": "CGK",
      "flight_code": "JT2014",
      "city": "JAKARTA",
"departure_date": "2017-12-08 11:00:00"
  }, {
      "type": 2,
      "airport_code": "SOC",
      "flight_code": "JT213",
      "city": "SOLO",
      "departure_date": "2017-12-10 09:00:00"
  }],
  "booking_code": "123456789",
  "inception_date": "2017-12-07",
  "package_id": 1,
  "travel_type": 2,
  "transit_type": 0,
  "travel_destination": 1,
  "trip_reason": 1,
  "note": ""
}
```


> The above code snippet returns would return:

```json
{
    "application_no":"APP-000008152",
    "ref":"e30ef12cafa6007868ad67fe3b6ee82095f8174e"
}
```

Travel company or provider can register their passenger for travel insurance.

### Definition

Endpoint | HTTP Method | Definition
--------- | ------- | -------
/api/v2/createpolicy/ | POST | Send travel protection application data to generate policy

### JSON Request Body Data:

**Main attribute**

JSON Attribute | Definition | Required | Validation
--------- | ------- | ------- | ------- | -------
product | "travel-protection" | Yes | |
reference_id | reference id from to uniquely identify a request, must unique for every travel protection registration | Yes | |
policy_insured | Person who will be insured by insurance | Yes | |
policy_beneficiary | Person who will receive the death benefit in the event of the insured death | Yes | |
flight_information | Flight Information. **Possible Values**: (1,2,3) 0 => (Departure) 1 => (Departure Transit) 2 => (Return) 3 => (Return Transit) | Yes | |
booking_code | Passenger flight booking code | Yes | |
inception_date | Initial period of insurance coverage (YYYY-MM-DD) | Yes | |
package_id | travel protection package id. **Possible Values**: (1,2,3,4,5,6,7,8,9,10) 1 => 1 => PLAN A DOMESTIK ONEWAY , 2 => PLAN B DOMESTIK ONEWAY , 3 => PLAN A DOMESTIK ROUNDTRIP , 4 => PLAN B DOMESTIK ROUNDTRIP , 5 => PLAN A OVERSEAS ONE WAY , 6 => PLAN A OVERSEAS ROUNDTRIP , 7 => PLAN C OVERSEAS ONE WAY , 8 => PLAN C OVERSEAS ROUNDTRIP , 9 => PLAN B OVERSEAS ONE WAY , 10 => PLAN B OVERSEAS ROUNDTRIP | Yes | |
travel_type | Travel type to know wether the passenger have one way trip or round trip. **Possible Values**: (1, 2)    1 => (One way trip), 2 =>(Round trip) | Yes | |
transit_type | Transit type to know what transits the passenger's flight has. **Possible Values**: (0,1,2,3)  0 => (No transits), 1 => (Transit on departure), 2 => (Transit on return), 3 => (Transit on both departure and return) | Yes | |
travel_destination | passenger travel destination **Possible Values**: (1,2) 1 => (Domestic flight in Indonesia), 2 => (International flight) | Yes | |
trip_reason | passenger trip reason. **Possible Values** (1,2,3,4,5,6)  1 => (Wisata), 2 => (Bisnis), 3 => (Kunjungan Keluarga), 4 => (Seminar), 5 => (Training), 6 => (Tugas Kantor) | Yes | |
note | Note for description | No | |

<aside class="notice">If package is for domestic flight, then travel_destination value must be 1 (Domestic flight in Indonesia) - If package is for international flight, then travel_destination value must be 2 (International flight)
</aside>

**policy_insured attribute**

JSON Attribute | Definition | Required | Validation 
--------- | ------- | ------- | ------- | -------
name | insured name | Yes | |
gender | insured | No | |
date_of_birth | insured date of birth (YYYY-MM-DD) | No | |
identity_type | the identity used in travel protection registration **Possible Values:** 1 => (KTP), 2 => (SIM), 3 => (PASSPORT) | No | Max length 100 |
address | insured address | Yes | Max length 500 |
city | city where insured live | Yes | Max length 200 |
zip_code | insured postal code | Yes | Max length 100 |
phone_no | insured phone number | Yes | Max length 100 |
handphone_no | insured handphone number | No | Max length 100 |
email | insured email address | Yes | Max length 200 |
policy_holder_status | status to verify insured is a policy holder **Possible Values:** 1 => (Policy Holder), 2 => (Participant)| Yes | |
insured_status | insured family status **Possible Values:** 1 => (Insured), 2 => (Spouse), 3 => (Child) **NOTE:** If policy_holder_status is 1, then insured_status must be 1 (Insured) | Yes  | |



**policy_beneficiary attribute**

JSON Attribute | Definition | Required | Validation 
--------- | ------- | ------- | ------- | -------
name | benficary insurance name | Yes | |
relationship | beneficiary relationship with policy holder **Possible Values:** (1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21) 1 => (Adik), 2 => (Anak), 3 => (Ayah), 4 => (Cucu), 5 => (Dealer), 6 => (Ibu), 7 => (Istri), 8 => (Kakak), 9 => (Kakek), 10 => (Keponakan), 11 => (Kerabat), 12 => (Kreditur), 13 => (Lembaga Pendidikan), 14(Majikan =>), 15 => (Nenek), 16 => (Orang Tua), 17 => (Perusahaan =>), 18 => (Pimpinan), 19 => (Saudara), 20 => (Sepupu), 21 => (Suami) | Yes | |

## Travel Protection Status API

> Sample URL Request body

```json
#Body accepts an array of references returned from the application post

{
    "ids": ["ref1"]
}
```

> The above code snippet returns would return:

```json
[
  {
    "ref": "ref1",
    "application_no": "APP-000000XXX",
    "document_url": "http://sampledomain.com/pid.html",
    "policy_no": "0000XXXX",
    "status_code": 1,
    "issue_date": "2018-07-16 07:37:38"
} ]

```

Check the statuses of given policies

### Definition

Endpoint | HTTP Method | Definition
--------- | ------- | -------
/api/v2/policystatus/  | POST | Check statuses of given policies  
