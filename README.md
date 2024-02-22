---
toc: content
---

# MBFUniversalSDK

Ads and Analytics Framework

## Installation

To install MBFUniversalSDK, you need to add the following script tag to your HTML file:

```javascript
<script>
  window.MBFSDK||(window.MBFSDK={}),AiactivSDK.load=function(a){var b=document.createElement("script");b.async=!0,b.type="text/javascript",b.src="https://raw.githubusercontent.com/MBFTECH/MBFUniversalSDK_Web/main/mbf-sdk.min.js",b.addEventListener?b.addEventListener("load",function(b){"function"==typeof a&&a(b)},!1):b.onreadystatechange=function(){("complete"==this.readyState||"loaded"==this.readyState)&&a(window.event)};let c=document.getElementsByTagName("script")[0];c.parentNode.insertBefore(b,c)},MBFSDK.load(function(){AiactivSDK.initialize({containerId:"{ContainerID}", type: {SDK_TYPES}}),MBFSDK.callMethodsFromContainer()});
</script>
```

This will load MBFUniversalSDK from our CDN and make it available as a global variable named `MBFSDK`.

- `{SDK_TYPE}` is adnetwork or dmp.
  - Exp: ['adnetwork', 'dmp']
- `{ContainerID}` is containerID from AdNetwork portal site: https://adnetwork-portal.aiactiv.io/#/container/{ContainerID}
  - Exp: 4ceed9c8-c28f-4a7a-8646-9b071f4c29c7@web

## Usage

### AdNetwork

#### Banner Ad

- Create an ads placement on your site:

```html
<div id="display_ads"></div>
```

- Then call for display ads

```javascript
window.AiactivSDK.requestAds([
  {
    inventoryId: 1, // your inventory ID from portal,
    placementId: 'display_ads',
  },
]);
```

- Or this way:

```html
<div data-mbf-sdk-placement="1">
  <script type="text/javascript">
    window.addEventListener('load', function () {
      MBFSDK.requestAd(1);
    });
  </script>
</div>
```

#### Video Ad

- Option 1

```html
<div data-mbf-sdk-placement="377">
  <script type="text/javascript">
      window.addEventListener('load', function () {
        MBFSDK.requestAd(377 ,{
        // Only when using MBF's player
        video: {
            player: // true/false,
            preventPauseWhenClick:  // true/false;
            loop:  // true/false;
            controls:  // true/false;
        },
        debug: // true/false,
    });
    });
  </script>
</div>
```

- Option 2

```javascript
window.MBFSDK.requestAds([{
    inventoryId: 377,
    placementId: "//your placement ID in DOM",
    options: {
        // Only when using 's player
        video: {
            player: // true/false,
            preventPauseWhenClick:  // true/false;
            loop:  // true/false;
            controls:  // true/false;
        },
        debug: // true/false,
    }
  }]);
```

- Option 3: In-stream video ad

```javascript
window.MBFSDK.requestAds([{
    inventoryId: "// your inventory ID number", // eg: 123
    placementId: "//your video youtube iframe ID in DOM", // eg: "youtube-vid-id"
    options: {
        // Only when using MBF's player
        video: {
            videoContentId:"//your video youtube iframe ID in DOM", // REQUIRED!!, string - it must be id of iframe include youtube video, eg: "youtube-vid-id"
            player: true, // REQUIRED!! - true
            preventPauseWhenClick:  // true/false;
            loop:  // true/false;
            controls:  // true/false;

        },
        debug: // true/false,
    }
  }]);
```

Example iframe youtube

```html
<iframe
  id={VIDEO_ID} // This one should be pass to videoContentId and inventoryId params in above config
  width="560"
  height="315"
  src="https://www.youtube.com/embed/${VIDEO_CODE}"
  title="YouTube video player"
  frameBorder="0"
  allowFullScreen
></iframe>
```

#### Native Ad

1. Show Native ad by programmatically

Usage

```javascript
const sdkResponse = window.MBFSDK.requestAds([
  {
    inventoryId: 1, // your inventory ID from portal,
  },
]);
```

In **sdkResponse** the SDK will response native ad response from ad service with friendly format
Example:

```JSON
[
  {
    "inventoryId": "ID of inventory", // your inventory ID from portal,
    "placementId": "Placement to display ad (optional)",
    "native": {
      "title": "",
      "icon": {
        "url": "",
        "width": 0,
        "height": 0
      },
      "video": {
        "vast": "//VAST CONTENT"
      },
      "description": "",
      "callToAction": "",
      "previewImage": {
        "url": "",
        "width": 0,
        "height": 0
      },
      "link": {
        "landingURL": "",
        "clickTrackers": [
          ""
        ],
        "fallbackLandingURL": ""
      },
      "events": [
        {
          "event": 0,
          "method": 0,
          "value": ""
        }
      ]
    },
    "type": "native"
  }
]
```

