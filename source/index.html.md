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

You can use our API to access and modify your forms, fields, and users. You can leverage our [React library](https://github.com/feathery-org/feathery-react) for much of th same functionality.

# Authentication

Provide your Feathery admin API key to authenticate access.
You can get an API key by creating an [account](https://app.feathery.io). 

API keys are environment-specific (production, development, etc.), which allows you to
separate real from test data. All API keys have access to the same fields and forms,
but users are environment-specific.

Include your API key as a request header that looks like the following:

`Authorization: <API KEY>`

<aside class="notice">
You must replace <code>&lt;API KEY&gt;</code> with your personal API key.
</aside>

# Account

## Retrieve your account information

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

# Forms

## Retrieve a form schema

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
  "form_id": "my_form",
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
form_id | String | The name of your form
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


## Create a form

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

Create a form based off an existing template form.

### HTTP Request

`POST https://api.feathery.io/api/form/`

### Body Parameters

Parameter | Type | Description
--------- | --------- | -----------
form_id | String | The ID of the new form being created (must be unique)
template_form_id | String | The ID of the template form to copy from
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

List all of your users in Feathery.

### HTTP Request

`GET https://api.feathery.io/api/user/`

### Response Parameters

The response will be an array of objects with the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
id | String | Your unique user ID
created_at | Datetime | When this user was created
updated_at | Datetime | When this user was last updated

## Get User Session Data

```python
import requests

url = "https://api.feathery.io/api/user/<user_id>/session/<form_key>/";
headers = {"Authorization": "Token <API KEY>"}
result = requests.get(url, headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.io/api/user/<user_id>/session/<form_key>/" \
    -H "Authorization: Token <API KEY>"
```

```javascript
const url = "https://api.feathery.io/api/user/<user_id>/session/<form_key>/";
const options = { headers: { Authorization: "Token <API KEY>" } };
fetch(url, options)
    .then((response) => response.json())
    .then(result => console.log(result));
```

> The above command outputs JSON structured like this:

```json
{
  "current_step_key": "Step_3",
  "auth_id": "identity-code",
  "auth_email": "identity-email",
  "auth_phone": "identity-phone-number"
}

```

Get session data for a form user.

### HTTP Request

`GET https://api.feathery.io/api/user/<user_id>/session/<form_key>/`

### Response Parameters

The response will be an object with the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
current_step_key | String | ID of the step the user most recently visited
auth_id | String (Optional) | Identity provider ID
auth_email | String (Optional) | Identity provider email
auth_phone | String (Optional) | Identity provider phone number

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
    -H "Content-Type: application/json" \
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

If the user belonging to the key you specify doesn't exist, they'll be created
based on the request body parameters. Otherwise, the user will be updated based
on those parameters.

The user API key will also be returned, which can be used for authenticating
our React library for displaying forms.

### HTTP Request

`POST https://api.feathery.io/api/user/`

### Body Parameters

Parameter | Type | Description
--------- | --------- | -----------
id | String | Your unique user ID

### Response Parameters

Parameter | Type | Description
--------- | --------- | -----------
sdk_key | String | This user's API key
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
    -H "Content-Type: application/json" \
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


# Fields

## List All Fields for a User 

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
  },
  {
    "id": "age",
    "type": "integer_field",
    "display_text": "How old are you?",
    "value": null,
    "created_at": "2020-06-01T00:00:00Z",
    "updated_at": "2020-06-02T00:00:00Z"
  }
]
```


For a specific user, list all of their form and hidden fields and values

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

## Create Field Value for User

```python
import requests

url = "https://api.feathery.io/api/field/alice@feathery.io/";
data = {"fields": {"age": 21}}
headers = {
    "Authorization": "Token <API KEY>",
    "Content-Type": "application/json",
}
result = requests.post(url, data=data, headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.io/api/field/alice@feathery.io/" \
    -X POST \
    -d "{'fields': {'age': 21}}" \
    -H "Authorization: Token <API KEY>" \
    -H "Content-Type: application/json" \
```

```javascript
const url = "https://api.feathery.io/api/field/alice@feathery.io/";
const data = {"fields": {"age": 21}}
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
}
```

Create or update the value of a field for a specific user

### HTTP Request

`POST https://api.feathery.io/api/field/<id>/`

### URL Parameters

Parameter | Description
--------- | -----------
id | Your unique user ID

### Request Body Parameters

Parameter | Type | Description
--------- | --------- | -----------
fields | Object | A mapping from unique field IDs to field values to create for them

### Response Parameters

Parameter | Type | Description
--------- | --------- | -----------
fields | Object | A mapping from unique field IDs to field values to create for them
