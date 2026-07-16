---
title: "Request"
source: "https://fastapi.tiangolo.com/reference/request/"
---

# `Request` class¶

You can declare a parameter in a _path operation function_ or dependency to be of type `Request` and then you can access the raw request object directly, without any validation, etc.

Read more about it in the [FastAPI docs about using Request directly](https://fastapi.tiangolo.com/advanced/using-request-directly/)

You can import it directly from `fastapi`:
[code] 
    from fastapi import Request
    
[/code]

Tip

When you want to define dependencies that should be compatible with both HTTP and WebSockets, you can define a parameter that takes an `HTTPConnection` instead of a `Request` or a `WebSocket`.

##  `` fastapi.Request ¶
[code] 
    Request(scope, receive=empty_receive, send=empty_send)
    
[/code]

Bases: `[HTTPConnection](httpconnection_#fastapi.requests.HTTPConnection "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">fastapi.requests.HTTPConnection<_span>".md)[StateT]`

Source code in `starlette/requests.py`
[code] 
    def __init__(self, scope: Scope, receive: Receive = empty_receive, send: Send = empty_send):
        super().__init__(scope)
        assert scope["type"] == "http"
        self._receive = receive
        self._send = send
        self._stream_consumed = False
        self._is_disconnected = False
        self._form = None
    
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

###  `` method `property` ¶
[code] 
    method
    
[/code]

###  `` receive `property` ¶
[code] 
    receive
    
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

###  `` stream `async` ¶
[code] 
    stream()
    
[/code]

Source code in `starlette/requests.py`
[code] 
    async def stream(self) -> AsyncGenerator[bytes, None]:
        if hasattr(self, "_body"):
            yield self._body
            yield b""
            return
        if self._stream_consumed:
            raise RuntimeError("Stream consumed")
        while not self._stream_consumed:
            message = await self._receive()
            if message["type"] == "http.request":
                body = message.get("body", b"")
                if not message.get("more_body", False):
                    self._stream_consumed = True
                if body:
                    yield body
            elif message["type"] == "http.disconnect":  # pragma: no branch
                self._is_disconnected = True
                raise ClientDisconnect()
        yield b""
    
[/code]

###  `` body `async` ¶
[code] 
    body()
    
[/code]

Source code in `starlette/requests.py`
[code] 
    async def body(self) -> bytes:
        if not hasattr(self, "_body"):
            chunks: list[bytes] = []
            async for chunk in self.stream():
                chunks.append(chunk)
            self._body = b"".join(chunks)
        return self._body
    
[/code]

###  `` json `async` ¶
[code] 
    json()
    
[/code]

Source code in `starlette/requests.py`
[code] 
    async def json(self) -> Any:
        if not hasattr(self, "_json"):  # pragma: no branch
            body = await self.body()
            self._json = json.loads(body)
        return self._json
    
[/code]

###  `` form ¶
[code] 
    form(
        *,
        max_files=1000,
        max_fields=1000,
        max_part_size=1024 * 1024
    )
    
[/code]

Source code in `starlette/requests.py`
[code] 
    def form(
        self,
        *,
        max_files: int | float = 1000,
        max_fields: int | float = 1000,
        max_part_size: int = 1024 * 1024,
    ) -> AwaitableOrContextManager[FormData]:
        return AwaitableOrContextManagerWrapper(
            self._get_form(max_files=max_files, max_fields=max_fields, max_part_size=max_part_size)
        )
    
[/code]

###  `` close `async` ¶
[code] 
    close()
    
[/code]

Source code in `starlette/requests.py`
[code] 
    async def close(self) -> None:
        if self._form is not None:  # pragma: no branch
            await self._form.close()
    
[/code]

###  `` is_disconnected `async` ¶
[code] 
    is_disconnected()
    
[/code]

Source code in `starlette/requests.py`
[code] 
    async def is_disconnected(self) -> bool:
        if not self._is_disconnected:
            message: Message = {}
    
            # If message isn't immediately available, move on
            with anyio.CancelScope() as cs:
                cs.cancel()
                message = await self._receive()
    
            if message.get("type") == "http.disconnect":
                self._is_disconnected = True
    
        return self._is_disconnected
    
[/code]

###  `` send_push_promise `async` ¶
[code] 
    send_push_promise(path)
    
[/code]

Source code in `starlette/requests.py`
[code] 
    async def send_push_promise(self, path: str) -> None:
        if "http.response.push" in self.scope.get("extensions", {}):
            raw_headers: list[tuple[bytes, bytes]] = []
            for name in SERVER_PUSH_HEADERS_TO_COPY:
                for value in self.headers.getlist(name):
                    raw_headers.append((name.encode("latin-1"), value.encode("latin-1")))
            await self._send({"type": "http.response.push", "path": path, "headers": raw_headers})
    
[/code]
