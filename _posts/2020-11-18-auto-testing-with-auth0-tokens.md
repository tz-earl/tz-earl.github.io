---
layout: post
title:  'Automated Testing API Endpoints with Auth0 Tokens'
date:   2020-11-18 00:00:00 -0700
categories: 
---
As part of a learning exercise, I am writing an API server app implemented in Python, Flask, et al, 
along with automated tests using the [Python unittest module](https://docs.python.org/3/library/unittest.html).

After knocking out some initial functionality, my partner Ryan (who is writing a frontend as a
native Android app) and I decided to add authorization of users to restrict access to the API. We
chose the [Auth0 service](https://auth0.com/) for this.

Testing implies that you are simulating a client. That means you must have a valid and 
unexpired authorization token issued by Auth0.

At the beginning of developing this I tested manually by using [Postman](https://www.postman.com/),
which is a great tool for
constructing and sending requests to APIs. To acquire an Auth0 token I used the `curl` command to
send a request to an Auth0 endpoint that is specific to our account. That token has an expiration of
twenty-four hours, after which I would just use curl again to get a fresh token.

However, getting the token requires providing secret Auth0 credentials that are unique to our Auth0
account. You don't want to put these into code that is committed to a public repo.

As with other kinds of credentials, e.g. database login, I decided to put the Auth0 ones into
environment variables, a mechanism which I had already put in place using the Python
[python-dotenv](https://pypi.org/project/python-dotenv/) module. That module uses a `.env` file
placed at the top of the app directory, that contains variable definitions. The file is never
committed to a public repo and is only kept in the intended environment.

~~~~~ python
# Put these definitions into the .env file.
# You get the values from your Auth0 dashboard.
AUTHO_CLIENT_ID="xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
AUTHO_CLIENT_SECRET="xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
~~~~~


Following are the contents of `get_auth0.token.py`. 

The first several lines read the Auth0 values from the environment.

Function`get_auth0_access_token()` uses those values to construct a request to send
to the Auth0 endpoint, extracts the access token from the response, and returns the token.
The body of the function is largely copied from Auth0 documentation (which is extensive
and very well-written).

~~~~~ python
# get_auth0_token.py

import http.client
import json
import os
from dotenv import load_dotenv, find_dotenv

# Get the AUTHO_CLIENT_ID and AUTHO_CLIENT_SECRET credentials.
ENV_FILE = find_dotenv()
if ENV_FILE:
    load_dotenv(ENV_FILE)

AUTHO_CLIENT_ID = os.environ['AUTHO_CLIENT_ID']
AUTHO_CLIENT_SECRET = os.environ['AUTHO_CLIENT_SECRET']


def get_auth0_access_token():
    conn = http.client.HTTPSConnection("espresso-dev.us.auth0.com")

    request_items = { "client_id": AUTHO_CLIENT_ID, \
                      "client_secret": AUTHO_CLIENT_SECRET, \
                      "audience": "api.espresso.routinew.com", \
                      "grant_type": "client_credentials"
                    }

    payload = json.dumps(request_items)
    headers = {'content-type': "application/json"}

    conn.request("POST", "/oauth/token", payload, headers)

    res = conn.getresponse()
    data_json = res.read()
    data_dict = json.loads(data_json)
    access_token = data_dict.get('access_token', None)

    return access_token


if __name__ == '__main__':
    access_token = get_auth0_access_token()
    print(access_token)

~~~~~

Finally, the test code calls the above function. Test cases use the returned token to authorize
themselves as part of their calls to the API server. For example,

~~~~~ python
import unittest
import get_auth0_token

# Get the Auth0 access token just once to avoid repeated hits to the Auth0 API
access_token = get_auth0_token.get_auth0_access_token()
auth_header = {'authorization': 'Bearer ' + access_token}


class RestaurantsTestCases(unittest.TestCase):

    def test_get_restaurants(self):
        resp = self.test_client.get('/api/v1/restaurants', headers=auth_header)
        self.assertEqual(resp.status_code, 200)

~~~~~