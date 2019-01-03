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


# Policy

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
