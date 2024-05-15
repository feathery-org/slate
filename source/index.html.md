---
title: Feathery API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - python
  - javascript

toc_footers:
  - <a href='https://feathery.io'>Sign up for an API key</a>

includes:
  - errors

search: true

code_clipboard: true
---

# Introduction

The Feathery API is organized around REST. Our API has predictable resource-oriented URLs, accepts form-encoded request bodies, returns JSON-encoded responses, and uses standard HTTP response codes, authentication, and verbs.

You can use our API to access and modify your forms, fields, and users. You can leverage our [React library](https://github.com/feathery-org/feathery-react) for much of the same functionality.

# Authentication

Provide your Feathery admin API key to authenticate access.
You can get an API key by creating an [account](https://app.feathery.io). 

API keys are environment-specific (production, development, etc.), which allows you to
separate real from test data. All API keys have access to the same fields and forms,
but users are environment-specific.

Include your API key as a request header that looks like the following:

`Authorization: Token <API KEY>`

<aside class="notice">
You must replace <code>&lt;API KEY&gt;</code> with your personal API key.
</aside>

# Account

## Retrieve Account Information

```python
import requests

url = "https://api.feathery.io/api/account/";
headers = {"Authorization": "Token <API KEY>"}
result = requests.get(url, headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.io/api/account/" \
    -H "Authorization: Token <API KEY>"
```

```javascript
const url = "https://api.feathery.io/api/account/";
const options = { headers: { Authorization: "Token <API KEY>" } };
fetch(url, options)
    .then((response) => response.json())
    .then(result => console.log(result));
```

> The above command outputs JSON structured like this:

```json
{"team": "my-team-name"}
```

Retrieve your Feathery account information.

### HTTP Request

`GET https://api.feathery.io/api/account/`

### Response Parameters

The response will be an object containing the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
team | String | The name of your team in Feathery

# Documents

## Fill a PDF Document Template 

```python
import requests

url = "https://api.feathery.io/api/document/fill/";
headers = {"Authorization": "Token <API KEY>"}
data = {"document": <DOCUMENT ID>, "field_values": {<FIELD ID>: <FIELD VALUE>}}
result = requests.post(url, json=headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.io/api/document/fill/" \
    -X POST \
    -d "{'document': <DOCUMENT ID>, 'field_values': {<FIELD ID>: <FIELD VALUE>}" \
    -H "Authorization: Token <API KEY>" \
    -H "Content-Type: application/json"
```

```javascript
const url = "https://api.feathery.io/api/document/fill/";
const data = {document: '<DOCUMENT ID>', field_values: {'<FIELD ID>': '<FIELD VALUE>'}}
const headers = {
    Authorization: "Token <API KEY>",
    "Content-Type": "application/json"
};
const options = {
    headers, 
    method: 'POST',
    body: JSON.stringify(data)
};
fetch(url, options)
    .then((response) => response.json())
    .then(result => console.log(result));
```

> The above command outputs JSON structured like this:

```json
{"file_url": "URL TO FILE"}
```

Pass in data to fill out a PDF document template that you've 
uploaded and mapped in your Feathery account.

### HTTP Request

`POST https://api.feathery.io/api/document/fill/`

### Response Parameters

The response will be an object containing the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
file_url | String | The URL to the filled out PDF document

# Forms

## Retrieve a Form Schema

```python
import requests

url = "https://api.feathery.io/api/form/my_form/";
headers = {"Authorization": "Token <API KEY>"}
result = requests.get(url, headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.io/api/form/my_form/" \
    -H "Authorization: Token <API KEY>"
```

```javascript
const url = "https://api.feathery.io/api/form/my_form/";
const options = { headers: { Authorization: "Token <API KEY>" } };
fetch(url, options)
    .then((response) => response.json())
    .then(result => console.log(result));
```

> The above command outputs JSON structured like this:

```json
            {
  "form_id": "aSdsa5",
  "form_name": "My Form",
  "steps": [
    {
      "id": "Step 1",
      "origin": true,
      "images": [],
      "progress_bars": [],
      "texts": [],
      "buttons": [],
      "fields": [
        {
          "id": "field_key",
          "internal_id": "46d1ba89-0dcc-42cd-839c-a12ce6668347",
          "icon_url": "",
          "placeholder": "",
          "tooltipText": "",
          "submit_trigger": "none",
          "metadata": {},
          "display_text": "a",
          "format": "",
          "repeated": false,
          "repeat_trigger": "",
          "required": true,
          "type": "integer_field",
          "max_length": null,
          "min_length": null,
          "has_data": true
        }
      ],
      "previous_conditions": [],
      "next_conditions": [],
      "created_at": "2020-06-01T00:00:00Z",
      "updated_at": "2020-06-01T00:00:00Z"
    },
    {
      "id": "Step 2",
      "origin": false,
      "images": [],
      "videos": [],
      "progress_bars": [],
      "texts": [],
      "buttons": [],
      "fields": [],
      "previous_conditions": [],
      "next_conditions": [],
      "created_at": "2020-06-01T00:00:00Z",
      "updated_at": "2020-06-01T00:00:00Z"
    }
  ]
}
```

Retrieve the schema of a form created in Feathery.

### HTTP Request

`GET https://api.feathery.io/api/form/<form_id>/`

### Response Parameters

The response will be an object containing the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
form_id | String | Your form's ID
form_name | String | Your form's name
steps | Array<Obj> | An array of step objects

Each `steps` object contains the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
id | String | The ID of the step, unique to the form
origin | Boolean | Is this the first step of the form
images | Array<Obj> | Images on this step
videos | Array<Obj> | Videos on this step
progress_bars | Array<Obj> | Progress bars on this step
texts | Array<Obj> | Text elements on this step
buttons | Array<Obj> | Buttons on this step
fields | Array<Obj> | Fields on this step
previous_conditions | Array<Obj> | Navigation rules that connect previous steps to the current step
next_conditions | Array<Obj> | Navigation rules that connect the current step to next steps
created_at | Datetime | When this step was created
updated_at | Datetime | When this step was last updated

Each form element (images, videos, progress_bars, texts, buttons, fields) has a common `id` parameter that uniquely identifies it.


## Create a Form

```python
import requests

url = "https://api.feathery.io/api/form/";
headers = {"Authorization": "Token <API KEY>"}
data = {
    "form_id": "new_form_id",
    "template_form_id": "id_of_form_to_copy",
    "steps": [
        {
            "step_id": "name_of_new_step",
            "template_step_id": "step_to_copy",
            "origin": True,
            "fields": [
                {
                    "id": "field_to_change",
                    "field_id": "new_field_name"
                }
            ],
            "texts": [
                {
                    "id": "text_element_to_change",
                    "text": "New Text to replace copied text",
                }
            ],
            "buttons": [
                {
                    "id": "button_element_to_change",
                    "text": "New Button Text",
                }
            ],
            "images": [
                {
                    "id": "image_element_to_change",
                    "source_url": "https://new-image-to-replace-old.com",
                }
            ],
            "progress_bars": [
                {
                    "id": "name_of_progress_bar_to_change",
                    "progress": 10,
                }
            ],
        },
        {
            "step_id": "name_of_new_step_2",
            "template_step_id": "step_to_copy_2",
            "fields": [],
            "texts": [],
            "buttons": [],
            "images": [],
            "progress_bars": [],
        },
    ],
    "navigation_rules": [
        {
            "previous_step_id": "name_of_new_step",
            "next_step_id": "name_of_new_step_2",
            "trigger": "click",
            "element_type": "button",
            "element_id": "button_element_to_change",
            "rules": [
                {
                    "comparison": "equal",
                    "field_key": "",
                    "value": "",
                },
            ],
        },
    ],
}
result = requests.post(url, data=data, headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.io/api/form/" \
    -X POST \
    -d "{
        'form_id': 'new_form_id',
        'template_form_id': 'id_of_form_to_copy',
        'steps': [
            {
                'step_id': 'name_of_new_step',
                'template_step_id': 'step_to_copy',
                'origin': True,
                'fields': [
                    {
                        'id': 'field_to_change',
                        'field_id': 'new_field_name'
                    }
                ],
                'texts': [
                    {
                        'id': 'text_element_to_change',
                        'text': 'New Text to replace copied text',
                    }
                ],
                'buttons': [
                    {
                        'id': 'button_element_to_change',
                        'text': 'New Button Text',
                    }
                ],
                'images': [
                    {
                        'id': 'image_element_to_change',
                        'source_url': 'https://new-image-to-replace-old.com',
                    }
                ],
                'progress_bars': [
                    {
                        'id': 'name_of_progress_bar_to_change',
                        'progress': 10,
                    }
                ],
            },
            {
                'step_id': 'name_of_new_step_2',
                'template_step_id': 'step_to_copy_2',
                'fields': [],
                'texts': [],
                'buttons': [],
                'images': [],
                'progress_bars': [],
            },
        ],
        'navigation_rules': [
            {
                'previous_step_id': 'name_of_new_step',
                'next_step_id': 'name_of_new_step_2',
                'trigger': 'click',
                'element_type': 'button',
                'element_id': 'button_element_to_change',
                'rules': [
                    {
                        'comparison': 'equal',
                        'field_key': '',
                        'value': '",
                    },
                ],
            },
        ],
    }" \
    -H "Authorization: Token <API KEY>" \
    -H "Content-Type: application/json"
```

```javascript
const url = "https://api.feathery.io/api/form/";
const data = {
  "form_id": "new_form_id",
  "template_form_id": "id_of_form_to_copy",
  "steps": [
    {
      "step_id": "name_of_new_step",
      "template_step_id": "step_to_copy",
      "origin": True,
      "fields": [
        {
          "id": "field_to_change",
          "field_id": "new_field_name"
        }
      ],
      "texts": [
        {
          "id": "text_element_to_change",
          "text": "New Text to replace copied text",
        }
      ],
      "buttons": [
        {
          "id": "button_element_to_change",
          "text": "New Button Text",
        }
      ],
      "images": [
        {
          "id": "image_element_to_change",
          "source_url": "https://new-image-to-replace-old.com",
        }
      ],
      "progress_bars": [
        {
          "id": "name_of_progress_bar_to_change",
          "progress": 10,
        }
      ],
    },
    {
      "step_id": "name_of_new_step_2",
      "template_step_id": "step_to_copy_2",
      "fields": [],
      "texts": [],
      "buttons": [],
      "images": [],
      "progress_bars": [],
    },
  ],
  "navigation_rules": [
    {
      "previous_step_id": "name_of_new_step",
      "next_step_id": "name_of_new_step_2",
      "trigger": "click",
      "element_type": "button",
      "element_id": "button_element_to_change",
      "rules": [
        {
          "comparison": "equal",
          "field_key": "",
          "value": "",
        },
      ],
    },
  ],
};
const headers = {
    Authorization: "Token <API KEY>",
    "Content-Type": "application/json"
};
const options = {
    headers, 
    method: 'POST',
    body: JSON.stringify(data)
};
fetch(url, options)
    .then((response) => response.json())
    .then(result => console.log(result));
```

> The above command returns a 201 status on success

Create a form based off an existing template form. Any integrations on the template form will be copied over as well.

### HTTP Request

`POST https://api.feathery.io/api/form/`

### Body Parameters

Parameter | Type | Description
--------- | --------- | -----------
form_id | String | The name of the new form being created (must be unique)
template_form_id | String | The name of the template form to copy from
steps | Array<Obj> | An array of steps to create
navigation_rules | Array<Obj> | An array of navigation rule connecting steps to be created

Each `steps` object contains the following parameters.
The form element arrays allow you to edit attributes from the elements in the copied step.
If nothing is specified, the elements on the step will be copied as they are.

Parameter | Type | Description
--------- | --------- | -----------
step_id | String | The ID of the new step to create, unique to the form
template_step_id | Optional String | The ID of the step to copy from the original form. If not specified, a step will be auto-created with a single column layout.
origin | Boolean | `true` if this step is the first of the form
images | Array<Obj> | Image elements to edit on this step
videos | Array<Obj> | Video elements to edit on this step
progress_bars | Array<Obj> | Progress bar elements to edit on this step
texts | Array<Obj> | Text elements to edit on this step
buttons | Array<Obj> | Button elements to edit on this step
fields | Array<Obj> | Field elements to edit on this step

Each `image` object contains the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
id | String | The ID of the image to edit. If `template_step_id` is specified, it must be from that step.
source_url | String | A new image URL
asset | Optional String | The name of the image asset to inherit from. This asset must belong to the template form's theme.

Each `video` object contains the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
id | String | The ID of the video to edit. If `template_step_id` is specified, it must be from that step.
source_url | String | A new video URL
asset | Optional String | The name of the video asset to inherit from. This asset must belong to the template form's theme.

Each `progress_bar` object contains the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
id | String | The ID of the progress bar to edit. If `template_step_id` is specified, it must be from that step.
progress | Integer | The percentage of progress to display between 0 and 100
asset | Optional String | The name of the progress bar asset to inherit from. This asset must belong to the template form's theme.

Each `text` object contains the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
id | String | The ID of the text element to edit. If `template_step_id` is specified, it must be from that step.
text | String | The new text to display
asset | Optional String | The name of the text asset to inherit from. This asset must belong to the template form's theme.

Each `button` object contains the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
id | String | The ID of the button to edit. If `template_step_id` is specified, it must be from that step.
text | String | Text to display on the new button
asset | Optional String | The name of the button asset to inherit from. This asset must belong to the template form's theme.

Each `field` object may contain the following parameters, depending on what type of field you are copying or changing.

Parameter | Type | Description
--------- | --------- | -----------
id | String | The ID of the field to copy. If `template_step_id` is specified, it must be from that step.
field_id | Optional String | The ID to set for the field on the new step. If this ID references an existing field, it will automatically link to the field's properties and user data. If field_id isn't specified, it will be automatically generated.
type | Optional String | The new [field type](https://docs.feathery.io/platform/components/fields/button-group#example)
description | Optional String | The label / description of the field
required | Optional Boolean | Does the user need to fill out this field before submitting the step? Defaults to `true`
max_length | Optional Integer | Maximum length of the field value
min_length | Optional Integer | Minimum length of the field value 
placeholder | Optional String | New placeholder text for the field
tooltipText | Optional String | New tooltip text for the field
submit_trigger | Optional String | Does filling out the field trigger submission of the current step? Options are "auto" or "none"
metadata | Optional Object | Specifies field options and file types
asset | Optional String | The name of the field asset to inherit from. This asset must belong to the template form's theme.

Below we specify which parameters apply to which field type. They are only required if you aren't linking from an existing field.

Parameter | [Field Types](https://docs.feathery.io/platform/components/fields/button-group#example) | Required
--------- | --------- | -----------
max_length, min_length | text_field, text_area, integer_field, select, multiselect, email | Optional
max_length | pin_input | Required
placeholder, toolTipText | dropdown, email, login, phone_number, gmap_line_1, gmap_line_2, gmap_city, gmap_state, gmap_zip, integer_field, ssn, text_field, text_area, url | Optional
submit_trigger | button_group, file_upload, dropdown, gmap_state, pin_input, select, hex_color, login, phone_number, ssn | Optional
metadata.other | select, multiselect, dropdown | Required
metadata.options | button_group, select, multiselect, dropdown | Required
metadata.file_types | file_upload | Required

Each `navigation_rules` object controls how the user navigates between 2 steps. 
For example, one navigation rule might specify that clicking the button `Next` on `Step 1` leads to `Step 2`.

Navigation rules contain the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
previous_step_id | String | The step the user is coming from.
next_step_id | String | The step the user is going to.
trigger | String Enum | How navigation is trigger - "click", "change", or "load"
element_type | String Enum | What type of element triggers navigation - "button", "field", or "text"
element_id | String | The ID of the element on the previous step that triggers navigation. 
rules | Array<Obj> | The conditions that must be fulfilled for navigation to occur


Each `rules` object contains the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
comparison | String | Comparison - "equal", "not_equal"
field_key | String | The ID of the field whose value is being used for this comparison
value | String | The value to compare the field value against

## Create Form Submissions

```python
import requests

url = "https://api.feathery.io/api/form/submission/";
data = {"fields": {"age": 21}, "user_id": "alice@feathery.io", "forms": ["My Form"], "complete": True}
headers = {
    "Authorization": "Token <API KEY>",
    "Content-Type": "application/json",
}
result = requests.post(url, data=data, headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.io/api/form/submission/" \
    -X POST \
    -d "{'fields': {'age': 21}, 'user_id': 'alice@feathery.io', 'forms': ['My Form'], 'complete': True}" \
    -H "Authorization: Token <API KEY>" \
    -H "Content-Type: application/json"
```

```javascript
const url = "https://api.feathery.io/api/form/submission/";
const data = {"fields": {"age": 21}, "user_id": "alice@feathery.io", "forms": ["My Form"], "complete": true}
const headers = {
    Authorization: "Token <API KEY>",
    "Content-Type": "application/json"
};
const options = {
    headers, 
    method: 'POST',
    body: JSON.stringify(data)
};
fetch(url, options)
    .then((response) => response.json())
    .then(result => console.log(result));
```

> The above command outputs JSON structured like this:

```json
{
  "fields": {"age": 21},
  "user_id": "alice@feathery.io",
  "forms": ["My Form"],
  "complete": true
}
```

Set field values for a user and initialize form submissions

### HTTP Request

`POST https://api.feathery.io/api/form/submission/`

### Request Body Parameters

Parameter | Type | Description
--------- | --------- | -----------
fields | Object | A mapping from field IDs to associated field values to create. For a signature field, pass `{"file": <base64 string>, "name": <file name>}`
user_id | Optional String | A unique user ID. If not provided, a random user ID will be generated and returned
forms | Optional String[] | An array of form IDs to initialize submissions for
complete | Optional Boolean | A boolean value to indicate if this  submission will be set as a form completion. Default to false if not provided 

### Response Parameters
Same as request body parameters

## Retrieve All Form Submissions

```python
import requests

url = "https://api.feathery.io/api/field/batch/TestForm/";
headers = {"Authorization": "Token <API KEY>"}
result = requests.get(url, headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.io/api/field/batch/TestForm/" \
    -H "Authorization: Token <API KEY>"
```

```javascript
const url = "https://api.feathery.io/api/field/batch/TestForm/";
const headers = { Authorization: "Token <API KEY>" };
fetch(url, { headers })
    .then((response) => response.json())
    .then(result => console.log(result));
```

> The above command outputs JSON structured like this:

```json
[{
  "user_id": "alice@feathery.io",
  "last_submitted": "2020-06-01T00:00:00Z",
  "Field1": "Test",
  "Field2": "Test 2",
}]
```

Fetch all submission data for a particular form

### HTTP Request

`GET https://api.feathery.io/api/field/batch/<FORM ID>/`

### Request Query Parameters

Parameter | Type                | Description
--------- |---------------------| -----------
start_time | Datetime (Optional) | Fetch submissions from after this start time
end_time | Datetime (Optional) | Fetch submissions from before this end time

### Response Parameters
An array of submission entries

# Users

## List All Users

```python
import requests

url = "https://api.feathery.io/api/user/";
headers = {"Authorization": "Token <API KEY>"}
result = requests.get(url, headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.io/api/user/" \
    -H "Authorization: Token <API KEY>"
```

```javascript
const url = "https://api.feathery.io/api/user/";
const options = { headers: { Authorization: "Token <API KEY>" } };
fetch(url, options)
    .then((response) => response.json())
    .then(result => console.log(result));
```

> The above command outputs JSON structured like this:

```json
[
  {
    "id": "alice@feathery.io",
    "created_at": "2020-06-01T00:00:00Z",
    "updated_at": "2020-06-02T00:00:00Z"
  },
  {
    "id": "bob@feathery.io",
    "created_at": "2020-06-03T00:00:00Z",
    "updated_at": "2020-06-04T00:00:00Z"
  }
]
```

List all of your users (corresponding to form submissions) in Feathery.

### HTTP Request

`GET https://api.feathery.io/api/user/`

### Request Query Parameters

Parameter | Type | Description
--------- | --------- | -----------
filter_field_id | String (Optional) | The ID of a form or hidden field to filter users by.
filter_field_value | String (Optional) | The value of the field to filter on. Paired with `filter_field_id` to only return users who have this field value.

### Response Parameters

The response will be an array of objects with the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
id | String | Your unique user ID
created_at | Datetime | When this user was created
updated_at | Datetime | When this user was last updated

## List All Data for a User

```python
import requests

url = "https://api.feathery.io/api/field/?id=alice@feathery.io";
headers = {"Authorization": "Token <API KEY>"}
result = requests.get(url, headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.io/api/field/?id=alice@feathery.io" \
    -H "Authorization: Token <API KEY>"
```

```javascript
const url = "https://api.feathery.io/api/field/?id=alice@feathery.io";
const options = { headers: { Authorization: "Token <API KEY>" } };
fetch(url, options)
    .then((response) => response.json())
    .then(result => console.log(result));
```

> The above command outputs JSON structured like this:

```json
[
  {
    "id": "name",
    "type": "text_field",
    "display_text": "What is your name?",
    "value": "Alice Smith",
    "created_at": "2020-06-01T00:00:00Z",
    "updated_at": "2020-06-02T00:00:00Z"
  }
]
```

For a specific user, list all of the field data they submitted

### HTTP Request

`GET https://api.feathery.io/api/field/`

### Request Query Parameters

Parameter | Type | Description
--------- | --------- | -----------
id | String (Optional) | Your unique user ID

### Response Parameters

The response will be an array of objects with the following parameters.

Parameter | Type                   | Description
--------- |------------------------| -----------
id | String                 | Your unique field ID
hidden | Boolean                | If true, this is a hidden field. Otherwise, it's a form field.
type | String Enum (Optional) | The [form field type](https://docs.feathery.io/platform/components/fields/button-group#example). Not present for hidden fields.
display_text | String (Optional)      | Human-friendly text to display for this field
value | Polymorphic (Optional) | Submitted value of the user whose key was passed in.
created_at | Datetime               | When this field was created
updated_at | Datetime               | When this field was last updated

## Get User Session Data

```python
import requests

url = "https://api.feathery.io/api/user/<user_id>/session/";
headers = {"Authorization": "Token <API KEY>"}
result = requests.get(url, headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.io/api/user/<user_id>/session/" \
    -H "Authorization: Token <API KEY>"
```

```javascript
const url = "https://api.feathery.io/api/user/<user_id>/session/";
const options = { headers: { Authorization: "Token <API KEY>" } };
fetch(url, options)
    .then((response) => response.json())
    .then(result => console.log(result));
```

> The above command outputs JSON structured like this:

```json
{
  "auth_id": "identity-code",
  "internal_id": "user-id",
  "forms": [
    {
      "current_step_id": "step 1",
      "completed_at": null,
      "name": "My form",
      "track_location": false
    },
    {
      "current_step_id": null,
      "completed_at": "2024-04-17T23:10:45.992617Z",
      "name": "My other form",
      "track_location": false
    }
  ]
}
```

Get session data for a user, including all forms and their progress

### HTTP Request

`GET https://api.feathery.io/api/user/<user_id>/session/`

### Response Parameters

The response will be an object with the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
auth_id | String (Optional) | Identity provider ID
internal_id | String | Unique User ID
forms | Array | List of forms the user is working on

Each form object will have the following parameters:

Parameter | Type | Description
--------- | --------- | -----------
id | String | Form's ID
current_step_id | String | ID of the step the user most recently visited
completed_at | String | Timestamp of when the user completed the form - null if not complete
name | String | Name of the form
track_location | Boolean | Whether the form remembers the user's location


## Create and Fetch a User

```python
import requests

url = "https://api.feathery.io/api/user/";
data = {"id": "alice@feathery.io"}
headers = {
    "Authorization": "Token <API KEY>",
    "Content-Type": "application/json",
}
result = requests.post(url, data=data, headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.io/api/user/" \
    -X POST \
    -d "{'id': 'alice@feathery.io'}" \
    -H "Authorization: Token <API KEY>" \
    -H "Content-Type: application/json"
```

```javascript
const url = "https://api.feathery.io/api/user/";
const data = {id: "alice@feathery.io"}
const headers = {
    Authorization: "Token <API KEY>",
    "Content-Type": "application/json"
};
const options = {
    headers, 
    method: 'POST',
    body: JSON.stringify(data)
};
fetch(url, options)
    .then((response) => response.json())
    .then(result => console.log(result));
```

> The above command outputs JSON structured like this:

```json
{
  "sdk_key": "b8629a53-b1ba-49c1-99bc-0117cf12fd98",
  "id": "alice@feathery.io",
  "created_at": "2020-06-03T00:00:00Z",
  "updated_at": "2020-06-04T00:00:00Z"
}
```

Create, update, or retrieve the information of a specific user.

If the user corresponding to the ID you specify doesn't exist, they'll be created
based on the request body parameters. Otherwise, the user will be updated based
on those parameters.

The user SDK key will also be returned, which can be used for authenticating
the React library for displaying forms.

### HTTP Request

`POST https://api.feathery.io/api/user/`

### Body Parameters

Parameter | Type | Description
--------- | --------- | -----------
id | String | Your unique user ID

### Response Parameters

Parameter | Type | Description
--------- | --------- | -----------
sdk_key | String | This user's SDK key
id | String | Your unique user ID
created_at | Datetime | When this user was created
updated_at | Datetime | When this user was last updated

## Delete a User

```python
import requests

url = "https://api.feathery.io/api/user/alice@feathery.io/";
headers = {
    "Authorization": "Token <API KEY>",
    "Content-Type": "application/json",
}
result = requests.delete(url, headers=headers)
print(result.status_code)
```

```shell
curl "https://api.feathery.io/api/user/alice@feathery.io/" \
    -X DELETE \
    -d "{'key': 'alice@feathery.io'}" \
    -H "Authorization: Token <API KEY>" \
    -H "Content-Type: application/json"
```

```javascript
const url = "https://api.feathery.io/api/user/alice@feathery.io/";
const headers = {
    Authorization: "Token <API KEY>",
    "Content-Type": "application/json"
};
const options = { headers, method: 'DELETE' };
fetch(url, options).then((response) => console.log(response.status))
```

> The above command does not return a response body

Delete a specific user.

### HTTP Request

`DELETE https://api.feathery.io/api/user/<id>/`

### URL Parameters

Parameter | Description
--------- | -----------
id | The ID of the user to delete
