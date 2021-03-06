# Metafields

Metafields are powerful components that can be added to Objects and Object Types. Metafields added to Object Types will be default for all new Objects in the type.

## Model

| Parameter     | Required | Type   | Description                                                                                                                               |
| ------------- | -------- | ------ | ----------------------------------------------------------------------------------------------------------------------------------------- |
| type          | required | Enum   | text, textarea, html-textarea, select-dropdown, object, objects, file, date, radio-buttons, check-boxes, repeater, parent, markdown, json, switch, number |
| title         | required | String | Your Metafield title |
| key           | required | String | Unique identifier for your Metafield                                                                                                      |
| value         |    varies   | Varies by type | Metafield value. Property required to be present (empty string ok) except for repeater and parent (should not be present). See example model below for various value types and requirements.                                                                                                                          |
| required      |          | Bool   | A value is required                                                                                                                       |
| regex         |          | String | Restrict the value to match a regular expresssion                                                                                         |
| regex_message |          | String  | The message displayed when the value fails the regex                                                                                    |
| minlength     |          | Number | Add minlength to text or textarea Metafields                                                                                              |
| maxlength     |          | Number | Add maxlength to text or textarea Metafields                             |
| options     | Required for options, radio, checkbox and switch | Varies by type | Array of options for select, radio, and checkbox Metafields and string for switch Metafield with possible values `true,false` and `yes,no` |
| children      |          | Array  | Add nested Metafields                                                                                                                     |

**Example Metafields**

```json
{
  "metafields": [
    {
      "type": "text",
      "title": "Headline",
      "key": "headline",
      "value": "3030 Palo Alto Blvd.",
      "required": true
    },
    {
      "type": "textarea",
      "title": "Basic Text",
      "key": "basic_text",
      "value": "This home is a must see!",
      "required": true
    },
    {
      "type": "html-textarea",
      "title": "Extended Text",
      "key": "extended_text",
      "value": "<p>Some <strong>HTML content</strong> for <em>dramatic</em> effect!</p>"
    },
    {
      "type": "markdown",
      "title": "Markdown Text",
      "key": "markdown_text",
      "value": "# Hello World!"
    },
    {
      "type": "select-dropdown",
      "title": "State",
      "key": "state",
      "value": "California",
      "options": [
        {
          "key": "CA",
          "value": "California"
        },
        {
          "key": "TX",
          "value": "Texas"
        }
      ]
    },
    {
      "type": "object",
      "title": "Pages",
      "key": "pages",
      "object_type": "pages",
      "value": "5a4806974fa85fc8a7000002" // Object ID
    },
    {
      "type": "objects",
      "title": "Other Listings",
      "key": "other_listings",
      "object_type": "listings",
      "value": "5a4806974fa85fc8a7000007,5a4806974fa85fc8a7000008" // Comma-separated Object IDs
    },
    {
      "type": "file",
      "title": "Hero",
      "key": "hero",
      "value": "media-name-property-in-bucket.jpg" // This is the name of your media. Gets converted to url and imgix_url automatically
    },
    {
      "type": "date",
      "title": "Listing Start Date",
      "key": "listing_start_date",
      "value": "2020-10-15"
    },
    {
      "type": "json",
      "title": "JSON Data",
      "key": "json_data",
      "value": {
        "strings": "cheese",
        "arrays": ["Bradbury","Charles","Ramono","the last Jedi","Liotta"],
        "objects": {
          "bools": true,
          "nestable": true
        }
      }
    },
    {
      "type": "radio-buttons",
      "title": "Deposit Required",
      "key": "deposit_required",
      "value": "The Other",
      "options": [
        {
          "value": "This"
        },
        {
          "value": "That"
        },
        {
          "value": "The Other"
        }
      ]
    },
    {
      "type": "check-boxes",
      "title": "Amenities",
      "key": "amenities",
      "value": [
        "Pool",
        "Gym"
      ],
      "options": [
        {
          "value": "Pool"
        },
        {
          "value": "Gym"
        },
        {
          "value": "Landscaping"
        }
      ]
    },
    {
      "type": "switch",
      "title": "This is great",
      "key": "this_is_great",
      "value": true,
      "options": "true,false"
    },
     {
      "type": "parent",  // ! No value !
      "title": "Call Out Section",
      "key": "call_out_section",
      "children": [
        {
          "type": "text",
          "title": "Headline",
          "key": "headline",
          "value": "You're Going to Love it Here!"
        },
        {
          "type": "html-textarea",
          "title": "Section Content",
          "key": "section_content",
          "value": "<p>You are going to <strong>love</strong> it in Cosmic Village. The best place to raise a team!</p>"
        },
        {
          "type": "file",
          "title": "Hero",
          "key": "hero",
          "value": "big-beautiful-image.jpg"
        }
      ]
    },
    {
      "type": "repeater", // ! No value !
      "title": "Testimonials",
      "key": "testimonials",
      "repeater_fields": [
        {
          "title": "Name",
          "key": "name",
          "value": "",
          "type": "text",
          "required": false
        },
        {
          "title": "Quote",
          "key": "quote",
          "value": "",
          "type": "text",
          "required": false
        }
      ],
      "children": [
        {
          "type": "repeating_item",
          "children": [
            {
              "type": "text",
              "title": "Name",
              "key": "name",
              "value": "Fiona Apple"
            },
            {
              "type": "text",
              "title": "Name",
              "key": "name",
              "value": "Jon Brion"
            }
          ]
        }
      ]
    }
  ]
}
```

## Validation

You can use optional validation parameters to make sure editors using the Web Dashboard enter the correct values.

Reference the [Metafield model](/rest-api/metafields.html) to learn more.

