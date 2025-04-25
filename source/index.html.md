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

You can use our API to access and modify your Feathery resources - forms, fields, documents, and more.

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

## Rotate API Key

```python
import requests

url = "https://api.feathery.io/api/account/rotate_key/";
headers = {"Authorization": "Token <API KEY>"}
result = requests.patch(url, headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.io/api/account/rotate_key/" \
    -X PATCH \
    -H "Authorization: Token <API KEY>"
```

```javascript
const url = "https://api.feathery.io/api/account/rotate_key/";
const headers = {
  Authorization: "Token <API KEY>",
};
const options = { headers, method: 'PATCH' };
fetch(url, options)
  .then((response) => response.json())
  .then(result => console.log(result));
```

> The above command outputs JSON structured like this:

```json
{
  "old_api_key": "<API KEY>",
  "new_api_key": "<API KEY>"
}
```

Rotate your Feathery API key. The rotated key is the one used to authenticate the request.

### HTTP Request

`PATCH https://api.feathery.io/api/account/rotate_key/`

### Response Body

The response will be an object containing the following parameters.

Parameter | Type   | Description
--------- |--------| -----------
old_api_key | String | The former API key that was rotated out
new_api_key | String | The new API key that is in effect

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
{
  "team": "my-team-name",
  "accounts": [
    {
      "email": "user@company.com",
      "role": "admin",
      "id": "<ACCOUNT ID>"
    }
  ]
}
```

Retrieve your Feathery account information.

### HTTP Request

`GET https://api.feathery.io/api/account/`

### Response Body

The response will be an object containing the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
team | String | The name of your team in Feathery
accounts | Array | List of accounts that belong to your team

## Invite Accounts

```python
import requests

url = "https://api.feathery.io/api/account/invite/";
headers = {"Authorization": "Token <API KEY>"}
result = requests.post(url, headers=headers, json=[{"email": "new@invite.com"}])
print(result.json())
```

```shell
curl "https://api.feathery.io/api/account/invite/" \
    -X POST \
    -d "[{'email': 'new@invite.com'}]" \
    -H "Authorization: Token <API KEY>" \
    -H "Content-Type: application/json"
```

```javascript
const url = "https://api.feathery.io/api/account/invite/";
const data = [{email: 'new@invite.com'}];
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

Invite new users to your Feathery team.

### HTTP Request

`POST https://api.feathery.io/api/account/invite/`

### Request Body Parameters
An array of objects with the following parameters, where each object is a new
user to invite:

Parameter | Type                  | Description
--------- |-----------------------| -----------
email | string                | The email of the new user to invite 
role | string (optional)     | Either 'admin', 'editor', or 'viewer'. Defaults to 'admin'.
permission_edit_form_results | boolean (optional)    | If they're allowed to edit form results. Defaults to true.
permission_invite_collaborators | boolean (optional)    | If they're allowed to invite form collaborators. Defaults to true.
permission_edit_collaborator_template | boolean (optional)    | If they're allowed to edit form collaborator settings. Defaults to true.
permission_edit_logic | boolean (optional)    | If they're allowed to edit form custom logic rules. Defaults to true.
permission_edit_theme | boolean (optional)    | If they're allowed to edit form themes. Defaults to true.
user_groups | string\[\] (optional) | An array of user group names to add the invited account to

### Response Body

The response will be an object containing the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
team | String | The name of your team in Feathery
accounts | Array | List of accounts that belong to your team

## Edit Account

```python
import requests

url = "https://api.feathery.io/api/account/";
headers = {"Authorization": "Token <API KEY>"}
result = requests.patch(url, headers=headers, json=[{"email": "user@invite.com", "role": "viewer"}])
print(result.json())
```

```shell
curl "https://api.feathery.io/api/account/invite/" \
    -X PATCH \
    -d "[{'email': 'user@invite.com', 'role': 'viewer'}]" \
    -H "Authorization: Token <API KEY>" \
    -H "Content-Type: application/json"
```

```javascript
const url = "https://api.feathery.io/api/account/invite/";
const data = [{email: 'user@invite.com', role: 'viewer'}];
const headers = {
  Authorization: "Token <API KEY>",
  "Content-Type": "application/json"
};
const options = {
  headers,
  method: 'PATCH',
  body: JSON.stringify(data)
};
fetch(url, options)
  .then((response) => response.json())
  .then(result => console.log(result));
```

Edit a Feathery account.

### HTTP Request

`PATCH https://api.feathery.io/api/account/`

### Request Body Parameters
The following parameters can be specified in the request body.

Parameter | Type                  | Description
--------- |-----------------------| -----------
email | string (optional)     | The email of the account to edit
account_id | UUID (optional) | The ID of the account to edit. Either the account ID or the email must be specified.
role | string (optional)     | Either 'admin', 'editor', or 'viewer'
permission_edit_form_results | boolean (optional)    | If they're allowed to edit form results.
permission_invite_collaborators | boolean (optional)    | If they're allowed to invite form collaborators.
permission_edit_collaborator_template | boolean (optional)    | If they're allowed to edit form collaborator settings.
permission_edit_logic | boolean (optional)    | If they're allowed to edit form custom logic rules.
permission_edit_theme | boolean (optional)    | Iif they're allowed to edit form themes.

### Response Body

The response will be an object containing attributes of the account that was updated.

## Remove Account

```python
import requests

url = "https://api.feathery.io/api/account/uninvite/";
headers = {"Authorization": "Token <API KEY>"}
result = requests.patch(url, headers=headers, json=[{"email": "new@invite.com"}])
print(result.json())
```

```shell
curl "https://api.feathery.io/api/account/uninvite/" \
    -X PATCH \
    -d "[{'email': 'new@invite.com'}]" \
    -H "Authorization: Token <API KEY>" \
    -H "Content-Type: application/json"
```

```javascript
const url = "https://api.feathery.io/api/account/uninvite/";
const data = [{email: 'new@invite.com'}];
const headers = {
  Authorization: "Token <API KEY>",
  "Content-Type": "application/json"
};
const options = {
  headers,
  method: 'PATCH',
  body: JSON.stringify(data)
};
fetch(url, options)
  .then((response) => response.json())
  .then(result => console.log(result));
```

Remove user from your Feathery team.

### HTTP Request

`PATCH https://api.feathery.io/api/account/uninvite/`

### Request Body Parameters

Parameter | Type              | Description
--------- |-------------------| -----------
email | String (Optional) | The email of the user to remove. Either the account ID or the email needs to be specified.
account_id | String (Optional) | The account ID of the user to remove. Either the account ID or the email needs to be specified.

### Response Body

The response will be an object containing the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
team | String | The name of your team in Feathery
accounts | Array | List of accounts that belong to your team

