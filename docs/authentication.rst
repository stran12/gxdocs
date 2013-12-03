.. How to authenticate

OAuth2 Authentication
=====================

.. attention:: All requests must use secure https (SSL)

General Steps
-------------

This is for two-legged authentication

#. Obtain an authorization key
#. With authorization_key, client_key, client_secret, obtain access_token
#. Set access_token and client_key headers with all subsequent request for data


Authorization Key
-----------------

    ``GET https://api.gradientx.com/v1/oauth/authorize``

Required Query String Parameters

* ``client_id`` - This is your API key that Gradient X will provide
* ``response_type`` - This will **ALWAYS** be ``"code"``
* ``redirect_uri`` - Set this to ``"https://api.gradientx.com/v1/oauth/mirror"`` for
  2-legged authorization

Response::

    {'code': '18ASDF8asdchkd9098DDf'}


Access Token
------------

    ``POST https://api.gradientx.com/v1/oauth/token``

Required POST form-arguments

* ``client_id`` - Same as the one used for authorization
* ``client_secret`` - Provided by Gradient X
* ``grant_type`` - Always set to ``"authorization_code"``
* ``redirect_uri`` - Set to ``"https://api.gradientx.com/v1/oauth/mirror"`` for
  2-legged authorization
* ``code`` - Authorization code obtained in the previous step

Response::

    {'access_token': 'a98sdf98709sda098ad089d098',
     'refresh_token': 'asdasdf798asdf98asd987fas9asdf987asdf'
    }


Authenticated Request
---------------------

Set request headers:

* ``oauth`` - Set this to ``"Bearer <access_token>"``
* ``client_id`` - Provided by GradientX

Here is a production test url to test your authentiation process
    ``GET https://api.gradientx.com/v1/oauth/test``


Example Python Script
---------------------

This Python script will authenticate and do a sample protected request

.. code-block:: python

    import json
    import requests

    url = 'https://api.gradientx.com/v1/oauth/authorize'
    params = {'client_id': 'fe01ce2a7fbac8fafaed7c982a04e229',
            'redirect_uri': 'https://api.gradientx.com/v1/oauth/mirror',
              'response_type': 'code'}

    s = requests.Session()

    r = s.get(url, params=params)
    res = json.loads(r.text)

    code = res['code'] # Getting the authorization code from response

    url = 'https://api.gradientx.com/v1/oauth/token'
    payload = { 'client_id': 'fe01ce2a7fbac8fafaed7c982a04e229',
            'client_secret': 'c047135100c85557ab45e20485f9ae9f',
            'grant_type': 'authorization_code',
            'redirect_uri': 'https://api.gradientx.com/v1/oauth/mirror',
            'code': code
            }
    r = s.post(url, data=json.dumps(payload))

    res = json.loads(r.text)
    access_token = res['access_token']


    # Url to test if you authenticated properly
    url = 'https://api.gradientx.com/v1/oauth/test'
    headers = {
            'oauth': 'Bearer {0}'.format(access_token),
            'client_id': 'fe01ce2a7fbac8fafaed7c982a04e229'}
    r = s.get(url, headers=headers)

    # Success?
    print r.text
    print r.status_code


