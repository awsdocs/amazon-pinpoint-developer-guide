# Validating Phone Numbers in Amazon Pinpoint<a name="validate-phone-numbers"></a>

Amazon Pinpoint includes a phone number validation service that you can use to determine if a phone number is valid, and to obtain additional information about the phone number itself\. For example, when you use the phone number validation service, it returns the following information:
+ The phone number in E\.164 format\.
+ The phone number type \(such as mobile, landline, or VoIP\)\.
+ The city and country where the phone number is based\.
+ The service provider that's associated with the phone number\.

There is an additional charge for using the phone number validation service\. For more information, see [Amazon Pinpoint Pricing](https://aws.amazon.com/pinpoint/pricing/#Phone_Number_Validate)\.

## Phone Number Validation Use Cases<a name="validate-phone-numbers-use-cases"></a>

You can use the phone number validation service to enable several use cases, including the following:
+ **Verifying phone numbers provided on a web form** – If you use web\-based forms to collect contact information for your customers, you validate the phone numbers that customers provide before submitting the form\. Use your website's backend to validate the number by using the Amazon Pinpoint API\. The API response states whether the number is invalid—for example, if the phone number is formatted incorrectly\. If you determine that the phone number that the customer provided is invalid, your web form can prompt the customer to provide a different number\.
+ **Cleansing your existing contact database** – If you have a database of customer phone numbers, you can validate each phone number, and then update your database based on your findings\. For example, if you find endpoints with phone numbers that aren't capable of receiving SMS messages, you can change the `ChannelType` property for the endpoint from `SMS` to `VOICE`\.
+ **Choosing the right channel before you send a message** – If you intend to send an SMS message but you determine that the destination number is invalid, you can send a message to the recipient through a different channel\. For example, if the endpoint isn't able to receive SMS messages, you can send a voice message instead\.

## Using the Phone Number Validation Service<a name="validate-phone-numbers-request"></a>

To validate a number, send an HTTP POST request to the `/v1/phone/number/validate/` URI in the Amazon Pinpoint API\. The request in the following example includes the required HTTP headers and a simple JSON body\. The body specifies the number to validate with the `PhoneNumber` parameter\.

```
POST /v1/phone/number/validate/ HTTP/1.1
Host: pinpoint.us-east-1.amazonaws.com
Content-Type: application/json
X-Amz-Date: 20190805T031042Z
Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20190805/us-east-1/mobiletargeting/aws4_request, SignedHeaders=content-length;content-type;host;x-amz-date, Signature=39df573629ddb283aea1fa2f7eef4106c0fb4826edf72e9934f03cf77example
Cache-Control: no-cache

{
	"PhoneNumber": "+12065550100"
}
```

For information about supported methods, parameters, and schemas, see [Phone Number Validate](https://docs.aws.amazon.com/pinpoint/latest/apireference/phone-number-validate.html) in the *Amazon Pinpoint API Reference*\.

You can also use the AWS CLI to quickly validate individual phone numbers\.

**To use the phone number validation service by using the AWS CLI**
+ At the command line, enter the following command:

  ```
  aws pinpoint phone-number-validate --number-validate-request PhoneNumber=+442079460881
  ```

  In the preceding command, replace *\+442079460881* with the phone number that you want to validate\.
**Note**  
When you provide a phone number to the phone number validation service, you should always include the country code\. If you don't include the country code, the service might return information for a phone number in a different country\.

## Phone Number Validation Responses<a name="validate-phone-numbers-example-responses"></a>

The information that the phone number validation service provides varies slightly based on the data that's available for the phone number that you provide\. This section contains examples of the responses that the phone number validation service returns\.

**Note**  
The data that's provided by the phone number validation service is based on information provided by telecommunication providers and other entities around the world\. Providers in some countries might update this information less frequently than providers in other countries do\. For example, if you issue a request to validate a mobile phone number, and the number that you provided was ported from one mobile carrier to another, the response from the phone number validation service might include the name of the original carrier, as opposed to the current one\.

**Valid mobile phone numbers**  
When you send a request to the phone number validation service, and the phone number is a valid mobile phone number, it returns information that resembles the following example:

```
{
    "NumberValidateResponse": {
        "Carrier": "ExampleCorp Mobile",
        "City": "Seattle",
        "CleansedPhoneNumberE164": "+12065550142",
        "CleansedPhoneNumberNational": "2065550142",
        "Country": "United States",
        "CountryCodeIso2": "US",
        "CountryCodeNumeric": "1",
        "OriginalPhoneNumber": "+12065550142",
        "PhoneType": "MOBILE",
        "PhoneTypeCode": 0,
        "Timezone": "America/Los_Angeles",
        "ZipCode": "98101"
    }
}
```

**Valid landline phone numbers**  
If your request contains a valid landline phone number, the phone number validation service returns information that resembles the following example:

```
{
    "CountryCodeIso2": "US",
    "CountryCodeNumeric": "1",
    "Country": "United States",
    "City": "Santa Clara",
    "ZipCode": "95037",
    "Timezone": "America/Los_Angeles",
    "CleansedPhoneNumberNational": "4085550101",
    "CleansedPhoneNumberE164": "14085550101",
    "Carrier": "AnyCompany",
    "PhoneTypeCode": 1,
    "PhoneType": "LANDLINE",
    "OriginalPhoneNumber": "+14085550101"
}
```

**Valid VoIP phone numbers**  
If your request contains a valid Voice over Internet Protocol \(VoIP\) phone number, the phone number validation service returns information that resembles the following example:

```
{
    "NumberValidateResponse": {
        "Carrier": "ExampleCorp",
        "City": "Countrywide",
        "CleansedPhoneNumberE164": "+441514960001",
        "CleansedPhoneNumberNational": "1514960001",
        "Country": "United Kingdom",
        "CountryCodeIso2": "GB",
        "CountryCodeNumeric": "44",
        "OriginalPhoneNumber": "+441514960001",
        "PhoneType": "VOIP",
        "PhoneTypeCode": 2
    }
}
```

**Invalid phone numbers**  
If your request contains an invalid phone number, the phone number validation service returns information that resembles the following example:

```
{
    "NumberValidateResponse": {
        "CleansedPhoneNumberE164": "+44163296076",
        "CleansedPhoneNumberNational": "163296076",
        "Country": "United Kingdom",
        "CountryCodeIso2": "GB",
        "CountryCodeNumeric": "44",
        "OriginalPhoneNumber": "+440163296076",
        "PhoneType": "INVALID",
        "PhoneTypeCode": 3
    }
}
```

Note that the `PhoneType` property in this response indicates that this phone number is `INVALID`, and that it doesn't include information about the carrier or location associated with the phone number\. You should avoid sending SMS or voice messages to phone numbers where the `PhoneType` is `INVALID`, because these numbers are unlikely to belong to actual recipients\.

**Other phone numbers**  
Occasionally, the response from the phone number validation service includes a `PhoneType` value of `OTHER`\. The service might return this kind of response in the following situations:
+ The phone number is a toll\-free \(freephone\) number\.
+ The phone number is reserved for use in TV shows and movies, such as North American phone numbers that begin with *555*\.
+ The phone number includes an area code that is not currently in use, such as the *999* area code in North America\.
+ The phone number is reserved for some other purpose\.

The following example shows the response that the phone number validation services provides when your request includes a fictitious North American phone number:

```
{
    "NumberValidateResponse": {
        "Carrier": "Multiple OCN Listing",
        "CleansedPhoneNumberE164": "+14255550199",
        "CleansedPhoneNumberNational": "4255550199",
        "Country": "United States",
        "CountryCodeIso2": "US",
        "CountryCodeNumeric": "1",
        "OriginalPhoneNumber": "+14255550199",
        "PhoneType": "OTHER",
        "PhoneTypeCode": 4,
        "Timezone": "America/Los_Angeles"
    }
}
```

For more information about the information that's contained in these responses, see [Phone Number Validate](https://docs.aws.amazon.com/pinpoint/latest/apireference/phone-number-validate.html) in the *Amazon Pinpoint API Reference*\.