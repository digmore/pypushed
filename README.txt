Unofficial Pushed.co API wrapper
================================

Overview
---------

Python wrapper for the pushed.co API. https://pushed.co

Send push notifications to mobile devices without writing your own mobile
app. Have notifications pushed to you rather than relying on polling RSS feeds
and the like.


Usage
-----

Currently only the push functionality and some of the OAuth is implemented
here. 

Basic usage
.. code-block:: python
    
    import pushed

    APP_KEY = 'MY_PUSHED_APP_KEY'
    APP_SECRET = 'MY_PUSHED_APP_SECRET'
    CHANNEL_ALIAS = 'MY_CHANNEL_ALIAS'
    PUSHED_ID = 'PUSHED_ID_FOR_SUBSCRIBER'

    pushed = Pushed(APP_KEY, APP_SECRET)

    # Push message to your application's subscribers
    shipment = pushed.push_app('test app push')

    # Push message to your channel's subscribers
    shipment = pushed.push_channel('test channel push', CHANNEL_ALIAS)

    # Push message to a user by Pushed ID
    shipment = pushed.push_pushed_id('test Pushed ID push', PUSHED_ID)

Pushing to a user requires an OAuth access token. You must swap a temporary
code for this access token using the Pushed API. These temporary codes arrive
by webhook, when a subscriber follows your authorization link and agrees to the
access.

To generate an authorization link to share with your users
... code-block:: python
    pushed = Pushed(APP_KEY, APP_SECRET)
    uri = pushed.authorization_link('https://example.org/my-webhook-handler')

Using a code to get an access token, then sending a message to the user
... code-block:: python
    pushed = Pushed(APP_KEY, APP_SECRET)
    access_token = pushed.access_token(temporary_code)
    shipment = pushed.push_user('test user push', access_token)

All 4 push methods return an alphanumeric Shipment ID which you can check
against your Pushed.co control panel. If the Pushed API returns a JSON failure
message then a PushedAPIError will be raised using its type and message fields.

Installation
------------

Using pip ::

.. code-block:: bash
    
    pip install pushed

Using easy install ::

.. code-block:: bash
    
    easy_install pushed


