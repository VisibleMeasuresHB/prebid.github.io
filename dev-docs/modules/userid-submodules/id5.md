---
layout: userid
title: ID5 Universal ID
description: ID5 Universal ID User ID sub-module
useridmodule: id5IdSystem
---


The ID5 ID is a shared, neutral identifier that publishers and ad tech platforms can use to recognise users even in environments where 3rd party cookies are not available. The ID5 ID is designed to respect users' privacy choices and publishers’ preferences throughout the advertising value chain. For more information about the ID5 ID and detailed integration docs, please visit [our documentation](https://support.id5.io/portal/en/kb/articles/prebid-js-user-id-module).


## ID5 ID Registration

The ID5 ID is free to use, but requires a simple registration with ID5. Please visit [our website](https://id5.io/solutions/#publishers) to sign up and request your ID5 Partner Number to get started.

The ID5 privacy policy is at [https://id5.io/platform-privacy-policy](https://id5.io/platform-privacy-policy).

## ID5 ID Configuration

First, make sure to add the ID5 submodule to your Prebid.js package with:

{: .alert.alert-info :}
gulp build --modules=id5IdSystem,userId

The following configuration parameters are available:

{: .table .table-bordered .table-striped }
| Param under userSync.userIds[] | Scope | Type | Description | Example |
| --- | --- | --- | --- | --- |
| name | Required | String | The name of this module: `"id5Id"` | `"id5Id"` |
| params | Required | Object | Details for the ID5 ID. | |
| params.partner | Required | Number | This is the ID5 Partner Number obtained from registering with ID5. | `173` |
| params.pd | Optional | String | Partner-supplied data used for linking ID5 IDs across domains. See [our documentation](https://support.id5.io/portal/en/kb/articles/passing-partner-data-to-id5) for details on generating the string. Omit the parameter or leave as an empty string if no data to supply | `"MT1iNTBjY..."` |
| params.abTesting | Optional | Object | Allows publishers to easily run an A/B Test. If enabled and the user is in the Control Group, the ID5 ID will NOT be exposed to bid adapters for that request | Disabled by default |
| params.abTesting.enabled | Optional | Boolean | Set this to `true` to turn on this feature | `true` or `false` |
| params.abTesting.controlGroupPct | Optional | Number | Must be a number between `0.0` and `1.0` (inclusive) and is used to determine the percentage of requests that fall into the control group (and thus not exposing the ID5 ID). For example, a value of `0.20` will result in 20% of requests without an ID5 ID and 80% with an ID. | `0.1` |
| params.disableExtensions | Optional | Boolean | Set this to `true` to force turn off extensions call. Default `false` | `true` or `false` |

{: .alert.alert-info :}
**NOTE:** The ID5 ID that is delivered to Prebid will be encrypted by ID5 with a rotating key to avoid unauthorized usage and to enforce privacy requirements. Therefore, we strongly recommend setting `storage.refreshInSeconds` to `8` hours (`8*3600` seconds) or less to ensure all demand partners receive an ID that has been encrypted with the latest key, has up-to-date privacy signals, and allows them to transact against it.

### A Note on A/B Testing

Publishers may want to test the value of the ID5 ID with their downstream partners. While there are various ways to do this, A/B testing is a standard approach. Instead of publishers manually enabling or disabling the ID5 User ID Module based on their control group settings (which leads to fewer calls to ID5, reducing our ability to recognize the user), we have baked this in to our module directly.

To turn on A/B Testing, simply edit the configuration (see above table) to enable it and set what percentage of users you would like to set for the control group. The control group is the set of user where an ID5 ID will not be exposed in to bid adapters or in the various user id functions available on the `pbjs` global. An additional value of `ext.abTestingControlGroup` will be set to `true` or `false` that can be used to inform reporting systems that the user was in the control group or not. It's important to note that the control group is user based, and not request based. In other words, from one page view to another, a user will always be in or out of the control group.

## ID5 Universal ID Examples

{: .alert.alert-warning :}
**ATTENTION:** As of Prebid.js v4.14.0, ID5 requires `storage.type` to be `"html5"` and `storage.name` to be `"id5id"`. Using other values will display a warning today, but in an upcoming release, it will prevent the ID5 module from loading. This change is to ensure the ID5 module in Prebid.js interoperates properly with the [ID5 API](https://github.com/id5io/id5-api.js) and to reduce the size of publishers' first-party cookies that are sent to their web servers. If you have any questions, please reach out to us at [prebid@id5.io](mailto:prebid@id5.io).

Publisher wants to retrieve the ID5 ID through Prebid.js

{% highlight javascript %}
pbjs.setConfig({
  userSync: {
    userIds: [{
      name: 'id5Id',
      params: {
        partner: 173,            // change to the Partner Number you received from ID5
        pd: 'MT1iNTBjY...',      // optional, see table above for a link to how to generate this
        abTesting: {             // optional
          enabled: true,         // false by default
          controlGroupPct: 0.1   // valid values are 0.0 - 1.0 (inclusive)
        }
      },
      storage: {
        type: 'html5',           // "html5" is the required storage type
        name: 'id5id',           // "id5id" is the required storage name
        expires: 90,             // storage lasts for 90 days
        refreshInSeconds: 8*3600 // refresh ID every 8 hours to ensure it's fresh
      }
    }],
    auctionDelay: 50             // 50ms maximum auction delay, applies to all userId modules
  }
});
{% endhighlight %}
