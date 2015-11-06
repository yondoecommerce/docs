{% raw %}

# Template Development #

## What is a Yondo Template? ##
Included with a [Yondo](http://www.yondo.com) account is a *hosted store website*. This provides a front-end for your customers to use when browsing your live session listings, and video-on-demand products. A highly customizable template engine provides the web content for this front-end. Every Yondo *store* uses either a standard template, or a custom installed template. **A template is basically a collection of html, css and other standard files you would find on a website.**

## How do I develop a Template? ##
Go to the [Yondo Partner Network](http://partners.yondo.com/signup) and sign up for a developer account. Here you can create and upload your template, and create a development store to test your template. The editing functions are quite basic, most developers create their template externally and just upload when done.

###My template is empty, how do I get started?###
[Download a Zip](https://github.com/yondoecommerce/classic-template/archive/master.zip) of the [sample classic template](https://github.com/yondoecommerce/classic-template) and upload it to the *files* section of your template. This will load the template with the files from the sample template.

## Template Structure ##
A template is made up of template files. These are regular html/liquid files, css, js etc. Templates which require server side rendering have a `.liquid` extension and use the [Liquid template engine](https://github.com/Shopify/liquid/wiki). 

### Root directory `/` ###
The root directory contains all of the `.liquid` files. The names of the `.liquid` files must match the page to be rendered. See each page section for more information.

**Standard template files:**

- base.liquid

Pages:

- cancel-booking.liquid
- checkout-complete.liquid
- dashboard.liquid
- forgot-password.liquid
- home.liquid
- listing.liquid
- login.liquid
- materials.liquid
- package.liquid
- page.liquid
- profile.liquid
- reset-password.liquid
- search.liquid
- session-calendar.liquid
- video-search.liquid
- video.liquid

Partials:

- custom-fields.liquid
- listing-card.liquid
- portfolio.liquid
- video-player-scripts.liquid
- video-player.liquid
- video-purchase-partial.liquid

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

	<script>
		var listing = {{ listing | json }};
	</script>
Outputs:

    <script>
    	var listing = { name:'sample', price:25 };
    </script>


# Template Data
The  following data objects are available on different template pages. 

## Store 

This is an object to represent a store. It contains store information about what is the store's name, logo, header color, alias Url, subdomain Url and setting.

| Attributes | --- |
| --- | --- |
| id | *integer* An unique integer identifier of a store |
| name | *string* The name of a store |
| logo | *string* The logo URL of a store |
| headerColor | *string* Header color of a store |
| aliasUrl | *string* Alias URL of a store |
| subdomainUrl | *string* Subdomain URL of a store |
| setting | *Object* Additional settings enabled for the store. |

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

## Session

This is an object to represent current user's session. FirstName, LastName, DisplayName and ProfileImage are only shown when a user is logged in.

| Attributes | --- |
| --- | --- |
| loggedIn | *boolean* Login status of an user. It is true if an user is already logged in |
| firstName | *string* First name of an user. It is null if an user is not logged in |
| lastName | *string* Last name of an user. It is null if an user is not logged in |
| displayName | *string* An unique display name of an user. It is null if an user is not logged in |
| profileImage | *string* The URL of an user's profile image. It is null if an user is not logged in |

## Listing

This is an object to represent a live session listing. It represents a single class/consultation for a specific duration and price.

| Attributes | --- |
| --- | --- |
| id | *integer* An unique integer identifier of a listing |
| title | *string* A title of a listing |
| smallImageUrl | *string* The URL of a listing image size 220px x 165px |
| bigImageUrl | *string* The URL of a listing image size 360px x 270px |
| price | *integer* Price of a listing in dollar per duration |
| duration | *integer* Duration of a listing |
| shortDescription | *string* Short description of a listing |
| descriptionHtml | *string* Description of a listing. It contains html content. |
| prerequisiteHtml | *string* A listing is required to be taken before taking a listing. It contains html content |
| url | *string* Relative URL of a listing |
| tags | *string* |
| availability | *boolean* Availability of a listing |
| materials | *Array of Material objects* A list of listing materials |
| portfolios | *Portfolio object* A portfolio for the listing |
| teamMember | *Team Member object* Details of a team member providing a listing |
| category | *string* The name of category that a listing is classified |

## Material

This is an object to represent a material attached to a listing. eg worksheet or file download.

| Attributes | --- |
| --- | --- |
| id | *integer* An unique integer identifier of a material |
| fileName | *string* Actual file name of a material |
| name | *string* The name of a material |
| description | *string* Description of a material |
| downloadUrl | *string* Download URL of a material. Download URL is only provided when an user booked a listing |

## Paging Information

This is an object to represent paging information for the current result set.

| Attributes | --- |
| --- | --- |
| number | *integer* Current page number |
| size | *integer* The number of results to be returned per page |
| totalResults | *integer* The total number of results |

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


## Search Results
`search.liquid`

Search results page

Http Method: `GET`

Url Patterns:  

* `/search/{query}?page={number}`
* `/search?query={query}&page={number}`

`{query}`: keyword used to search listings
`{number: integer)`: page number (if page number is not set, it returns the first page of the results)



| Attributes | --- |
| --- | --- |
| store | *Store Object* Store basic details |
| session | *Session Object* Current user's details |
| search | *Object* `{query: 'query text' }`
| listings | *Array of Listing objects* A list of listings in a category |
| page | *Page Object* Current page's details |

## Checkout Complete
`checkout-complete.liquid`

## Customer Dashboard
`dashboard.liquid`

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

{% endraw %}
