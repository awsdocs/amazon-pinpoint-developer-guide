# Managing an audience members maximum number of endpoints<a name="audience-define-auto-inactive"></a>

Each member of your audience can have a maximum of 15 endpoints associated with their **UserId**, see [Endpoint quotas](quotas.md#quotas-endpoint)\. If you try to add a 16th endpoint then, depending on the **ChannelType**, you will either get **BadRequestException** or it will succeed by removing the endpoint with the oldest **EffectiveDate**\.

**Adding a 16th endpoint**
+ If the new channel type for the endpoint is SMS, PUSH, VOICE, EMAIL, CUSTOM or IN\_APP then **BadRequestException** is returned because the audience member is at their maximum number of endpoints\. You need to remove an endpoint associated with the audience member and try again, see [Deleting endpoints from Amazon Pinpoint](audience-define-remove.md)\.
+ If the new channel type for the endpoint is ADM, GCM, APNS, APNS\_VOIP, APNS\_VOIP\_SANDBOX or BAIDU:
  + Check that at least one endpoint currently associated with the audience member has a **ChannelType** of ADM, GCM, APNS, APNS\_VOICE, APNS\_VOIP\_SANDBOX or BAIDU\. If there isn't then **BadRequestException** is returned and an endpoint needs to be removed before you try again, see [Deleting endpoints from Amazon Pinpoint](audience-define-remove.md)\. 
  + Otherwise, the endpoint with the oldest **EffectiveDate** is set to `INACTIVE` where the **ChannelType** is ADM, GCM, APNS, APNS\_VOIP, APNS\_VOIP\_SANDBOX or BAIDU\.
    + The **UserId** from the old endpoint is removed\.
    + The new endpoint is associated to the audience member and they still have the maximum number of endpoints\. 

The endpoint can be re–enabled by setting the **Status** to `ACTIVE` and add the **UserId** back to the endpoint\.