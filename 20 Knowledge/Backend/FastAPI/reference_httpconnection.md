---
title: "Httpconnection"
source: "https://fastapi.tiangolo.com/reference/httpconnection/"
---

# `HTTPConnection` class¶

When you want to define dependencies that should be compatible with both HTTP and WebSockets, you can define a parameter that takes an `HTTPConnection` instead of a `Request` or a `WebSocket`.

You can import it from `fastapi.requests`:
[code] 
    from fastapi.requests import HTTPConnection
    
[/code]

##  `` fastapi.requests.HTTPConnection ¶
[code] 
    HTTPConnection(scope, receive=None)
    
[/code]

Bases: `Mapping[str, Any]`, `Generic[StateT]`

A base class for incoming HTTP connections, that is used to provide any functionality that is common to both `Request` and `WebSocket`.

Source code in `starlette/requests.py`
[code] 
    def __init__(self, scope: Scope, receive: Receive | None = None) -> None:
        assert scope["type"] in ("http", "websocket")
        self.scope = scope
    
[/code]

###  `` scope `instance-attribute` ¶
[code] 
    scope = scope
    
[/code]

###  `` app `property` ¶
[code] 
    app
    
[/code]

###  `` url `property` ¶
[code] 
    url
    
[/code]

###  `` base_url `property` ¶
[code] 
    base_url
    
[/code]

###  `` headers `property` ¶
[code] 
    headers
    
[/code]

###  `` query_params `property` ¶
[code] 
    query_params
    
[/code]

###  `` path_params `property` ¶
[code] 
    path_params
    
[/code]

###  `` cookies `property` ¶
[code] 
    cookies
    
[/code]

###  `` client `property` ¶
[code] 
    client
    
[/code]

###  `` session `property` ¶
[code] 
    session
    
[/code]

###  `` auth `property` ¶
[code] 
    auth
    
[/code]

###  `` user `property` ¶
[code] 
    user
    
[/code]

###  `` state `property` ¶
[code] 
    state
    
[/code]

###  `` url_for ¶
[code] 
    url_for(name, /, **path_params)
    
[/code]

Source code in `starlette/requests.py`
[code] 
    def url_for(self, name: str, /, **path_params: Any) -> URL:
        url_path_provider: Router | Starlette | None = self.scope.get("router") or self.scope.get("app")
        if url_path_provider is None:
            raise RuntimeError("The `url_for` method can only be used inside a Starlette application or with a router.")
        url_path = url_path_provider.url_path_for(name, **path_params)
        return url_path.make_absolute_url(base_url=self.base_url)
    
[/code]
