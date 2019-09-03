# Template Development


{% raw %}
## What is a Yondo Template? ##
Included with a [Yondo](http://www.yondo.com) account is a *hosted store website*. This provides a front-end for your customers to use when browsing your live session listings, video-on-demand and webinar products. A highly customizable template engine provides the web content for this front-end. Every Yondo *store* uses either a standard template, or a custom installed template. **A template is basically a collection of html, css and other standard files you would find on a website.**

## How do I develop a Template? ##
Go to the [Yondo Partner Network](http://partners.yondo.com/signup) and sign up for a developer account. Here you can create and upload your template, and create a development store to test your template. The editing functions are quite basic, most developers create their template externally and just upload when done.

### My template is empty, how do I get started? ###
Download a Zip of the [sample classic template](https://github.com/yondoecommerce/classic-template), [sample affinity template](https://github.com/yondoecommerce/affinity-template) or [sample circular template](https://github.com/yondoecommerce/classic-template), and upload it to the *files* section of your template. This will load the template with the files from the sample template.

## Template Structure ##
A template is made up of template files. These are regular html/liquid files, css, js etc. Templates which require server side rendering have a `.liquid` extension and use the [Liquid template engine](https://github.com/Shopify/liquid/wiki). 

### Root directory `/` ###
The root directory contains all of the `.liquid` files. The names of the `.liquid` files must match the page to be rendered. See each page section for more information.

**Standard template files:**

- base.liquid

Pages:
- base.liquid
- cancel-booking.liquid
- checkout.liquid
- checkout-complete.liquid
- dashboard.liquid
- embed-video.liquid
- forgot-password.liquid
- home.liquid
- listing.liquid
- login.liquid
- materials.liquid
- package.liquid
- page.liquid
- profile.liquid
- recording.liquid
- reschedule-booking.liquid
- reset-password.liquid
- search.liquid
- session-archive.liquid
- session-calendar.liquid
- start.liquid
- video-search.liquid
- video.liquid
- webinar-access.liquid
- webinar-attachments.liquid
- webinar-calendar.liquid
- webinar-topic.liquid

Partials:

- custom-fields.liquid
- listing-card.liquid
- portfolio.liquid
- team-member-card.liquid
- video-player-scripts.liquid
- video-player.liquid
- video-purchase-partial.liquid
- video-tab.liquid
- webinar-topic-card.liquid

### Assets directory `/assets` ###
This contains all of the files you want to reference from within your pages. Things like `.css`, `.less`, `.js`. These files must be in the `/assets` directory because this is where the `asset_url` filter references files.

Within the `/assets` directory you can lay out your files anyway you like, and use sub-directories many levels deep.

#### .less files ####
`.less` files can be used within the `/assets` directory. They will be processed server side. The sample template uses bootstrap `.less` files which are compiled dynamically. This allows you to make changes to a few settings in the `variables.less` to have great impact on your site.

#### Store varables ####
`.css` and `.less` files within the `/assets` directory can make use of liquid variables which are pumped into the file from settings related to the store. eg

	// assets/style.css
	.header {
		background-color: {{ store-header-color }};
	}

Current store varables are:

|Variable                 	|Info                                                       	|
|-------------------------	|-----------------------------------------------------------	|
| {{ store-header-color}} 	| The hex color set in the Store Settings &gt; Header Color 	|


## Liquid Markup ##
Yondo handles most of the standard liqiuid features including:

* Extend (layout / master page). eg `{% extends base %}` use the `base.liquid` as the layout/master page.
* Blocks defined in layout page. eg `{% block content %} Hello World {% endblock %}` fills the *content* block from the layout page.
* Include (partials / snippets). eg `{% include mypartial %}` inserts `mypartial.liquid` into the page.
* For loops.
* If conditions.
* Filters.

More details here: [Liquid for Designers](https://github.com/Shopify/liquid/wiki/Liquid-for-Designers)

### Custom Filters ###
Yondo has some custom filters which can be used when referencing variables using Liquid.

**HTML Filters**

* `img_tag` - takes a URL (relative or absolute) and returns an image tag.
* `stylesheet_tag` - takes a URL (relative or absolute) and returns a link tag.
* `script_tag` - takes a URL (relative or absolute) and returns a script tag.
* `meta_tags` - takes the page name (eg 'home', 'listing' etc) and returns a series of meta tags used for SEO. This should be added to the `<head>`.
* `local_time` - takes a datetime in UTC and returns a `script` tag which renders the date as the browser's local time. **Requires moment.js.**
* `from_x` - takes a datetime in UTC and returns a `script` tag which renders the date as a time from now in a human readable format. eg 'in 5 days'.
* `google_analytics_page_view_script_tag` - takes [Google Tracking Id](https://support.google.com/analytics/answer/1032385?hl=en) and and returns [Google Analytics script](https://developers.google.com/analytics/devguides/collection/analyticsjs/) tag.
* `google_analytics_event_script_tag` - takes [Google Tracking Id](https://support.google.com/analytics/answer/1032385?hl=en) and return [Google Analytics event tracking script](https://developers.google.com/analytics/devguides/collection/analyticsjs/events) tag.

**URL Filters**

* `asset_url` - takes a relative reference to a file in the `/assets` folder and returns a public URL for the file. eg `{{ 'css/style.css' | asset_url }}` returns a public url for the `/assets/css/style.css` file. This URL may be served through a CDN for improved speed.

> Note: Filters can be chained together. A common usage is like this:  
> `{{ 'css/style.css' | asset_url | stylesheet_tag  }}` 
> which outputs:
> `<link href='/cdn/1/assets/css/style.css' rel='stylesheet' type='text/css' media='all' />`

**Money Filters**

* `money` - takes a number and returns a formatted money using the store's currency symbol. eg '$25.00'. Zero is output as 'Free'.
* `money_with_currency` - same as `money` but appends the ISO currency code. eg '$25.00 USD'.
* `money_with_currency` - same as `money` but drops the currency symbol. eg "25.00'.

**JSON Filters**

* `json` - takes the variable and outputs the JSON format. eg `'this is a string variable'` or `'{ prop: 'value' }'`

> JSON filter is mostly used for rendering a liquid variable inside a `<script>` tag to use in javascript. eg:
```html
	<script>
		var listing = {{ listing | json }};
	</script>
```

Outputs:

```html
    <script>
    	var listing = { name:'sample', price:25 };
    </script>
```

# Template Data
The  following data objects are available on different template pages. 

## Store 

This is an object to represent a store. It contains store information about what is the store's name, logo, header color, alias Url, subdomain Url and setting.

| Attributes | --- |
| --- | --- |
| id | *integer* An unique integer identifier of a store |
| name | *string* The name of a store |
| logo | *string* The logo URL of a store |
| wideLogo | *string* The wide logo URL of a store |
| selectedLogo | *string* The selected logo URL of a store |
| selectedLogoClass | *string* The class name of selected logo of a store |
| headerColor | *string* Header color of a store |
| aliasUrl | *string* Alias URL of a store |
| subdomainUrl | *string* Subdomain URL of a store |
| currency | *string* The currency which a store uses for payment |
| currencySymbol | *string* The currency symbol used as as shorthand for a currency's name   |
| bannerImageUrl | *string* The banner image URL of a store |
| ssoType | *string* The type of single sign on of a store |
| webinarNoun | *string* The name of webinar that a store uses |
| webinarNounPlural | *string* The name of webinar that a store uses |
| setting | *Object* Additional settings enabled for the store |
| googleAnalyticsTrackingId | *string* Google analytics tracking Id of a store |
| customTrackingScript | *string* custom javascript tracking code of a store |

## Team Member

This is an object to represent a team member in a store.

| Attributes | --- |
| --- | --- |
| id | *integer* An unique integer identifier of a team member |
| displayName | *string* An unique display name of a team member |
| url | *string* Relative URL of a team member's profile page |
| profileDescription | *string* Description of a team member's profile |
| profileImageUrl | *string* The URL of team member's profile image |
| profileThumbUrl | *string* The URL of team member's profile image thumbnail size 50px x 50px |
| profileThumb32Url | *string* The URL of team member's profile image thumbnail size 32px x 32px |
| country | *string* The name of a team member's country |
| qualificationsHtml | *string* Qualification that a team member hold. It contains html content. |
| location | *string* Location of a team member |
| bannerImageUrl | *string* The banner image URL of a team member |
| firstName | *string* The first name of a team member |
| LastName | *string* The last name of a team member |
| fullName | *string* The full name of a team member |

## Session

This is an object to represent current user's session. FirstName, LastName, DisplayName and ProfileImage are only shown when a user is logged in.

| Attributes | --- |
| --- | --- |
| isLoggedIn | *boolean* Login status of an user. It is true if an user is already logged in |
| firstName | *string* First name of an user. It is null if an user is not logged in |
| lastName | *string* Last name of an user. It is null if an user is not logged in |
| customerId | *integer* An unique integer identifier of an user |
| email | *string* The email address of an user |
| languages | *string* Languages of an user's browser |
| dateFormat | *string* Date format of an user |

## Listing

This is an object to represent a live session listing. It represents a single class/consultation for a specific duration and price.

| Attributes | --- |
| --- | --- |
| id | *integer* An unique integer identifier of a listing |
| title | *string* A title of a listing |
| price | *integer* Price of a listing in dollar per duration |
| duration | *integer* Duration of a listing |
| descriptionHtml | *string* Description of a listing. It contains html content. |
| prerequisiteHtml | *string* A listing is required to be taken before taking a listing. It contains html content |
| url | *string* Relative URL of a listing |
| availability | *boolean* Availability of a listing |
| materials | *Array of Material objects* A list of listing materials |
| portfolios | *Portfolio object* A portfolio for the listing |
| teamMember | *Team Member object* Details of a team member providing a listing |
| category | *string* The name of category that a listing is classified |
| type | *string* The type of a listing to specifiy  |
| image220Url | *string* The URL of a listing image size 220px x 165px |
| image360Url | *string* The URL of a listing image size 360px x 270px |
| image580Url | *string* The URL of a listing image size 580px x 435px |
| upcommingAvaliability | *Array of Date* A list of upcomming availability in UTC of a listing |
| published | *boolean* status of a listing |

## Material

This is an object to represent a material attached to a listing. eg worksheet or file download.

| Attributes | --- |
| --- | --- |
| id | *integer* An unique integer identifier of a material |
| fileName | *string* Actual file name of a material |
| name | *string* The name of a material |
| description | *string* Description of a material |
| downloadUrl | *string* Download URL of a material. Download URL is only provided when an user booked a listing |

## Menu

This is an object to represent a custom page.

| Attributes | --- |
| --- | --- |
| title | *string* The name of a custom page |
| url | *string* The URL of a custom page |

## Paging Information

This is an object to represent paging information for the current result set.

| Attributes | --- |
| --- | --- |
| number | *integer* Current page number |
| size | *integer* The number of results to be returned per page |
| totalResults | *integer* The total number of results |
| totalPages | *integer* The total pages of results |
| prev | *boolean* Availability of previous page |
| next | *boolean* Availability of next page |

## Portfolio

This is an object to represent a portfolio item of a store or a team member.

| Attributes | --- |
| --- | --- |
| id | *integer* An unique integer identifier of a portfolio |
| sequence | *integer* Sequence of portfolio |
| portfolioAsset | *PortfolioAsset Object*

### Portfolio Asset

| Attributes | --- |
| --- | --- |
| id | *integer* An unique integer identifier of a portfolio asset |
| assetType | *integer* A type of an asset (1: Video, 2: Document and 3: Image) |
| assetTypeName | *string* The name of an asset (none, Video, Document and Image) |
| imageUrl | *string* Asset's image UrlWhen an asset type is image, this field is set |
| imageThumbUrl | *string* Asset's image thumb UrlWhen an asset type is image, this field is set |
| videoUrl | *string* Asset's video UrlWhen an asset type is video, this field is set |
| comment | *string* Explain about what a document is. When an asset type is document, this field is set |
| documnetUrl | *string* Asset's document Url. When an asset type is document, this field is set |
| documentThumbUrl | *string* Asset's document thumb Url. When an asset type is document, this field is set |

## Site Infomation

This is an object to represent Yondo website details.

| Attributes | --- |
| --- | --- |
| sitePath | *string* The URL of a store's dashboard |
| alternateHomePage | *string* The URL of Yondo |
| nakedDomain | *string* Naked domain of Yondo |
| siteName | *string* The name of Yondo |
| copyrightThe | *string* The copyright of Yondo |
| stripeConnectClientId | *string* |

## Section

This is an object to represent a section provided by a store.

| Attributes | --- |
| --- | --- |
| id | *string* An unique string identifier of a section |
| heading | *string* The name of a section |

## Video

This is an object to represent a video in a store

| Attributes | --- |
| --- | --- |
| id | *integer* An unique integer identifier of a video |
| title | *string* A title of a video |
| author | *string* The name of publisher or producer of a video |
| uploadedDate | *string* Uploaded date of a video |
| duration | *integer* Duration of a video |
| descriptionHtml | *string* Description of a video. It contains html content. |
| url | *string* Relative URL of a listing |
| thumbnail220Url | *string* The URL of a video thumbnail image size 220px x 165px |
| thumbnail360Url | *string* The URL of a video thumbnail image size 360px x 270px |
| thumbnail720Url | *string* The URL of a video thumbnail image size 720px x 405px |
| tags | *Array of string* A list of string to identify a video |
| helpLink | *string* The URL of a help page containing information of rentals and subscriptions |
| rentalPrice | *integer* Rental price of a video in dollar. It is null if a video is not available to rent. |
| rentalPeriod | *integer* Rental period of a video. It is null if a video is not available to rent. |
| rentalAvailable | *boolean* Availability of a video to rent |
| rented | *boolean* Rental status of an user againt a video |
| rentalExpire | *string* Expiry date of a rental video. It is null if a video is not rented. |
| subscriptionPrice | *integer* Subscription price of a video in dollar. It is null if a video is not available to rent. |
| subscriptionAvailable | *integer* Availability of a video to subscribe |
| subscribed | *integer* Subscription status of an user |
| subscriptionExpire | *integer* Expiry date of a subscription. It is null if a video is not subscribed. |
| subscribeAllVideos | *integer* Availablity to subscibe all videos in a store |
| assets | *Array of Video Asset object* A list of video assets. It is null if a video is not rented or subscribed |
| availableToWatch | *boolean* Availability to watch a video |
| previewVideoUrl360p | *integer* The URL of preview video |
| fileAttachments | *Array of FileAttachment object* A list of FileAttachment objects |
| callToActionUrl | *string* Relative URL of a call to action |

## Video Asset

This is an object to represent a video asset.

| Attributes | --- |
| --- | --- |
| guid | *string* An unique string identifier of a video asset |
| url | *string* The URL of a video asset |
| type | *string* The type of a video asset (144p Low Quality, 360p SD or 720p HD) |

## File Attachment

This is an object to represent an attachment on a video

| Attributes | --- |
| --- | --- |
| filename | *string* The name of an attached file |
| title | *string* The title of an attachment  |
| description | *string* Description of an attachement |
| downloadUrl | *string* Download URL of an attachement |
| icon | *string* The icon to represent an attachment |

## Webinar 

This is an object to represent a webinar 

| Attributes | --- |
| --- | --- |
| id | *integer* An unique integer identifier of a webinar |
| name | *string* A title of a webinar |
| startTimeUtc | *string* The start time of a webinar in UTC |
| maxAttendees | *integer* The maximum number of attendees for a webinar |
| currentAttendees | *integer* The current number of attendees joining a webinar |

## Webinar Topic

This is an object to represent a webinar topic

| Attributes | --- |
| --- | --- |
| id | *integer* An unique integer identifier of a webinar topic |
| title | *string* A title of a webinar topic |
| descriptionHtml | *string* Description of a webinar topic. It contains html content. |
| imageSrc | *string* The URL of a webinar topic image |
| imageThumbSrc | *string* The URL of a webinar topic thumbnail image |
| duration | *integer* Duration of a webinar topic |
| price | *integer* Price of a webinar topic in dollar. |
| url | *string* The URL of a webinar topic |
| upcoming | *Array of Webinar object* A list of webinar object |

## Package Object

This is an object to represent a package of 1-to-1 sessions.

| Attributes | --- |
| --- | --- |
| type | *string* "Package" |
| id | *integer* |
| title | *string* |
| price | *decimal* |
| listings | *Array of [Listing](#listing) Objects* |
| image220Url | *decimal* |
| image360Url | *decimal* |
| image580Url | *decimal* |
| descriptionHtml | *string* |
| prerequisiteHtml | *string* |

# Template Pages

This outlines each page in a Yondo store and the properties available to the template.

> To view the properties available on a page add `.json` to the end of the page you are requesting. eg http://yourstore.yondo.com/.json .
> This can also be used as an API for retrieving data.

## Listing 
`listing.liquid`

Contains a single listing provided by a store.

Http Method: `GET`

Request URL Pattern: `/{category}/{name}/{id}`

`{category}`: the name of a category (url safe)
`{name}`: the name of a listing (url safe)
`{id}`: A unique integer identifier of a listing

> `{category}` and `{name}` are not validated and can essentially contain any string. They are part of the URL for SEO purposes.

| Attributes | --- |
| --- | --- |
| store | *Store Object* Store basic details providing a listing |
| session | *Session Object* Current user's details |
| listing | *Listing Object* the listing |
| upcommingAvaliability | *Array of string* A list of upcomming availability in UTC of a listing |

## Store Landing 
`home.liquid`

The index / home page of a store.

Http Method: `GET`

Url Pattern: `/`


| Attributes | --- |
| --- | --- |
| store | *Store Object* Store basic details |
| session | *Session Object* Current user's details |
| banner | *string* The URL of a store's banner image |
| about | *string* Explanation of a store |
| portfolios | *Array of Portfolio objects* A list of portfolio items of a store |
| listings | *Array of Listing objects* A list of live session listings provided by a store |
| teamMembers | *Array of TeamMember objects* A list of team members belonging to a store |
| dateTime | *string* The current Date |
| templatePage | *string* The name of template |
| checkoutUrl | *string* The URL of checkout |
| menus | *Menu object* |
| siteInfo | *SiteInfo object* |
| sections | *Sections object* |
| banner | *string* The URL of banner image |
| about | *string* Description of a store |
| videos | *Array of Video objects* A list of all Videos |
| popularVideos | *Array of Video objects* A list of popular Videos |
| latestVideos | *Array of Video objects* A list of the latest uploaded videos |
| webinarTopics | *Array of Webinar Topic objects* A list of Webinar Topics |

## Team Member Profile
`profile.liquid`

Team member's profile page.

Http Method: `GET`

Url Pattern:  `/{displayname}`

`{ displayname }`: an unique display name of a team member


| Attributes | --- |
| --- | --- |
| store | *Store Object* Store basic details |
| session | *Session Object* Current user's details |
| teamMember | *Team Member object* Details of a team member |
| listings | *Array of Listing objects* A list of live session listings provided by a team member |
| portfolios | *Array of Portfolio objects* A list of portfolio of a team member |


## Search Results
`search.liquid`

Search results page

Http Method: `GET`

Url Patterns:  

* `/search?query={query}&page={number}`

`{query}`: keyword used to search listings, videos and webinar topics
`{number: integer)`: page number (if page number is not set, it returns the first page of the results)



| Attributes | --- |
| --- | --- |
| store | *Store Object* Store basic details |
| session | *Session Object* Current user's details |
| search | *Object* `{query: 'query text' }`
| listings | *Array of Listing objects* A list of listings search by a query |
| videos | *Array of Video objects* A list of videos search by a query |
| webinarTopics | *Array of Webinar Topic objects* A list of webinar topics search by a query |
| page | *Page Object* Current page's details |

## Checkout Complete
`checkout-complete.liquid`

## Customer Dashboard
`dashboard.liquid`

Customer's personal dashboard showing purchases, upcoming/past bookings etc. User must be logged in to access.

Http Method: `GET`

Url:  `/dashboard`

JSON Endpoint:  `/dashboard.json`

| Attributes | --- |
| --- | --- |
| store | *Store Object* Store basic details |
| session | *Session Object* Current user's details |
| upcomingBookings | *Array of [Booking](#booking-object) objects* |
| pastBookings | *Array of [Booking](#booking-object) objects* |
| rentalVideos | *Array of Rental objects* |
| subscription | *Subscription object* or `null` if user does not have a VOD subscription. |
| packages | *Array of [Package Purchase](#package-purchase-object) objects* A list of packages which have been purchased by the user. |
| recordings | *Array of Recording objects* Recordings from 1-to-1 sessions. |
| rentalPlaylists | *Array of Playlist Rental objects* A list of playlists which have been purchased/rented by the user. |
| page | *Page Object* Current page's details |

### Booking Object

| Attributes | --- |
| --- | --- |
| startSessionUrl | *string* URL to start the live 1-to-1 session |
| id | *integer* Unique ID of the booking |
| listing | *[Listing](#listing) Object* |
| startTimeUTC | *string* |
| cancelBookingUrl | *string* URL to initiate a cancellation. Can be `null` if cancellation not allowed. |
| rescheduleBookingUrl | *string* URL to initiate a reschedule. Can be `null` if reschedule not allowed. |
| materialsUrl | *string* URL to view and download attached files / materials. |
| connectionType | *string* One of Video, DialIn, DirectContact, InOffice |
| customFieldResults | *Object* Dictionary of custom field results. |

### Package Purchase Object

| Attributes | --- |
| --- | --- |
| id | *integer* Unique ID of the Package Purchase |
| packageName | *string* |
| purchaseDate | *string* In UTC |
| status | *integer* 0: Closed, 1: Open  |
| items | *Array of Package Item Object* |

### Package Item Object

| Attributes | --- |
| --- | --- |
| id | *integer* Unique ID of the Item in the Package Purchase |
| isBooked | *boolean* |
| listing | *[Listing](#listing) Object* |

#### Liquid Example to redeem package item:

```liquid
{% for package in packages %}
    Package: {{ package.packageName | escape }} purchased {{ package.purchaseDate | local_time }}
    {% for item in package.items %}
    	{% if item.isBooked == false %}
	    {{ item.listing.image220Url | img_tag:item.listing.title }} 
	    {{ item.listing.title | escape }}
	    <button class="make-booking" data-listing="{{ item.listing.id }}" data-from-package="{{ package.id }}">Book Now</button>    
    	{% endif %}
    {% endfor %}
{% endfor %}
<script src="/bookingplugin.js"></script>
```

#### Javascript Example to redeem package when embedded on external site:

```html
<script src="https://yourstore.yondo.com/bookingplugin.js"></script>
<script> 
	// Listing ID and Package Purchase ID must be known. 
	// eg AJAX GET request to https://yourstore.yondo.com/dashboard.json
	var listingId = 1234;
	var fromPackage = 5678;
	bookingcontrol.ShowBookingCalendar(listingId, fromPackage);
</script>
```

## Listing Materials
`materials.liquid`

## Login
`login.liquid`

## Forgot Password
`forgot-password.liquid`

## Reset Password
`reset-password.liquid`

## Package
`package.liquid`

Landing page for a Package of 1-to-1 sessions.

Http Method: `GET`

Url:  `/package/{package-name}/{id}` 

JSON Endpoint:  `/package/{package-name}/{id}.json` {package-name} is included to be SEO friendly. It can be anything. Only the ID is used.

| Attributes | --- |
| --- | --- |
| store | *Store Object* Store basic details |
| session | *Session Object* Current user's details |
| package | *[Package](#package-object) Object* |
| page | *Page Object* Current page's details |

## Custom Page
`page.liquid`

## Video Search
`video-search.liquid`

## Session add to calendar
`session-calendar.liquid`

## Cancel Booking
`cancel-booking.liquid`

## Video on demand 
`video.liquid`

Contains a single video provided by a store.

Http Method: `GET`

Request URL Pattern: `/video/{name}/{id}`

`{name}`: the name of a video (url safe)
`{id}`: A unique integer identifier of a video

> `{name}` are not validated and can essentially contain any string. They are part of the URL for SEO purposes.

| Attributes | --- |
| --- | --- |
| store | *Store Object* Store basic details providing a listing |
| session | *Session Object* Current user's details |
| video | *Video Object* the video |

## Webinar
`webinar-topic.liquid`

Contains a single webinar topic provided by a store.

Http Method: `GET`

Request URL Pattern: `/webinar/{name}/{id}`

`{name}`: the name of a webinar topic (url safe)
`{id}`: A unique integer identifier of a video

> `{name}` are not validated and can essentially contain any string. They are part of the URL for SEO purposes.

| Attributes | --- |
| --- | --- |
| store | *Store Object* Store basic details providing a listing |
| session | *Session Object* Current user's details |
| webinarTopic | *Webinar Topic Object* the webinar topic |

## Webinar Access
`webinar-access.liquid`

## Webinar Attachments
`webinar-attachments.liquid`

## Embed Video
`embed-video.liquid`

## Empty Base
`empty-base.liquid`

## Live Session Test
`live-session-test.liquid`

## Recording
`recording.liquid`

## Reschedule Booking
`reschedule-booking.liquid`

## Session Archive
`session-archive.liquid`



# Public API - JSON endpoints

A JSON data feed is made available for all information shown in your Yondo store website. Use this to fetch data from external services. 

Accessing the API is simple. Use a http `GET` request with `Accept: application/json` header to any public page on your Yondo store website. Alternatively, add `.json` suffix to any URL on your Yondo store website.

> You can browse your store website and when you find the product which you want as JSON data, add `.json` to the end of the page you are requesting to view the JSON data in your browser. eg http://yourstore.yondo.com/.json .

> Note: The JSON endpoint is currently restricted for Cross Origin domains. Speak to our support staff or dev team if you require access to the API from Cross Origin domains.

## Resources

### Products List (Home)
`https://{subdomain}.yondo.com/`

This lists all published Products and Team Members.

Example Response (truncated):
```javascript
{
	"listings": [
		{
			"duration": 30.0,
			"availability": true,
			"materials": [],
			"portfolios": [],
			"teamMember": {
				"profileDescription": "... profile text",
				"country": "United States of America",
				"qualificationsHtml": "",
				"location": "New York, United States of America",
				"bannerImageUrl": null,
				"bookingCountAllTime": 0,
				"bookingCount30Days": 0,
				"id": 1234,
				"firstName": "John",
				"lastName": "Smith",
				"fullName": "John Smith",
				"displayName": "jsmith",
				"url": "/jsmith",
				"profileImageUrl": "..image url",
				"profileThumbUrl": "..image url",
				"profileThumb32Url": "..image url"
			},
			"category": "Consulting",
			"type": "Listing",
			"id": 1234,
			"title": "This is the session title",
			"price": 400.0,
			"image220Url": "..image url",
			"image360Url": "..image url",
			"image580Url": "..image url",
			"url": "/consulting/This-is-the-session-title/1234",
			"descriptionHtml": "... session description",
			"prerequisiteHtml": null,
			"published": true
		}
	],
	"videos": [
		{
			"id": 1234,
			"title": "Double Digit Growth Goals",
			"descriptionHtml": "Lorem ipsum dolor sit amet",
			"author": "John Smith",
			"tags": [
				"Growth",
				"Goals"
			],
			"uploadedDate": "2018-03-15T18:19:09.517",
			"duration": 360,
			"thumbnail220Url": "..image url",
			"thumbnail360Url": "..image url",
			"thumbnail720Url": "..image url",
			"url": "/video/double-digit-growth-goals/1234",
			"rentalPrice": 5.0,
			"rentalPeriod": 2,
			"rentalAvailable": true,
			"subscriptionPrice": 10.95,
			"subscriptionAvailable": false,
			"subscribeAllVideos": true,
			"previewVideoUrl360p": null,
			"fileAttachments": [],
			"published": true,
			"purchasePrice": null,
			"purchaseAvailable": false,
			"availablePurchaseOptions": [
				"rent"
			]
		}
	],
	"webinarTopics": [
		{
			"id": 1234,
			"title": "Growing Your Business",
			"descriptionHtml": "Lorem ipsum dolor sit amet",
			"imageSrc": "..image url",
			"imageThumbSrc": "..image url",
			"duration": 75,
			"price": 0.00,
			"url": "/webinar/growing-your-business/1234",
			"upcoming": [
				{
					"id": 12345,
					"name": "Tech Buzz",
					"startTimeUtc": "2020-12-31T00:00:00Z",
					"maxAttendees": 1000,
					"currentAttendees": 2
				}
			],
			"attachments": []
		}
	],
	"teamMembers": [
		{
			"profileDescription": "... profile text",
			"country": "United States of America",
			"qualificationsHtml": "",
			"location": "New York, United States of America",
			"bannerImageUrl": null,
			"bookingCountAllTime": 0,
			"bookingCount30Days": 0,
			"id": 1234,
			"firstName": "John",
			"lastName": "Smith",
			"fullName": "John Smith",
			"displayName": "jsmith",
			"url": "/jsmith",
			"profileImageUrl": "..image url",
			"profileThumbUrl": "..image url",
			"profileThumb32Url": "..image url"
		}
	]

}
```

### 1-to-1 Session detail
`https://{subdomain}.yondo.com/{category}/{title}/{id}`

This gives detailed data for a single 1-to-1 listing. Get the URL from the `url` field from the object in the `listings` array from the Products List.



Example Response (truncated):
```javascript

{
	"listing": {
		"duration": 30.0,
		"availability": true,
		"materials": [],
		"portfolios": [],
		"teamMember": {
			"profileDescription": "... profile text",
			"country": "United States of America",
			"qualificationsHtml": "",
			"location": "New York, United States of America",
			"bannerImageUrl": null,
			"bookingCountAllTime": 0,
			"bookingCount30Days": 0,
			"id": 1234,
			"firstName": "John",
			"lastName": "Smith",
			"fullName": "John Smith",
			"displayName": "jsmith",
			"url": "/jsmith",
			"profileImageUrl": "..image url",
			"profileThumbUrl": "..image url",
			"profileThumb32Url": "..image url"
		},
		"category": "Consulting",
		"type": "Listing",
		"id": 1234,
		"title": "This is the session title",
		"price": 400.0,
		"image220Url": "..image url",
		"image360Url": "..image url",
		"image580Url": "..image url",
		"url": "/consulting/This-is-the-session-title/1234",
		"descriptionHtml": "... session description",
		"prerequisiteHtml": null,
		"published": true
	},
	"upcomingAvaliability": [
		"2018-08-28T21:00:00Z",
		"2018-08-28T21:30:00Z",
		"2018-08-28T22:00:00Z",
		"2018-08-28T22:30:00Z",
		"2018-08-30T17:00:00Z"
	]
}

```

### Video On Demand detail
`https://{subdomain}.yondo.com/video/{title}/{id}`

This gives detailed data for a single video on demand title. Get the URL from the `url` field from the object in the `videos` array from the Products List.

If the video is free to view without purchasing, the video asset URLs will also be included.

Example Response (truncated):
```javascript

{
	"video": {
		"id": 1234,
		"title": "Double Digit Growth Goals",
		"descriptionHtml": "Lorem ipsum dolor sit amet",
		"author": "John Smith",
		"tags": [
			"Growth",
			"Goals"
		],
		"uploadedDate": "2018-03-15T18:19:09.517",
		"duration": 360,
		"thumbnail220Url": "..image url",
		"thumbnail360Url": "..image url",
		"thumbnail720Url": "..image url",
		"url": "/video/double-digit-growth-goals/1234",
		"rentalPrice": 5.0,
		"rentalPeriod": 2,
		"rentalAvailable": true,
		"subscriptionPrice": 10.95,
		"subscriptionAvailable": false,
		"subscribeAllVideos": true,
		"previewVideoUrl360p": null,
		"fileAttachments": [],
		"published": true,
		"purchasePrice": null,
		"purchaseAvailable": false,
		"availablePurchaseOptions": [
			"rent"
		]
	}
}

```

### Webinar detail
`https://{subdomain}.yondo.com/webinar/{title}/{id}`

This gives detailed data for a single webinar topic. Get the URL from the `url` field from the object in the `webinarTopics` array from the Products List.

Upcoming webinar sessions are listed in the `upcoming` field.

Example Response (truncated):
```javascript

{
	"webinarTopic": {
		"id": 1234,
			"title": "Growing Your Business",
			"descriptionHtml": "Lorem ipsum dolor sit amet",
			"imageSrc": "..image url",
			"imageThumbSrc": "..image url",
			"duration": 75,
			"price": 0.00,
			"url": "/webinar/growing-your-business/1234",
			"upcoming": [
				{
					"id": 12345,
					"name": "Tech Buzz",
					"startTimeUtc": "2020-12-31T00:00:00Z",
					"maxAttendees": 1000,
					"currentAttendees": 2
				}
			],
			"attachments": []
	}
}

```

{% endraw %}
