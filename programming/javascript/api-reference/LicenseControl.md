---
layout: default-layout
title: Dynamsoft Barcode Reader JavaScript API - License Control
description: This page shows the License Control APIs of Dynamsoft Barcode Reader JavaScript SDK.
keywords: License Control, api reference, javascript, js
needAutoGenerateSidebar: true
needGenerateH3Content: true
noTitleIndex: true
breadcrumbText: License Control
---

# License Control

The library provides flexible licensing options with the support of the following APIs

* [licenseServer](#licenseserver)
* [organizationID](#organizationid)
* [handshakeCode](#handshakecode)
* [sessionPassword](#sessionpassword)
* [productKeys](#productkeys)

<div class="doc-card-prefix"></div>

> ### licenseServer
> <hr>
> `static` licenseServer: *string&#91;&#93; &#124; string*
> <hr>
> Specifies the URL(s) for the main and stand-by License Tracking Server(s). This is only required when you host the License Tracking Server(s) yourself. If nothing is set, the Server(s) hosted by Dynamsoft will be used.
> #### Example
> ```js
> // You can specify only the main server
> Dynamsoft.DBR.licenseServer = ["YOUR-OWN-MAIN-LTS"];
> 
> //or you can specify both
> Dynamsoft.DBR.licenseServer = ["YOUR-OWN-MAIN-LTS", "YOUR-OWN-STANDBY-LTS"];
> ```

<div class="doc-card-prefix"></div>

> ### organizationID
> <hr>
> `static` organizationID: *string*
> <hr>
> When a license is purchased, it is registered to an Organization. This license is then hosted by a License Tracking Server which authorizes terminal devices and consumes the license. This API specifies which Organization you would like to acquire authorization from.
> #### Example
> ```js
> Dynamsoft.DBR.organizationID = "YOUR-ORGANIZATION-ID";
> ```

<div class="doc-card-prefix"></div>

> ### handshakeCode
> <hr>
> `static` handshakeCode: *string*
> <hr>
> Licenses registered to the same Organization are grouped by Handshake Codes. When an Organization is specified by `organizationID`, the default Handshake Code will be used unless another Code is specified with this API.
> 
> Generally, the first Handshake Code ever created for an organization is the default one. However, you can always make another Code default in the [customer portal](https://www.dynamsoft.com/lts/#/handshakeCodes).
> #### Example
> ```js
> Dynamsoft.DBR.handshakeCode = "YOUR-HANDSHAKE-CODE";
> ```

<div class="doc-card-prefix"></div>

> ### sessionPassword
> <hr>
> `static` sessionPassword: *string*
> <hr>
> Specifies a password to protect the [Handshake Code](#handshakeCode). If no Handshake Code is specified with the API `handshakeCode`, this password protects the default Handshake Code.
> 
> The password can be set for each Handshake Code when it was first created and can be changed later by editing the configuration of the Code.
> #### Example
> ```js
> Dynamsoft.DBR.sessionPassword = "YOUR-SESSION-PASSWORD";
> ```
  
<!--div class="doc-card-prefix"></div>

> ### deviceFriendlyName
> <hr>
> `static` deviceFriendlyName: *string*
> <hr>
> Sets a human-readable name that identifies the device. This name will appear in the device details table when you check the statistics of a Handshake Code or a License Item.
> #### Example
> ```js
> Dynamsoft.DBR.deviceFriendlyName = "Harry-Potter-iPhone";
> ```
-->

<div class="doc-card-prefix"></div>

> ### productKeys
> <hr>
> `static` productKeys: *string*
> <hr>
> A product key is an alphanumeric string used as an offline license. If such a key is specified in your program, you do not need to specify anything else for licensing purposes.
> #### Example
> ```js
> Dynamsoft.DBR.productKeys = "YOUR-PRODUCT-KEYS";
> ```
> 
> For convenience, you can even set `productKeys` in the `script` tag.
> 
> ```html
> <script src="https://cdn.jsdelivr.net/npm/dynamsoft-javascript-barcode@8.2.5/dist/dbr.js" data-productKeys="PRODUCT-KEYS"></script>
> ```