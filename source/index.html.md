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

You can use our API to access and modify your forms, fields, and users. Our [React library](https://github.com/feathery-org/feathery-react) wraps many of these endpoints.

# Authentication

Provide an admin-level API key to authenticate access.
You can register a new API key after joining our [waitlist](https://feathery.io). 

API keys are environment-specific (production, development, etc.), which allows you to
separate real from test data. All API keys have access to the same fields and forms,
but users are environment-specific.

Include your API key as a request header that looks like the following:

`Authorization: <API KEY>`

<aside class="notice">
You must replace <code>&lt;API KEY&gt;</code> with your personal API key.
</aside>

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
form_id | String | Your unique form ID
steps | Array<Obj> | An array of step objects

Each `steps` object contains the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
id | String | The ID of the step, unique to the form
origin | Boolean | Is this the first step of the form
images | Array<Obj> | Images on this step
progress_bars | Array<Obj> | Progress bars on this step
texts | Array<Obj> | Text elements on this step
buttons | Array<Obj> | Buttons on this step
fields | Array<Obj> | Fields on this step
previous_conditions | Array<Obj> | Navigation rules that connect previous steps to the current step
next_conditions | Array<Obj> | Navigation rules that connect the current step to next steps
created_at | Datetime | When this step was created
updated_at | Datetime | When this step was last updated

Each form element (images, progress_bars, texts, buttons, fields) has a common `id` parameter that uniquely identifies it.


## Generate a form via API

```python
import requests

url = "https://api.feathery.io/api/form/";
headers = {"Authorization": "Token <API KEY>"}
data = {
          "form_id": "new_form_id",
          "template_form_id": "id_of_form_to_copy",
          "steps": [
              {
                  "step_id": "new_copied_step",
                  "template_step_id": "step_from_copied_form",
                  "origin": True,
                  "fields": [
                      {
                          "id": "field_to_change",
                          "field_id": "name_of_new_field_value_or_value_to_copy"
                      }
                  ],
                  "texts": [
                      {
                          "id": "name_of_text_to_change",
                          "text": "New Text to replace copied text",
                      }
                  ],
                  "buttons": [
                      {
                          "id": "name_of_button_to_change",
                          "text": "New Button Text",
                      }
                  ],
                  "images": [
                      {
                          "id": "name_of_image_to_change",
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
                  "step_id": "new_copied_step_2",
                  "template_step_id": "Step 2",
                  "fields": [],
                  "texts": [],
                  "buttons": [],
                  "images": [],
                  "progress_bars": [],
              },
          ],
          "navigation_rules": [
              {
                  "previous_step_id": "new_copied_step",
                  "next_step_id": "new_copied_step_2",
                  "trigger": "click",
                  "element_type": "button",
                  "element_id": str(self.button.id),
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
                  'step_id': 'new_copied_step',
                  'template_step_id': 'step_from_copied_form',
                  'origin': True,
                  'fields': [
                      {
                          'id': 'field_to_change',
                          'field_id': 'name_of_new_field_value_or_value_to_copy'
                      }
                  ],
                  'texts': [
                      {
                          'id': 'name_of_text_to_change',
                          'text': 'New Text to replace copied text',
                      }
                  ],
                  'buttons': [
                      {
                          'id': 'name_of_button_to_change',
                          'text': 'New Button Text',
                      }
                  ],
                  'images': [
                      {
                          'id': 'name_of_image_to_change',
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
                  'step_id': 'new_copied_step_2',
                  'template_step_id': 'Step 2',
                  'fields': [],
                  'texts': [],
                  'buttons': [],
                  'images': [],
                  'progress_bars': [],
              },
          ],
          'navigation_rules': [
              {
                  'previous_step_id': 'new_copied_step',
                  'next_step_id': 'new_copied_step_2',
                  'trigger': 'click',
                  'element_type': 'button',
                  'element_id': 'str(self.button.id)',
                  'rules': [
                      {
                          'comparison': 'equal',
                          'field_key': '',
                          'value': '',
                      },
                  ],
              },
          ],
      }" \
    -H "Authorization: Token <API KEY>" \
    -H "Content-Type: application/json" \
```

```javascript
const url = "https://api.feathery.io/api/form/";
const data = {
          "form_id": "new_form_id",
          "template_form_id": "id_of_form_to_copy",
          "steps": [
              {
                  "step_id": "new_copied_step",
                  "template_step_id": "step_from_copied_form",
                  "origin": True,
                  "fields": [
                      {
                          "id": "field_to_change",
                          "field_id": "name_of_new_field_value_or_value_to_copy"
                      }
                  ],
                  "texts": [
                      {
                          "id": "name_of_text_to_change",
                          "text": "New Text to replace copied text",
                      }
                  ],
                  "buttons": [
                      {
                          "id": "name_of_button_to_change",
                          "text": "New Button Text",
                      }
                  ],
                  "images": [
                      {
                          "id": "name_of_image_to_change",
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
                  "step_id": "new_copied_step_2",
                  "template_step_id": "Step 2",
                  "fields": [],
                  "texts": [],
                  "buttons": [],
                  "images": [],
                  "progress_bars": [],
              },
          ],
          "navigation_rules": [
              {
                  "previous_step_id": "new_copied_step",
                  "next_step_id": "new_copied_step_2",
                  "trigger": "click",
                  "element_type": "button",
                  "element_id": str(self.button.id),
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

> The above command outputs HTTP_201_CREATED

Generate a form via API.

### HTTP Request

`POST https://api.feathery.io/api/form/`

### Body Parameters

Parameter | Type | Description
--------- | --------- | -----------
form_id | String | Your desired new form ID
template_form_id | String | The ID of the form you'd like to copy from
steps | Array<Obj> | An array of step objects
navigation_rules | Array<Obj> | An array of navigation rule objects

Each `steps` object contains the following parameters.
The arrays of images, progress bars, texts, buttons, and fields allow you to override qualities from the elements in the copied step.
If nothing is specified, the elements on the step will be copied as they are.

Parameter | Type | Description
--------- | --------- | -----------
step_id | String | The ID of the new step, unique to the form
template_step_id | String | The ID of the step to copy from the original form
origin | Boolean | Is this the first step of the form
images | Array<Obj> | Images on this step
progress_bars | Array<Obj> | Progress bars on this step
texts | Array<Obj> | Text elements on this step
buttons | Array<Obj> | Buttons on this step
fields | Array<Obj> | Fields on this step

Each `image` object contains the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
id | String | The ID of the image you'd like to override from the original step
source_url | String | A new source_url to take the image from

Each `progress_bar` object contains the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
id | String | The ID of the progress bar you'd like to override from the original step
progress | Integer | A specific amount of progress you'd like to display on your new step between 0 and 100

Each `text` object contains the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
id | String | The ID of the text you'd like to override from the original step
text | String | The new text you'd like to display

Each `button` object contains the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
id | String | The ID of the button you'd like to override from the original step
text | String | Text you'd like to display on your new button

Each `field` object may contain the following parameters, depending on what type of field you are copying or changing.

Parameter | Type | Description
--------- | --------- | -----------
id | String | The ID of the field you'd like to override from the original step
field_id | String | The ID of the field value you'd like to connect to this field
type | String | The type of value you'd like to use the field for (determines what you may need to override)
max_length | Integer | Maximum length of the value you'd like to store
min_length | Integer | Minimum length of the value you'd like to store
placeholder | String | Placeholder text for the field value
tooltipText | String | Tooltip text for the field value
submit_trigger | String | Submit trigger for the field (values can be Automatic or None)
metadata | Dict | may contain options array, other, or file_types array

Here is a table of what overrides are useful for each field type. If an override is noted as required for a type of field, you must use that override if you're not using the field directly as copied from the template.

Overrides | Valid Field Types | Required
--------- | --------- | -----------
max_length, min_length | text_field, text_area, integer_field, select, multiselect, email | Optional
max_length | pin_input | Required
placeholder, toolTipText | dropdown, email, login, phone_number, gmap_line_1, gmap_line_2, gmap_city, gmap_state, gmap_zip, integer_field, ssn, text_field, text_area, url | Optional
submit_trigger | button_group, file_upload, rich_file_upload, dropdown, gmap_state, pin_input, select, hex_color, login, phone_number, ssn | Optional
metadata.other | select, multiselect, dropdown | Required
metadata.options | button_group, select, multiselect, dropdown | Required
metadata.field_types | rich_file_upload, rich_multi_file_upload | Required

Each `navigation_rules` object contains the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
previous_step_id | String | The ID of the step you'd like to override from the original step
next_step_id | String | Progress bars on this step
trigger | String | How to trigger navigation - Choices are click, change, and load
element_type | String | What type of element triggers navigation - Choices are button, field, and text
element_id | String | ID of the element from the original form step that you'd like to trigger navigation in your templated form
rules | Array<Obj> | The rules to match for navigation to occur


Each `rules` object contains the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
comparison | String | Comparison type (gt, lt, equal)
field_key | String | The field whose value you want to navigate based on
value | String | The value you need to compare the value of that field to 

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
    "name": "Alice",
    "created_at": "2020-06-01T00:00:00Z",
    "updated_at": "2020-06-02T00:00:00Z"
  },
  {
    "id": "bob@feathery.io",
    "name": "Bob",
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
name | String (Optional) | Your human-friendly user name
created_at | Datetime | When this user was created
updated_at | Datetime | When this user was last updated

## Create and Fetch a User

```python
import requests

url = "https://api.feathery.io/api/user/";
data = {"id": "alice@feathery.io", "name": "Alice 2.0"}
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
    -d "{'id': 'alice@feathery.io', 'name': 'Alice 2.0'}" \
    -H "Authorization: Token <API KEY>" \
    -H "Content-Type: application/json" \
```

```javascript
const url = "https://api.feathery.io/api/user/";
const data = {id: "alice@feathery.io", name: "Alice 2.0"}
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
  "api_key": "b8629a53-b1ba-49c1-99bc-0117cf12fd98",
  "id": "alice@feathery.io",
  "name": "Alice 2.0",
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
name | String (Optional) | Your human-friendly user name.

### Response Parameters

Parameter | Type | Description
--------- | --------- | -----------
api_key | String | This user's API key
id | String | Your unique user ID
name | String (Optional) | Your human-friendly user name
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


List all of your fields in Feathery and their values for a specific user

### HTTP Request

`GET https://api.feathery.io/api/field/`

### Request Query Parameters

Parameter | Type | Description
--------- | --------- | -----------
id | String (Optional) | Your unique user ID

### Response Parameters

The response will be an array of objects with the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
id | String | Your unique field ID
type | String Enum | The field type (e.g. text_area, dropdown, etc.)
display_text | String (Optional) | Human-friendly text to display for this field
value | Polymorphic (Optional) | Submitted value of the user whose key was passed in.
created_at | Datetime | When this field was created
updated_at | Datetime | When this field was last updated

## Create Field Value for User

```python
import requests

url = "https://api.feathery.io/api/field/alice@feathery.io/";
data = {"field_id": "age", "value": 21}
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
    -d "{'field_id': 'age', 'value': 21}" \
    -H "Authorization: Token <API KEY>" \
    -H "Content-Type: application/json" \
```

```javascript
const url = "https://api.feathery.io/api/field/alice@feathery.io/";
const data = {field_id: "age", value: 21}
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
  "field_id": "age",
  "value":  21,
  "created_at": "2020-06-01T00:00:00Z",
  "updated_at": "2020-06-03T00:00:00Z"
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
field_id | String | Your unique field ID
value | Polymorphic | Field value to associate with the specified user

### Response Parameters

Parameter | Type | Description
--------- | --------- | -----------
field_id | String | Your unique field ID
value | Polymorphic (Optional) | Field value associated with the specified user
created_at | Datetime | When this value was created
updated_at | Datetime | When this value was last updated
