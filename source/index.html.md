---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - ruby
  - python
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Kittn API! You can use our API to access Kittn API endpoints, which can get information on various cats, kittens, and breeds in our database.

We have language bindings in Shell, Ruby, Python, and JavaScript! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

This example API documentation page was created with [Slate](https://github.com/lord/slate). Feel free to edit it and use it as a base for your own API's documentation.

# Authentication

> To authorize, use this code:

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
```

> Make sure to replace `meowmeowmeow` with your API key.

Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Delete a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete

# API Test Case

## Overview

API test case is used to test if all the API endpoints are working correctly.

`APITestCase` class is used to create the test cases. 
The test cases files are named `test_api.py` and are located in `src/ui/<model>` of each model. 

### How to Run

> Sample command:

```shell
cd otsecurity/dashboard/otdashboard
python manage.py test -p test_api.py
```

1. cd to your working directory
2. python manage.py test -p test_api.py

### Result

> Sample result:

```shell
Ran 20 tests in 2.630s

OK
Destroying test database for alias 'default'
```

If all the test cases are passed, it will return "OK" and the number of tests ran. Otherwise it will return the number of errors
and the detail of each error.

### Error

>Sample error:

```shell
FAIL: test_get_single_activity (src.ui.activity.test_api.NotificationTest)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "dashboard/otdashboard/src/ui/activity/test_api.py", line 48, in test_get_single_activity
    self.assertEqual(response.status_code, status.HTTP_200_OK)
AssertionError: 403 != 200

```

The test cases are run in parallel meaning that 1 error doesn't stop the whole process. 
Errors from different test cases can be found in one test run. Error occurs when the expected outcome
doesn't match the response. Sample error is shown on the right.

## How It Works

```python
def setUp(self):         
        User.objects.create_user(username='singtel',password='P@ssw0rd')
        call_command('loaddata', 'fixtures/init/activitytype.json', verbosity=0)
        Activity.objects.create(pk=1, time_created="1970-01-01T00:00:00", description='a', type=ActivityType.objects.get(pk=1), source='test', src_ip='123')

def test_get_all_activities(self):
        factory = APIRequestFactory()
        user = User.objects.get(username='singtel')
        request = factory.get('/activities')
        content_type = ContentType.objects.get_for_model(Activity)
        permission = Permission.objects.get(content_type=content_type, codename='view_activity')
        user.user_permissions.add(permission)
        permitteduser = User.objects.get(username='singtel')
        force_authenticate(request, user=permitteduser)
        response = ActivityList.as_view()(request)
        activities = Activity.objects.all()
        serializer = ActivitySerializer(activities, many=True)
        self.assertEqual(response.status_code, status.HTTP_200_OK)
        self.assertEqual(response.data.get('results'), serializer.data)
```
This explains how the test cases work. 

You can see a sample test case code block on the python tab.

### setUp

In the `setUp()` function, you can initialize objects to be used in the test cases. We need to create
the User, model to be tested (for example, Activity), and necessary models to be used by the tested model 
(for example, ActivityType as it is a foreignkey of Activity).

The data can be generated either by `Model.object.create()` or `call_command('loaddata')`

### test_function
1. Request is made using `APIRequestFactory` class by specifying the method and endpoint.
2. Get the user initialized in the setUp.
3. Add the necessary permissions to the user.
4. Authenticate the user with force_authenticate() function.
6. Get the data to be compared with the response from the database.
7. Compare the response data with the data from the database.

<aside class="notice">
For more information on Django API Test please visit <a href=http://www.django-rest-framework.org/api-guide/testing/>this</a>
</aside>


