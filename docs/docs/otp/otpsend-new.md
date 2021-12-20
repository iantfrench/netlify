---
sidebar_position: 2
slug: /otp/send
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Sending One-Time Passcodes

Using the BlockID Developer SDK, you can test OTP functionality by sending passcodes to a phone number or email of your choosing. The SDK is currently available in Node.js, with support for more languages coming soon.   

## Getting Started


### Step 1: Install the BlockID SDK for Node.js

To get started, first install the `blockid-nodejs-helpers` npm package, which contains the Node.js BlockID SDK, and all the necessary libraries and functions needed for secure communication with our API and servers.    

Open a terminal and execute the following command to install the package:  
 

```bash
npm install blockid-nodejs-helpers
```
After installation, the BlockID SDK can be found in the `<project root>/node_modules/blockid-nodejs-helpers` folder.  

### Step 2: Configure the SDK with your Tenant and License Key

Next, import the SDK by defining it to a variable. You can then configure the SDK with your [tenant domain and license key](/docs/otp#required-information).  



```jsx 
const BIDSDK = require('blockid-nodejs-helpers/BIDSDK.js');
const loaded = await BIDSDK.setupTenant({dns: "1k-dev.1kosmos.net", communityName: "default"}, "<your license key>");
```




## Sending a One-Time Passcode via SMS

Developers can use the BlockID Node.js SDK to test OTP functionality. 

To send a one-time passcode via sms, create a new file, or open a Node.js console, and copy the following lines, replacing the number below with your own:


```jsx title=REQUEST
var SDK = require('nodejs-helper-files/OTPProvider.js')
SDK.GenerateOTP('john.smith', { smsTo: "1234567890", smsISDCode: "1" });
```

The response should look similar to the one below. If it doesn't, check to make sure the number you entered is from a supported country.    

```bash title=RESPONSE
node myfile.js

{"messageId":"15f9dd86-9a8a-4c66-900a-f711f5acceb1","info":"OTP request accepted"}
```

Checking your phone, you should have received an SMS containing the one-time passcode: 

<img src='../../../img/otpsms.png' width='250' />

	



## Sending a One-Time Passcode via Email

To send a one-time passcode via email, create a new file, or open a Node.js console, and copy the following lines, replacing the email below with your own:


```jsx title=REQUEST
var SDK = require('nodejs-helper-files/OTPProvider.js')
SDK.GenerateOTP('john.smith', { emailTo: "john.smith@mysite.com" });
```

The response should look similar to the one below.   

```bash title=RESPONSE
node myfile.js

{"messageId":"d5410b22-93fb-4260-b3fd-09e435177377","info":"OTP request accepted"}
```

Checking you email inbox, you should have received a message containing the one-time passcode:

<img src='../../../img/otpemail.png' width='250' />


## Sending a One-Time Passcode via SMS and Email

If desired, we can send the one-time passcode to a user's email and phone at the same time. 



```jsx title=REQUEST
var SDK = require('nodejs-helper-files/OTPProvider.js')
SDK.GenerateOTP('john.smith', { emailTo: "john.smith@mysite.com", smsTo: "1234567890", smsISDCode: "1" });
```

## Troubleshooting

Sometimes the SDK doesn't function as expected and returns an error message.  Common error messages users might see include: 

```jsx title=RESPONSE
{"message":"Unable to load tenant/community"}
```

If the SDK is not first initialized with a tenant or community, users will receive this error message. See [Initializing the SDK](/docs/uwl2) for information on how to initialize the SDK for your tenant or community.

```jsx title=RESPONSE
{
    "error_code": 404,
    "message": "Invalid OTP.",
    "status": false
}
```

Users entering the incorrect one-time passcode will receive this error. Ensure that you are entering the one-time passcode from the most recent sms or email.  

```jsx title=RESPONSE
{
    "error_code": 403,
    "message": "OTP request locked for the user.",
    "status": false
}
```
If an incorrect one-time passcode is entered more than five times in a thirty (30) minute window, users will be temporarily locked-out and unable to authenticate and complete their sign-in. 

Accounts will automatically unlock after fifteen (15) minutes. 

Tenant and community administrators can unlock a user account at any time. See [Unlocking a Locked Account](/docs/otp/verify#Unlock) for more information.   

