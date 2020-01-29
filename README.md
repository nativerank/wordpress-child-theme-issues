# NR Template Documentation  
  
The NR Template system references **data**, **variables**, and **helpers**, all using the `{{ mustache }}` syntax.  

You will use this system by creating `.hbs` files in the `yootheme_child/templates` folder and creating page templates that can be used throughout your site. 

These `.hbs` files have the same syntax as a normal `.html` file, but they also allow for using that `{{ mustache }}` syntax and some handy built-in helpers, as well as "partial" code fragments, to make your life as a developer easier.  
  
## Built-In Helpers 

### `{{image}}`

Acts like a function that accepts two attributes:  
  
`id`  
`uk-cover`  
  
This helper returns the html image tag of the image provided in the `id` attribute.  

`uk-cover` adds **[UIkit 3's uk-cover component](https://getuikit.com/docs/cover)** to the image tag.  
  
#### Usage

_Input_

`<div>{{image id=215}}</div>`

_Output_

`<img width="768" height="513" data-src="/wp-content/image-large.jpeg" uk-img`
`data-srcset="/wp-content/image.jpeg 765w, /wp-content/image-large.jpeg 1400w" />`

**-----**

### `{{imageSrc}}`

Returns the url/src of the image provided in the `id` attribute.

#### Usage
_Input_

`<div data-src="{{imageSrc id=215}}" uk-img></div>`

_Output_

`<div data-src="/wp-content/image-large.jpeg" uk-img></div>`

**-----**
### `headerImage`

Variable, returns the featured image ID of the current page. 
Can be used with the [`{{image}}`](#image) and [`{{imageSrc}}`](#imagesrc) helpers.

**-----**
### `{{getImageFromPageId}}`

Returns the image tag of the featured image of the post [ID] provided in the id attribute. Accepts two parameters:

Post/Page `id`

`uk-cover`

**-----**
### `{{getPermalink}}`

Returns URL/Permalink of the page [ID] provided.

Only accepts page id without attributes  `{{getPermalink PAGE_ID}}`

#### Usage
`<a href="{{getPermalink 125}}">Fire Damage Services</a>`

**-----**
### `{{label}}`

Returns the current page label.

**-----**
### `{{h1}}`

Returns the H1 for the current page

**-----**
### Content

#### `{{firstSection}}`

Returns the first/intro section of the content.

#### `{{content}}`

Returns an array of objects, each corresponding to a section of content, containing two variables  `h2`  &  `p`. 

Can be used independently and with an [`{{each}}`](#each) loop.

However, there is a default `each` loop partial for content built into the Master site already, so you generally won't need to write an `each` loop for content, you can just call that partial.

This partial will not include the `firstSection` of content, you'll have to explicitly call that section wherever you need it, but then the rest of your content will use the `{{> content-loop}}` [partial](#partials), or independently call each section (see second Usage section below).

#### Usage

_Using the default `each` loop partial_

`{{> content-loop}}`


_Independently_

`{{content.[_INDEX_].h2}}`,  `{{content.[_INDEX_].p}}`

```html
<div class="card">
    <h2>{{content.[1].h2}}</h2>
    <p>{{content.[1].p}}</p>
</div>
```

**-----**
### `{{bodyImage}}`

Returns an array of image IDs that you will have set within the Wordpress page editor. This is an indexed array, and individual image be used with the format:

`{{bodyImage.[index]}}`

For example, the first bodyImage would be:

`{{bodyImage.[1]}}`

You can use this with the [`image`](#image) or [`imageSrc`](#imagesrc) helpers:

`{{image id=bodyImage.[1]}}`

**-----**
### `{{count}}`

Accepts a context array and returns the number of items in that array. For example, you can get the number of [`children`](#children) of the current page:

`{{count children}}`

**-----**
### `{{sanitize}}`

Given a string, will return a 'sanitized' version of the string. Can be useful for things like auto-generating URLs based on an array of strings.

**-----**
### `{{post_parent}}`

Returns an array that contains two values from the current page's parent page:  `{{title}}`,  `{{permalink}}`

**-----**
### `{{children}}`

Returns an array of all the **children** pages of the current page.

#### Variables

Each child page object in the **children** array contains the following variables:

| Key | Value |
|---|---|
| post_title | Label of the page |
| image_id | Featured Image ID |
| post_excerpt | First 120 characters of the content |
| permalink | URL/permalink of the page |

#### Usage

_Check if children exist_

Use handlebars 
`{{#if}` `{{/if}}` [block helper](#block-helpers) to identify whether the current page has children pages.

```html
{{#if children}}
    <div>
     	<!-- Display each child's post title, if children exist-->
        <h1>{{post_title}}</h1>
    </div>
{{/if}}
```


_Usage with an `{{#each}}` loop_

Use handlebars `{{#each}}` `{{/each}}` [block helper](#block-helpers) to loop through all the children pages.

```html
{{#if}}
    <section>
        <div class="container">
	    {{#each children}}
                <a href="{{permalink}}" class="card">
                    {{image id=image_id}}
            	    <h2>{{post_title}}</h2>
            	    <p>{{post_excerpt}}</p>
                </a>
            {{/each}}
     	</div>
    </section>
{{/if}}
```

**-----**
### `{{relatedPages}}`

Returns an array of all the neighbors of the current page, as well as their children. You can exclude the children by using an `exclude` property with the value `grand`, e.g. `{{relatedPages exclude=grand}}`

**-----**
## Structured Data

The SEO team inputs a set of "structured data" into the Native Rank plugin, and this is used to ensure our clients' business information is accurately displayed on the site. This data is all available to you as an array, returned by using the `{{structuredData}}` helper, with a number of different keys available within to access different information within the structured data.

If you want to view all the keys available to you, you could do this in a template as a test:

```html
{{#each structuredData}}
    {{@key}}
{{/each}}
```

This will spit out all the 'keys' within the structuredData array.

_See [Block Helpers](#block-helpers) for more on what that `#each` means and how to use it_

**-----**
### Built-In Structured Data Helpers

There are a number of built-in helpers that provide a more shorthand way of accessing the more important and commonly used structured data keys, e.g. the contact info for the business or organization. 

It is **absolutely necessary** that you use these helpers **wherever possible**, to ensure you are not accountable for any inaccuracies in business information anywhere in the site, and that all business info is consistent wherever it is displayed on the site.


### `{{phone}}`

`{{phone.link}}`  ,  `{{phone.formatted}}`

Returns an array that contains the business phone number in two different formats;
tel:+10000000000 & (000) 000-0000.

_Use  `{{phone.raw}}`  to print out in this format ->  `+10000000000`_

**-----**
### `{{email}}`

Returns the primary business email from the structured data. 

**-----**
### `{{address}}`

Returns an array with keys for each part of a standard address.

**Address Keys**


**street** 

Returns the street portion of the address from structured data

**city** 

Returns the city from the structured data address

**state**

Returns the state from the structured data address in 2 letter format, e.g. 'CO'

**postalCode**

Returns the 5-digit postal or ZIP code from the structured data address

**Address Key Usage**

Your `address` keys can be used in this format:
`{{address.key}}`

For example, the full address in letter format, with a link to the client's Google My Business listing would look like this:

```html
<a href="{{mapURL}}" target="_blank" rel="nofollow noopener"
title="Get Directions to {{structuredData.businessName}}">
	{{address.street}}
	<br>
        {{address.city}}, {{address.state}} {{address.postalCode}}
</a>
```

_The above example is in the footer.hbs [partial](#partials), and contact.hbs  in the Master if you ever need it_

**-----**
### `{{mapURL}}`

Returns the link to the client's primary Google My Business listing location, should be used with `address`.

**-----**
## `{{city}}`

Most Native Rank sites will make use of service areas, which will be another "site" within the site that is more or less the same as the main site, but the SEO strategy will be "optimized" for another city or location. These areas will have special templates that follow the naming convention of adding `city` or `location` and a dash '-' as a prefix to the name of the templates for those page types, e.g. `location-frontpage.hbs` will be very similar to `frontpage.hbs` but with a few minor differences.

The `{{city}}` helper will return the name of the current city within your templates, this is one way that your templates will differ for service area pages.

### Secondary Navigation

All your service area pages will use the `{{> secondaryNav}}` [partial](#partials) that will add navigation for that "site" below the main navigation bar. You won't need to worry about how this works or edit that partial too much, just ensure that `{{> secondaryNav}}` is called at the **very** top of all your `city` or `location` templates.


## Block Helpers

**Block helpers** allow you to write loops or conditionals for your templates. You saw in the **Content** and **Children** sections above, how you can loop over content with an `{{#each}}` helper, you can use these types of helpers in other ways as well. 

One (simplified) example that you will use a lot is to loop over the [`{{children}}`](#children) of the page:

```html
{{#each children}}
    <p>{{post_title}}</p>
{{/each}}
```

**-----**
### Built-In Block Helpers

### `{{#if}}`

This is a simplified version of an `boolean if` statement, it allows you to check if a variable or helper has a value, no other conditions allowed. Returns **true** or **false** based on if a variable or a helper contains some type of data.

`{{#if}}` `{{/if}}`

This helper allows for an `else` block, which runs if the `if` condition returns **false**.

#### Usage

```html
{{#if children}}
    <div>
     	<!-- Display each child's post title, if children exist-->
        <h1>{{post_title}}</h1>
    </div>
{{else}}
    <div>
	 <!-- Display each child's post title, if children exist-->
	 <h1>No children!</h1>
    </div>
{{/if}}
```

#### Shorthand
This helper can also be used with a shorthand syntax, using the name of the variable or helper in place of `if`. 
The example below will achieve the same result as the one above:

```html
{{#children}}
    <div>
     	<!-- Display each child's post title, if children exist-->
        <h1>{{post_title}}</h1>
    </div>
{{else}}
    <div>
	 <!-- Display each child's post title, if children exist-->
	 <h1>No children!</h1>
    </div>
{{/children}}
```

**-----**
### `{{#_if}}`

This is a more powerful `if` statement block helper, that allows you to select from a set of pre-determined operators, and set your own values for the conditional.

#### Usage

`{{#_if value 'operator' context}}` `{{/_if}}`

Note that the operator is in quotes, this is necessary or you will throw an error.

This helper also allows for the use of an `else` block, just like `if`.

#### Operators

This helper accepts the operators in the table below.
Each operator returns true if the condition in the "functionality" column is met.

| Operators | Functionality |
|---|---|
| < | Value is less than context |
| > | Value is greater than context |
| <= | Value is less than OR equal to context |
| >= | Value is greater than OR equal to context |
| != | Value is not equal to context |
| == | Value is equal to context |
| contains | (String) context contains value  |
| contains_in | (Array) context contains value |
| !contains_in | (Array) context does not contain value |

#### Example

```html
{{#_if label 'contains_in' 'Residential;Commercial'}}
    <p>{{label}}</p>
{{/_if}}
```

**-----**
### `{{#each}}`

This helper will loop over an array, which you provide as the **context** for the helper, and perform a set of commands that you specify, for **each** item in the array, or iteration.

This helper also allows for an `else` block, like the `if` and `_if` helpers, which runs if the context array is empty.

#### Usage

`{{#each context}}` `{{/each}}`

```html
{{#each children}}
    <p>{{post_title}}</p>
{{/each}}
```

#### `{{.}}`

If you loop over an object, you will use that object's `keys` to display data within the loop, but if you loop over an array without keys, you may need to reference the current item, which can be done with the `{{.}}` variable.

#### `{{@key}}`

`@key` will reference the key or index of the current iteration of your `each` loop. 

#### `{{@root}}`

`@root` will allow you to "escape" the context of your `each` loop and access data that is at the "root" level, or in many cases, at the "page" or "template" level. For example, if you're looping over an array of objects that each have the key `label`, but you need the `label` of the current _page_ and not the label of the current _object_, you would write something like this:

`{{@root.label}}`

**-----**
### `{{#filter}}`

This helper works like a combination of `each` and `_if`. It loops over a **context** array, and checks a specified **key** within each item against a **value** or **value** set, using an **operator** you select from a set list. If the condition is met for that item, it runs the code inside the block.

#### Usage

`{{#filter context key 'operator' value}}` `{{/filter}}`

Just like the `_if` helper, the operator is in quotes, this is necessary or you will throw an error.

You can use a page ID as the **context** for this helper, and the context will resolve to the `children` of the page ID that you supply as context.

#### Operators

This helper accepts the operators in the table below. Each operator returns true if the condition in the "functionality" column is met.

| Operators | Functionality |
|---|---|
| contains | Key contains value or 'any of the set of values' |
| does_not_contain | Key contains value or ' ' |
| exactly_matches | Key matches value or ' ' |
| does_not_exactly_match | Key does not match value or ' ' |
| starts_with | Key starts with value or ' ' |
| does_not_start_with | Key does not start with value ' ' |
| ends_with | Key ends with value or ' ' |
| does_not_end_with | Key does not end with value or ' ' |

#### Example

```html
{{#filter children key='post_title' 'contains' value='Residential;Commercial'}}
   <p>{{post_title}}</p>
{{/filter}}
```

#### `{{.}}`

If you `filter` an object, you will use that object's `keys` to display data within the loop, but if you `filter` an array without keys, you may need to reference the current item, which can be done with the `{{.}}` variable.

#### `{{@key}}`

`@key` will reference the key or index of the current iteration of your `filter` loop. 

#### `@root`

`@root` will allow you to "escape" the context of your `filter` loop and access data that is at the "root" level, or in many cases, at the "page" or "template" level. For example, if you're looping over an array of objects that each have the key `label`, but you need the `label` of the current _page_ and not the label of the current _object_, you would write something like this:

`{{@root.label}}`

## Custom Data and JSON

The NR Template system has a lot of use for storing custom data in JavaScript Object Notation, or JSON. The Master site comes with a file in the `yootheme_child` folder called `data.json`, and if you store data in JSON format inside this file, it will become globally available for your site within your templates.

This will make it very easy for you to create custom arrays or objects of whatever useful data you need to access in your templates. 

### Usage

First you'll need to set up JSON with the data that you want to use. Here are the default contents of the `data.json` file from the Master site that we'll use for an example.

```json
{
  "name": "Custom Data",
  "author": "Native Rank",
  "demo_key": "demo_value",
  "placeholder": "/wp-content/themes/yootheme_child/src/nativerank/assets/placeholder.jpg",
  "map": {
    "lat": 39.7392364,
    "long": -104.9848623,
    "zoom": 10
  },
  "forms": [
    {
      "page_id": 667,
      "form_id": 1
    }
  ],
  "cta": {
    "text": "<span class='uk-text-middle'>Get A Free Estimate</span>",
    "icon": "<span uk-icon='check-circle' class='uk-margin-small-right'></span>",
    "page_id": 677
  },
  "service_areas": [
    {
      "name": "Denver",
      "page_id": 0,
      "loc": {
        "lat": 39.7392364,
        "long": -104.9848623
      }
    }
  ]
}
```

Once you've set up valid JSON with your data in it, you can access it like so:

`{{data.yourkey}}`


#### Examples

#### `each` loop

```json
"service_areas": [
    {
      "name": "Denver",
      "page_id": 0,
      "loc": {
        "lat": 39.7392364,
        "long": -104.9848623
      }
    }
  ]
```

For example, let's say we wanted to loop over the **service_areas** array of objects, and link to the **page_id** for each service area, and display its **name** as the link text. Copied above is a snippet from the full Master example file, containing that **service_areas** array.
This is how you would accomplish that:

```html
{{#each data.service_areas}}
    <a href="{{getPermalink page_id}}">{{name}}</a>
{{/each}}
```

In our example `data.json` from the master site, there's only one service area, but if we just added more objects, or sets of curly brackets with our necessary **keys**, inside the `service_areas` array of objects, it would display all of the `service_areas` objects with our `each` loop code above.

#### Single object

```json
  "cta": {
    "text": "<span class='uk-text-middle'>Get A Free Estimate</span>",
    "icon": "<span uk-icon='check-circle' class='uk-margin-small-right'></span>",
    "page_id": 677
  }
```
Another example from the example JSON above (snippet copied from the full Master example file with just our object for reference) is creating a _CTA_ using the `cta` object. 
That would look something like this:

```html
<a href="{{getPermalink data.cta.page_id}}">{{data.cta.icon}} {{data.cta.text}}</a>
```

Notice there's no `each` loop there because we're just using a single object, not an array of object, and we're _directly_ accessing the **keys** for that object within each of our `{{ mustaches }}`. 

Feel free to find other ways to use JSON to your advantage, use your imagination as this is a _very_ powerful tool at your disposal!

### Forms and JSON

We use Gravity Forms to handle all html form creation, delivery, etc. These forms typically use shortcodes for embedding in pages, but this won't work in Handlebars. So, we've found a way around this by using `data.json`. Copied below for reference, the Master site's `data.json` file has a `forms` array of objects, that you'll modify to add forms, and add them to different pages.

```json
"forms": [
    {
      "page_id": 667,
      "form_id": 1
    }
  ]
```

Notice the default object included has a `page_id` key and a `form_id` key, these are the two keys you need to make use of a `forms` object. The `form_id` you'll get from Gravity Forms, if you go to the full list of forms it shows each form's ID in one of the columns. Then you'll get the Wordpress page ID and add your object. Once the JSON is set up, you'll just add this helper:

** `{{getForm}}` **

That will use the `page_id` and `form_id` pair to determine if there's a `form_id` set up in JSON that corresponds with the `page_id` in which you're using the `getForm` helper, and display the appropriate form.

## Partials

**Partials** will be a very important part of your templates. There are a number of built in partials for you to use in the **master** site, but you can create your own.
Think of these as your own **custom helpers** that will cut down on repetition, as well as widespread bugs or errors. 

To create your own, you'll create an `.hbs` file in the `templates/partials` directory just like you create templates in templates directory, but they're able to be "called" inside your templates, or even inside other partials, to add in blocks of code that you've already written and want to reuse.

There are a lot of possibilities, so use your imagination and see what ways you can come up with to utilize **partials** and make your code more streamlined and organized!

**-----**
### Usage

Let's say you have a file in the partials folder called `example-partial.hbs`.

You would then reference your partial like so:

`{{> example-partial}}`

This will compile and insert all the code found in example-partial.hbs,
 into wherever you call `{{> example-partial}}`

#### Partial Folders
You can also create one level of folders within `templates/partials` to organize your partials. For example, you could organize all your body content section partials in a folder titled `body-content/`, and then number each section's partial, like so:

- `body-content/`
	- `  1.hbs`
	- `  2.hbs`
	- `  3.hbs`

This will change your calls slightly, as you have to include the folder name in your partial call, using 'dot' notation syntax, with the format `{{> folder.partial}}`.
Below is how you would call the `2.hbs` partial inside the folder `body-content/`:

`{{> body-content.1}}`

#### Properties
You can pass properties, or options, into partial "calls" and then reference their values within the partial like this:

*Input in example-partial.hbs*

`<p>{{someData}}</p>`

*Input for the partial "call" in some other template file or partial*

`{{> example-partial someData="foo"}}`

*Output*

`<p>foo</p>`


## Icons  (in progress)
  
### Usage  
  
```html  
<span uk-icon="phone"></span>  
<span uk-icon="icon: phone; ratio: 2"></span>  
```  
> Learn more about the [UIkit Icon](https://getuikit.com/docs/icon) component  
  
### Library  
  
|   |   |  |  |  |  
|---|---|---|---|---|  
| phone | phone-in-talk | -- | check-circle | location |  
|  ![Phone](./icons/phone.png) |  |  |  |  |  
| clipboard | chevron-right | chevron-left | chevron-down | chevron-up |  
|  |  |  |  |  
| truck-fast |  
|  |

# Contribution  
  
## To-do List  
  
- [x] init Commit  
- [ ] Download Black .24-PNG Icon images from  https://materialdesignicons.com/  
- [ ] Link Icon images in the [Library table](#library)  
- [ ] Summary for each section  
  
## Resources  
  
- [Old Documentation](https://nativerank.dev/docs/)  
