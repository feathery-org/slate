---
title: Feathery API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - python
  - javascript

toc_footers:
  - <a href='https://feathery.tech'>Sign up for an API key</a>

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
You can register a new API key after joining our [waitlist](https://feathery.tech). 

API keys are environment-specific (production, development, etc.), which allows you to
separate real from test data. All API keys have access to the same fields and forms,
but users are environment-specific.

Include your API key as a request header that looks like the following:

`Authorization: <API KEY>`

<aside class="notice">
You must replace <code>&lt;API KEY&gt;</code> with your personal API key.
</aside>

# Users

## List All Users

```python
import requests

url = "https://api.feathery.tech/api/user/";
headers = {"Authorization": "Token <API KEY>"}
result = requests.get(url, headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.tech/api/user/" \
    -H "Authorization: Token <API KEY>"
```

```javascript
const url = "https://api.feathery.tech/api/user/";
const options = { headers: { Authorization: "Token <API KEY>" } };
fetch(url, options)
    .then((response) => response.json())
    .then(result => console.log(result));
```

> The above command outputs JSON structured like this:

```json
[
  {
    "key": "alice@feathery.tech",
    "name": "Alice",
    "created_at": "2020-06-01T00:00:00Z",
    "updated_at": "2020-06-02T00:00:00Z"
  },
  {
    "key": "bob@feathery.tech",
    "name": "Bob",
    "created_at": "2020-06-03T00:00:00Z",
    "updated_at": "2020-06-04T00:00:00Z"
  }
]
```

List all of your users in Feathery.

### HTTP Request

`GET https://api.feathery.tech/api/user/`

### Response Parameters

The response will be an array of objects with the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
key | String | Your unique user key
name | String (Optional) | Your human-friendly user name
created_at | Datetime | When this user was created
updated_at | Datetime | When this user was last updated

## Create and Fetch a User

```python
import requests

url = "https://api.feathery.tech/api/user/";
data = {"key": "alice@feathery.tech", "name": "Alice 2.0"}
headers = {
    "Authorization": "Token <API KEY>",
    "Content-Type": "application/json",
}
result = requests.post(url, data=data, headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.tech/api/user/" \
    -X POST \
    -d "{'key': 'alice@feathery.tech', 'name': 'Alice 2.0'}" \
    -H "Authorization: Token <API KEY>" \
    -H "Content-Type: application/json" \
```

```javascript
const url = "https://api.feathery.tech/api/user/";
const data = {key: "alice@feathery.tech", name: "Alice 2.0"}
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
  "key": "alice@feathery.tech",
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

`POST https://api.feathery.tech/api/user/`

### Body Parameters

Parameter | Type | Description
--------- | --------- | -----------
key | String | Your unique user key
name | String (Optional) | Your human-friendly user name.

### Response Parameters

Parameter | Type | Description
--------- | --------- | -----------
api_key | String | This user's API key
key | String | Your unique user key
name | String (Optional) | Your human-friendly user name
created_at | Datetime | When this user was created
updated_at | Datetime | When this user was last updated

## Delete a User

```python
import requests

url = "https://api.feathery.tech/api/user/alice@feathery.tech/";
headers = {
    "Authorization": "Token <API KEY>",
    "Content-Type": "application/json",
}
result = requests.delete(url, headers=headers)
print(result.status_code)
```

```shell
curl "https://api.feathery.tech/api/user/alice@feathery.tech/" \
    -X DELETE \
    -d "{'key': 'alice@feathery.tech'}" \
    -H "Authorization: Token <API KEY>" \
    -H "Content-Type: application/json" \
```

```javascript
const url = "https://api.feathery.tech/api/user/alice@feathery.tech/";
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

`DELETE https://api.feathery.tech/api/user/<key>/`

### URL Parameters

Parameter | Description
--------- | -----------
key | The key of the user to delete


# Fields

## List All Fields for a User 

```python
import requests

url = "https://api.feathery.tech/api/field/?key=alice@feathery.tech";
headers = {"Authorization": "Token <API KEY>"}
result = requests.get(url, headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.tech/api/field/?key=alice@feathery.tech" \
    -H "Authorization: Token <API KEY>"
```

```javascript
const url = "https://api.feathery.tech/api/field/?key=alice@feathery.tech";
const options = { headers: { Authorization: "Token <API KEY>" } };
fetch(url, options)
    .then((response) => response.json())
    .then(result => console.log(result));
```

> The above command outputs JSON structured like this:

```json
[
  {
    "key": "name",
    "type": "text_field",
    "display_text": "What is your name?",
    "value": "Alice Smith",
    "created_at": "2020-06-01T00:00:00Z",
    "updated_at": "2020-06-02T00:00:00Z"
  },
  {
    "key": "age",
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

`GET https://api.feathery.tech/api/field/`

### Request Query Parameters

Parameter | Type | Description
--------- | --------- | -----------
key | String (Optional) | Your unique user key

### Response Parameters

The response will be an array of objects with the following parameters.

Parameter | Type | Description
--------- | --------- | -----------
key | String | Your unique field key
type | String Enum | The field type (e.g. text_area, dropdown, etc.)
display_text | String (Optional) | Human-friendly text to display for this field
value | Polymorphic (Optional) | Submitted value of the user whose key was passed in.
created_at | Datetime | When this field was created
updated_at | Datetime | When this field was last updated

## Create Field Value for User

```python
import requests

url = "https://api.feathery.tech/api/field/alice@feathery.tech/";
data = {"key": "age", "value": 21}
headers = {
    "Authorization": "Token <API KEY>",
    "Content-Type": "application/json",
}
result = requests.post(url, data=data, headers=headers)
print(result.json())
```

```shell
curl "https://api.feathery.tech/api/field/alice@feathery.tech/" \
    -X POST \
    -d "{'key': 'age', 'value': 21}" \
    -H "Authorization: Token <API KEY>" \
    -H "Content-Type: application/json" \
```

```javascript
const url = "https://api.feathery.tech/api/field/alice@feathery.tech/";
const data = {key: "age", value: 21}
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
  "key": "age",
  "value":  21,
  "created_at": "2020-06-01T00:00:00Z",
  "updated_at": "2020-06-03T00:00:00Z"
}
```

Create or update the value of a field for a specific user

### HTTP Request

`POST https://api.feathery.tech/api/field/<key>/`

### URL Parameters

Parameter | Description
--------- | -----------
key | Your unique user key

### Request Body Parameters

Parameter | Type | Description
--------- | --------- | -----------
key | String | Your unique field key
value | Polymorphic | Field value to associate with the specified user

### Response Parameters

Parameter | Type | Description
--------- | --------- | -----------
key | String | Your unique field key
value | Polymorphic (Optional) | Field value associated with the specified user
created_at | Datetime | When this value was created
updated_at | Datetime | When this value was last updated
