.. Gradient X Reporting API


#############
Reporting API
#############

.. attention:: All requests must use secure https (SSL)

API Endpoint
============

    ``POST https://api.gradientx.com/reports``

Sample POST Raw Data **Required**
=================================

::

    data = {
      "dimension": "country",
      "metrics": [
        "impressions",
        "clicks",
        "bid_depth",
        "bid_wins",
        "ctr",
        "bid_win_rate"
      ],
      "filters": [
        {
          "col": "campaign_name",
          "value": "Audi_CPC_FEB_13"
        },
        {
           "col": "source_name",
           "value": "App",
           "exclude": "1"
        }
      ],
      "dates": {
        "range": "last7"
      }
    }    

Response
========

Response text will be have fields delimited by Ctrl+A (\\001)
and lines delimited by newline character (\\n)

The Ctrl+A delimiter in the data below is substitued with tabs for viewing
purposes

::

    date    bid_requests    ctr clicks  conversions
    20131117    1380985175  0.708982511677  59467   0
    20131116    1302325768  0.726903391225  88516   0
    20131115    1198623948  0.526670504946  56304   0
    20131114    1234091173  0.47102811547   14883   0
    20131113    1275942859  0.444288779551  20702   0
    20131112    1245990650  0.592072766457  54143   0
    20131111    1369575088  0.558350133181  24140   0


Metrics
-------

#. clicks
#. conversions
#. conversion_rate
#. cost
#. ctr
#. ecpa
#. ecpc
#. ecpm
#. gross_profit
#. impressions
#. margin
#. revenue

Dimensions
----------

#. appsite_name
#. app_name
#. campaign_name
#. carrier_name
#. category_name
#. client_name
#. connection_type_name
#. country_name
#. creative_name
#. day_of_week
#. device_family_name
#. device_make_name
#. device_model_name
#. device_type_name
#. dma_name
#. group_name
#. industry_name
#. item_name
#. lander_name
#. os_platform_name
#. os_version_name
#. publisher_name
#. region_name
#. site_name
#. size_code
#. source_name
#. state_name
#. subcategory_name
#. time_of_day
#. user_local_day
#. user_local_daytime
#. user_local_time

Available Date Ranges
---------------------

#. custom
#. today
#. yesterday
#. last3
#. last7
#. last30
#. this_month
#. last_month
#. this_quarter
#. last_quarter
#. this_year
#. lifetime


Example Python Script
---------------------

This Python script will authenticate and do a sample protected request
Additionally, it will request statistics for incomfing traffic that is 
not from the United States in the past 7 days.

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


    # Reporting API
    data = {'dates': {'range': 'last7'},
    'dimension': 'date',
    'filters': [{'col': 'country_name',
              'exclude': 1,
              'type': 'list',
              'value': ['United States']},
             ],
    'metrics': ['bid_requests', 'ctr', 'clicks', 'conversions'],
    'timezone': 'UTC'}

    url = 'https://api.gradientx.com/reports'
    headers = {
            'oauth': 'Bearer {0}'.format(access_token),
            'client_id': 'fe01ce2a7fbac8fafaed7c982a04e229',
            'content-type': 'application/json'}
    r = s.post(url, data=json.dumps(data), headers=headers)
    print r.status_code
    print r.text

