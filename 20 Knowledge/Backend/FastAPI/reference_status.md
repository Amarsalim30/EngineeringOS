---
title: "Status"
source: "https://fastapi.tiangolo.com/reference/status/"
---

# Status Codes¶

You can import the `status` module from `fastapi`:
[code] 
    from fastapi import status
    
[/code]

`status` is provided directly by Starlette.

It contains a group of named constants (variables) with integer status codes.

For example:

  * 200: `status.HTTP_200_OK`
  * 403: `status.HTTP_403_FORBIDDEN`
  * etc.


It can be convenient to quickly access HTTP (and WebSocket) status codes in your app, using autocompletion for the name without having to memorize the integer status codes.

Read more about it in the [FastAPI docs about Response Status Code](https://fastapi.tiangolo.com/tutorial/response-status-code/).

## Example¶
[code] 
    from fastapi import FastAPI, status
    
    app = FastAPI()
    
    
    @app.get("/items/", status_code=status.HTTP_418_IM_A_TEAPOT)
    def read_items():
        return [{"name": "Plumbus"}, {"name": "Portal Gun"}]
    
[/code]

##  `` fastapi.status ¶

HTTP codes See HTTP Status Code Registry: https://www.iana.org/assignments/http-status-codes/http-status-codes.xhtml

And RFC 9110 - https://www.rfc-editor.org/rfc/rfc9110

###  `` HTTP_100_CONTINUE `module-attribute` ¶
[code] 
    HTTP_100_CONTINUE = 100
    
[/code]

###  `` HTTP_101_SWITCHING_PROTOCOLS `module-attribute` ¶
[code] 
    HTTP_101_SWITCHING_PROTOCOLS = 101
    
[/code]

###  `` HTTP_102_PROCESSING `module-attribute` ¶
[code] 
    HTTP_102_PROCESSING = 102
    
[/code]

###  `` HTTP_103_EARLY_HINTS `module-attribute` ¶
[code] 
    HTTP_103_EARLY_HINTS = 103
    
[/code]

###  `` HTTP_200_OK `module-attribute` ¶
[code] 
    HTTP_200_OK = 200
    
[/code]

###  `` HTTP_201_CREATED `module-attribute` ¶
[code] 
    HTTP_201_CREATED = 201
    
[/code]

###  `` HTTP_202_ACCEPTED `module-attribute` ¶
[code] 
    HTTP_202_ACCEPTED = 202
    
[/code]

###  `` HTTP_203_NON_AUTHORITATIVE_INFORMATION `module-attribute` ¶
[code] 
    HTTP_203_NON_AUTHORITATIVE_INFORMATION = 203
    
[/code]

###  `` HTTP_204_NO_CONTENT `module-attribute` ¶
[code] 
    HTTP_204_NO_CONTENT = 204
    
[/code]

###  `` HTTP_205_RESET_CONTENT `module-attribute` ¶
[code] 
    HTTP_205_RESET_CONTENT = 205
    
[/code]

###  `` HTTP_206_PARTIAL_CONTENT `module-attribute` ¶
[code] 
    HTTP_206_PARTIAL_CONTENT = 206
    
[/code]

###  `` HTTP_207_MULTI_STATUS `module-attribute` ¶
[code] 
    HTTP_207_MULTI_STATUS = 207
    
[/code]

###  `` HTTP_208_ALREADY_REPORTED `module-attribute` ¶
[code] 
    HTTP_208_ALREADY_REPORTED = 208
    
[/code]

###  `` HTTP_226_IM_USED `module-attribute` ¶
[code] 
    HTTP_226_IM_USED = 226
    
[/code]

###  `` HTTP_300_MULTIPLE_CHOICES `module-attribute` ¶
[code] 
    HTTP_300_MULTIPLE_CHOICES = 300
    
[/code]

###  `` HTTP_301_MOVED_PERMANENTLY `module-attribute` ¶
[code] 
    HTTP_301_MOVED_PERMANENTLY = 301
    
[/code]

###  `` HTTP_302_FOUND `module-attribute` ¶
[code] 
    HTTP_302_FOUND = 302
    
[/code]

###  `` HTTP_303_SEE_OTHER `module-attribute` ¶
[code] 
    HTTP_303_SEE_OTHER = 303
    
[/code]

###  `` HTTP_304_NOT_MODIFIED `module-attribute` ¶
[code] 
    HTTP_304_NOT_MODIFIED = 304
    
[/code]

###  `` HTTP_305_USE_PROXY `module-attribute` ¶
[code] 
    HTTP_305_USE_PROXY = 305
    
[/code]

###  `` HTTP_306_RESERVED `module-attribute` ¶
[code] 
    HTTP_306_RESERVED = 306
    
[/code]

###  `` HTTP_307_TEMPORARY_REDIRECT `module-attribute` ¶
[code] 
    HTTP_307_TEMPORARY_REDIRECT = 307
    
[/code]

###  `` HTTP_308_PERMANENT_REDIRECT `module-attribute` ¶
[code] 
    HTTP_308_PERMANENT_REDIRECT = 308
    
[/code]

###  `` HTTP_400_BAD_REQUEST `module-attribute` ¶
[code] 
    HTTP_400_BAD_REQUEST = 400
    
[/code]

###  `` HTTP_401_UNAUTHORIZED `module-attribute` ¶
[code] 
    HTTP_401_UNAUTHORIZED = 401
    
[/code]

###  `` HTTP_402_PAYMENT_REQUIRED `module-attribute` ¶
[code] 
    HTTP_402_PAYMENT_REQUIRED = 402
    
[/code]

###  `` HTTP_403_FORBIDDEN `module-attribute` ¶
[code] 
    HTTP_403_FORBIDDEN = 403
    
[/code]

###  `` HTTP_404_NOT_FOUND `module-attribute` ¶
[code] 
    HTTP_404_NOT_FOUND = 404
    
[/code]

###  `` HTTP_405_METHOD_NOT_ALLOWED `module-attribute` ¶
[code] 
    HTTP_405_METHOD_NOT_ALLOWED = 405
    
[/code]

###  `` HTTP_406_NOT_ACCEPTABLE `module-attribute` ¶
[code] 
    HTTP_406_NOT_ACCEPTABLE = 406
    
[/code]

###  `` HTTP_407_PROXY_AUTHENTICATION_REQUIRED `module-attribute` ¶
[code] 
    HTTP_407_PROXY_AUTHENTICATION_REQUIRED = 407
    
[/code]

###  `` HTTP_408_REQUEST_TIMEOUT `module-attribute` ¶
[code] 
    HTTP_408_REQUEST_TIMEOUT = 408
    
[/code]

###  `` HTTP_409_CONFLICT `module-attribute` ¶
[code] 
    HTTP_409_CONFLICT = 409
    
[/code]

###  `` HTTP_410_GONE `module-attribute` ¶
[code] 
    HTTP_410_GONE = 410
    
[/code]

###  `` HTTP_411_LENGTH_REQUIRED `module-attribute` ¶
[code] 
    HTTP_411_LENGTH_REQUIRED = 411
    
[/code]

###  `` HTTP_412_PRECONDITION_FAILED `module-attribute` ¶
[code] 
    HTTP_412_PRECONDITION_FAILED = 412
    
[/code]

###  `` HTTP_413_CONTENT_TOO_LARGE `module-attribute` ¶
[code] 
    HTTP_413_CONTENT_TOO_LARGE = 413
    
[/code]

###  `` HTTP_414_URI_TOO_LONG `module-attribute` ¶
[code] 
    HTTP_414_URI_TOO_LONG = 414
    
[/code]

###  `` HTTP_415_UNSUPPORTED_MEDIA_TYPE `module-attribute` ¶
[code] 
    HTTP_415_UNSUPPORTED_MEDIA_TYPE = 415
    
[/code]

###  `` HTTP_416_RANGE_NOT_SATISFIABLE `module-attribute` ¶
[code] 
    HTTP_416_RANGE_NOT_SATISFIABLE = 416
    
[/code]

###  `` HTTP_417_EXPECTATION_FAILED `module-attribute` ¶
[code] 
    HTTP_417_EXPECTATION_FAILED = 417
    
[/code]

###  `` HTTP_418_IM_A_TEAPOT `module-attribute` ¶
[code] 
    HTTP_418_IM_A_TEAPOT = 418
    
[/code]

###  `` HTTP_421_MISDIRECTED_REQUEST `module-attribute` ¶
[code] 
    HTTP_421_MISDIRECTED_REQUEST = 421
    
[/code]

###  `` HTTP_422_UNPROCESSABLE_CONTENT `module-attribute` ¶
[code] 
    HTTP_422_UNPROCESSABLE_CONTENT = 422
    
[/code]

###  `` HTTP_423_LOCKED `module-attribute` ¶
[code] 
    HTTP_423_LOCKED = 423
    
[/code]

###  `` HTTP_424_FAILED_DEPENDENCY `module-attribute` ¶
[code] 
    HTTP_424_FAILED_DEPENDENCY = 424
    
[/code]

###  `` HTTP_425_TOO_EARLY `module-attribute` ¶
[code] 
    HTTP_425_TOO_EARLY = 425
    
[/code]

###  `` HTTP_426_UPGRADE_REQUIRED `module-attribute` ¶
[code] 
    HTTP_426_UPGRADE_REQUIRED = 426
    
[/code]

###  `` HTTP_428_PRECONDITION_REQUIRED `module-attribute` ¶
[code] 
    HTTP_428_PRECONDITION_REQUIRED = 428
    
[/code]

###  `` HTTP_429_TOO_MANY_REQUESTS `module-attribute` ¶
[code] 
    HTTP_429_TOO_MANY_REQUESTS = 429
    
[/code]

###  `` HTTP_431_REQUEST_HEADER_FIELDS_TOO_LARGE `module-attribute` ¶
[code] 
    HTTP_431_REQUEST_HEADER_FIELDS_TOO_LARGE = 431
    
[/code]

###  `` HTTP_451_UNAVAILABLE_FOR_LEGAL_REASONS `module-attribute` ¶
[code] 
    HTTP_451_UNAVAILABLE_FOR_LEGAL_REASONS = 451
    
[/code]

###  `` HTTP_500_INTERNAL_SERVER_ERROR `module-attribute` ¶
[code] 
    HTTP_500_INTERNAL_SERVER_ERROR = 500
    
[/code]

###  `` HTTP_501_NOT_IMPLEMENTED `module-attribute` ¶
[code] 
    HTTP_501_NOT_IMPLEMENTED = 501
    
[/code]

###  `` HTTP_502_BAD_GATEWAY `module-attribute` ¶
[code] 
    HTTP_502_BAD_GATEWAY = 502
    
[/code]

###  `` HTTP_503_SERVICE_UNAVAILABLE `module-attribute` ¶
[code] 
    HTTP_503_SERVICE_UNAVAILABLE = 503
    
[/code]

###  `` HTTP_504_GATEWAY_TIMEOUT `module-attribute` ¶
[code] 
    HTTP_504_GATEWAY_TIMEOUT = 504
    
[/code]

###  `` HTTP_505_HTTP_VERSION_NOT_SUPPORTED `module-attribute` ¶
[code] 
    HTTP_505_HTTP_VERSION_NOT_SUPPORTED = 505
    
[/code]

###  `` HTTP_506_VARIANT_ALSO_NEGOTIATES `module-attribute` ¶
[code] 
    HTTP_506_VARIANT_ALSO_NEGOTIATES = 506
    
[/code]

###  `` HTTP_507_INSUFFICIENT_STORAGE `module-attribute` ¶
[code] 
    HTTP_507_INSUFFICIENT_STORAGE = 507
    
[/code]

###  `` HTTP_508_LOOP_DETECTED `module-attribute` ¶
[code] 
    HTTP_508_LOOP_DETECTED = 508
    
[/code]

###  `` HTTP_510_NOT_EXTENDED `module-attribute` ¶
[code] 
    HTTP_510_NOT_EXTENDED = 510
    
[/code]

###  `` HTTP_511_NETWORK_AUTHENTICATION_REQUIRED `module-attribute` ¶
[code] 
    HTTP_511_NETWORK_AUTHENTICATION_REQUIRED = 511
    
[/code]

WebSocket codes https://www.iana.org/assignments/websocket/websocket.xml#close-code-number https://developer.mozilla.org/en-US/docs/Web/API/CloseEvent

###  `` WS_1000_NORMAL_CLOSURE `module-attribute` ¶
[code] 
    WS_1000_NORMAL_CLOSURE = 1000
    
[/code]

###  `` WS_1001_GOING_AWAY `module-attribute` ¶
[code] 
    WS_1001_GOING_AWAY = 1001
    
[/code]

###  `` WS_1002_PROTOCOL_ERROR `module-attribute` ¶
[code] 
    WS_1002_PROTOCOL_ERROR = 1002
    
[/code]

###  `` WS_1003_UNSUPPORTED_DATA `module-attribute` ¶
[code] 
    WS_1003_UNSUPPORTED_DATA = 1003
    
[/code]

###  `` WS_1005_NO_STATUS_RCVD `module-attribute` ¶
[code] 
    WS_1005_NO_STATUS_RCVD = 1005
    
[/code]

###  `` WS_1006_ABNORMAL_CLOSURE `module-attribute` ¶
[code] 
    WS_1006_ABNORMAL_CLOSURE = 1006
    
[/code]

###  `` WS_1007_INVALID_FRAME_PAYLOAD_DATA `module-attribute` ¶
[code] 
    WS_1007_INVALID_FRAME_PAYLOAD_DATA = 1007
    
[/code]

###  `` WS_1008_POLICY_VIOLATION `module-attribute` ¶
[code] 
    WS_1008_POLICY_VIOLATION = 1008
    
[/code]

###  `` WS_1009_MESSAGE_TOO_BIG `module-attribute` ¶
[code] 
    WS_1009_MESSAGE_TOO_BIG = 1009
    
[/code]

###  `` WS_1010_MANDATORY_EXT `module-attribute` ¶
[code] 
    WS_1010_MANDATORY_EXT = 1010
    
[/code]

###  `` WS_1011_INTERNAL_ERROR `module-attribute` ¶
[code] 
    WS_1011_INTERNAL_ERROR = 1011
    
[/code]

###  `` WS_1012_SERVICE_RESTART `module-attribute` ¶
[code] 
    WS_1012_SERVICE_RESTART = 1012
    
[/code]

###  `` WS_1013_TRY_AGAIN_LATER `module-attribute` ¶
[code] 
    WS_1013_TRY_AGAIN_LATER = 1013
    
[/code]

###  `` WS_1014_BAD_GATEWAY `module-attribute` ¶
[code] 
    WS_1014_BAD_GATEWAY = 1014
    
[/code]

###  `` WS_1015_TLS_HANDSHAKE `module-attribute` ¶
[code] 
    WS_1015_TLS_HANDSHAKE = 1015
    
[/code]