Friendly Native Object Documentation

```go
// Title of ads
// -> position 2 in sample layout
Title string

// main description of ads
//   - Get from nativeAd.assets[*].data.value if nativeAd.assets[*].data.type = desc
// normally contain main information of product or ads
// -> position 3 in sample layout
Description string

// addition description of ads
//   - Get from nativeAd.assets[*].data.value if nativeAd.assets[*].data.type = desc2
SubDescription string

// Sponsored text
//    - Get from nativeAd.assets[*].data.value if nativeAd.assets[*].data.type = sponsored
// e.g: "Sponsored by Vinamilk"
Sponsored string

// Rating text
//    - Get from nativeAd.assets[*].data.value if nativeAd.assets[*].data.type = rating
// e.g: "3.4 stars out of 5"
Rating string

// Like text
//    - Get from nativeAd.assets[*].data.value if nativeAd.assets[*].data.type = like
// e.g: "1.409.000 Liked"
// position 5 in sample layout
Like string

// Downloads text
//    - Get from nativeAd.assets[*].data.value if nativeAd.assets[*].data.type = downloads
// e.g: "1.409.000 Downloaded"
Downloads string

// Price text
//    - Get from nativeAd.assets[*].data.value if nativeAd.assets[*].data.type = price
// e.g: "$10"
Price string

// Sale Price text
//    - Get from nativeAd.assets[*].data.value if nativeAd.assets[*].data.type = saleprice
// e.g: "$7" ( mostly use combine with price e.g "(-$10-) $7"
SalePrice string

// Phone text
//    - Get from nativeAd.assets[*].data.value if nativeAd.assets[*].data.type = phone
// e.g: "0123456789"
Phone string

// Address text
//    - Get from nativeAd.assets[*].data.value if nativeAd.assets[*].data.type = address
// e.g: "01 Loc 145 Street, Somewhere"
Address string

// Display URL - the url used to display in native ads, depend on usage
//    - Get from nativeAd.assets[*].data.value if nativeAd.assets[*].data.type = displayurl
// e.g: "https://vinamilk.com/product/abc"
DisplayURL string

// Call to Action text
//    - Get from nativeAd.assets[*].data.value if nativeAd.assets[*].data.type = ctatext
// e.g: "Buy Now"
// position 6 in sample layout
CallToAction string

// Icon - display icon of product or brand logo
//    - Get from nativeAd.assets[*].img if nativeAd.assets[*].img.type = 1
// An object contain url and size information of icon
// Detail about Image object bellow
Icon ImageObject

// Preview image of product or ads
//    - Get from nativeAd.assets[*].img if nativeAd.assets[*].img.type = 3
// An object contain url and size information of main image
// Detail about Image object bellow
PreviewImage ImageObject

// Video of product or ads
//    - Get from nativeAd.assets[*].video
// An object contain vasttag and landing url plus addition information
// Detail about Image object bellow
Video VideoObject

// Link whenever user clicked on ads
//    - get from nativeAd.link
// Detail bellow
Link LinkObject

// Events when serving ads
//    - get from nativeAd.eventtrackers
// Detail bellow
Events []EventObject
```

##### Image Object

```go
// URL to display image
// e.g: "https://abc.com/image.png"
URL string

// width of image
Width int

// height of image
Height int
```

##### Video Object

```go
// XML VastTag to display video
//    - Get from nativeAd.assets[*].video
Vast string
```

##### Link Object

```go
// Landing url of the clickable link
// When user is clicked the action should take user to the location of this link.
// e.g: "https://vinamilk.com/product/abc" -> open webpage
// e.g: "appstore://app1.com" -> open appstore
LandingURL string

// A list of click trackers link
// This must be fired when click to ads or URL
ClickTrackers []string

// In case of LandingURL is not supported on device, this URL should be called instead
FallbackLandingURL string
```

##### Event Object

```go
// Type of event
//    - Get from nativeAd.eventtrackers[*].event
// List of event will be present below
Event int

// Calling method (JS tag or pixel url)
//    - Get from nativeAd.eventtrackers[*].method
// List of method will be present below
Method int

// Can be URL of method is pixel url or Javascript tag
//    - Get from nativeAd.eventtrackers[*].url
Value string
```

2. Using native ad template
   we have 3 template to select

- template-1
- template-2
- template-3

