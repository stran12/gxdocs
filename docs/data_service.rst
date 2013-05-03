.. Data service API documentation

Data Service API
================

Get a List of all Feeds
-----------------------

::

    GET api.gradientx.com/a/feed/

Response::

    {
        "1": "pacing_report", 
        "2": "Billing Report", 
        "3": "Traffic Quality Report", 
        "4": "Monthly User Report"
    } 

Get Feed by feed_name
---------------------

    ``GET api.gradientx.com/a/feed/<feed_name|feed_id>``

Optional query parameters

* ``start_date`` 
* ``end_date``

Example::

    GET api.gradientx.com/a/feed/Billing%20Report?start_date=2013-02-13
    GET api.gradientx.com/a/feed/Monthly%20User%20Report?end_date=2013-02-14

Response::

   {
    "status": "active", 
    "updated": "2013-02-16 01:48:24", 
    "batches": {
        "1": {
            "status": "complete", 
            "updated": "2013-02-19 21:38:04", 
            "created": null, 
            "feed_schema_id": 1, 
            "start_time": null, 
            "parts": {
                "1": "http://hdfs/file1.csv", 
                "2": "http://hdfs/file2.csv"
            }, 
            "daytime": "2013-02-19 21:36:12", 
            "feed_version": 1, 
            "columns": "impressions int, clicks int"
        }
    }, 
    "name": "pacing_report", 
    "created": null, 
    "period": "1 day", 
    "feed_id": 1, 
    "frequency": "1 day", 
    "current_version": 1
    } 

Notes

#. You can specify one or both query parameters
#. Specifying only start_date will yield every batch report since that start_date
#. Specifying only end_date will yield every batch report up until and including that end_date
#. Specifying both will yield batch reports within that range inclusively
#. Dates are treated as midnight of that day (e.g. 2013-02-13 == 2013-02-13 00:00:00)


Get Schema Details
------------------

    ``GET api.gradientx.com/a/feed/schema/<schema_id>``

Other Methods to Query

* ``feed_id``
* ``ver``

Examples::
    
    GET api.gradientx.com/a/feed/schema?feed_id=1&ver=2
    GET api.gradientx.com/a/feed/schema/3

Response::

    {
        "version": 2, 
        "feed_schema_id": 1, 
        "feed_id": 1, 
        "columns": "impressions int, clicks int"
    }

Notes

#. Either the schema_id or **BOTH** ``feed_id`` && ``ver`` parameters are required.

