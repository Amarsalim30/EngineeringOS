---
title: "Models"
source: "https://fastapi.tiangolo.com/reference/openapi/models/"
---

# OpenAPI `models`¶

OpenAPI Pydantic models used to generate and validate the generated OpenAPI.

##  `` fastapi.openapi.models ¶

###  `` SchemaType `module-attribute` ¶
[code] 
    SchemaType = Literal[
        "array",
        "boolean",
        "integer",
        "null",
        "number",
        "object",
        "string",
    ]
    
[/code]

###  `` SchemaOrBool `module-attribute` ¶
[code] 
    SchemaOrBool = [Schema](#fastapi.openapi.models.Schema "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">Schema<_span>".md) | bool
    
[/code]

###  `` SecurityScheme `module-attribute` ¶
[code] 
    SecurityScheme = (
        [APIKey](#fastapi.openapi.models.APIKey "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">APIKey<_span>".md) | [HTTPBase](#fastapi.openapi.models.HTTPBase "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">HTTPBase<_span>".md) | [OAuth2](#fastapi.openapi.models.OAuth2 "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">OAuth2<_span>".md) | [OpenIdConnect](#fastapi.openapi.models.OpenIdConnect "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">OpenIdConnect<_span>".md) | [HTTPBearer](#fastapi.openapi.models.HTTPBearer "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">HTTPBearer<_span>".md)
    )
    
[/code]

###  `` BaseModelWithConfig ¶

Bases: `BaseModel`

####  `` model_config `class-attribute` `instance-attribute` ¶
[code] 
    model_config = {'extra': 'allow'}
    
[/code]

###  `` Contact ¶

Bases: `[BaseModelWithConfig](#fastapi.openapi.models.BaseModelWithConfig "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">BaseModelWithConfig<_span>".md)`

####  `` name `class-attribute` `instance-attribute` ¶
[code] 
    name = None
    
[/code]

####  `` url `class-attribute` `instance-attribute` ¶
[code] 
    url = None
    
[/code]

####  `` email `class-attribute` `instance-attribute` ¶
[code] 
    email = None
    
[/code]

####  `` model_config `class-attribute` `instance-attribute` ¶
[code] 
    model_config = {'extra': 'allow'}
    
[/code]

###  `` License ¶

Bases: `[BaseModelWithConfig](#fastapi.openapi.models.BaseModelWithConfig "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">BaseModelWithConfig<_span>".md)`

####  `` name `instance-attribute` ¶
[code] 
    name
    
[/code]

####  `` identifier `class-attribute` `instance-attribute` ¶
[code] 
    identifier = None
    
[/code]

####  `` url `class-attribute` `instance-attribute` ¶
[code] 
    url = None
    
[/code]

####  `` model_config `class-attribute` `instance-attribute` ¶
[code] 
    model_config = {'extra': 'allow'}
    
[/code]

###  `` Info ¶

Bases: `[BaseModelWithConfig](#fastapi.openapi.models.BaseModelWithConfig "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">BaseModelWithConfig<_span>".md)`

####  `` title `instance-attribute` ¶
[code] 
    title
    
[/code]

####  `` summary `class-attribute` `instance-attribute` ¶
[code] 
    summary = None
    
[/code]

####  `` description `class-attribute` `instance-attribute` ¶
[code] 
    description = None
    
[/code]

####  `` termsOfService `class-attribute` `instance-attribute` ¶
[code] 
    termsOfService = None
    
[/code]

####  `` contact `class-attribute` `instance-attribute` ¶
[code] 
    contact = None
    
[/code]

####  `` license `class-attribute` `instance-attribute` ¶
[code] 
    license = None
    
[/code]

####  `` version `instance-attribute` ¶
[code] 
    version
    
[/code]

####  `` model_config `class-attribute` `instance-attribute` ¶
[code] 
    model_config = {'extra': 'allow'}
    
[/code]

###  `` ServerVariable ¶

Bases: `[BaseModelWithConfig](#fastapi.openapi.models.BaseModelWithConfig "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">BaseModelWithConfig<_span>".md)`

####  `` enum `class-attribute` `instance-attribute` ¶
[code] 
    enum = None
    
[/code]

####  `` default `instance-attribute` ¶
[code] 
    default
    
[/code]

####  `` description `class-attribute` `instance-attribute` ¶
[code] 
    description = None
    
[/code]

####  `` model_config `class-attribute` `instance-attribute` ¶
[code] 
    model_config = {'extra': 'allow'}
    
[/code]

###  `` Server ¶

Bases: `[BaseModelWithConfig](#fastapi.openapi.models.BaseModelWithConfig "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">BaseModelWithConfig<_span>".md)`

####  `` url `instance-attribute` ¶
[code] 
    url
    
[/code]

####  `` description `class-attribute` `instance-attribute` ¶
[code] 
    description = None
    
[/code]

####  `` variables `class-attribute` `instance-attribute` ¶
[code] 
    variables = None
    
[/code]

####  `` model_config `class-attribute` `instance-attribute` ¶
[code] 
    model_config = {'extra': 'allow'}
    
[/code]

###  `` Reference ¶

Bases: `BaseModel`

####  `` ref `class-attribute` `instance-attribute` ¶
[code] 
    ref = Field(alias='$ref')
    
[/code]

###  `` Discriminator ¶

Bases: `BaseModel`

####  `` propertyName `instance-attribute` ¶
[code] 
    propertyName
    
[/code]

####  `` mapping `class-attribute` `instance-attribute` ¶
[code] 
    mapping = None
    
[/code]

###  `` XML ¶

Bases: `[BaseModelWithConfig](#fastapi.openapi.models.BaseModelWithConfig "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">BaseModelWithConfig<_span>".md)`

####  `` name `class-attribute` `instance-attribute` ¶
[code] 
    name = None
    
[/code]

####  `` namespace `class-attribute` `instance-attribute` ¶
[code] 
    namespace = None
    
[/code]

####  `` prefix `class-attribute` `instance-attribute` ¶
[code] 
    prefix = None
    
[/code]

####  `` attribute `class-attribute` `instance-attribute` ¶
[code] 
    attribute = None
    
[/code]

####  `` wrapped `class-attribute` `instance-attribute` ¶
[code] 
    wrapped = None
    
[/code]

####  `` model_config `class-attribute` `instance-attribute` ¶
[code] 
    model_config = {'extra': 'allow'}
    
[/code]

###  `` ExternalDocumentation ¶

Bases: `[BaseModelWithConfig](#fastapi.openapi.models.BaseModelWithConfig "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">BaseModelWithConfig<_span>".md)`

####  `` description `class-attribute` `instance-attribute` ¶
[code] 
    description = None
    
[/code]

####  `` url `instance-attribute` ¶
[code] 
    url
    
[/code]

####  `` model_config `class-attribute` `instance-attribute` ¶
[code] 
    model_config = {'extra': 'allow'}
    
[/code]

###  `` Schema ¶

Bases: `[BaseModelWithConfig](#fastapi.openapi.models.BaseModelWithConfig "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">BaseModelWithConfig<_span>".md)`

####  `` schema_ `class-attribute` `instance-attribute` ¶
[code] 
    schema_ = Field(default=None, alias='$schema')
    
[/code]

####  `` vocabulary `class-attribute` `instance-attribute` ¶
[code] 
    vocabulary = Field(default=None, alias='$vocabulary')
    
[/code]

####  `` id `class-attribute` `instance-attribute` ¶
[code] 
    id = Field(default=None, alias='$id')
    
[/code]

####  `` anchor `class-attribute` `instance-attribute` ¶
[code] 
    anchor = Field(default=None, alias='$anchor')
    
[/code]

####  `` dynamicAnchor `class-attribute` `instance-attribute` ¶
[code] 
    dynamicAnchor = Field(default=None, alias='$dynamicAnchor')
    
[/code]

####  `` ref `class-attribute` `instance-attribute` ¶
[code] 
    ref = Field(default=None, alias='$ref')
    
[/code]

####  `` dynamicRef `class-attribute` `instance-attribute` ¶
[code] 
    dynamicRef = Field(default=None, alias='$dynamicRef')
    
[/code]

####  `` defs `class-attribute` `instance-attribute` ¶
[code] 
    defs = Field(default=None, alias='$defs')
    
[/code]

####  `` comment `class-attribute` `instance-attribute` ¶
[code] 
    comment = Field(default=None, alias='$comment')
    
[/code]

####  `` allOf `class-attribute` `instance-attribute` ¶
[code] 
    allOf = None
    
[/code]

####  `` anyOf `class-attribute` `instance-attribute` ¶
[code] 
    anyOf = None
    
[/code]

####  `` oneOf `class-attribute` `instance-attribute` ¶
[code] 
    oneOf = None
    
[/code]

####  `` not_ `class-attribute` `instance-attribute` ¶
[code] 
    not_ = Field(default=None, alias='not')
    
[/code]

####  `` if_ `class-attribute` `instance-attribute` ¶
[code] 
    if_ = Field(default=None, alias='if')
    
[/code]

####  `` then `class-attribute` `instance-attribute` ¶
[code] 
    then = None
    
[/code]

####  `` else_ `class-attribute` `instance-attribute` ¶
[code] 
    else_ = Field(default=None, alias='else')
    
[/code]

####  `` dependentSchemas `class-attribute` `instance-attribute` ¶
[code] 
    dependentSchemas = None
    
[/code]

####  `` prefixItems `class-attribute` `instance-attribute` ¶
[code] 
    prefixItems = None
    
[/code]

####  `` items `class-attribute` `instance-attribute` ¶
[code] 
    items = None
    
[/code]

####  `` contains `class-attribute` `instance-attribute` ¶
[code] 
    contains = None
    
[/code]

####  `` properties `class-attribute` `instance-attribute` ¶
[code] 
    properties = None
    
[/code]

####  `` patternProperties `class-attribute` `instance-attribute` ¶
[code] 
    patternProperties = None
    
[/code]

####  `` additionalProperties `class-attribute` `instance-attribute` ¶
[code] 
    additionalProperties = None
    
[/code]

####  `` propertyNames `class-attribute` `instance-attribute` ¶
[code] 
    propertyNames = None
    
[/code]

####  `` unevaluatedItems `class-attribute` `instance-attribute` ¶
[code] 
    unevaluatedItems = None
    
[/code]

####  `` unevaluatedProperties `class-attribute` `instance-attribute` ¶
[code] 
    unevaluatedProperties = None
    
[/code]

####  `` type `class-attribute` `instance-attribute` ¶
[code] 
    type = None
    
[/code]

####  `` enum `class-attribute` `instance-attribute` ¶
[code] 
    enum = None
    
[/code]

####  `` const `class-attribute` `instance-attribute` ¶
[code] 
    const = None
    
[/code]

####  `` multipleOf `class-attribute` `instance-attribute` ¶
[code] 
    multipleOf = Field(default=None, gt=0)
    
[/code]

####  `` maximum `class-attribute` `instance-attribute` ¶
[code] 
    maximum = None
    
[/code]

####  `` exclusiveMaximum `class-attribute` `instance-attribute` ¶
[code] 
    exclusiveMaximum = None
    
[/code]

####  `` minimum `class-attribute` `instance-attribute` ¶
[code] 
    minimum = None
    
[/code]

####  `` exclusiveMinimum `class-attribute` `instance-attribute` ¶
[code] 
    exclusiveMinimum = None
    
[/code]

####  `` maxLength `class-attribute` `instance-attribute` ¶
[code] 
    maxLength = Field(default=None, ge=0)
    
[/code]

####  `` minLength `class-attribute` `instance-attribute` ¶
[code] 
    minLength = Field(default=None, ge=0)
    
[/code]

####  `` pattern `class-attribute` `instance-attribute` ¶
[code] 
    pattern = None
    
[/code]

####  `` maxItems `class-attribute` `instance-attribute` ¶
[code] 
    maxItems = Field(default=None, ge=0)
    
[/code]

####  `` minItems `class-attribute` `instance-attribute` ¶
[code] 
    minItems = Field(default=None, ge=0)
    
[/code]

####  `` uniqueItems `class-attribute` `instance-attribute` ¶
[code] 
    uniqueItems = None
    
[/code]

####  `` maxContains `class-attribute` `instance-attribute` ¶
[code] 
    maxContains = Field(default=None, ge=0)
    
[/code]

####  `` minContains `class-attribute` `instance-attribute` ¶
[code] 
    minContains = Field(default=None, ge=0)
    
[/code]

####  `` maxProperties `class-attribute` `instance-attribute` ¶
[code] 
    maxProperties = Field(default=None, ge=0)
    
[/code]

####  `` minProperties `class-attribute` `instance-attribute` ¶
[code] 
    minProperties = Field(default=None, ge=0)
    
[/code]

####  `` required `class-attribute` `instance-attribute` ¶
[code] 
    required = None
    
[/code]

####  `` dependentRequired `class-attribute` `instance-attribute` ¶
[code] 
    dependentRequired = None
    
[/code]

####  `` format `class-attribute` `instance-attribute` ¶
[code] 
    format = None
    
[/code]

####  `` contentEncoding `class-attribute` `instance-attribute` ¶
[code] 
    contentEncoding = None
    
[/code]

####  `` contentMediaType `class-attribute` `instance-attribute` ¶
[code] 
    contentMediaType = None
    
[/code]

####  `` contentSchema `class-attribute` `instance-attribute` ¶
[code] 
    contentSchema = None
    
[/code]

####  `` title `class-attribute` `instance-attribute` ¶
[code] 
    title = None
    
[/code]

####  `` description `class-attribute` `instance-attribute` ¶
[code] 
    description = None
    
[/code]

####  `` default `class-attribute` `instance-attribute` ¶
[code] 
    default = None
    
[/code]

####  `` deprecated `class-attribute` `instance-attribute` ¶
[code] 
    deprecated = None
    
[/code]

####  `` readOnly `class-attribute` `instance-attribute` ¶
[code] 
    readOnly = None
    
[/code]

####  `` writeOnly `class-attribute` `instance-attribute` ¶
[code] 
    writeOnly = None
    
[/code]

####  `` examples `class-attribute` `instance-attribute` ¶
[code] 
    examples = None
    
[/code]

####  `` discriminator `class-attribute` `instance-attribute` ¶
[code] 
    discriminator = None
    
[/code]

####  `` xml `class-attribute` `instance-attribute` ¶
[code] 
    xml = None
    
[/code]

####  `` externalDocs `class-attribute` `instance-attribute` ¶
[code] 
    externalDocs = None
    
[/code]

####  `` example `class-attribute` `instance-attribute` ¶
[code] 
    example = None
    
[/code]

Deprecated in OpenAPI 3.1.0 that now uses JSON Schema 2020-12, although still supported. Use examples instead.

####  `` model_config `class-attribute` `instance-attribute` ¶
[code] 
    model_config = {'extra': 'allow'}
    
[/code]

###  `` Example ¶

Bases: `TypedDict`

####  `` summary `instance-attribute` ¶
[code] 
    summary
    
[/code]

####  `` description `instance-attribute` ¶
[code] 
    description
    
[/code]

####  `` value `instance-attribute` ¶
[code] 
    value
    
[/code]

####  `` externalValue `instance-attribute` ¶
[code] 
    externalValue
    
[/code]

###  `` ParameterInType ¶

Bases: `Enum`

####  `` query `class-attribute` `instance-attribute` ¶
[code] 
    query = 'query'
    
[/code]

####  `` header `class-attribute` `instance-attribute` ¶
[code] 
    header = 'header'
    
[/code]

####  `` path `class-attribute` `instance-attribute` ¶
[code] 
    path = 'path'
    
[/code]

####  `` cookie `class-attribute` `instance-attribute` ¶
[code] 
    cookie = 'cookie'
    
[/code]

###  `` Encoding ¶

Bases: `[BaseModelWithConfig](#fastapi.openapi.models.BaseModelWithConfig "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">BaseModelWithConfig<_span>".md)`

####  `` contentType `class-attribute` `instance-attribute` ¶
[code] 
    contentType = None
    
[/code]

####  `` headers `class-attribute` `instance-attribute` ¶
[code] 
    headers = None
    
[/code]

####  `` style `class-attribute` `instance-attribute` ¶
[code] 
    style = None
    
[/code]

####  `` explode `class-attribute` `instance-attribute` ¶
[code] 
    explode = None
    
[/code]

####  `` allowReserved `class-attribute` `instance-attribute` ¶
[code] 
    allowReserved = None
    
[/code]

####  `` model_config `class-attribute` `instance-attribute` ¶
[code] 
    model_config = {'extra': 'allow'}
    
[/code]

###  `` MediaType ¶

Bases: `[BaseModelWithConfig](#fastapi.openapi.models.BaseModelWithConfig "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">BaseModelWithConfig<_span>".md)`

####  `` schema_ `class-attribute` `instance-attribute` ¶
[code] 
    schema_ = Field(default=None, alias='schema')
    
[/code]

####  `` example `class-attribute` `instance-attribute` ¶
[code] 
    example = None
    
[/code]

####  `` examples `class-attribute` `instance-attribute` ¶
[code] 
    examples = None
    
[/code]

####  `` encoding `class-attribute` `instance-attribute` ¶
[code] 
    encoding = None
    
[/code]

####  `` model_config `class-attribute` `instance-attribute` ¶
[code] 
    model_config = {'extra': 'allow'}
    
[/code]

###  `` ParameterBase ¶

Bases: `[BaseModelWithConfig](#fastapi.openapi.models.BaseModelWithConfig "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">BaseModelWithConfig<_span>".md)`

####  `` description `class-attribute` `instance-attribute` ¶
[code] 
    description = None
    
[/code]

####  `` required `class-attribute` `instance-attribute` ¶
[code] 
    required = None
    
[/code]

####  `` deprecated `class-attribute` `instance-attribute` ¶
[code] 
    deprecated = None
    
[/code]

####  `` style `class-attribute` `instance-attribute` ¶
[code] 
    style = None
    
[/code]

####  `` explode `class-attribute` `instance-attribute` ¶
[code] 
    explode = None
    
[/code]

####  `` allowReserved `class-attribute` `instance-attribute` ¶
[code] 
    allowReserved = None
    
[/code]

####  `` schema_ `class-attribute` `instance-attribute` ¶
[code] 
    schema_ = Field(default=None, alias='schema')
    
[/code]

####  `` example `class-attribute` `instance-attribute` ¶
[code] 
    example = None
    
[/code]

####  `` examples `class-attribute` `instance-attribute` ¶
[code] 
    examples = None
    
[/code]

####  `` content `class-attribute` `instance-attribute` ¶
[code] 
    content = None
    
[/code]

####  `` model_config `class-attribute` `instance-attribute` ¶
[code] 
    model_config = {'extra': 'allow'}
    
[/code]

###  `` Parameter ¶

Bases: `[ParameterBase](#fastapi.openapi.models.ParameterBase "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">ParameterBase<_span>".md)`

####  `` name `instance-attribute` ¶
[code] 
    name
    
[/code]

####  `` in_ `class-attribute` `instance-attribute` ¶
[code] 
    in_ = Field(alias='in')
    
[/code]

####  `` model_config `class-attribute` `instance-attribute` ¶
[code] 
    model_config = {'extra': 'allow'}
    
[/code]

####  `` description `class-attribute` `instance-attribute` ¶
[code] 
    description = None
    
[/code]

####  `` required `class-attribute` `instance-attribute` ¶
[code] 
    required = None
    
[/code]

####  `` deprecated `class-attribute` `instance-attribute` ¶
[code] 
    deprecated = None
    
[/code]

####  `` style `class-attribute` `instance-attribute` ¶
[code] 
    style = None
    
[/code]

####  `` explode `class-attribute` `instance-attribute` ¶
[code] 
    explode = None
    
[/code]

####  `` allowReserved `class-attribute` `instance-attribute` ¶
[code] 
    allowReserved = None
    
[/code]

####  `` schema_ `class-attribute` `instance-attribute` ¶
[code] 
    schema_ = Field(default=None, alias='schema')
    
[/code]

####  `` example `class-attribute` `instance-attribute` ¶
[code] 
    example = None
    
[/code]

####  `` examples `class-attribute` `instance-attribute` ¶
[code] 
    examples = None
    
[/code]

####  `` content `class-attribute` `instance-attribute` ¶
[code] 
    content = None
    
[/code]

###  `` Header ¶

Bases: `[ParameterBase](#fastapi.openapi.models.ParameterBase "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">ParameterBase<_span>".md)`

####  `` model_config `class-attribute` `instance-attribute` ¶
[code] 
    model_config = {'extra': 'allow'}
    
[/code]

####  `` description `class-attribute` `instance-attribute` ¶
[code] 
    description = None
    
[/code]

####  `` required `class-attribute` `instance-attribute` ¶
[code] 
    required = None
    
[/code]

####  `` deprecated `class-attribute` `instance-attribute` ¶
[code] 
    deprecated = None
    
[/code]

####  `` style `class-attribute` `instance-attribute` ¶
[code] 
    style = None
    
[/code]

####  `` explode `class-attribute` `instance-attribute` ¶
[code] 
    explode = None
    
[/code]

####  `` allowReserved `class-attribute` `instance-attribute` ¶
[code] 
    allowReserved = None
    
[/code]

####  `` schema_ `class-attribute` `instance-attribute` ¶
[code] 
    schema_ = Field(default=None, alias='schema')
    
[/code]

####  `` example `class-attribute` `instance-attribute` ¶
[code] 
    example = None
    
[/code]

####  `` examples `class-attribute` `instance-attribute` ¶
[code] 
    examples = None
    
[/code]

####  `` content `class-attribute` `instance-attribute` ¶
[code] 
    content = None
    
[/code]

###  `` RequestBody ¶

Bases: `[BaseModelWithConfig](#fastapi.openapi.models.BaseModelWithConfig "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">BaseModelWithConfig<_span>".md)`

####  `` description `class-attribute` `instance-attribute` ¶
[code] 
    description = None
    
[/code]

####  `` content `instance-attribute` ¶
[code] 
    content
    
[/code]

####  `` required `class-attribute` `instance-attribute` ¶
[code] 
    required = None
    
[/code]

####  `` model_config `class-attribute` `instance-attribute` ¶
[code] 
    model_config = {'extra': 'allow'}
    
[/code]

###  `` Link ¶

Bases: `[BaseModelWithConfig](#fastapi.openapi.models.BaseModelWithConfig "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">BaseModelWithConfig<_span>".md)`

####  `` operationRef `class-attribute` `instance-attribute` ¶
[code] 
    operationRef = None
    
[/code]

####  `` operationId `class-attribute` `instance-attribute` ¶
[code] 
    operationId = None
    
[/code]

####  `` parameters `class-attribute` `instance-attribute` ¶
[code] 
    parameters = None
    
[/code]

####  `` requestBody `class-attribute` `instance-attribute` ¶
[code] 
    requestBody = None
    
[/code]

####  `` description `class-attribute` `instance-attribute` ¶
[code] 
    description = None
    
[/code]

####  `` server `class-attribute` `instance-attribute` ¶
[code] 
    server = None
    
[/code]

####  `` model_config `class-attribute` `instance-attribute` ¶
[code] 
    model_config = {'extra': 'allow'}
    
[/code]

###  `` Response ¶

Bases: `[BaseModelWithConfig](#fastapi.openapi.models.BaseModelWithConfig "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">BaseModelWithConfig<_span>".md)`

####  `` description `instance-attribute` ¶
[code] 
    description
    
[/code]

####  `` headers `class-attribute` `instance-attribute` ¶
[code] 
    headers = None
    
[/code]

####  `` content `class-attribute` `instance-attribute` ¶
[code] 
    content = None
    
[/code]

####  `` links `class-attribute` `instance-attribute` ¶
[code] 
    links = None
    
[/code]

####  `` model_config `class-attribute` `instance-attribute` ¶
[code] 
    model_config = {'extra': 'allow'}
    
[/code]

###  `` Operation ¶

Bases: `[BaseModelWithConfig](#fastapi.openapi.models.BaseModelWithConfig "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">BaseModelWithConfig<_span>".md)`

####  `` tags `class-attribute` `instance-attribute` ¶
[code] 
    tags = None
    
[/code]

####  `` summary `class-attribute` `instance-attribute` ¶
[code] 
    summary = None
    
[/code]

####  `` description `class-attribute` `instance-attribute` ¶
[code] 
    description = None
    
[/code]

####  `` externalDocs `class-attribute` `instance-attribute` ¶
[code] 
    externalDocs = None
    
[/code]

####  `` operationId `class-attribute` `instance-attribute` ¶
[code] 
    operationId = None
    
[/code]

####  `` parameters `class-attribute` `instance-attribute` ¶
[code] 
    parameters = None
    
[/code]

####  `` requestBody `class-attribute` `instance-attribute` ¶
[code] 
    requestBody = None
    
[/code]

####  `` responses `class-attribute` `instance-attribute` ¶
[code] 
    responses = None
    
[/code]

####  `` callbacks `class-attribute` `instance-attribute` ¶
[code] 
    callbacks = None
    
[/code]

####  `` deprecated `class-attribute` `instance-attribute` ¶
[code] 
    deprecated = None
    
[/code]

####  `` security `class-attribute` `instance-attribute` ¶
[code] 
    security = None
    
[/code]

####  `` servers `class-attribute` `instance-attribute` ¶
[code] 
    servers = None
    
[/code]

####  `` model_config `class-attribute` `instance-attribute` ¶
[code] 
    model_config = {'extra': 'allow'}
    
[/code]

###  `` PathItem ¶

Bases: `[BaseModelWithConfig](#fastapi.openapi.models.BaseModelWithConfig "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">BaseModelWithConfig<_span>".md)`

####  `` ref `class-attribute` `instance-attribute` ¶
[code] 
    ref = Field(default=None, alias='$ref')
    
[/code]

####  `` summary `class-attribute` `instance-attribute` ¶
[code] 
    summary = None
    
[/code]

####  `` description `class-attribute` `instance-attribute` ¶
[code] 
    description = None
    
[/code]

####  `` get `class-attribute` `instance-attribute` ¶
[code] 
    get = None
    
[/code]

####  `` put `class-attribute` `instance-attribute` ¶
[code] 
    put = None
    
[/code]

####  `` post `class-attribute` `instance-attribute` ¶
[code] 
    post = None
    
[/code]

####  `` delete `class-attribute` `instance-attribute` ¶
[code] 
    delete = None
    
[/code]

####  `` options `class-attribute` `instance-attribute` ¶
[code] 
    options = None
    
[/code]

####  `` head `class-attribute` `instance-attribute` ¶
[code] 
    head = None
    
[/code]

####  `` patch `class-attribute` `instance-attribute` ¶
[code] 
    patch = None
    
[/code]

####  `` trace `class-attribute` `instance-attribute` ¶
[code] 
    trace = None
    
[/code]

####  `` servers `class-attribute` `instance-attribute` ¶
[code] 
    servers = None
    
[/code]

####  `` parameters `class-attribute` `instance-attribute` ¶
[code] 
    parameters = None
    
[/code]

####  `` model_config `class-attribute` `instance-attribute` ¶
[code] 
    model_config = {'extra': 'allow'}
    
[/code]

###  `` SecuritySchemeType ¶

Bases: `Enum`

####  `` apiKey `class-attribute` `instance-attribute` ¶
[code] 
    apiKey = 'apiKey'
    
[/code]

####  `` http `class-attribute` `instance-attribute` ¶
[code] 
    http = 'http'
    
[/code]

####  `` oauth2 `class-attribute` `instance-attribute` ¶
[code] 
    oauth2 = 'oauth2'
    
[/code]

####  `` openIdConnect `class-attribute` `instance-attribute` ¶
[code] 
    openIdConnect = 'openIdConnect'
    
[/code]

###  `` SecurityBase ¶

Bases: `[BaseModelWithConfig](#fastapi.openapi.models.BaseModelWithConfig "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">BaseModelWithConfig<_span>".md)`

####  `` type_ `class-attribute` `instance-attribute` ¶
[code] 
    type_ = Field(alias='type')
    
[/code]

####  `` description `class-attribute` `instance-attribute` ¶
[code] 
    description = None
    
[/code]

####  `` model_config `class-attribute` `instance-attribute` ¶
[code] 
    model_config = {'extra': 'allow'}
    
[/code]

###  `` APIKeyIn ¶

Bases: `Enum`

####  `` query `class-attribute` `instance-attribute` ¶
[code] 
    query = 'query'
    
[/code]

####  `` header `class-attribute` `instance-attribute` ¶
[code] 
    header = 'header'
    
[/code]

####  `` cookie `class-attribute` `instance-attribute` ¶
[code] 
    cookie = 'cookie'
    
[/code]

###  `` APIKey ¶

Bases: `[SecurityBase](#fastapi.openapi.models.SecurityBase "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">SecurityBase<_span>".md)`

####  `` type_ `class-attribute` `instance-attribute` ¶
[code] 
    type_ = Field(default=[apiKey](#fastapi.openapi.models.SecuritySchemeType.apiKey "<code class="doc-symbol doc-symbol-heading doc-symbol-attribute"><_code>            <span class="doc doc-object-name doc-attribute-name">apiKey<_span>
    
    
      <span class="doc doc-labels">
          <small class="doc doc-label doc-label-class-attribute"><code>class-attribute<_code><_small>
          <small class="doc doc-label doc-label-instance-attribute"><code>instance-attribute<_code><_small>
      <_span>".md), alias='type')
    
[/code]

####  `` in_ `class-attribute` `instance-attribute` ¶
[code] 
    in_ = Field(alias='in')
    
[/code]

####  `` name `instance-attribute` ¶
[code] 
    name
    
[/code]

####  `` model_config `class-attribute` `instance-attribute` ¶
[code] 
    model_config = {'extra': 'allow'}
    
[/code]

####  `` description `class-attribute` `instance-attribute` ¶
[code] 
    description = None
    
[/code]

###  `` HTTPBase ¶

Bases: `[SecurityBase](#fastapi.openapi.models.SecurityBase "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">SecurityBase<_span>".md)`

####  `` type_ `class-attribute` `instance-attribute` ¶
[code] 
    type_ = Field(default=[http](#fastapi.openapi.models.SecuritySchemeType.http "<code class="doc-symbol doc-symbol-heading doc-symbol-attribute"><_code>            <span class="doc doc-object-name doc-attribute-name">http<_span>
    
    
      <span class="doc doc-labels">
          <small class="doc doc-label doc-label-class-attribute"><code>class-attribute<_code><_small>
          <small class="doc doc-label doc-label-instance-attribute"><code>instance-attribute<_code><_small>
      <_span>".md), alias='type')
    
[/code]

####  `` scheme `instance-attribute` ¶
[code] 
    scheme
    
[/code]

####  `` model_config `class-attribute` `instance-attribute` ¶
[code] 
    model_config = {'extra': 'allow'}
    
[/code]

####  `` description `class-attribute` `instance-attribute` ¶
[code] 
    description = None
    
[/code]

###  `` HTTPBearer ¶

Bases: `[HTTPBase](#fastapi.openapi.models.HTTPBase "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">HTTPBase<_span>".md)`

####  `` scheme `class-attribute` `instance-attribute` ¶
[code] 
    scheme = 'bearer'
    
[/code]

####  `` bearerFormat `class-attribute` `instance-attribute` ¶
[code] 
    bearerFormat = None
    
[/code]

####  `` model_config `class-attribute` `instance-attribute` ¶
[code] 
    model_config = {'extra': 'allow'}
    
[/code]

####  `` type_ `class-attribute` `instance-attribute` ¶
[code] 
    type_ = Field(default=[http](#fastapi.openapi.models.SecuritySchemeType.http "<code class="doc-symbol doc-symbol-heading doc-symbol-attribute"><_code>            <span class="doc doc-object-name doc-attribute-name">http<_span>
    
    
      <span class="doc doc-labels">
          <small class="doc doc-label doc-label-class-attribute"><code>class-attribute<_code><_small>
          <small class="doc doc-label doc-label-instance-attribute"><code>instance-attribute<_code><_small>
      <_span>".md), alias='type')
    
[/code]

####  `` description `class-attribute` `instance-attribute` ¶
[code] 
    description = None
    
[/code]

###  `` OAuthFlow ¶

Bases: `[BaseModelWithConfig](#fastapi.openapi.models.BaseModelWithConfig "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">BaseModelWithConfig<_span>".md)`

####  `` refreshUrl `class-attribute` `instance-attribute` ¶
[code] 
    refreshUrl = None
    
[/code]

####  `` scopes `class-attribute` `instance-attribute` ¶
[code] 
    scopes = {}
    
[/code]

####  `` model_config `class-attribute` `instance-attribute` ¶
[code] 
    model_config = {'extra': 'allow'}
    
[/code]

###  `` OAuthFlowImplicit ¶

Bases: `[OAuthFlow](#fastapi.openapi.models.OAuthFlow "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">OAuthFlow<_span>".md)`

####  `` authorizationUrl `instance-attribute` ¶
[code] 
    authorizationUrl
    
[/code]

####  `` model_config `class-attribute` `instance-attribute` ¶
[code] 
    model_config = {'extra': 'allow'}
    
[/code]

####  `` refreshUrl `class-attribute` `instance-attribute` ¶
[code] 
    refreshUrl = None
    
[/code]

####  `` scopes `class-attribute` `instance-attribute` ¶
[code] 
    scopes = {}
    
[/code]

###  `` OAuthFlowPassword ¶

Bases: `[OAuthFlow](#fastapi.openapi.models.OAuthFlow "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">OAuthFlow<_span>".md)`

####  `` tokenUrl `instance-attribute` ¶
[code] 
    tokenUrl
    
[/code]

####  `` model_config `class-attribute` `instance-attribute` ¶
[code] 
    model_config = {'extra': 'allow'}
    
[/code]

####  `` refreshUrl `class-attribute` `instance-attribute` ¶
[code] 
    refreshUrl = None
    
[/code]

####  `` scopes `class-attribute` `instance-attribute` ¶
[code] 
    scopes = {}
    
[/code]

###  `` OAuthFlowClientCredentials ¶

Bases: `[OAuthFlow](#fastapi.openapi.models.OAuthFlow "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">OAuthFlow<_span>".md)`

####  `` tokenUrl `instance-attribute` ¶
[code] 
    tokenUrl
    
[/code]

####  `` model_config `class-attribute` `instance-attribute` ¶
[code] 
    model_config = {'extra': 'allow'}
    
[/code]

####  `` refreshUrl `class-attribute` `instance-attribute` ¶
[code] 
    refreshUrl = None
    
[/code]

####  `` scopes `class-attribute` `instance-attribute` ¶
[code] 
    scopes = {}
    
[/code]

###  `` OAuthFlowAuthorizationCode ¶

Bases: `[OAuthFlow](#fastapi.openapi.models.OAuthFlow "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">OAuthFlow<_span>".md)`

####  `` authorizationUrl `instance-attribute` ¶
[code] 
    authorizationUrl
    
[/code]

####  `` tokenUrl `instance-attribute` ¶
[code] 
    tokenUrl
    
[/code]

####  `` model_config `class-attribute` `instance-attribute` ¶
[code] 
    model_config = {'extra': 'allow'}
    
[/code]

####  `` refreshUrl `class-attribute` `instance-attribute` ¶
[code] 
    refreshUrl = None
    
[/code]

####  `` scopes `class-attribute` `instance-attribute` ¶
[code] 
    scopes = {}
    
[/code]

###  `` OAuthFlows ¶

Bases: `[BaseModelWithConfig](#fastapi.openapi.models.BaseModelWithConfig "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">BaseModelWithConfig<_span>".md)`

####  `` implicit `class-attribute` `instance-attribute` ¶
[code] 
    implicit = None
    
[/code]

####  `` password `class-attribute` `instance-attribute` ¶
[code] 
    password = None
    
[/code]

####  `` clientCredentials `class-attribute` `instance-attribute` ¶
[code] 
    clientCredentials = None
    
[/code]

####  `` authorizationCode `class-attribute` `instance-attribute` ¶
[code] 
    authorizationCode = None
    
[/code]

####  `` model_config `class-attribute` `instance-attribute` ¶
[code] 
    model_config = {'extra': 'allow'}
    
[/code]

###  `` OAuth2 ¶

Bases: `[SecurityBase](#fastapi.openapi.models.SecurityBase "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">SecurityBase<_span>".md)`

####  `` type_ `class-attribute` `instance-attribute` ¶
[code] 
    type_ = Field(default=[oauth2](#fastapi.openapi.models.SecuritySchemeType.oauth2 "<code class="doc-symbol doc-symbol-heading doc-symbol-attribute"><_code>            <span class="doc doc-object-name doc-attribute-name">oauth2<_span>
    
    
      <span class="doc doc-labels">
          <small class="doc doc-label doc-label-class-attribute"><code>class-attribute<_code><_small>
          <small class="doc doc-label doc-label-instance-attribute"><code>instance-attribute<_code><_small>
      <_span>".md), alias='type')
    
[/code]

####  `` flows `instance-attribute` ¶
[code] 
    flows
    
[/code]

####  `` model_config `class-attribute` `instance-attribute` ¶
[code] 
    model_config = {'extra': 'allow'}
    
[/code]

####  `` description `class-attribute` `instance-attribute` ¶
[code] 
    description = None
    
[/code]

###  `` OpenIdConnect ¶

Bases: `[SecurityBase](#fastapi.openapi.models.SecurityBase "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">SecurityBase<_span>".md)`

####  `` type_ `class-attribute` `instance-attribute` ¶
[code] 
    type_ = Field(default=[openIdConnect](#fastapi.openapi.models.SecuritySchemeType.openIdConnect "<code class="doc-symbol doc-symbol-heading doc-symbol-attribute"><_code>            <span class="doc doc-object-name doc-attribute-name">openIdConnect<_span>
    
    
      <span class="doc doc-labels">
          <small class="doc doc-label doc-label-class-attribute"><code>class-attribute<_code><_small>
          <small class="doc doc-label doc-label-instance-attribute"><code>instance-attribute<_code><_small>
      <_span>".md), alias='type')
    
[/code]

####  `` openIdConnectUrl `instance-attribute` ¶
[code] 
    openIdConnectUrl
    
[/code]

####  `` model_config `class-attribute` `instance-attribute` ¶
[code] 
    model_config = {'extra': 'allow'}
    
[/code]

####  `` description `class-attribute` `instance-attribute` ¶
[code] 
    description = None
    
[/code]

###  `` Components ¶

Bases: `[BaseModelWithConfig](#fastapi.openapi.models.BaseModelWithConfig "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">BaseModelWithConfig<_span>".md)`

####  `` schemas `class-attribute` `instance-attribute` ¶
[code] 
    schemas = None
    
[/code]

####  `` responses `class-attribute` `instance-attribute` ¶
[code] 
    responses = None
    
[/code]

####  `` parameters `class-attribute` `instance-attribute` ¶
[code] 
    parameters = None
    
[/code]

####  `` examples `class-attribute` `instance-attribute` ¶
[code] 
    examples = None
    
[/code]

####  `` requestBodies `class-attribute` `instance-attribute` ¶
[code] 
    requestBodies = None
    
[/code]

####  `` headers `class-attribute` `instance-attribute` ¶
[code] 
    headers = None
    
[/code]

####  `` securitySchemes `class-attribute` `instance-attribute` ¶
[code] 
    securitySchemes = None
    
[/code]

####  `` links `class-attribute` `instance-attribute` ¶
[code] 
    links = None
    
[/code]

####  `` callbacks `class-attribute` `instance-attribute` ¶
[code] 
    callbacks = None
    
[/code]

####  `` pathItems `class-attribute` `instance-attribute` ¶
[code] 
    pathItems = None
    
[/code]

####  `` model_config `class-attribute` `instance-attribute` ¶
[code] 
    model_config = {'extra': 'allow'}
    
[/code]

###  `` Tag ¶

Bases: `[BaseModelWithConfig](#fastapi.openapi.models.BaseModelWithConfig "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">BaseModelWithConfig<_span>".md)`

####  `` name `instance-attribute` ¶
[code] 
    name
    
[/code]

####  `` description `class-attribute` `instance-attribute` ¶
[code] 
    description = None
    
[/code]

####  `` externalDocs `class-attribute` `instance-attribute` ¶
[code] 
    externalDocs = None
    
[/code]

####  `` model_config `class-attribute` `instance-attribute` ¶
[code] 
    model_config = {'extra': 'allow'}
    
[/code]

###  `` OpenAPI ¶

Bases: `[BaseModelWithConfig](#fastapi.openapi.models.BaseModelWithConfig "<code class="doc-symbol doc-symbol-heading doc-symbol-class"><_code>            <span class="doc doc-object-name doc-class-name">BaseModelWithConfig<_span>".md)`

####  `` openapi `instance-attribute` ¶
[code] 
    openapi
    
[/code]

####  `` info `instance-attribute` ¶
[code] 
    info
    
[/code]

####  `` jsonSchemaDialect `class-attribute` `instance-attribute` ¶
[code] 
    jsonSchemaDialect = None
    
[/code]

####  `` servers `class-attribute` `instance-attribute` ¶
[code] 
    servers = None
    
[/code]

####  `` paths `class-attribute` `instance-attribute` ¶
[code] 
    paths = None
    
[/code]

####  `` webhooks `class-attribute` `instance-attribute` ¶
[code] 
    webhooks = None
    
[/code]

####  `` components `class-attribute` `instance-attribute` ¶
[code] 
    components = None
    
[/code]

####  `` security `class-attribute` `instance-attribute` ¶
[code] 
    security = None
    
[/code]

####  `` tags `class-attribute` `instance-attribute` ¶
[code] 
    tags = None
    
[/code]

####  `` externalDocs `class-attribute` `instance-attribute` ¶
[code] 
    externalDocs = None
    
[/code]

####  `` model_config `class-attribute` `instance-attribute` ¶
[code] 
    model_config = {'extra': 'allow'}
    
[/code]
