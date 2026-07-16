---
title: "Testclient"
source: "https://fastapi.tiangolo.com/reference/testclient/"
---

# Test Client - `TestClient`¶

You can use the `TestClient` class to test FastAPI applications without creating an actual HTTP and socket connection, just communicating directly with the FastAPI code.

Read more about it in the [FastAPI docs for Testing](https://fastapi.tiangolo.com/tutorial/testing/).

You can import it directly from `fastapi.testclient`:
[code] 
    from fastapi.testclient import TestClient
    
[/code]

##  `` fastapi.testclient.TestClient ¶
[code] 
    TestClient(
        app,
        base_url="http://testserver",
        raise_server_exceptions=True,
        root_path="",
        backend="asyncio",
        backend_options=None,
        cookies=None,
        headers=None,
        follow_redirects=True,
        client=("testclient", 50000),
    )
    
[/code]

Bases: `Client`

Source code in `starlette/testclient.py`
[code] 
    def __init__(
        self,
        app: ASGIApp,
        base_url: str = "http://testserver",
        raise_server_exceptions: bool = True,
        root_path: str = "",
        backend: Literal["asyncio", "trio"] = "asyncio",
        backend_options: dict[str, Any] | None = None,
        cookies: httpx._types.CookieTypes | None = None,
        headers: dict[str, str] | None = None,
        follow_redirects: bool = True,
        client: tuple[str, int] = ("testclient", 50000),
    ) -> None:
        self.async_backend = _AsyncBackend(backend=backend, backend_options=backend_options or {})
        if _is_asgi3(app):
            asgi_app = app
        else:
            app = cast(ASGI2App, app)  # type: ignore[assignment]
            asgi_app = _WrapASGI2(app)  # type: ignore[arg-type]
        self.app = asgi_app
        self.app_state: dict[str, Any] = {}
        transport = _TestClientTransport(
            self.app,
            portal_factory=self._portal_factory,
            raise_server_exceptions=raise_server_exceptions,
            root_path=root_path,
            app_state=self.app_state,
            client=client,
        )
        if headers is None:
            headers = {}
        headers.setdefault("user-agent", "testclient")
        super().__init__(
            base_url=base_url,
            headers=headers,
            transport=transport,
            follow_redirects=follow_redirects,
            cookies=cookies,
        )
    
[/code]

###  `` headers `property` `writable` ¶
[code] 
    headers
    
[/code]

HTTP headers to include when sending requests.

###  `` follow_redirects `instance-attribute` ¶
[code] 
    follow_redirects = follow_redirects
    
[/code]

###  `` max_redirects `instance-attribute` ¶
[code] 
    max_redirects = max_redirects
    
[/code]

###  `` is_closed `property` ¶
[code] 
    is_closed
    
[/code]

Check if the client being closed

###  `` trust_env `property` ¶
[code] 
    trust_env
    
[/code]

###  `` timeout `property` `writable` ¶
[code] 
    timeout
    
[/code]

###  `` event_hooks `property` `writable` ¶
[code] 
    event_hooks
    
[/code]

###  `` auth `property` `writable` ¶
[code] 
    auth
    
[/code]

Authentication class used when none is passed at the request-level.

See also [Authentication](quickstart_#authentication.md).

###  `` base_url `property` `writable` ¶
[code] 
    base_url
    
[/code]

Base URL to use when sending requests with relative URLs.

###  `` cookies `property` `writable` ¶
[code] 
    cookies
    
[/code]

Cookie values to include when sending requests.

###  `` params `property` `writable` ¶
[code] 
    params
    
[/code]

Query parameters to include in the URL when sending requests.

###  `` task `instance-attribute` ¶
[code] 
    task
    
[/code]

###  `` portal `class-attribute` `instance-attribute` ¶
[code] 
    portal = None
    
[/code]

###  `` async_backend `instance-attribute` ¶
[code] 
    async_backend = _AsyncBackend(
        backend=backend, backend_options=backend_options or {}
    )
    
[/code]

###  `` app `instance-attribute` ¶
[code] 
    app = asgi_app
    
[/code]

###  `` app_state `instance-attribute` ¶
[code] 
    app_state = {}
    
[/code]

###  `` build_request ¶
[code] 
    build_request(
        method,
        url,
        *,
        content=None,
        data=None,
        files=None,
        json=None,
        params=None,
        headers=None,
        cookies=None,
        timeout=USE_CLIENT_DEFAULT,
        extensions=None
    )
    
[/code]

Build and return a request instance.

  * The `params`, `headers` and `cookies` arguments are merged with any values set on the client.
  * The `url` argument is merged with any `base_url` set on the client.


See also: [Request instances](advanced_clients_#request-instances.md)

Source code in `httpx/_client.py`
[code] 
    def build_request(
        self,
        method: str,
        url: URL | str,
        *,
        content: RequestContent | None = None,
        data: RequestData | None = None,
        files: RequestFiles | None = None,
        json: typing.Any | None = None,
        params: QueryParamTypes | None = None,
        headers: HeaderTypes | None = None,
        cookies: CookieTypes | None = None,
        timeout: TimeoutTypes | UseClientDefault = USE_CLIENT_DEFAULT,
        extensions: RequestExtensions | None = None,
    ) -> Request:
        """
        Build and return a request instance.
    
        * The `params`, `headers` and `cookies` arguments
        are merged with any values set on the client.
        * The `url` argument is merged with any `base_url` set on the client.
    
        See also: [Request instances][0]
    
        [0]: /advanced/clients/#request-instances
        """
        url = self._merge_url(url)
        headers = self._merge_headers(headers)
        cookies = self._merge_cookies(cookies)
        params = self._merge_queryparams(params)
        extensions = {} if extensions is None else extensions
        if "timeout" not in extensions:
            timeout = (
                self.timeout
                if isinstance(timeout, UseClientDefault)
                else Timeout(timeout)
            )
            extensions = dict(**extensions, timeout=timeout.as_dict())
        return Request(
            method,
            url,
            content=content,
            data=data,
            files=files,
            json=json,
            params=params,
            headers=headers,
            cookies=cookies,
            extensions=extensions,
        )
    
[/code]

###  `` stream ¶
[code] 
    stream(
        method,
        url,
        *,
        content=None,
        data=None,
        files=None,
        json=None,
        params=None,
        headers=None,
        cookies=None,
        auth=USE_CLIENT_DEFAULT,
        follow_redirects=USE_CLIENT_DEFAULT,
        timeout=USE_CLIENT_DEFAULT,
        extensions=None
    )
    
[/code]

Alternative to `httpx.request()` that streams the response body instead of loading it into memory at once.

**Parameters** : See `httpx.request`.

See also: [Streaming Responses](quickstart#streaming-responses.md)

Source code in `httpx/_client.py`
[code] 
    @contextmanager
    def stream(
        self,
        method: str,
        url: URL | str,
        *,
        content: RequestContent | None = None,
        data: RequestData | None = None,
        files: RequestFiles | None = None,
        json: typing.Any | None = None,
        params: QueryParamTypes | None = None,
        headers: HeaderTypes | None = None,
        cookies: CookieTypes | None = None,
        auth: AuthTypes | UseClientDefault | None = USE_CLIENT_DEFAULT,
        follow_redirects: bool | UseClientDefault = USE_CLIENT_DEFAULT,
        timeout: TimeoutTypes | UseClientDefault = USE_CLIENT_DEFAULT,
        extensions: RequestExtensions | None = None,
    ) -> typing.Iterator[Response]:
        """
        Alternative to `httpx.request()` that streams the response body
        instead of loading it into memory at once.
    
        **Parameters**: See `httpx.request`.
    
        See also: [Streaming Responses][0]
    
        [0]: /quickstart#streaming-responses
        """
        request = self.build_request(
            method=method,
            url=url,
            content=content,
            data=data,
            files=files,
            json=json,
            params=params,
            headers=headers,
            cookies=cookies,
            timeout=timeout,
            extensions=extensions,
        )
        response = self.send(
            request=request,
            auth=auth,
            follow_redirects=follow_redirects,
            stream=True,
        )
        try:
            yield response
        finally:
            response.close()
    
[/code]

###  `` send ¶
[code] 
    send(
        request,
        *,
        stream=False,
        auth=USE_CLIENT_DEFAULT,
        follow_redirects=USE_CLIENT_DEFAULT
    )
    
[/code]

Send a request.

The request is sent as-is, unmodified.

Typically you'll want to build one with `Client.build_request()` so that any client-level configuration is merged into the request, but passing an explicit `httpx.Request()` is supported as well.

See also: [Request instances](advanced_clients_#request-instances.md)

Source code in `httpx/_client.py`
[code] 
    def send(
        self,
        request: Request,
        *,
        stream: bool = False,
        auth: AuthTypes | UseClientDefault | None = USE_CLIENT_DEFAULT,
        follow_redirects: bool | UseClientDefault = USE_CLIENT_DEFAULT,
    ) -> Response:
        """
        Send a request.
    
        The request is sent as-is, unmodified.
    
        Typically you'll want to build one with `Client.build_request()`
        so that any client-level configuration is merged into the request,
        but passing an explicit `httpx.Request()` is supported as well.
    
        See also: [Request instances][0]
    
        [0]: /advanced/clients/#request-instances
        """
        if self._state == ClientState.CLOSED:
            raise RuntimeError("Cannot send a request, as the client has been closed.")
    
        self._state = ClientState.OPENED
        follow_redirects = (
            self.follow_redirects
            if isinstance(follow_redirects, UseClientDefault)
            else follow_redirects
        )
    
        self._set_timeout(request)
    
        auth = self._build_request_auth(request, auth)
    
        response = self._send_handling_auth(
            request,
            auth=auth,
            follow_redirects=follow_redirects,
            history=[],
        )
        try:
            if not stream:
                response.read()
    
            return response
    
        except BaseException as exc:
            response.close()
            raise exc
    
[/code]

###  `` close ¶
[code] 
    close()
    
[/code]

Close transport and proxies.

Source code in `httpx/_client.py`
[code] 
    def close(self) -> None:
        """
        Close transport and proxies.
        """
        if self._state != ClientState.CLOSED:
            self._state = ClientState.CLOSED
    
            self._transport.close()
            for transport in self._mounts.values():
                if transport is not None:
                    transport.close()
    
[/code]

###  `` request ¶
[code] 
    request(
        method,
        url,
        *,
        content=None,
        data=None,
        files=None,
        json=None,
        params=None,
        headers=None,
        cookies=None,
        auth=USE_CLIENT_DEFAULT,
        follow_redirects=USE_CLIENT_DEFAULT,
        timeout=USE_CLIENT_DEFAULT,
        extensions=None
    )
    
[/code]

Source code in `starlette/testclient.py`
[code] 
    def request(  # type: ignore[override]
        self,
        method: str,
        url: httpx._types.URLTypes,
        *,
        content: httpx._types.RequestContent | None = None,
        data: _RequestData | None = None,
        files: httpx._types.RequestFiles | None = None,
        json: Any = None,
        params: httpx._types.QueryParamTypes | None = None,
        headers: httpx._types.HeaderTypes | None = None,
        cookies: httpx._types.CookieTypes | None = None,
        auth: httpx._types.AuthTypes | httpx._client.UseClientDefault = httpx._client.USE_CLIENT_DEFAULT,
        follow_redirects: bool | httpx._client.UseClientDefault = httpx._client.USE_CLIENT_DEFAULT,
        timeout: httpx._types.TimeoutTypes | httpx._client.UseClientDefault = httpx._client.USE_CLIENT_DEFAULT,
        extensions: dict[str, Any] | None = None,
    ) -> httpx.Response:
        if timeout is not httpx.USE_CLIENT_DEFAULT:
            warnings.warn(
                "You should not use the 'timeout' argument with the TestClient. "
                "See https://github.com/Kludex/starlette/issues/1108 for more information.",
                StarletteDeprecationWarning,
                stacklevel=2,
            )
        url = self._merge_url(url)
        return super().request(
            method,
            url,
            content=content,
            data=data,
            files=files,
            json=json,
            params=params,
            headers=headers,
            cookies=cookies,
            auth=auth,
            follow_redirects=follow_redirects,
            timeout=timeout,
            extensions=extensions,
        )
    
[/code]

###  `` get ¶
[code] 
    get(
        url,
        *,
        params=None,
        headers=None,
        cookies=None,
        auth=USE_CLIENT_DEFAULT,
        follow_redirects=USE_CLIENT_DEFAULT,
        timeout=USE_CLIENT_DEFAULT,
        extensions=None
    )
    
[/code]

Source code in `starlette/testclient.py`
[code] 
    def get(  # type: ignore[override]
        self,
        url: httpx._types.URLTypes,
        *,
        params: httpx._types.QueryParamTypes | None = None,
        headers: httpx._types.HeaderTypes | None = None,
        cookies: httpx._types.CookieTypes | None = None,
        auth: httpx._types.AuthTypes | httpx._client.UseClientDefault = httpx._client.USE_CLIENT_DEFAULT,
        follow_redirects: bool | httpx._client.UseClientDefault = httpx._client.USE_CLIENT_DEFAULT,
        timeout: httpx._types.TimeoutTypes | httpx._client.UseClientDefault = httpx._client.USE_CLIENT_DEFAULT,
        extensions: dict[str, Any] | None = None,
    ) -> httpx.Response:
        return super().get(
            url,
            params=params,
            headers=headers,
            cookies=cookies,
            auth=auth,
            follow_redirects=follow_redirects,
            timeout=timeout,
            extensions=extensions,
        )
    
[/code]

###  `` options ¶
[code] 
    options(
        url,
        *,
        params=None,
        headers=None,
        cookies=None,
        auth=USE_CLIENT_DEFAULT,
        follow_redirects=USE_CLIENT_DEFAULT,
        timeout=USE_CLIENT_DEFAULT,
        extensions=None
    )
    
[/code]

Source code in `starlette/testclient.py`
[code] 
    def options(  # type: ignore[override]
        self,
        url: httpx._types.URLTypes,
        *,
        params: httpx._types.QueryParamTypes | None = None,
        headers: httpx._types.HeaderTypes | None = None,
        cookies: httpx._types.CookieTypes | None = None,
        auth: httpx._types.AuthTypes | httpx._client.UseClientDefault = httpx._client.USE_CLIENT_DEFAULT,
        follow_redirects: bool | httpx._client.UseClientDefault = httpx._client.USE_CLIENT_DEFAULT,
        timeout: httpx._types.TimeoutTypes | httpx._client.UseClientDefault = httpx._client.USE_CLIENT_DEFAULT,
        extensions: dict[str, Any] | None = None,
    ) -> httpx.Response:
        return super().options(
            url,
            params=params,
            headers=headers,
            cookies=cookies,
            auth=auth,
            follow_redirects=follow_redirects,
            timeout=timeout,
            extensions=extensions,
        )
    
[/code]

###  `` head ¶
[code] 
    head(
        url,
        *,
        params=None,
        headers=None,
        cookies=None,
        auth=USE_CLIENT_DEFAULT,
        follow_redirects=USE_CLIENT_DEFAULT,
        timeout=USE_CLIENT_DEFAULT,
        extensions=None
    )
    
[/code]

Source code in `starlette/testclient.py`
[code] 
    def head(  # type: ignore[override]
        self,
        url: httpx._types.URLTypes,
        *,
        params: httpx._types.QueryParamTypes | None = None,
        headers: httpx._types.HeaderTypes | None = None,
        cookies: httpx._types.CookieTypes | None = None,
        auth: httpx._types.AuthTypes | httpx._client.UseClientDefault = httpx._client.USE_CLIENT_DEFAULT,
        follow_redirects: bool | httpx._client.UseClientDefault = httpx._client.USE_CLIENT_DEFAULT,
        timeout: httpx._types.TimeoutTypes | httpx._client.UseClientDefault = httpx._client.USE_CLIENT_DEFAULT,
        extensions: dict[str, Any] | None = None,
    ) -> httpx.Response:
        return super().head(
            url,
            params=params,
            headers=headers,
            cookies=cookies,
            auth=auth,
            follow_redirects=follow_redirects,
            timeout=timeout,
            extensions=extensions,
        )
    
[/code]

###  `` post ¶
[code] 
    post(
        url,
        *,
        content=None,
        data=None,
        files=None,
        json=None,
        params=None,
        headers=None,
        cookies=None,
        auth=USE_CLIENT_DEFAULT,
        follow_redirects=USE_CLIENT_DEFAULT,
        timeout=USE_CLIENT_DEFAULT,
        extensions=None
    )
    
[/code]

Source code in `starlette/testclient.py`
[code] 
    def post(  # type: ignore[override]
        self,
        url: httpx._types.URLTypes,
        *,
        content: httpx._types.RequestContent | None = None,
        data: _RequestData | None = None,
        files: httpx._types.RequestFiles | None = None,
        json: Any = None,
        params: httpx._types.QueryParamTypes | None = None,
        headers: httpx._types.HeaderTypes | None = None,
        cookies: httpx._types.CookieTypes | None = None,
        auth: httpx._types.AuthTypes | httpx._client.UseClientDefault = httpx._client.USE_CLIENT_DEFAULT,
        follow_redirects: bool | httpx._client.UseClientDefault = httpx._client.USE_CLIENT_DEFAULT,
        timeout: httpx._types.TimeoutTypes | httpx._client.UseClientDefault = httpx._client.USE_CLIENT_DEFAULT,
        extensions: dict[str, Any] | None = None,
    ) -> httpx.Response:
        return super().post(
            url,
            content=content,
            data=data,
            files=files,
            json=json,
            params=params,
            headers=headers,
            cookies=cookies,
            auth=auth,
            follow_redirects=follow_redirects,
            timeout=timeout,
            extensions=extensions,
        )
    
[/code]

###  `` put ¶
[code] 
    put(
        url,
        *,
        content=None,
        data=None,
        files=None,
        json=None,
        params=None,
        headers=None,
        cookies=None,
        auth=USE_CLIENT_DEFAULT,
        follow_redirects=USE_CLIENT_DEFAULT,
        timeout=USE_CLIENT_DEFAULT,
        extensions=None
    )
    
[/code]

Source code in `starlette/testclient.py`
[code] 
    def put(  # type: ignore[override]
        self,
        url: httpx._types.URLTypes,
        *,
        content: httpx._types.RequestContent | None = None,
        data: _RequestData | None = None,
        files: httpx._types.RequestFiles | None = None,
        json: Any = None,
        params: httpx._types.QueryParamTypes | None = None,
        headers: httpx._types.HeaderTypes | None = None,
        cookies: httpx._types.CookieTypes | None = None,
        auth: httpx._types.AuthTypes | httpx._client.UseClientDefault = httpx._client.USE_CLIENT_DEFAULT,
        follow_redirects: bool | httpx._client.UseClientDefault = httpx._client.USE_CLIENT_DEFAULT,
        timeout: httpx._types.TimeoutTypes | httpx._client.UseClientDefault = httpx._client.USE_CLIENT_DEFAULT,
        extensions: dict[str, Any] | None = None,
    ) -> httpx.Response:
        return super().put(
            url,
            content=content,
            data=data,
            files=files,
            json=json,
            params=params,
            headers=headers,
            cookies=cookies,
            auth=auth,
            follow_redirects=follow_redirects,
            timeout=timeout,
            extensions=extensions,
        )
    
[/code]

###  `` patch ¶
[code] 
    patch(
        url,
        *,
        content=None,
        data=None,
        files=None,
        json=None,
        params=None,
        headers=None,
        cookies=None,
        auth=USE_CLIENT_DEFAULT,
        follow_redirects=USE_CLIENT_DEFAULT,
        timeout=USE_CLIENT_DEFAULT,
        extensions=None
    )
    
[/code]

Source code in `starlette/testclient.py`
[code] 
    def patch(  # type: ignore[override]
        self,
        url: httpx._types.URLTypes,
        *,
        content: httpx._types.RequestContent | None = None,
        data: _RequestData | None = None,
        files: httpx._types.RequestFiles | None = None,
        json: Any = None,
        params: httpx._types.QueryParamTypes | None = None,
        headers: httpx._types.HeaderTypes | None = None,
        cookies: httpx._types.CookieTypes | None = None,
        auth: httpx._types.AuthTypes | httpx._client.UseClientDefault = httpx._client.USE_CLIENT_DEFAULT,
        follow_redirects: bool | httpx._client.UseClientDefault = httpx._client.USE_CLIENT_DEFAULT,
        timeout: httpx._types.TimeoutTypes | httpx._client.UseClientDefault = httpx._client.USE_CLIENT_DEFAULT,
        extensions: dict[str, Any] | None = None,
    ) -> httpx.Response:
        return super().patch(
            url,
            content=content,
            data=data,
            files=files,
            json=json,
            params=params,
            headers=headers,
            cookies=cookies,
            auth=auth,
            follow_redirects=follow_redirects,
            timeout=timeout,
            extensions=extensions,
        )
    
[/code]

###  `` delete ¶
[code] 
    delete(
        url,
        *,
        params=None,
        headers=None,
        cookies=None,
        auth=USE_CLIENT_DEFAULT,
        follow_redirects=USE_CLIENT_DEFAULT,
        timeout=USE_CLIENT_DEFAULT,
        extensions=None
    )
    
[/code]

Source code in `starlette/testclient.py`
[code] 
    def delete(  # type: ignore[override]
        self,
        url: httpx._types.URLTypes,
        *,
        params: httpx._types.QueryParamTypes | None = None,
        headers: httpx._types.HeaderTypes | None = None,
        cookies: httpx._types.CookieTypes | None = None,
        auth: httpx._types.AuthTypes | httpx._client.UseClientDefault = httpx._client.USE_CLIENT_DEFAULT,
        follow_redirects: bool | httpx._client.UseClientDefault = httpx._client.USE_CLIENT_DEFAULT,
        timeout: httpx._types.TimeoutTypes | httpx._client.UseClientDefault = httpx._client.USE_CLIENT_DEFAULT,
        extensions: dict[str, Any] | None = None,
    ) -> httpx.Response:
        return super().delete(
            url,
            params=params,
            headers=headers,
            cookies=cookies,
            auth=auth,
            follow_redirects=follow_redirects,
            timeout=timeout,
            extensions=extensions,
        )
    
[/code]

###  `` websocket_connect ¶
[code] 
    websocket_connect(url, subprotocols=None, **kwargs)
    
[/code]

Source code in `starlette/testclient.py`
[code] 
    def websocket_connect(
        self,
        url: str,
        subprotocols: Sequence[str] | None = None,
        **kwargs: Any,
    ) -> WebSocketTestSession:
        url = urljoin("ws://testserver", url)
        headers = kwargs.get("headers", {})
        headers.setdefault("connection", "upgrade")
        headers.setdefault("sec-websocket-key", "testserver==")
        headers.setdefault("sec-websocket-version", "13")
        if subprotocols is not None:
            headers.setdefault("sec-websocket-protocol", ", ".join(subprotocols))
        kwargs["headers"] = headers
        try:
            super().request("GET", url, **kwargs)
        except _Upgrade as exc:
            session = exc.session
        else:
            raise RuntimeError("Expected WebSocket upgrade")  # pragma: no cover
    
        return session
    
[/code]

###  `` lifespan `async` ¶
[code] 
    lifespan()
    
[/code]

Source code in `starlette/testclient.py`
[code] 
    async def lifespan(self) -> None:
        scope = {"type": "lifespan", "state": self.app_state}
        try:
            await self.app(scope, self.stream_receive.receive, self.stream_send.send)
        finally:
            await self.stream_send.send(None)
    
[/code]

###  `` wait_startup `async` ¶
[code] 
    wait_startup()
    
[/code]

Source code in `starlette/testclient.py`
[code] 
    async def wait_startup(self) -> None:
        await self.stream_receive.send({"type": "lifespan.startup"})
    
        async def receive() -> Any:
            message = await self.stream_send.receive()
            if message is None:
                self.task.result()
            return message
    
        message = await receive()
        assert message["type"] in (
            "lifespan.startup.complete",
            "lifespan.startup.failed",
        )
        if message["type"] == "lifespan.startup.failed":
            await receive()
    
[/code]

###  `` wait_shutdown `async` ¶
[code] 
    wait_shutdown()
    
[/code]

Source code in `starlette/testclient.py`
[code] 
    async def wait_shutdown(self) -> None:
        async def receive() -> Any:
            message = await self.stream_send.receive()
            if message is None:
                self.task.result()
            return message
    
        await self.stream_receive.send({"type": "lifespan.shutdown"})
        message = await receive()
        assert message["type"] in (
            "lifespan.shutdown.complete",
            "lifespan.shutdown.failed",
        )
        if message["type"] == "lifespan.shutdown.failed":
            await receive()
    
[/code]