# Document Intelligence

## Extract Data from Documents
```python
import requests

url = "https://api.feathery.io/api/ai/run/<extraction_id>/";
headers = {"Authorization": "Token <API KEY>"}
files = [
    ('files', ('file1.txt', document_1, 'text/plain')),
    ('files', ('file2.txt', document_2, 'text/plain'))
]
data = {"files": files}
result = requests.post(url, files=data, headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.io/api/ai/run/<extraction_id>/" \
    -X POST \
    -F "files=@file1.txt" \
    -F "files=@file2.txt" \
    -H "Authorization: Token <API KEY>"
```

```javascript
const url = "https://api.feathery.io/api/ai/run/<extraction_id>/";
const formData = new FormData();
formData.append('files', fs.createReadStream('file1.txt'));
formData.append('files', fs.createReadStream('file2.txt'));
const headers = { Authorization: "Token <API KEY>" };
const options = {
    headers, 
    method: 'POST',
    body: formData
};
fetch(url, options)
    .then((response) => response.json())
    .then(result => console.log(result));
```

> The above command outputs JSON structured like this:

```json
{"user_id": "<USER ID>"}
```
Extract data from documents you send to Feathery via API. 
The extraction must already be defined in your Feathery account, 
and you must have document intelligence enabled.

### HTTP Request

`POST https://api.feathery.io/api/ai/run/<extraction_id>/`

### URL Parameters

Parameter | Type              | Description
--------- |-------------------| -----------
extraction_id | String | The ID of the extraction to run, which is configured in your Feathery dashboard.

### Request Body Parameters
Note that this needs to be formatted as a multipart/form-data request.

Parameter | Type              | Description
--------- |-------------------| -----------
files | File Array        | An array of files that will be parsed by the extraction for data.

### Response Body

The response will be an object containing the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
user_id | String | The new Feathery user / submission who the extracted data is stored under.

## List Extraction Runs

```python
import requests

url = "https://api.feathery.io/api/ai/run/batch/<extraction_id>/";
headers = {"Authorization": "Token <API KEY>"}
result = requests.get(url, headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.io/api/ai/run/batch/<extraction_id>/" \
    -H "Authorization: Token <API KEY>"
```

```javascript
const url = "https://api.feathery.io/api/ai/run/batch/<extraction_id>/";
const headers = { Authorization: "Token <API KEY>" };
fetch(url, { headers })
    .then((response) => response.json())
    .then(result => console.log(result));
```

> The above command outputs JSON structured like this:

```json
[{
  "id": "<Run Id>",
  "user_id": "<User Id>",
  "file_name": "document.pdf",
  "success": true,
  "approved": true,
  "approver": "reviewer@mail.com",
  "email_extracted_at": "2020-06-02T00:00:00Z",
  "document_extracted_at": "2020-06-03T00:00:00Z",
  "data": [{"field_internal_id":  "<FIELD ID>", "value": ""}],
  "created_at": "2020-06-01T00:00:00Z",
  "updated_at": "2020-06-04T00:00:00Z"
}]
```

List runs for a specific AI document extraction

### HTTP Request

`GET https://api.feathery.io/api/ai/run/batch/<EXTRACTION ID>/`

### Request Query Parameters

Parameter | Type                | Description
--------- |---------------------| -----------
start_time | Datetime (Optional) | Fetch runs that started after this start time
end_time | Datetime (Optional) | Fetch runs that started before this end time

### Response Body

The response is an array of extraction run entries with the following parameters.

Parameter | Type                | Description
--------- |---------------------| -----------
id | String              | The unique ID of the extraction run
user_id | String              | The unique ID of the user who the extraction run is associated with
file_name | String | The name of the document that was processed
success | Boolean             | If the run was successful
data | {field_internal_id: string; value: any}[] | A list of datapoints extracted during this run
approved | Boolean             | If the run required review and was approved
approver | Email               | The email of the account who approved the run
email_extracted_at | Datetime (Optional) | If the inbox integration is turned on for this extraction, when the email associated with this run finished extracting
document_extracted_at | Datetime | When the document associated with this run finished extracting
created_at | Datetime | When this extraction run was created
updated_at | Datetime | When this extraction run was last updated

# Document Templates

## Fill or Sign a Document Template 

```python
import requests

url = "https://api.feathery.io/api/document/fill/";
headers = {"Authorization": "Token <API KEY>"}
data = {"document": <DOCUMENT ID>, "field_values": {<FIELD ID>: <FIELD VALUE>}, "signer_email": "signer@company.com"}
result = requests.post(url, json=data, headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.io/api/document/fill/" \
    -X POST \
    -d "{'document': <DOCUMENT ID>, 'field_values': {<FIELD ID>: <FIELD VALUE>}, 'signer_email': 'signer@company.com'" \
    -H "Authorization: Token <API KEY>" \
    -H "Content-Type: application/json"
```

```javascript
const url = "https://api.feathery.io/api/document/fill/";
const data = {document: '<DOCUMENT ID>', field_values: {'<FIELD ID>': '<FIELD VALUE>'}, signer_email: 'signer@company.com'}
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

Fill out and/or sign a document that you've 
uploaded and mapped in your Feathery account.

### HTTP Request

`POST https://api.feathery.io/api/document/fill/`

### Request Body Parameters

Parameter | Type              | Description
--------- |-------------------| -----------
document | UUID String       | The ID of the document to fill
field_values | Object (optional) | A mapping of field ID to field value to fill the document
signer_email | String (optional) | The document will route to the specified email for signature after being filled
user_id | String (Optional) | Associate an existing Feathery user with the generated document

### Response Body

The response will be an object containing the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
file_url | String | The URL to the filled out document

## List Document Envelopes

```python
import requests

url = "https://api.feathery.io/api/document/envelope/";
headers = {"Authorization": "Token <API KEY>"}
params = {"type": "document", "id": <DOCUMENT ID>}
result = requests.get(url, params=params, headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.io/api/document/envelope/?type=document&id=<DOCUMENT ID>" \
    -H "Authorization: Token <API KEY>"
```

```javascript
const url = "https://api.feathery.io/api/document/envelope/?type=document&id=<DOCUMENT ID>";
const headers = { Authorization: "Token <API KEY>" };
const options = { headers };
fetch(url, options)
    .then((response) => response.json())
    .then(result => console.log(result));
```

> The above command outputs JSON structured like this:

```json
[{
  "id": "<ENVELOPE ID>",
  "document": "<DOCUMENT ID>",
  "user": "<FEATHERY USER ID>",
  "signer": "signer@mail.com",
  "sender": "sender@mail.com",
  "file": "https://link-to-filled-file.com",
  "type": "pdf",
  "viewed": true,
  "signed": true,
  "tags": ["document-tag"],
  "created_at": "2020-06-03T00:00:00Z"
}]
```

