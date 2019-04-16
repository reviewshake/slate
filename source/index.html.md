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

This API documentation is [open-source](https://github.com/reviewshake/slate) so feel free to make changes and submit a pull request if you would like.

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