Refer this link to see [template design](https://www.figma.com/file/RK61kyH2EEVZeubKyQA3os/AI-ACTIV---M360N?node-id=1-4&t=AD4zJSe5nFmiCpb5-0).

Usage

```javascript
await sdk.requestAds([
  {
    inventoryId: 1, // your inventory ID from portal,
    placementId: 'placement_id_to_show_ad',
    options: {
      native: {
        template_id: 'template-1',
      },
    },
  },
]);
```

3. Using native ad with auto-fill option
   For this option the SDK will render native ad content to ad placement

Usage

```javascript
const adUnits = [
  {
    inventoryId: 1, // your inventory ID from portal,
    placementId: 'placement to show ad',
    options: {
      native: {
        placements: {
          title: 'placement of title',
          description: 'placement of description',
          subDescription: 'placement of sub description',
          sponsored: 'placement of sponsored',
          rating: 'placement of rating',
          like: 'placement of like',
          downloads: 'placement of downloads',
          price: 'placement of price',
          salePrice: 'placement of sale price',
          phone: 'placement of phone',
          address: 'placement of address',
          displayURL: 'placement of display url',
          callToAction: 'placement of call to action',
          icon: 'placement of icon',
          previewImage: 'placement of preview image',
          link: 'placement of link',
        },
      },
    },
  },
];
await sdk.requestAds(adUnits);
```

### Analytics

#### Track Events

```javascript
MBFSDK.track(event: String, properties: Object?)
```

To track an event, you need to call the `MBFSDK.track()` method and pass in two parameters: `event` and `properties`.

- The `event` parameter is a string to name the event, for example: "Post view", "Sign up", "Purchase", etc.
- The `properties` parameter is an object to contain detailed information about the event, for example: post title, product type, order value, etc.

```javascript
// Create an object to contain the properties of the event
const postViewEventProperties = {
  title: 'Post Title',
  category: 'Category 1, Category 2',
  keyword: 'Keyword 1, Keyword 2, ...',
};

// Track the event "Post view" with the properties created
MBFSDK.track('Post view', postViewEventProperties);
```

#### Identify Events

```javascript
MBFSDK.identify(userId: String, traits: Object?)
```

To identify a user, you need to call the `MBFSDK.identify()` method and pass in one parameter: `userId`.

- The `userId` parameter is a string to identify your user, for example: "PartnerUserID-01".
- You can also pass in another object to contain additional information about the user, for example: user name, user type, etc.

```javascript
// Create an object to contain the properties of the user
const userTraits = {
    userName: "Username 1",
    userType: "Normal",
    ...
}

// Identify user with user ID and properties created
MBFSDK.identify("PartnerUserID-01", userTraits)
```

```javascript
// Create an object to contain the properties of the user
const userTraits = {
    userName: "Username 1",
    userType: "Normal",
    ...
}

// Identify user without user ID and properties created
MBFSDK.identify(userTraits)
```

**Traits recommended for Analytics identify**

| TRAIT       | TYPE   | DESCRIPTION                                                                                                                                          |
| :---------- | :----- | :--------------------------------------------------------------------------------------------------------------------------------------------------- |
| address     | Object | Street address of a user optionally containing: city, country, postalCode, state, or street                                                          |
| age         | Number | Age of a user                                                                                                                                        |
| avatar      | String | URL to an avatar image for the user                                                                                                                  |
| birthday    | Date   | User’s birthday                                                                                                                                      |
| company     | Object | Company the user represents, optionally containing: name(String), id (String or Number), industry (String), employee_count (Number) or plan (String) |
| createdAt   | Date   | Date the user’s account was first created. Segment recommends using ISO-8601 date strings.                                                           |
| description | String | Description of the user                                                                                                                              |
| email       | String | Email address of a user                                                                                                                              |
| firstName   | String | First name of a user                                                                                                                                 |
| gender      | String | Gender of a user                                                                                                                                     |
| id          | String | Unique ID in your database for a user                                                                                                                |
| lastName    | String | Last name of a user                                                                                                                                  |
| name        | String | Full name of a user. If you only pass a first and last name Segment automatically fills in the full name for you.                                    |
| phone       | String | Phone number of a user                                                                                                                               |
| title       | String | Title of a user, usually related to their position at a specific company. Example: “VP of Engineering”                                               |
| username    | String | User’s username. This should be unique to each user, like the usernames of Twitter or GitHub.                                                        |
| website     | String | Website of a user                                                                                                                                    |
| deviceId    | String | ID if user's device                                                                                                                                  |

## Author

MBF TECH, tech@mbf.vn

## License

MBFUniversalSDK is available under the MIT license. See the LICENSE file for more info.