List document envelopes that your users have filled out in Feathery.
You can find the envelopes corresponding to either a particular document template
or a particular submission.

### HTTP Request

`GET https://api.feathery.io/api/document/envelope/

### Request Query Parameters

Parameter | Type              | Description
--------- |-------------------| -----------
type | String            | Either `document` or `user`, specifying how to look up envelopes of interest.
id | String            | If method is `document`, this is the document ID. If method is `user`, this is the user ID.

### Response Body

The response will be an array of objects containing the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
id | String | The ID of the envelope
document | String (Optional) | The ID of the document template this envelope was generated from
user | String (Optional) | The ID of the Feathery user that filled out this envelope
signer | String (Optional) | The email of the envelope signer
sender | String (Optional) | The email of the envelope sender
file | URL | The URL to the file associated with this envelope
type | String Enum | The type of document - `pdf`, `docx`, `xlsx`
viewed | Boolean | If envelope was routed for signature, if the signer has viewed it
signed | Boolean | If envelope was routed for signature, if the signer has signed it
tags | String[] | An array of tags that contain metadata about the envelope
created_at | Datetime | When this envelope was created

## Delete Document Envelope

```python
import requests

url = "https://api.feathery.io/api/document/envelope/<ENVELOPE ID>/";
headers = {"Authorization": "Token <API KEY>"}
result = requests.delete(url, headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.io/api/document/envelope/<ENVELOPE ID>/" \
    -X DELETE \
    -H "Authorization: Token <API KEY>" 
```

```javascript
const url = "https://api.feathery.io/api/document/envelope/<ENVELOPE ID>/";
const headers = {
  Authorization: "Token <API KEY>",
};
const options = { headers, method: 'DELETE' };
fetch(url, options).then((response) => console.log(response.status))
```

Delete a specific Feathery document envelope.

### HTTP Request

`DELETE https://api.feathery.io/api/document/envelope/<ENVELOPE ID>/`

### URL Parameters

Parameter | Type   | Description
--------- |--------| -----------
envelope_id | UUID   | The ID of the envelope to delete.

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
      "internal_id": "46d1ba89-0dcc-42cd-839c-a12ce6668347",
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
      "internal_id": "46d1ba89-0dcc-42cd-839c-a12ce6668347",
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
  ],
  "rules": [
    {
      "name": "Rule 1",
      "description": "Rule description",
      "trigger_event": "form_complete",
      "index": 0,
      "steps": [],
      "code": "code",
      "elements": [],
      "enabled": true,
      "valid": true,
      "mode": "code_editor",
      "created_at": "2020-06-01T00:00:00Z",
      "updated_at": "2020-06-01T00:00:00Z"
    }  
  ]
}
```

Retrieve the schema of a form created in Feathery.

### HTTP Request

`GET https://api.feathery.io/api/form/<form_id>/`

### Response Body

The response will be an object containing the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
form_id | String | Your form's ID
form_name | String | Your form's name
steps | Array<Obj> | An array of step objects
rules | Array<Obj> | An array of rule objects

Each `steps` object contains the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
id | String | The ID of the step, unique to the form
internal_id | UUID | The internal ID of the step, globally unique
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
Each element also contains a `hide_rules` parameter that stores any conditions that would trigger the element to be hidden on the form.

Each `rule` object contains the following parameters.

Parameter | Type   | Description
--------- |--------| -----------
name | String | The user-friendly name of the rule
description | String | Description of the rule
trigger_event | Enum   | On what event the rule runs
index | Number | The ordering of this rule when executing
steps | Array  | A list of step IDs corresponding to the steps that the rule runs on
elements | Array  | A list of element IDs that trigger the rule
enabled | Boolean | Is this rule runnable
valid | Boolean | Is the rule configuration in a valid state
mode | Enum | Is the rule defined via the code editor or no-code logic
created_at | Datetime | When the rule was created
updated_at | Datetime | When the rule was last updated

## List Forms

```python
import requests

url = "https://api.feathery.io/api/form/";
headers = {"Authorization": "Token <API KEY>"}
result = requests.get(url, headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.io/api/form/" \
    -H "Authorization: Token <API KEY>"
```

```javascript
const url = "https://api.feathery.io/api/form/";
const options = { headers: { Authorization: "Token <API KEY>" } };
fetch(url, options)
    .then((response) => response.json())
    .then(result => console.log(result));
```

> The above command outputs JSON structured like this:

```json
[
  {
    "id": "aSdsa5",
    "name": "My Form",
    "active": true,
    "created_at": "2020-06-01T00:00:00Z",
    "updated_at": "2020-06-01T00:00:00Z"
  }
]
```

List your Feathery forms

### HTTP Request

`GET https://api.feathery.io/api/form/`

### Request Query Parameters

Parameter | Type              | Description
--------- |-------------------| -----------
tags | String (Optional) | Only return forms that have all of these tags in a comma-separated list.

### Response Body

The response will be an array of objects containing the following parameters.

Parameter | Type            | Description
--------- |-----------------| -----------
id | String          | The form ID
name | String          | The form name
active | Boolean         | Is the form turned on
tags | String[]        | The tags on your form
internal_id | UUID (Optional) | Feathery-specific identifier for the form. Returned only for white label workspaces.
created_at | Datetime        | When this form was created
updated_at | Datetime        | When this form was last updated

## Create a Form

