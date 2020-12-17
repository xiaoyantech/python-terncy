# python-terncy
A python library for control [Terncy](https://www.terncy.com/) devices.

[![PyPI version](https://badge.fury.io/py/terncy.svg)](https://badge.fury.io/py/terncy) [![Build Status](https://travis-ci.org/rxwen/python-terncy.svg?branch=master)](https://travis-ci.org/rxwen/python-terncy)


Before use this library, developer need to get a client id from terncy. The client id is a string that uniquely identifies the the client app connectes to Terncy system. For istance, Home Assistant integration has its own client id. By presenting a valid client id to Terncy, which use allow access to the system.
An access credential is also needed to make sure the owner of the Terncy system is aware of, and permit the client access. The approval of the access credential is approved in Terncy app.


Example:

```
import asyncio
import terncy


def event_hander(t, msg):
    print("got event", msg)

async def main():
    terncy_homecenters_in_lan = await t.discover()
    t = terncy.Terncy(
        "client_id",
        "192.168.1.187",
        443,
        "homeassistant_user",
        "",
    )
    token_id, token = await t.request_token("homeassistant_user", "HA User")
    # approve the token request in Terncy app
    t.register_event_handler(event_hander)
    await t.start()
    t.set_onoff("uuid_of_device", 1)


loop = asyncio.get_event_loop()
loop.run_until_complete(main())

```

