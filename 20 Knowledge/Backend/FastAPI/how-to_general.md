---
title: "General"
source: "https://fastapi.tiangolo.com/how-to/general/"
---

# General - How To - Recipes¶

Here are several pointers to other places in the docs, for general or frequent questions.

## Filter Data - Security¶

To ensure that you don't return more data than you should, read the docs for [Tutorial - Response Model - Return Type](tutorial_response-model.md).

## Optimize Response Performance - Response Model - Return Type¶

To optimize performance when returning JSON data, use a return type or response model, that way Pydantic will handle the serialization to JSON on the Rust side, without going through Python. Read more in the docs for [Tutorial - Response Model - Return Type](tutorial_response-model.md).

## Documentation Tags - OpenAPI¶

To add tags to your _path operations_ , and group them in the docs UI, read the docs for [Tutorial - Path Operation Configurations - Tags](tutorial_path-operation-configuration_#tags.md).

## Documentation Summary and Description - OpenAPI¶

To add a summary and description to your _path operations_ , and show them in the docs UI, read the docs for [Tutorial - Path Operation Configurations - Summary and Description](tutorial_path-operation-configuration_#summary-and-description.md).

## Documentation Response description - OpenAPI¶

To define the description of the response, shown in the docs UI, read the docs for [Tutorial - Path Operation Configurations - Response description](tutorial_path-operation-configuration_#response-description.md).

## Documentation Deprecate a _Path Operation_ \- OpenAPI¶

To deprecate a _path operation_ , and show it in the docs UI, read the docs for [Tutorial - Path Operation Configurations - Deprecation](tutorial_path-operation-configuration_#deprecate-a-path-operation.md).

## Convert any Data to JSON-compatible¶

To convert any data to JSON-compatible, read the docs for [Tutorial - JSON Compatible Encoder](tutorial_encoder.md).

## OpenAPI Metadata - Docs¶

To add metadata to your OpenAPI schema, including a license, version, contact, etc, read the docs for [Tutorial - Metadata and Docs URLs](tutorial_metadata.md).

## OpenAPI Custom URL¶

To customize the OpenAPI URL (or remove it), read the docs for [Tutorial - Metadata and Docs URLs](tutorial_metadata_#openapi-url.md).

## OpenAPI Docs URLs¶

To update the URLs used for the automatically generated docs user interfaces, read the docs for [Tutorial - Metadata and Docs URLs](tutorial_metadata_#docs-urls.md).