```python
import requests

url = "https://api.feathery.io/api/form/";
headers = {"Authorization": "Token <API KEY>"}
data = {
    "form_name": "My New Form",
    "template_form_id": "aaaaaa",
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
        'form_name': 'My New Form',
        'template_form_id': 'aaaaaa',
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
  "form_name": "My New Form",
  "template_form_id": "aaaaaa",
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
form_name | String | The name of the new form being created (must be unique)
template_form_id | String | The ID of the template form to copy from
steps | Array<Obj> | An array of steps to create
navigation_rules | Array<Obj> | An array of navigation rule connecting steps to be created
logic_rules | Array<Obj> (Optional) | An array of advanced logic rules to associate with the form
enabled | Boolean (Optional) | If the created form should be enabled or disabled. If not specified, will inherit the status of the template form.

Each `steps` object contains the following parameters.
The form element arrays allow you to edit attributes from the elements in the copied step.
If nothing is specified, the elements on the step will be copied as they are.

Parameter | Type | Description
--------- | --------- | -----------
step_id | String | The ID of the new step to create, unique to the form
template_step_id | Optional String | The ID of the step to copy from the original form. If not specified, a step will be auto-created with a single column layout.
origin | Optional Boolean | `true` if this step is the first of the form. Defaults to `false`
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
type | Optional String | The new [field type](https://docs.feathery.io/platform/build-forms/elements/fields#example)
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


Each `navigation_rules` `rules` object contains the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
comparison | String | Comparison - "equal", "not_equal"
field_key | String | The ID of the field whose value is being used for this comparison
value | String | The value to compare the field value against

Each `logic_rules` object contains the following parameters:

Parameter | Type                                                                                                       | Description                     
--------- |------------------------------------------------------------------------------------------------------------|---------------------------------
name | String                                                                                                     | The name of the logic rule
code | String | The Javascript code to run in the logic rule
trigger_event | 'change' &#124; 'load' &#124; 'form_complete' &#124; 'submit' &#124; 'error' &#124; 'view' &#124; 'action' | The event that triggers the logic rule to run
steps | String\[\] (Optional)                                                                                      | If `trigger_event` is 'submit' or 'load', the step IDs that will trigger the rule to run.
elements | String\[\] (Optional)                                                                                      | If `trigger_event` is 'change', 'error', 'view', 'or 'action', the elements that will trigger the rule to run.
description | String (Optional)                                                                                          | A description of the logic rule
index | Number (Optional) The execution order of the logic rule


### Response Body

The response will be an object containing the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
id | String | The form ID
name | String | The form name
internal_id | UUID (Optional) | Feathery-specific identifier for the form. Returned only for white label workspaces.

## Update a Form

```python
import requests

url = "https://api.feathery.io/api/form/<form_id>/";

data = {"enabled": False}

headers = {
    "Authorization": "Token <API KEY>",
    "Content-Type": "application/json",
}