| Parameter     | Required | Type   | Description                                          |
| ------------- | -------- | ------ | ---------------------------------------------------- |
| required      |          | Bool   | A value is required                                  |
| regex         |          | String | Restrict the value to match a regular expresssion    |
| regex_message |          | String  | The message displayed when the value fails the regex |
| minlength     |          | Number | Add minlength to text or textarea Metafields         |
| maxlength     |          | Number | Add maxlength to text or textarea Metafields         |


**Example Metafields with Validations**

```json
{
  "title": "Users",
  "singular": "User",
  "slug": "users",
  "metafields": [
    {
      "key": "first_name",
      "title": "First Name",
      "type": "text",
      "value": "",
      "required": true,
      "minlength": 2
    },
    {
      "key": "last_name",
      "title": "Last Name",
      "type": "text",
      "value": "",
      "required": true,
      "minlength": 2
    },
    {
      "key": "email",
      "title": "Email",
      "type": "text",
      "value": "",
      "regex": "/^([a-z0-9_.-]+)@([da-z.-]+).([a-z.]{2,6})$/",
      "regex_message": "You must enter a valid email."
    },
    {
      "key": "avatar",
      "title": "Avatar",
      "type": "file",
      "value": ""
    },
    {
      "key": "tagline",
      "title": "Tagline",
      "type": "text",
      "value": "",
      "maxlength": 50
    }
  ]
}
```


## Connect Objects

You can connect Objects to create "one to many" and "many to many" relationships between Objects using Single and Multiple Object Metafields.

:::: tabs :options="{ useUrlFragment: false }"

::: tab Bash
**Definition**

```
POST https://api.cosmicjs.com/v1/:bucket_slug/add-object
```

**Example Request**

```json
{
  "title": "Blog Post Example",
  "type_slug": "blog-posts",
  "content": "This is some amazing blog content...",
  "metafields": [
    {
      "title": "Headline",
      "key": "headline",
      "type": "text",
      "value": "What I Learned Today"
    },
    {
      "title": "Author",
      "key": "author",
      "type": "object",
      "object_type": "authors",
      "value": "5a4ab6e0cf2289b18a2f7599"
    },
    {
      "title": "Categories",
      "key": "categories",
      "type": "objects",
      "object_type": "categories",
      "value": "5a4ab6e0cf2289b18a2f7599,5a49d524c1174db128ca2bce"
    }
  ]
}
```

:::

::: tab Node.js
**Definition**

```js
bucket.addObject()
```

**Example Request**

```js
const params = {
  title: 'Blog Post Example',
  type_slug: 'blog-posts',
  content: 'This is some amazing blog content...',
  metafields: [
    {
      title: 'Headline',
      key: 'headline',
      type: 'text',
      value: 'What I Learned Today'
    },
    {
      title: 'Author',
      key: 'author',
      type: 'object',
      object_type: 'authors',
      value: '5a4ab6e0cf2289b18a2f7599'
    },
    {
      title: 'Categories',
      key: 'categories',
      type: 'objects',
      object_type: 'categories',
      value: '5a4ab6e0cf2289b18a2f7599,5a49d524c1174db128ca2bce'
    }
  ]
}
const bucket = Cosmic.bucket({
  slug: 'bucket-slug',
  write_key: ''
})
bucket.addObject(params)
.then(data => {
  console.log(data)
})
.catch(err => {
  console.log(err)
})
```

:::

::::

### Single Objects

For a Single Object Metafield, add the Object ID (`_id`) to the value to connect the Object. The full Object will be returned on the Metadata response in the `object` property.

### Multiple Objects

For Multiple Object type Metafields, you can add the Object IDs as comma-separated values. The full Objects will be returned on the Metadata response in the `objects` property.

| Parameter   | Required | Type   | Description                                                                                    |
| ----------- | -------- | ------ | ---------------------------------------------------------------------------------------------- |
| type        | required | Enum   | object, objects                                                                                |
| title       | required | String | Your Metafield title                                                                           |
| key         | required | String | Unique identifier for your Metafield                                                           |
| object_type | required | String | Object Type slug                                                                               |
| value       |          | String | For single Object this is the \_id property. For multiple Objects it is comma-separated \_ids. |

## Edit Metafields

You can edit an existing Object's Metafields by using the following method. This method allows you to edit specific Metafields identified by `key`, without affecting other Metafields.

| Parameter     | Required | Type   | Description                                          |
| ------------- | -------- | ------ | ---------------------------------------------------- |
| slug      |          | String   | Object Slug                                  |
| metafields         |          | Array | Array of new or existing Metafields to add or edit. Required properties are `{ type, key, title }`    |

:::: tabs :options="{ useUrlFragment: false }"

::: tab Bash
**Definition**

```
PATCH https://api.cosmicjs.com/v1/:bucket_slug/edit-object-metafields
```

**Example Request**

```json
{
  "slug": "my-object",
  "metafields": [
    {
      "title": "Headline",
      "key": "headline",
      "type": "text",
      "value": "What I Learned Today"
    },
    {
      "title": "Subheadline",
      "key": "subheadline",
      "type": "text",
      "value": "Something different"
    }
  ]
}
```

:::

::: tab Node.js
**Definition**

```js
bucket.editObjectMetafields()
```

**Example Request**

```js
const params = {
  slug: 'my-object',
  metafields: [
    {
      title: 'Headline',
      key: 'headline',
      type: 'text',
      value: 'What I Learned Today'
    },
    {
      title: 'Subheadline',
      key: 'subheadline',
      type: 'text',
      value: 'Something different'
    }
  ]
}
const bucket = Cosmic.bucket({
  slug: 'bucket-slug',
  write_key: ''
})
bucket.editObjectMetafields(params)
.then(data => {
  console.log(data)
})
.catch(err => {
  console.log(err)
})
```

:::

::::
