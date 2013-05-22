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