result = requests.patch(url, data=data, headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.io/api/form/<form_id>/" \
    -X PATCH \
    -d "{'enabled': false}" \
    -H "Authorization: Token <API KEY>" \
    -H "Content-Type: application/json"
```

```javascript
const url = "https://api.feathery.io/api/form/<form_id>/";
const data = {"enabled": false}
const headers = {
    Authorization: "Token <API KEY>",
    "Content-Type": "application/json"
};
const options = {
    headers, 
    method: 'PATCH',
    body: JSON.stringify(data)
};
fetch(url, options)
    .then((response) => response.json())
    .then(result => console.log(result));
```

> The above command outputs JSON structured like this:

```json
{
  "enabled": false
}
```

Update a form's properties, including its status.

### HTTP Request

`POST https://api.feathery.io/api/form/<form_id>/`

### Request Body Parameters

Parameter | Type | Description
--------- | --------- | -----------
enabled | Boolean | Whether the form should be enabled or disabled

### Response Body
Same as request body parameters

## Create or Update Form Submissions

```python
import requests

url = "https://api.feathery.io/api/form/submission/";

# Using field ID or internal ID
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
fields | Object | A mapping from field identifier (ID or Internal ID) to associated field values to create. For a signature field, pass `{"file": <base64 string>, "name": <file name>}`
user_id | Optional String | A new or existing user ID. If not provided, a random ID will be generated and returned.
forms | Optional String[] | An array of form IDs to initialize submissions for
complete | Optional Boolean | If this submission is complete or incomplete. If the submission already exists and this flag is not specified, the completion status will not be changed.

### Response Body
Same as request body parameters

## List Form Submissions

```python
import requests

url = "https://api.feathery.io/api/form/submission/batch/abcdef/";
headers = {"Authorization": "Token <API KEY>"}
result = requests.get(url, headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.io/api/form/submission/batch/abcdef/" \
    -H "Authorization: Token <API KEY>"
```

```javascript
const url = "https://api.feathery.io/api/form/submission/batch/abcdef/";
const headers = { Authorization: "Token <API KEY>" };
fetch(url, { headers })
    .then((response) => response.json())
    .then(result => console.log(result));
```

> The above command outputs JSON structured like this:

```json
[{
  "values": [
    {
      "id": "TestField",
      "type": "text_field",
      "created_at": "2024-10-28T07:56:09.391398Z",
      "updated_at": "2024-10-28T16:39:32.577794Z",
      "value": "Test Value",
      "hidden": false,
      "display_text": "",
      "internal_id": "ef5ed054-73de-4463-ba61-82c36aca5afc",
    }
  ],
    "user_id": "131e7132-dg6d-4a8c-9d70-cgd493c2a368",
    "submission_start": "2024-10-30T02:07:32Z",
    "last_submitted": "2024-10-30T02:07:32Z",
}]
```

List submission data for a particular form

### HTTP Request

`GET https://api.feathery.io/api/form/submission/batch/<FORM ID>/`

### Request Query Parameters

Parameter | Type                                        | Description
--------- |---------------------------------------------| -----------
start_time | Datetime (Optional)                         | Limit submissions to after this start time
end_time | Datetime (Optional)                         | Limit submissions to before this end time
count | Number (Optional)                           | Limit the number of returned submissions to the specified number (sorted by either last submission time or similarity if fuzzy search is leveraged).
completed | Boolean (Optional)                          | If specified, only fetch submissions that are either completed or incomplete
field_search | {field_id: string; value: any}[] (Optional) | Fetch submissions with specific field values. Pass in a stringified array of objects, where each object specifies the field ID and the value to matched against.
fuzzy_search | JSON (Optional)                             | Fuzzy search allows you to fetch submissions whose field values are similar to search terms that you pass in. To leverage fuzzy search, pass in a stringified JSON object of the format described below. Fuzzy search is implemented via a trigram similarity score. Returned results will be sorted by similarity. There is a delay of a few seconds between when a field value is updated and when it is available to be fuzzy searched against.
fields | String (Optional)                           | Comma-separated list of field IDs. If specified, limit returned data to the specified field IDs.
no_field_values | Boolean (Optional)                          | Don't return field data. If this is enabled, you may fetch more records and the endpoint is more performant.
sort | String (Optional)                           | If "layout", the returned field values will be sorted in the way fields are laid out in the form. Otherwise, values will be sorted by field ID alphabetically.

### Fuzzy Search Parameters
Parameter | Type                                                  | Description
--------- |-------------------------------------------------------| -----------
threshold | Number                                                | A number between 0 and 1. Only submissions with a score higher than the threshold will be returned.
parameters | { field_id: string; term: string; weight: number; }[] | Each parameter specifies a field whose value will be compared against the included term. The weight is a number between 0 and 1 describing the importance of this parameter to the final score. The sum of all parameter weights must be 1.


### Response Body
An array of submission entries. The similarity score will be returned as well if fuzzy search is implemented.

## Create Hidden Field

```python
import requests

url = "https://api.feathery.io/api/form/hidden_field/";

data = {"field_id": "NewField"}

headers = {
    "Authorization": "Token <API KEY>",
    "Content-Type": "application/json",
}

result = requests.post(url, data=data, headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.io/api/form/hidden_field/" \
    -X POST \
    -d "{'field_id': 'NewField'}" \
    -H "Authorization: Token <API KEY>" \
    -H "Content-Type: application/json"
```

```javascript
const url = "https://api.feathery.io/api/form/hidden_field/";
const data = {"field_id": "NewField"}
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
  "field_id": "NewField"
}
```

Create a new hidden field in your Feathery account.

### HTTP Request

`POST https://api.feathery.io/api/form/hidden_field/`

### Request Body Parameters

Parameter | Type | Description
--------- | --------- | -----------
field_id | String | A new unique ID for the hidden field to create

### Response Body
Same as request body parameters

## Export Form Submission PDF

```python
import requests

url = "https://api.feathery.io/api/form/submission/pdf/";
data = {"form_id": "abcdef", "user_id": "alice@feathery.io"}
headers = {
    "Authorization": "Token <API KEY>",
    "Content-Type": "application/json",
}
result = requests.post(url, data=data, headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.io/api/form/submission/pdf/" \
    -X POST \
    -d "{'form_id': 'abcdef, 'user_id': 'alice@feathery.io'}" \
    -H "Authorization: Token <API KEY>" \
    -H "Content-Type: application/json"
```

```javascript
const url = "https://api.feathery.io/api/form/submission/pdf/";
const data = {"form_id": "abcdef", "user_id": "alice@feathery.io"}
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
{"pdf_url": "<PDF URL>"}
```

Create a PDF export for a specific form submission. The returned URL points to the generated PDF, with a few caveats:
* The PDF may not be immediately available. We recommend polling the URL up to 5 times, once per second, to ensure availability.
* Once a given submission is completed, the exported PDF will no longer be updated for it even if requested multiple times.

### HTTP Request

`POST https://api.feathery.io/api/form/submission/pdf/`

### Request Body Parameters

Parameter | Type   | Description
--------- |--------| -----------
form_id | String | The unique ID of the form whose submission you want to export.
user_id | String | The unique ID corresponding to a Feathery submission / user who you want to export.

### Response Body
Parameter | Type | Description
--------- |------| -----------
pdf_url | URL  | A URL where the PDF export can be downloaded from. The file may not be immediately available. 

# Logs

## List Recent API Connector Errors

```python
import requests

url = "https://api.feathery.io/api/logs/api-connector/<Form ID>/";
headers = {"Authorization": "Token <API KEY>"}
result = requests.get(url, headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.io/api/logs/api-connector/<Form ID>/" \
    -H "Authorization: Token <API KEY>"
```

```javascript
const url = "https://api.feathery.io/api/logs/api-connector/<Form ID>/";
const options = { headers: { Authorization: "Token <API KEY>" } };
fetch(url, options)
    .then((response) => response.json())
    .then(result => console.log(result));
```

> The above command outputs JSON structured like this:

```json
[
  {
    "url": "https://api.com",
    "status_code": 401,
    "request": "{'data': {'param1': ...",
    "response": "{'response': 'My m....",
    "created_at": "2020-06-03T00:00:00+00:00"
  }
]
```

List all recent response errors from API connector requests triggered from Feathery forms, up to the last week of data.

### HTTP Request

`GET https://api.feathery.io/api/logs/api-connector/<Form ID>/`

### Request Query Parameters

Parameter | Type                | Description
--------- |---------------------| -----------
start_time | Datetime (Optional) | Only return errors after this time.
end_time | Datetime (Optional) | Only return errors before this time.

### Response Body

The response will be an array of objects with the following parameters.

Parameter | Type        | Description
--------- |-------------| -----------
url | String      | The endpoint URL that returned an error
status_code | Number      | The erroring status code returned
request | JSON string | The API request parameters, truncated if above 400 characters
response | JSON string | The API response, truncated if above 400 characters
created_at | Datetime    | When this error was received.

## List Recently Sent Emails

```python
import requests

url = "https://api.feathery.io/api/logs/email/<Form ID>/";
headers = {"Authorization": "Token <API KEY>"}
result = requests.get(url, headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.io/api/logs/email/<Form ID>/" \
    -H "Authorization: Token <API KEY>"
```

```javascript
const url = "https://api.feathery.io/api/logs/email/<Form ID>/";
const options = { headers: { Authorization: "Token <API KEY>" } };
fetch(url, options)
    .then((response) => response.json())
    .then(result => console.log(result));
```

> The above command outputs JSON structured like this:

```json
[
  {
    "template_id": "alice@feathery.io",
    "recipients": "2020-06-01T00:00:00Z",
    "subject": "",
    "created_at": "2020-06-02T00:00:00Z"
  }
]
```

List all recent emails sent via Feathery's [email integration](https://feathery.io/integrations/email), up to the last week of data.

### HTTP Request

`GET https://api.feathery.io/api/logs/email/<Form ID>/`

### Request Query Parameters

Parameter | Type                | Description
--------- |---------------------| -----------
start_time | Datetime (Optional) | Only return emails sent after this time.
end_time | Datetime (Optional) | Only return emails sent before this time.

### Response Body

The response will be an array of objects with the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
template_id | String | The ID of the email template in the integration this email was based off of.
recipients | Datetime | The recipient addresses this email was sent to.
subject | Datetime | The subject of this email.
created_at | Datetime | When this email was sent.

## List Recent Quik Requests

```python
import requests

url = "https://api.feathery.io/api/logs/quik/<Form ID>/";
headers = {"Authorization": "Token <API KEY>"}
result = requests.get(url, headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.io/api/logs/quik/<Form ID>/" \
    -H "Authorization: Token <API KEY>"
```

```javascript
const url = "https://api.feathery.io/api/logs/quik/<Form ID>/";
const options = { headers: { Authorization: "Token <API KEY>" } };
fetch(url, options)
    .then((response) => response.json())
    .then(result => console.log(result));
```

> The above command outputs JSON structured like this:

```json
[
  {
    "url": "https://websvcs.quikforms.com/rest/quikformsengine/qfe/execute/html",
    "status_code": 200,
    "request": {"FormFields":  []},
    "response": {"data": []},
    "created_at": "2020-06-03T00:00:00+00:00"
  }
]
```

List all recent Quik integration requests and responses triggered from Feathery forms, up to the last week of data.

### HTTP Request

`GET https://api.feathery.io/api/logs/quik/<Form ID>/`

### Request Query Parameters

Parameter | Type                | Description
--------- |---------------------| -----------
start_time | Datetime (Optional) | Only return requests after this time.
end_time | Datetime (Optional) | Only return errors before this time.

### Response Body

The response will be an array of objects with the following parameters.

Parameter | Type        | Description
--------- |-------------| -----------
url | String      | The Quik API URL that returned an error
status_code | Number      | The status code returned
request | JSON string | The API request parameters
response | JSON string | The API response
created_at | Datetime    | When this request was made.

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

### Response Body

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
    "id": "TextField1",
    "type": "text_field",
    "display_text": "What is your name?",
    "value": "Alice Smith",
    "created_at": "2020-06-01T00:00:00Z",
    "updated_at": "2020-06-02T00:00:00Z",
    "internal_id": "50c15c23-7558-4d51-810a-1a02dlf0bf58",
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

### Response Body

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
internal_id | String                 | Feathery-internal identifier of your field. Always static

## Get User Form Session

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

Get form session data for a user, including all forms and their progress

### HTTP Request

`GET https://api.feathery.io/api/user/<user_id>/session/`

### Response Body

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

### Response Body

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

# Workspaces

### Workspace Object

Parameter | Type                                                                                                                                                   | Description                                                                                                                                                    
--------- |--------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------
id | UUID                                                                                                                                                   | Your unique workspace ID                                                                                                                                       
name | String                                                                                                                                                 | The human-readable name of the workspace, e.g. "Company 1"                                                                                                     
logo | URL                                                                                                                                                    | A URL to the logo to display in this workspace                                                                                                                 
brand_url | URL                                                                                                                                                    | A link to the brand website                                                                                                                                    
brand_favicon | URL                                                                                                                                                    | A link to the brand favicon to display                                                                                                                         
brand_name | String                                                                                                                                                 | The name of the white label brand                                                                                                                              
brand_primary_color | Hex Color                                                                                                                                              | 6-digit hex code of the primary color of the brand                                                                                                             
brand_secondary_color | Hex Color                                                                                                                                              | 6-digit hex code of the secondary color of the brand                                                                                                           
features | Object                                                                                                                                                 | Key-value pairs of account features. Available options are `live_forms` (# of live forms available), `submissions` (# of monthly submissions available), and `ab_testing` (is AB testing enabled)
disabled_global_tabs | ('themes' &#124; 'ab_tests' &#124; 'all_users')[]                                                                                                      | Hide specific tabs from the global workspace dashboard.
disabled_form_tabs | ('flow' &#124; 'logic' &#124; 'api_connectors' &#124; 'integrations' &#124; 'results' &#124; 'settings')[]                                             | Hide specific tabs from the form editor.                                                                                                                       
disabled_form_settings | ('form_properties' &#124; 'form_behavior' &#124; 'user_tracking' &#124; 'data_tracking' &#124; 'seo' &#124; 'international_support' &#124; 'form_promotion' &#124; 'delete')[] | Hide specific tabs from form settings.                                                                                                                         
disabled_form_elements | ElementType[]                                                                                                                                          | Hide specific elements from the form designer element selector. Element type enums can be found [here](https://docs.feathery.io/platform/build-forms/elements). 
metadata | Object                                                                                                                                                 | Key-value pairs of arbitrary metadata to configure and identify this workspace                                                                                 
accounts | {id: string; email: string; role: string}[]                                                                                                            | A list of accounts in this workspace                                                                                                                           
test | Boolean                                                                                                                                                | Is this a test workspace (created via test API key)                                                                                                            
created_at | Datetime                                                                                                                                               | When this workspace was created                                                                                                                                

## List All Workspaces

```python
import requests

url = "https://api.feathery.io/api/workspace/";
headers = {"Authorization": "Token <API KEY>"}
result = requests.get(url, headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.io/api/workspace/" \
    -H "Authorization: Token <API KEY>"
```

```javascript
const url = "https://api.feathery.io/api/workspace/";
const options = { headers: { Authorization: "Token <API KEY>" } };
fetch(url, options)
    .then((response) => response.json())
    .then(result => console.log(result));
```

> The above command outputs JSON structured like this:

```json
[
  {
    "id": "<WORKSPACE UUID>",
    "name": "Workspace 1",
    "logo": "https://url-to-logo.com",
    "brand_url": "https://feathery.io",
    "brand_favicon": "https://feathery.io/favicon.ico",
    "brand_name": "Brand 1",
    "brand_primary_color": "FFFFFF",
    "brand_secondary_color": "FFFFFF",
    "features": {},
    "disabled_global_tabs": [],
    "disabled_form_tabs": [],
    "disabled_form_settings": [],
    "disabled_form_elements": [],
    "metadata": {"tag1": "value"},
    "accounts": [{"email": "user@mail.com", "role": "admin"}],
    "created_at": "2020-06-01T00:00:00+00:00"
  }
]
```

List all of the Feathery workspaces connected to your main account. This is only available for Feathery's white label product. If querying with the test API key, will only return test workspaces.

### HTTP Request

`GET https://api.feathery.io/api/workspace/`

### Request Query Parameters
Parameter | Type               | Description
--------- |--------------------| -----------
submission_data | Boolean (Optional) | If set to true, each workspace's monthly submission data will be fetched as well.

### Response Body

The response will be an array of workspace objects with the following parameters.

Parameter | Type                                        | Description
--------- |---------------------------------------------| -----------
[Workspace Object](#workspace-object) | | Parameters from the Workspace Object definition
monthly_submissions | Number (Optional) | If submission_data is requested, the number of submissions in the workspace on the current monthly billing cycle is returned.
monthly_submission_usage | Number (Optional) | If submission data is requested, the percentage of the current month's submissions that have been used.
submission_cycle_start | Date (Optional) | If submission data is requested, the start date of the most recent billing cycle

## Create a Workspace

```python
import requests

url = "https://api.feathery.io/api/workspace/";
data = {"name": "New Workspace"}
headers = {
    "Authorization": "Token <API KEY>",
    "Content-Type": "application/json",
}
result = requests.post(url, data=data, headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.io/api/workspace/" \
    -X POST \
    -d "{'name': 'New Workspace'}" \
    -H "Authorization: Token <API KEY>" \
    -H "Content-Type: application/json"
```

```javascript
const url = "https://api.feathery.io/api/workspace/";
const data = {name: "New Workspace"}
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
  "id": "<WORKSPACE UUID>",
  "name": "Workspace 1",
  "logo": "https://url-to-logo.com",
  "brand_url": "https://feathery.io",
  "brand_favicon": "https://feathery.io/favicon.ico",
  "brand_name": "Brand 1",
  "brand_primary_color": "FFFFFF",
  "brand_secondary_color": "FFFFFF",
  "features": {},
  "disabled_global_tabs": [],
  "disabled_form_tabs": [],
  "disabled_form_settings": [],
  "disabled_form_elements": [],
  "metadata": {"tag1": "value"},
  "accounts": [{"email": "user@mail.com", "role": "admin"}],
  "created_at": "2020-06-01T00:00:00+00:00"
}
```

Create a new workspace connected to your main account. If querying with the test API key, will create a test workspace.

### HTTP Request

`POST https://api.feathery.io/api/workspace/`

### Body Parameters

Parameter | Type                                                                                                                                                                | Description
--------- |---------------------------------------------------------------------------------------------------------------------------------------------------------------------| -----------
name | String                                                                                                                                                              | The human-readable name of the workspace, e.g. "Company 1"
logo | URL (Optional)                                                                                                                                                      | A URL to the logo to display in this workspace
brand_url | URL (Optional)                                                                                                                                                      | A link to the brand website
brand_favicon | URL (Optional)                                                                                                                                                      | A link to the brand favicon to display
brand_name | String (Optional)                                                                                                                                                   | The name of the white label brand
brand_primary_color | Hex Color (Optional)                                                                                                                                                | 6-digit hex code of the primary color of the brand
brand_secondary_color | Hex Color (Optional)                                                                                                                                                | 6-digit hex code of the secondary color of the brand
features | Object (Optional)                                                                                                                                                   | Key-value pairs of account features. Available options are `live_forms` (# of live forms available), `submissions` (# of monthly submissions available), and `ab_testing` (is AB testing enabled)
disabled_global_tabs | ('themes' &#124; 'ab_tests' &#124; 'all_users')\[\] (Optional)                                                                                                      | Hide specific tabs from the global workspace dashboard.
disabled_form_tabs | ('flow' &#124; 'integrations' &#124; 'results' &#124; 'settings')\[\] (Optional)                                                                                    | Hide specific tabs from the form editor.
disabled_form_settings | ('form_properties' &#124; 'form_behavior' &#124; 'user_tracking' &#124; 'data_tracking' &#124; 'seo' &#124; 'international_support' &#124; 'form_promotion' &#124; 'delete')\[\] (Optional) | Hide specific tabs from form settings.
disabled_form_elements | ElementType\[\] (Optional)                                                                                                                                          | Hide specific elements from the form designer element selector. Element type enums can be found [here](https://docs.feathery.io/platform/build-forms/elements).
metadata | Object (Optional)                                                                                                                                                   | Key-value pairs of arbitrary metadata to configure and identify this workspace

### Response Body
Parameter | Type                  | Description
--------- |-----------------------| -----------
[Workspace Object](#workspace-object) | | Parameters from the Workspace Object definition

## Retrieve a Workspace

```python
import requests

url = "https://api.feathery.io/api/workspace/<workspace_id>/";
headers = {"Authorization": "Token <API KEY>"}
result = requests.get(url, headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.io/api/workspace/<workspace_id>/" \
    -H "Authorization: Token <API KEY>"
```

```javascript
const url = "https://api.feathery.io/api/workspace/<workspace_id>/";
const options = { headers: { Authorization: "Token <API KEY>" } };
fetch(url, options)
    .then((response) => response.json())
    .then(result => console.log(result));
```

> The above command outputs JSON structured like this:

```json
{
  "id": "<WORKSPACE UUID>",
  "name": "Workspace 1",
  "logo": "https://url-to-logo.com",
  "brand_url": "https://feathery.io",
  "brand_favicon": "https://feathery.io/favicon.ico",
  "brand_name": "Brand 1",
  "brand_primary_color": "FFFFFF",
  "brand_secondary_color": "FFFFFF",
  "features": {},
  "disabled_global_tabs": [],
  "disabled_form_tabs": [],
  "disabled_form_settings": [],
  "disabled_form_elements": [],
  "metadata": {"tag1": "value"},
  "live_api_key": "<LIVE API KEY>",
  "test_api_key": "<TEST API KEY>",
  "live_sdk_key": "<LIVE SDK KEY>",
  "test_sdk_key": "<TEST SDK KEY>",
  "accounts": [{"email": "user@mail.com", "role": "admin"}],
  "created_at": "2020-06-01T00:00:00+00:00"
}
```

Retrieve a specific Feathery workspace connected to your main account, including its API keys. This is only available for Feathery's white label product. If querying with the test API key, will only return test API keys.

### HTTP Request

`GET https://api.feathery.io/api/workspace/<workspace_id>/`

### URL Parameters

Parameter | Description
--------- | -----------
workspace_id | The ID of the workspace to retrieve

### Request Query Parameters
Parameter | Type               | Description
--------- |--------------------| -----------
submission_data | Boolean (Optional) | If set to true, each workspace's monthly submission data will be fetched as well.

### Response Body

The response will be an array of objects with the following parameters.

Parameter | Type                                        | Description
--------- |---------------------------------------------| -----------
[Workspace Object](#workspace-object) | | Parameters from the Workspace Object definition
live_api_key | String                                      | The live API key of the workspace which can be used to call APIs on its behalf.
test_api_key | String                                      | The test API key of the workspace which can be used to call APIs on its behalf.
live_sdk_key | String                                      | The live SDK key of the workspace which can be used to embed workspace forms.
test_sdk_key | String                                      | The test API key of the workspace which can be used to embed workspace forms.
monthly_submissions | Number (Optional) | If submission_data is requested, the number of submissions in the workspace on the current monthly billing cycle is returned.
monthly_submission_usage | Number (Optional) | If submission data is requested, the percentage of the current month's submissions that have been used.
submission_cycle_start | Date (Optional) | If submission data is requested, the start date of the most recent billing cycle

## Update a Workspace

```python
import requests

url = "https://api.feathery.io/api/workspace/<workspace_id>/";
data = {"brand_url": "https://feathery.io"}
headers = {
    "Authorization": "Token <API KEY>",
    "Content-Type": "application/json",
}
result = requests.post(url, data=data, headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.io/api/workspace/<workspace_id>/" \
    -X POST \
    -d "{'brand_url': 'https://feathery.io'}" \
    -H "Authorization: Token <API KEY>" \
    -H "Content-Type: application/json"
```

```javascript
const url = "https://api.feathery.io/api/workspace/<workspace_id>/";
const data = {brand_url: "https://feathery.io"}
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
  "id": "<WORKSPACE UUID>",
  "name": "Workspace 1",
  "logo": "https://url-to-logo.com",
  "brand_url": "https://feathery.io",
  "brand_favicon": "https://feathery.io/favicon.ico",
  "brand_name": "Brand 1",
  "brand_primary_color": "FFFFFF",
  "brand_secondary_color": "FFFFFF",
  "features": {},
  "disabled_global_tabs": [],
  "disabled_form_tabs": [],
  "disabled_form_settings": [],
  "disabled_form_elements": [],
  "metadata": {"tag1": "value"},
  "accounts": [{"email": "user@mail.com", "role": "admin"}],
  "created_at": "2020-06-01T00:00:00+00:00"
}
```

Update an existing workspace connected to your main account.

### HTTP Request

`POST https://api.feathery.io/api/workspace/<workspace_id>/`

### URL Parameters

Parameter | Description
--------- | -----------
workspace_id | The ID of the workspace to update

### Body Parameters

Parameter | Type                                                                                                                                                                | Description
--------- |---------------------------------------------------------------------------------------------------------------------------------------------------------------------| -----------
name | String (Optional)                                                                                                                                                   | The human-readable name of the workspace, e.g. "Company 1"
logo | URL (Optional)                                                                                                                                                      | A URL to the logo to display in this workspace
brand_url | URL (Optional)                                                                                                                                                      | A link to the brand website
brand_favicon | URL (Optional)                                                                                                                                                      | A link to the brand favicon to display
brand_name | String (Optional)                                                                                                                                                   | The name of the white label brand
brand_primary_color | Hex Color (Optional)                                                                                                                                                | 6-digit hex code of the primary color of the brand
brand_secondary_color | Hex Color (Optional)                                                                                                                                                | 6-digit hex code of the secondary color of the brand
features | Object (Optional)                                                                                                                                                   | Key-value pairs of account features. Available options are `live_forms` (# of live forms available), `submissions` (# of monthly submissions available), and `ab_testing` (is AB testing enabled)
disabled_global_tabs | ('themes' &#124; 'ab_tests' &#124; 'all_users')\[\] (Optional)                                                                                                      | Hide specific tabs from the global workspace dashboard.
disabled_form_tabs | ('flow' &#124; 'integrations' &#124; 'results' &#124; 'settings')\[\] (Optional)                                                                                    | Hide specific tabs from the form editor.
disabled_form_settings | ('form_properties' &#124; 'form_behavior' &#124; 'user_tracking' &#124; 'data_tracking' &#124; 'seo' &#124; 'international_support' &#124; 'delete')\[\] (Optional) | Hide specific tabs from form settings.
disabled_form_elements | ElementType\[\] (Optional)                                                                                                                                          | Hide specific elements from the form designer element selector. Element type enums can be found [here](https://docs.feathery.io/platform/build-forms/elements).
metadata | Object (Optional)                                                                                                                                                   | Key-value pairs of arbitrary metadata to configure and identify this workspace

### Response Body
Parameter | Type   | Description
--------- |--------| -----------
[Workspace Object](#workspace-object) | | Parameters from the Workspace Object definition

## Delete a Workspace

```python
import requests

url = "https://api.feathery.io/api/workspace/<workspace_id>/";
headers = {
    "Authorization": "Token <API KEY>",
    "Content-Type": "application/json",
}
result = requests.delete(url, headers=headers)
print(result.status_code)
```

```shell
curl "https://api.feathery.io/api/workspace/<workspace_id>/" \
    -X DELETE \
    -H "Authorization: Token <API KEY>" \
    -H "Content-Type: application/json"
```

```javascript
const url = "https://api.feathery.io/api/workspace/<workspace_id>/";
const headers = {
    Authorization: "Token <API KEY>",
    "Content-Type": "application/json"
};
const options = { headers, method: 'DELETE' };
fetch(url, options).then((response) => console.log(response.status))
```

> The above command does not return a response body

Delete a workspace connected to your main Feathery account.

### HTTP Request

`DELETE https://api.feathery.io/api/workspace/<workspace_id>/`

### URL Parameters

Parameter | Description
--------- | -----------
workspace_id | The ID of the workspace to delete

## Generate a Workspace Login Token

```python
import requests

url = "https://api.feathery.io/api/workspace/<workspace_id>/auth/";
data = {"account_id": "<ACCOUNT_ID>"}
headers = {
    "Authorization": "Token <API KEY>",
    "Content-Type": "application/json",
}
result = requests.post(url, data=data, headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.io/api/workspace/<workspace_id>/auth/" \
    -X POST \
    -d "{'account_id': '<ACCOUNT_ID>'}" \
    -H "Authorization: Token <API KEY>" \
    -H "Content-Type: application/json"
```

```javascript
const url = "https://api.feathery.io/api/workspace/<workspace_id>/auth/";
const data = {account_id: "<ACCOUNT_ID>"}
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
  "account_id": "<ACCOUNT ID>",
  "token": "<JWT TOKEN>",
}
```

Generate a login JWT token corresponding to an account in one of your workspaces.
This token can be used to automatically log the user into the Feathery application.

### HTTP Request

`POST https://api.feathery.io/api/workspace/<workspace_id>/auth/`

### URL Parameters

Parameter | Description
--------- | -----------
workspace_id | The ID of the workspace that the account token belongs to.

### Body Parameters

Parameter | Type          | Description
--------- |---------------| -----------
account_id | String (UUID) | The unique ID of the account to generate a login token for.

### Response Body
Parameter | Type          | Description
--------- |---------------| -----------
account_id | String (UUID) | The unique ID of the account
token | String        | A JWT token that can be passed to the Feathery dashboard to automatically log the account in.

## Populate Workspace with Template Form

```python
import requests

url = "https://api.feathery.io/api/workspace/<workspace_id>/create-template-form/";
data = {"template_id": "<TEMPLATE ID>", "form_name": "My new form"}
headers = {
    "Authorization": "Token <API KEY>",
    "Content-Type": "application/json",
}
result = requests.post(url, data=data, headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.io/api/workspace/<workspace_id>/create-template-form/" \
    -X POST \
    -d "{'template_id': '<TEMPLATE_ID>', 'form_name': 'My new form'}" \
    -H "Authorization: Token <API KEY>" \
    -H "Content-Type: application/json"
```

```javascript
const url = "https://api.feathery.io/api/workspace/<workspace_id>/create-template-form/";
const data = {template_id: "<TEMPLATE_ID>", form_name: 'My new form'}
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

Create a form in your workspace from one of your admin account forms or [custom templates](https://docs.feathery.io/platform/white-label-feathery/offer-custom-form-templates).

### HTTP Request

`POST https://api.feathery.io/api/workspace/<workspace_id>/create-template-form/`

### URL Parameters

Parameter | Description
--------- | -----------
workspace_id | The ID of the workspace to create the form in.

### Body Parameters

Parameter | Type         | Description
--------- |--------------| -----------
form_name | String | The name of the new form to create.
template_id | String (UUID) | The ID of the form template to populate in the workspace.

### Response Body

The response will be an object containing the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
id | String | The form ID
name | String | The form name
internal_id | UUID | Feathery-specific identifier for the form
