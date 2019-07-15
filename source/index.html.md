---
title: Supervisor API Docs

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - ruby
  - python
  - javascript

toc_footers:
  - <a href='https://supervisor.reviewshake.com/signup'>Sign up for an API key</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Supervisor API! You can use our API to access Supervisor endpoints, which include:

- Review Scraper API
- Review Response API
- Review Insights API
- URL Scraper API
- Brand Audit API

# Authentication

> To authorize, you will need to use your `spiderman-token` with every request.

```shell
# With shell, you can just pass the correct header with each request
curl "https://supervisor.reviewshake.com/api/v2"
  -H "spiderman-token: 1234567890"
```

> Make sure to replace `1234567890` with your API key.

Supervisor uses API keys to allow access to the API. You can register for an API key at our [developer portal](https://supervisor.reviewshake.com/signup).

We expect for the API key to be included in all requests to the server in a header that looks like the following:

`spiderman-token: 1234567890`

<aside class="notice">
You must replace <code>1234567890</code> with your personal API key.
</aside>

# Brand audit

## Add company

Add a company that you wish to receive a brand audit for.

```ruby
require 'uri'
require 'net/http'

url = URI("https://supervisor.reviewshake.com/api/v2/brand_audit/add?company_name=Brooklyn%20Burgers%20& Beer=&phone_number=%28718%29%20788-1458&postcode=11215&%20Beer=")

http = Net::HTTP.new(url.host, url.port)

request = Net::HTTP::Post.new(url)
request["spiderman-token"] = '1234567890'

response = http.request(request)
puts response.read_body
```

```python
import requests

url = "https://supervisor.reviewshake.com/api/v2/brand_audit/add"

querystring = {"company_name":"Brooklyn%20Burgers%20"," Beer":"","phone_number":"%28718%29%20788-1458","postcode":"11215","%20Beer":""}

headers = {
    'spiderman-token': "1234567890"
    }

response = requests.request("POST", url, headers=headers, params=querystring)

print(response.text)
```

```shell
curl --request POST --url 'https://supervisor.reviewshake.com/api/v2/brand_audit/add?company_name=Brooklyn%20Burgers%20& Beer=&phone_number=%28718%29%20788-1458&postcode=11215&%20Beer=' --header 'spiderman-token: 1234567890'
```

```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "https://supervisor.reviewshake.com/api/v2/brand_audit/add?company_name=Brooklyn%20Burgers%20& Beer=&phone_number=%28718%29%20788-1458&postcode=11215&%20Beer=",
  "method": "POST",
  "headers": {
    "spiderman-token": "1234567890"
  }
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

> The above command returns JSON structured like this:

```json
{
    "success": true,
    "job_id": 92724,
    "status": 200,
    "message": "Added brand audit to the queue..."
}
```

### HTTP Request

`POST https://supervisor.reviewshake.com/api/v2/brand_audit/add`

### Request Body

Key | Description
--------- | -----------
company_name | The name of the company you would like the brand audit for
phone_number | The phone number of the company you would like the brand audit for
postcode | The postcode of the company you would like the brand audit for

<aside class="info">
Remember to take note of the <code>job_id</code>, as you will need this to GET the status for this brand audit.
</aside>

### Response
The response contains the following keys:

Key | Description
--------- | -----------
success | `true` or `false` depending on outcome
job_id | The `job_id` assigned to the scrape
status | HTTP code for status, eg. `200`
message | Text response

## Get info

```ruby
require 'uri'
require 'net/http'

url = URI("https://supervisor.reviewshake.com/api/v2/brand_audit/info?job_id=92724")

http = Net::HTTP.new(url.host, url.port)

request = Net::HTTP::Get.new(url)
request["spiderman-token"] = '1234567890'

response = http.request(request)
puts response.read_body
```

```python
import requests

url = "https://supervisor.reviewshake.com/api/v2/brand_audit/info"

querystring = {"job_id":"92724"}

headers = {
    'spiderman-token': "1234567890"
    }

response = requests.request("GET", url, headers=headers, params=querystring)

print(response.text)
```

```shell
curl --request GET --url 'https://supervisor.reviewshake.com/api/v2/brand_audit/info?job_id=92724' --header 'spiderman-token: 1234567890'
```

```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "https://supervisor.reviewshake.com/api/v2/brand_audit/info?job_id=92724",
  "method": "GET",
  "headers": {
    "spiderman-token": "1234567890"
  }
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

> The above command returns JSON structured like this:

```json
xxx
```

> You will receive the following message if the provided `job_id` does not exist:

```json
{
    "success": false,
    "status": 400,
    "message": "This job ID doesn't exist, please create it"
}
```

This endpoint retrieves information for a given response `job_id`.

### HTTP Request

`GET https://supervisor.reviewshake.com/api/v2/brand_audit/info?job_id=92724`

### URL Parameters

Parameter | Description
--------- | -----------
job_id | The ID of the job to retrieve

### Response
The response contains the following keys:

Key | Description
--------- | -----------
success | `true` or `false` depending on outcome
status | HTTP code for status, eg. `200`

### `status` values

The `status` returned can be one of the following:

`crawl_status` | Description
--------- | -----------
`pending` | The job is still in the queue and is pending completion
`complete` | The job is complete
`maintenance` | There has been an issue with the brand audit - our team is automatically notified for investigation
`invalid_company` | The company you provided is invalid

# Insights

## Add insight

Use our API to receive machine learning/natural language processing insights built for reviews.

```ruby
require 'uri'
require 'net/http'

url = URI("https://supervisor.reviewshake.com/api/v2/insights/add?review_text=Artichoke%20Basille%27s%20is%20the%20perfect%20spot%20to%20go%20if%20you%27re%20looking%20for%20some%20late%20night%20eats.%20I%20came%20here%20after%20my%20graduation%20ceremony%20because%20I%20was%20craving%20a%20decent%20slice%20and%20dollar%20pizza%20wasn%27t%20going%20to%20cut%20it%21%20I%20ordered%20the%20pepperoni%20and%20margharita%20Sicilian.%20The%20price%20for%20a%20slice%20averages%20from%20$5-6.50.%20This%20may%20seem%20pricey%20but%20you%20can%20taste%20the%20quality,%20also%20these%20slices%20are%20not%20your%20average%20size...%20they%27re%20HUGE.&model=restaurants")

http = Net::HTTP.new(url.host, url.port)

request = Net::HTTP::Post.new(url)
request["spiderman-token"] = '1234567890'

response = http.request(request)
puts response.read_body
```

```python
import requests

url = "https://supervisor.reviewshake.com/api/v2/insights/add"

querystring = {"review_text":"Artichoke%20Basille%27s%20is%20the%20perfect%20spot%20to%20go%20if%20you%27re%20looking%20for%20some%20late%20night%20eats.%20I%20came%20here%20after%20my%20graduation%20ceremony%20because%20I%20was%20craving%20a%20decent%20slice%20and%20dollar%20pizza%20wasn%27t%20going%20to%20cut%20it%21%20I%20ordered%20the%20pepperoni%20and%20margharita%20Sicilian.%20The%20price%20for%20a%20slice%20averages%20from%20$5-6.50.%20This%20may%20seem%20pricey%20but%20you%20can%20taste%20the%20quality,%20also%20these%20slices%20are%20not%20your%20average%20size...%20they%27re%20HUGE.","model":"restaurants"}

headers = {
    'spiderman-token': "1234567890"
    }

response = requests.request("POST", url, headers=headers, params=querystring)

print(response.text)
```

```shell
curl --request POST --url 'https://supervisor.reviewshake.com/api/v2/insights/add?review_text=Artichoke%20Basille%27s%20is%20the%20perfect%20spot%20to%20go%20if%20you%27re%20looking%20for%20some%20late%20night%20eats.%20I%20came%20here%20after%20my%20graduation%20ceremony%20because%20I%20was%20craving%20a%20decent%20slice%20and%20dollar%20pizza%20wasn%27t%20going%20to%20cut%20it%21%20I%20ordered%20the%20pepperoni%20and%20margharita%20Sicilian.%20The%20price%20for%20a%20slice%20averages%20from%20$5-6.50.%20This%20may%20seem%20pricey%20but%20you%20can%20taste%20the%20quality,%20also%20these%20slices%20are%20not%20your%20average%20size...%20they%27re%20HUGE.&model=restaurants' --header 'spiderman-token: 1234567890'
```

```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "https://supervisor.reviewshake.com/api/v2/insights/add?review_text=Artichoke%20Basille%27s%20is%20the%20perfect%20spot%20to%20go%20if%20you%27re%20looking%20for%20some%20late%20night%20eats.%20I%20came%20here%20after%20my%20graduation%20ceremony%20because%20I%20was%20craving%20a%20decent%20slice%20and%20dollar%20pizza%20wasn%27t%20going%20to%20cut%20it%21%20I%20ordered%20the%20pepperoni%20and%20margharita%20Sicilian.%20The%20price%20for%20a%20slice%20averages%20from%20$5-6.50.%20This%20may%20seem%20pricey%20but%20you%20can%20taste%20the%20quality,%20also%20these%20slices%20are%20not%20your%20average%20size...%20they%27re%20HUGE.&model=restaurants",
  "method": "POST",
  "headers": {
    "spiderman-token": "1234567890"
  }
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

> The above command returns JSON structured like this:

```json
{
    "success": true,
    "job_id": 4012522,
    "status": 200,
    "message": "Added this insight to the queue..."
}
```

> If the model you are attempting to use doesn't exist or is unavailable, you will see:

```json
{
    "success": false,
    "status": 400,
    "message": "Please choose a valid model"
}
```

### HTTP Request

`POST https://supervisor.reviewshake.com/api/v2/insights/add`

### Request Body

Key | Description
--------- | -----------
review_text | The review text you would like insights for
model | The unique identifier of the model you would like to use

We have generic models available for several industries:

- `restaurants`
- `ecommerce`
- `hotels`

We add more models on a rolling basis, but feel free to contact us to train models for your specific industry or use case.

### Response
The response contains the following keys:

Key | Description
--------- | -----------
success | `true` or `false` depending on outcome
job_id | The `job_id` assigned to the insight
status | HTTP code for status, eg. `200`
message | Text response

## Get info

```ruby
require 'uri'
require 'net/http'

url = URI("https://supervisor.reviewshake.com/api/v2/insights/info?job_id=4")

http = Net::HTTP.new(url.host, url.port)

request = Net::HTTP::Get.new(url)
request["spiderman-token"] = '1234567890'

response = http.request(request)
puts response.read_body
```

```python
import requests

url = "https://supervisor.reviewshake.com/api/v2/insights/info"

querystring = {"job_id":"41231241"}

headers = {
    'spiderman-token': "1234567890"
    }

response = requests.request("GET", url, headers=headers, params=querystring)

print(response.text)
```

```shell
curl --request GET --url 'https://supervisor.reviewshake.com/api/v2/insights/info?job_id=41231241' --header 'spiderman-token: 1234567890'
```

```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "https://supervisor.reviewshake.com/api/v2/insights/info?job_id=41231241",
  "method": "GET",
  "headers": {
    "spiderman-token": "1234567890"
  }
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

> The above command returns JSON structured like this:

```json
{
    "success": true,
    "status": "success",
    "id": 41231241,
    "review_text": "Artichoke Basille's is the perfect spot to go if you're looking for some late night eats. I came here after my graduation ceremony because I was craving a decent slice and dollar pizza wasn't going to cut it! I ordered the pepperoni and margharita Sicilian. The price for a slice averages from $5-6.50. This may seem pricey but you can taste the quality, also these slices are not your average size... they're HUGE.",
    "response": "[{\"sentence\":\"Artichoke Basille's is the perfect spot to go if you're looking for some late night eats.\",\"classification\":\"food\",\"sentiment\":\"positive\"},{\"sentence\":\"I came here after my graduation ceremony because I was craving a decent slice and dollar pizza wasn't going to cut it!\",\"classification\":\"food\",\"sentiment\":\"positive\"},{\"sentence\":\"I ordered the pepperoni and margharita Sicilian.\",\"classification\":\"food\",\"sentiment\":null},{\"sentence\":\"The price for a slice averages from $5-6.50.\",\"classification\":\"value\",\"sentiment\":null},{\"sentence\":\"This may seem pricey but you can taste the quality, also these slices are not your average size... they're HUGE.\",\"classification\":\"value\",\"sentiment\":\"positive\"}]",
    "model": "restaurants"
}
```

> You will receive the following message if the provided `job_id` does not exist:

```json
{
    "success": false,
    "status": 400,
    "message": "This job ID doesn't exist, please create it"
}
```

This endpoint retrieves information for a given insight `job_id`.

### HTTP Request

`GET https://supervisor.reviewshake.xyz/api/v2/insights/info?job_id=41231241`

### URL Parameters

Parameter | Description
--------- | -----------
job_id | The ID of the job to retrieve

### Response
The response contains the following keys:

Key | Description
--------- | -----------
success | `true` or `false` depending on outcome
status | HTTP code for status, eg. `200`
id | The ID of the response from the Supervisor database (auto-increments)
response | The classification and sentiment of each sentence in the review

### `status` values

The `status` returned can be one of the following:

`crawl_status` | Description
--------- | -----------
`pending` | The job is still in the queue and is pending completion
`complete` | The job is complete
`maintenance` | There has been an issue with the response - our team is automatically notified for investigation

# Responses

## Add review response

Use our API to respond to reviews for sites which don't have an API. Currently available for Yelp and Tripadvisor.

```ruby
require 'uri'
require 'net/http'

url = URI("https://supervisor.reviewshake.com/api/v2/responds/add")

http = Net::HTTP.new(url.host, url.port)

request = Net::HTTP::Post.new(url)
request["spiderman-token"] = '1234567890'
request.body = "{\"username\": \"xxx\", \"password\": \"xxx\", \"profile_url\": \"xxx\", \"review_id\": \"xxx\", \"text\": \"xxx\", \"callback\": \"https://app.reviewcompany.com/supervisor_callback\", \"external_identifier\": \"xxx\"}"

response = http.request(request)
puts response.read_body
```

```python
import requests

url = "https://supervisor.reviewshake.com/api/v2/responds/add"

payload = "{\"username\": \"xxx\", \"password\": \"xxx\", \"profile_url\": \"xxx\", \"review_id\": \"xxx\", \"text\": \"xxx\", \"callback\": \"https://app.reviewcompany.com/supervisor_callback\", \"external_identifier\": \"xxx\"}"
headers = {
    'spiderman-token': "1234567890"
    }

response = requests.request("POST", url, data=payload, headers=headers)

print(response.text)
```

```shell
curl --request POST --url https://supervisor.reviewshake.com/api/v2/responds/add --header 'spiderman-token: 1234567890' --data '{ "username": "xxx", "password": "xxx", "profile_url": "xxx", "review_id": "xxx", "text": "xxx", "callback": "https://app.reviewcompany.com/supervisor_callback", "external_identifier": "xxx" }'
```

```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "https://supervisor.reviewshake.com/api/v2/responds/add",
  "method": "POST",
  "headers": {
    "spiderman-token": "1234567890"
  },
  "processData": false,
  "data": "{\"username\": \"xxx\", \"password\": \"xxx\", \"profile_url\": \"xxx\", \"review_id\": \"xxx\", \"text\": \"xxx\", \"callback\": \"https://app.reviewcompany.com/supervisor_callback\", \"external_identifier\": \"xxx\" }"
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

> The above command returns JSON structured like this:

```json
{
    "success": true,
    "job_id": 12312313,
    "status": 200,
    "message": "Added response to the queue..."
}
```

> If we don't support the review site you are submitting, you will see:

```json
{
    "success": false,
    "status": 400,
    "message": "We do not support www.deliveroo.com yet, please submit it to our feedback forum (https://feedback.reviewshake.com)"
}
```

### HTTP Request

`POST https://supervisor.reviewshake.com/api/v2/responds/add`

### Request Body

Key | Description
--------- | -----------
username | The username for logging into the review site in question
password | The password for logging into the review site in question
profile_url | The URL for the review profile
review_id | The unique ID of the review to be responded to, as provided by the review site
text | The text of the response
callback | The URL you would like to receive a status POST
external_identifier | An identifier for this response in your system, will be included in callback payload

<aside class="info">
Remember to take note of the <code>job_id</code>, as you will need this to GET the status for this response.
</aside>

### Response
The response contains the following keys:

Key | Description
--------- | -----------
success | `true` or `false` depending on outcome
job_id | The `job_id` assigned to the response
status | HTTP code for status, eg. `200`
message | Text response

## Get info

```ruby
require 'uri'
require 'net/http'

url = URI("https://supervisor.reviewshake.com/api/v2/responds/info?job_id=12312313")

http = Net::HTTP.new(url.host, url.port)

request = Net::HTTP::Get.new(url)
request["spiderman-token"] = '1234567890'

response = http.request(request)
puts response.read_body
```

```python
import requests

url = "https://supervisor.reviewshake.com/api/v2/responds/info"

querystring = {"job_id":"12312313"}

payload = ""
headers = {
    'spiderman-token': "1234567890"
    }

response = requests.request("GET", url, data=payload, headers=headers, params=querystring)

print(response.text)
```

```shell
curl --request GET --url 'https://supervisor.reviewshake.com/api/v2/responds/info?job_id=12312313' --header 'spiderman-token: 1234567890'
```

```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "https://supervisor.reviewshake.com/api/v2/responds/info?job_id=12312313",
  "method": "GET",
  "headers": {
    "spiderman-token": "1234567890"
  }
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

> The above command returns JSON structured like this:

```json
{
    "success": true,
    "status": 200,
    "respond": {
        "id": 12312313,
        "username": "xxx",
        "profile_url": "https://www.yelp.com/biz/naturally-delicious-brooklyn",
        "review_id": "xxx",
        "text": "xxx",
        "status": "pending",
        "external_identifier": 8394
    }
}
```

> You will receive the following message if the provided `job_id` does not exist:

```json
{
    "success": false,
    "status": 400,
    "message": "This job ID doesn't exist, please create it"
}
```

This endpoint retrieves information for a given response `job_id`.

### HTTP Request

`GET https://supervisor.reviewshake.com/api/v2/responds/info?job_id=123123132`

### URL Parameters

Parameter | Description
--------- | -----------
job_id | The ID of the job to retrieve

### Response
The response contains the following keys:

Key | Description
--------- | -----------
success | `true` or `false` depending on outcome
status | HTTP code for status, eg. `200`
id | The ID of the response from the Supervisor database (auto-increments)
username | The username for logging into the review site in question
profile_url | The URL for the review profile
review_id | The unique ID of the review to be responded to, as provided by the review site
text | The text of the response
status | `pending`, `complete`, `maintenance` or `invalid_credentials` (see below for explanation)
external_identifier | An identifier for this response in your system, will be included in callback payload

### `status` values

The `status` returned can be one of the following:

`crawl_status` | Description
--------- | -----------
`pending` | The job is still in the queue and is pending completion
`complete` | The job is complete
`maintenance` | There has been an issue with the response - our team is automatically notified for investigation
`invalid_credentials` | The credentials you have provided are invalid

# Reviews

## Add review profile

Add a review profile for scraping, must include either `url` or `query` parameter.

Notes:

- While we can scrape reviews from Facebook, they do not make all their reviews publicly available. Get in touch with us if you have any questions.

```ruby
require 'uri'
require 'net/http'

url = URI("https://supervisor.reviewshake.com/api/v2/profiles/add?url=https://www.amazon.com/dp/B003YH9MMI&from_date=2018-01-01")

http = Net::HTTP.new(url.host, url.port)

request = Net::HTTP::Post.new(url)
request["spiderman-token"] = '1234567890'

response = http.request(request)
puts response.read_body
```

```python
import requests

url = "https://supervisor.reviewshake.com/api/v2/profiles/add"

querystring = {"url":"https://www.amazon.com/dp/B003YH9MMI","from_date":"2018-01-01"}

headers = {
    'spiderman-token': "1234567890",
    }

response = requests.request("POST", url, headers=headers, params=querystring)

print(response.text)
```

```shell
curl --request POST --url 'https://supervisor.reviewshake.com/api/v2/profiles/add?url=https://www.amazon.com/dp/B003YH9MMI&from_date=2018-01-01' --header 'spiderman-token: 1234567890'
```

```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "https://supervisor.reviewshake.com/api/v2/profiles/add?url=https://www.amazon.com/dp/B003YH9MMI&from_date=2018-01-01",
  "method": "POST",
  "headers": {
    "spiderman-token": "1234567890",
  }
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

> The above command returns JSON structured like this:

```json
{
    "success": true,
    "job_id": 1234232,
    "status": 200,
    "message": "Added this profile to the queue..."
}
```

> If we don't support the review site you are submitting, you will see:

```json
{
    "success": false,
    "status": 200,
    "message": "We do not support www.deliveroo.com yet, please submit it to our feedback forum (https://feedback.reviewshake.com)"
}
```

### HTTP Request

`POST https://supervisor.reviewshake.com/api/v2/profiles/add`

### Query Parameters

Parameter | Description
--------- | -----------
url | Review profile URL.
query | Google search query that shows the Google My Business listing in the search results.
from_date | Only scrape reviews from a specific date (optional). Only works for `url` parameter. Format is `yyyy-mm-dd`
blocks | Number of blocks you want to return from search results (optional). In blocks of 10.
diff | Previous job ID for your given profile, for which you only need latest reviews.
callback | The URL where you would like us to `POST` the result payload when the status is complete
external_identifier | An identifier for this response in your system, will be included in callback payload

<aside class="info">
Remember to take note of the <code>job_id</code>, as you will need this to GET reviews for this profile.
</aside>

### Response
The response contains the following keys:

Key | Description
--------- | -----------
success | `true` or `false` depending on outcome
job_id | The `job_id` assigned to the scrape
status | HTTP code for status, eg. `200`
message | Text response

## Get info

```ruby
require 'uri'
require 'net/http'

url = URI("https://supervisor.reviewshake.com/api/v2/profiles/info?job_id=1234232")

http = Net::HTTP.new(url.host, url.port)

request = Net::HTTP::Get.new(url)
request["spiderman-token"] = '1234567890'

response = http.request(request)
puts response.read_body
```

```python
import requests

url = "https://supervisor.reviewshake.com/api/v2/profiles/info"

querystring = {"job_id":"1234232"}

payload = ""
headers = {
    'spiderman-token': "1234567890",
    }

response = requests.request("GET", url, data=payload, headers=headers, params=querystring)

print(response.text)
```

```shell
curl --request GET --url 'https://supervisor.reviewshake.com/api/v2/profiles/info?job_id=1234232' --header 'spiderman-token: 1234567890'
```

```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "https://supervisor.reviewshake.com/api/v2/profiles/info?job_id=1234232",
  "method": "GET",
  "headers": {
    "spiderman-token": "1234567890"
  }
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

> The above command returns JSON structured like this:

```json
{
    "success": true,
    "status": 200,
    "meta_data": null,
    "unique_id": null,
    "review_count": null,
    "average_rating": null,
    "last_crawl": null,
    "crawl_status": "pending",
    "percentage_complete": null,
    "result_count": 0
}
```

> You will receive the following message if the provided `job_id` does not exist:

```json
{
    "success": false,
    "status": 400,
    "message": "This job ID doesn't exist, please create it"
}
```

This endpoint retrieves information for a given review profile `job_id`.

### HTTP Request

`GET https://supervisor.reviewshake.com/api/v2/profiles/info?job_id=1234232`

### URL Parameters

Parameter | Description
--------- | -----------
job_id | The ID of the job to retrieve

### Response
The response contains the following keys:

Key | Description
--------- | -----------
success | `true` or `false` depending on outcome
status | HTTP code for status, eg. `200`
meta_data | Any meta data associated with the review profile
unique_id | The unique ID of the review profile
review_count | The total number of reviews as reported by the review site
average_rating | The average review rating as reported by the review site
last_crawl | The date of the last crawl
crawl_status | `pending`, `complete`, `maintenance` or `invalid_url` (see below for explanation)
percentage_complete | Number of reviews saved divided by total reviews available
result_count | The total number of reviews saved

### `crawl_status` values

The `crawl_status` returned can be one of the following:

`crawl_status` | Description
--------- | -----------
`pending` | The job is still in the queue and is pending completion
`complete` | The job is complete
`maintenance` | There has been an issue with the scrape - our team is automatically notified for investigation
`invalid_url` | The URL you have provided is invalid

## Get reviews

```ruby
require 'uri'
require 'net/http'

url = URI("https://supervisor.reviewshake.com/api/v2/profiles/reviews?job_id=112233")

http = Net::HTTP.new(url.host, url.port)

request = Net::HTTP::Get.new(url)
request["spiderman-token"] = '1234567890'

response = http.request(request)
puts response.read_body
```

```python
import requests

url = "https://supervisor.reviewshake.com/api/v2/profiles/reviews"

querystring = {"job_id":"112233"}

headers = {
    'spiderman-token': "1234567890",
    }

response = requests.request("GET", url, headers=headers, params=querystring)

print(response.text)
```

```shell
curl --request GET --url 'https://supervisor.reviewshake.com/api/v2/profiles/reviews?job_id=112233' --header 'spiderman-token: 1234567890'
```

```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "https://supervisor.reviewshake.com/api/v2/profiles/reviews?job_id=112233",
  "method": "GET",
  "headers": {
    "spiderman-token": "1234567890",
  }
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

> The above command returns JSON structured like this:

```json
{
    "success": true,
    "status": 200,
    "meta_data": "{\"star_ratings\"=>{\"1star\"=>\"0%\", \"2star\"=>\"2%\", \"3star\"=>\"3%\", \"4star\"=>\"18%\", \"5star\"=>\"77%\"}}",
    "unique_id": null,
    "review_count": 91,
    "average_rating": 4.6,
    "last_crawl": "2019-01-16",
    "crawl_status": "complete",
    "percentage_complete": 100,
    "result_count": 3,
    "reviews": [
        {
            "id": 56868,
            "name": "Mahmoud M. Abdel-Fattah",
            "date": "2018-12-03",
            "rating_value": 4,
            "review_text": "The book is good but repetitive, and it'll be great if there is a summary (tasks/todo list) with the output.",
            "url": "https://www.amazon.com/gp/customer-reviews/R2NFQ2VFAIGZ3G/ref=cm_cr_arp_d_rvw_ttl?ie=UTF8&ASIN=B003YH9MMI",
            "profile_picture": null,
            "location": null,
            "review_title": "I highly recommend it but it is repittive",
            "verified_order": true,
            "language_code": "en",
            "reviewer_title": null,
            "unique_id": "R2NFQ2VFAIGZ3G",
            "meta_data": null
        }
    ]
}
```

> You will receive the following message if the provided `job_id` does not exist:

```json
{
    "success": false,
    "status": 400,
    "message": "This job ID doesn't exist, please create it"
}
```

This endpoint returns the actual reviews from the scrape.

### HTTP Request

`GET https://supervisor.reviewshake.com/api/v2/profiles/reviews?job_id=112233`

### URL Parameters

Parameter | Example  | Description
--------- | -------- | -----------
job_id | 112233 | The job ID returned from the `/add` endpoint.
from_date | 2018-01-01 | Only return reviews from a given date (optional). Format is `yyyy-mm-dd`
language_code | en | Only return reviews in a specific language (support varies by review site).
page | 1 | Page number to retrieve (optional).
per_page | 100 | The number of reviews to retrieve per page (defaults to 20, max 100).
allow_response | true | Show responses for reviews (optional).

### Response
The response for the `reviews` key contains the following keys (see the `/info` endpoint for the other keys):

Key | Description
--------- | -----------
id | The ID of the review from the Supervisor database (auto-increments)
name | Name of the reviewer
date | Date of the review, format is `yyyy-mm-dd`
rating_value | Star rating of the review
review_text | Text of the review
url | Direct link to the review (when available)
profile_picture | URL to the profile picture of the reviewer (when available)
location | Location the review belongs to (when available)
review_title | Title of the review (when available)
verified_order | Verified order status, `true` or `false`
language_code | Language code of the review, for example `en` (when available)
reviewer_title | Title of the reviewer
unique_id | Unique ID of the review as reported by the review site (when available)
meta_data | Any available meta data for this review (when available)

# URLs

## Add URL

Add a URL to receive meta data from. Supported sites:

- Generic sites (eg. `http://www.naturallydelicious.com`)
- Yelp (eg. `https://yelp.com/biz/naturally-delicious-brooklyn`)
- Facebook (eg. `https://www.facebook.com/naturallydelicious/`)
- Google (eg. `https://www.google.com/search?q=naturally+delicious+caterers+nyc`)

```ruby
require 'uri'
require 'net/http'

url = URI("https://supervisor.reviewshake.com/api/v2/url/add?url=https://www.facebook.com/pg/AnnapolisPropertyServices/reviews/")

http = Net::HTTP.new(url.host, url.port)

request = Net::HTTP::Post.new(url)
request["spiderman-token"] = '1234567890'

response = http.request(request)
puts response.read_body
```

```python
import requests

url = "https://supervisor.reviewshake.com/api/v2/url/add"

querystring = {"url":"https://www.facebook.com/pg/AnnapolisPropertyServices/reviews/"}

headers = {
    'spiderman-token': "1234567890",
    'cache-control': "no-cache",
    'Postman-Token': "10f9a175-dbcd-44f1-9717-668fab7b0eee"
    }

response = requests.request("POST", url, headers=headers, params=querystring)

print(response.text)
```

```shell
curl --request POST --url 'https://supervisor.reviewshake.com/api/v2/url/add?url=https://www.facebook.com/pg/AnnapolisPropertyServices/reviews/' --header 'spiderman-token: 1234567890'
```

```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "https://supervisor.reviewshake.com/api/v2/url/add?url=https://www.facebook.com/pg/AnnapolisPropertyServices/reviews/",
  "method": "POST",
  "headers": {
    "spiderman-token": "1234567890"
  }
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

> The above command returns JSON structured like this:

```json
{
    "success": true,
    "job_id": 12312313,
    "status": 200,
    "message": "Added this URL to the queue..."
}
```

> If we don't support the URL you are submitting, you will see:

```json
{
    "success": false,
    "status": 400,
    "message": "We do not support www.deliveroo.com yet, please submit it to our feedback forum (https://feedback.reviewshake.com)"
}
```

### HTTP Request

`POST https://supervisor.reviewshake.com/api/v2/url/add`

### Request Body

Key | Description
--------- | -----------
url | The URL you would like meta data for

<aside class="info">
Remember to take note of the <code>job_id</code>, as you will need this to GET the status for this response.
</aside>

### Response
The response contains the following keys:

Key | Description
--------- | -----------
success | `true` or `false` depending on outcome
job_id | The `job_id` assigned to the scrape
status | HTTP code for status, eg. `200`
message | Text response

## Get info

```ruby
require 'uri'
require 'net/http'

url = URI("https://supervisor.reviewshake.com/api/v2/url/info?job_id=12312313")

http = Net::HTTP.new(url.host, url.port)

request = Net::HTTP::Get.new(url)
request["spiderman-token"] = '1234567890'

response = http.request(request)
puts response.read_body
```

```python
import requests

url = "https://supervisor.reviewshake.com/api/v2/url/info"

querystring = {"job_id":"12312313"}

payload = ""
headers = {
    'spiderman-token': "1234567890"
    }

response = requests.request("GET", url, data=payload, headers=headers, params=querystring)

print(response.text)
```

```shell
curl --request GET --url 'https://supervisor.reviewshake.com/api/v2/url/info?job_id=12312313' --header 'spiderman-token: 1234567890'
```

```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "https://supervisor.reviewshake.com/api/v2/url/info?job_id=12312313",
  "method": "GET",
  "headers": {
    "spiderman-token": "1234567890"
  }
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

> The above command returns JSON structured like this:

```json
{
    "success": true,
    "status": 200,
    "social_data": {
        "facebook": "https://www.facebook.com/ChicagoStyleSEO",
        "google": "https://plus.google.com/+Chicagostyleseo/about",
        "twitter": "https://twitter.com/chicagostyleseo",
        "instagram": "https://instagram.com/chicagostyleseo/",
        "linkedin": "https://www.linkedin.com/company/chicago-style-seo"
    },
    "contact_data": {
        "phone": "+17738095002",
        "email": "info@chicagostyleseo.com"
    },
    "review_data": {},
    "website": null,
    "last_crawl": "2018-10-03",
    "crawl_status": "complete"
}
```

> You will receive the following message if the provided `job_id` does not exist:

```json
{
    "success": false,
    "status": 400,
    "message": "This job ID doesn't exist, please create it"
}
```

This endpoint retrieves information for a given response `job_id`.

### HTTP Request

`GET https://supervisor.reviewshake.com/api/v2/url/info?job_id=92724`

### URL Parameters

Parameter | Description
--------- | -----------
job_id | The ID of the job to retrieve

### Response
The response contains the following keys:

Key | Description
--------- | -----------
success | `true` or `false` depending on outcome
status | HTTP code for status, eg. `200`
social_data | Hash containing links to the social profiles found on the URL
contact_data | Hash containing contact data found on the URL
review_data | Hash containing links to reviw sites found on the URL

### `status` values

The `status` returned can be one of the following:

`crawl_status` | Description
--------- | -----------
`pending` | The job is still in the queue and is pending completion
`complete` | The job is complete
`maintenance` | There has been an issue with the scrape - our team is automatically notified for investigation
`invalid_url` | The URL you provided is invalid