---
layout: bidder
title: Adprime
description: Prebid Adprime Bidder Adapter
biddercode: adprime
gdpr_supported: true
usp_supported: true
media_types: banner, video, native
tcf2_supported: true
pbjs: true
pbs: true
pbs_app_supported: true
sidebarType: 1
---

### Note:

The Adprime Bidding adapter requires setup before beginning. Please contact us at rafal@adprime.com

### Bid Params

{: .table .table-bordered .table-striped }
| Name          | Scope    | Description           | Example   | Type      |
|---------------|----------|-----------------------|-----------|-----------|
| `placementId`      | required | Adprime placement id         | `'1234asdf'`    | `string` |
| `keywords`    | optional | page context keywords | ['car','sport'] | `array` |
| `audiences`    | optional | publisher audiences | ['aud1','aud2'] | `array` |

