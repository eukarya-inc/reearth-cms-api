# reearthcmsapi
This is ReEarth CMS API manual.

This Python package is automatically generated by the [OpenAPI Generator](https://openapi-generator.tech) project:

- API version: 1.0.0
- Package version: 1.0.0
- Build package: org.openapitools.codegen.languages.PythonClientCodegen

## Requirements.

Python &gt;&#x3D;3.7

## Migration from other generators like python and python-legacy

### Changes
1. This generator uses spec case for all (object) property names and parameter names.
    - So if the spec has a property name like camelCase, it will use camelCase rather than camel_case
    - So you will need to update how you input and read properties to use spec case
2. Endpoint parameters are stored in dictionaries to prevent collisions (explanation below)
    - So you will need to update how you pass data in to endpoints
3. Endpoint responses now include the original response, the deserialized response body, and (todo)the deserialized headers
    - So you will need to update your code to use response.body to access deserialized data
4. All validated data is instantiated in an instance that subclasses all validated Schema classes and Decimal/str/list/tuple/frozendict/NoneClass/BoolClass/bytes/io.FileIO
    - This means that you can use isinstance to check if a payload validated against a schema class
    - This means that no data will be of type None/True/False
        - ingested None will subclass NoneClass
        - ingested True will subclass BoolClass
        - ingested False will subclass BoolClass
        - So if you need to check is True/False/None, instead use instance.is_true_oapg()/.is_false_oapg()/.is_none_oapg()
5. All validated class instances are immutable except for ones based on io.File
    - This is because if properties were changed after validation, that validation would no longer apply
    - So no changing values or property values after a class has been instantiated
6. String + Number types with formats
    - String type data is stored as a string and if you need to access types based on its format like date,
    date-time, uuid, number etc then you will need to use accessor functions on the instance
    - type string + format: See .as_date_oapg, .as_datetime_oapg, .as_decimal_oapg, .as_uuid_oapg
    - type number + format: See .as_float_oapg, .as_int_oapg
    - this was done because openapi/json-schema defines constraints. string data may be type string with no format
    keyword in one schema, and include a format constraint in another schema
    - So if you need to access a string format based type, use as_date_oapg/as_datetime_oapg/as_decimal_oapg/as_uuid_oapg
    - So if you need to access a number format based type, use as_int_oapg/as_float_oapg
7. Property access on AnyType(type unset) or object(dict) schemas
    - Only required keys with valid python names are properties like .someProp and have type hints
    - All optional keys may not exist, so properties are not defined for them
    - One can access optional values with dict_instance['optionalProp'] and KeyError will be raised if it does not exist
    - Use get_item_oapg if you need a way to always get a value whether or not the key exists
        - If the key does not exist, schemas.unset is returned from calling dict_instance.get_item_oapg('optionalProp')
        - All required and optional keys have type hints for this method, and @typing.overload is used
        - A type hint is also generated for additionalProperties accessed using this method
    - So you will need to update you code to use some_instance['optionalProp'] to access optional property
    and additionalProperty values
8. The location of the api classes has changed
    - Api classes are located in your_package.apis.tags.some_api
    - This change was made to eliminate redundant code generation
    - Legacy generators generated the same endpoint twice if it had > 1 tag on it
    - This generator defines an endpoint in one class, then inherits that class to generate
      apis by tags and by paths
    - This change reduces code and allows quicker run time if you use the path apis
        - path apis are at your_package.apis.paths.some_path
    - Those apis will only load their needed models, which is less to load than all of the resources needed in a tag api
    - So you will need to update your import paths to the api classes

### Why are Oapg and _oapg used in class and method names?
Classes can have arbitrarily named properties set on them
Endpoints can have arbitrary operationId method names set
For those reasons, I use the prefix Oapg and _oapg to greatly reduce the likelihood of collisions
on protected + public classes/methods.
oapg stands for OpenApi Python Generator.

### Object property spec case
This was done because when payloads are ingested, they can be validated against N number of schemas.
If the input signature used a different property name then that has mutated the payload.
So SchemaA and SchemaB must both see the camelCase spec named variable.
Also it is possible to send in two properties, named camelCase and camel_case in the same payload.
That use case should be support so spec case is used.

### Parameter spec case
Parameters can be included in different locations including:
- query
- path
- header
- cookie

Any of those parameters could use the same parameter names, so if every parameter
was included as an endpoint parameter in a function signature, they would collide.
For that reason, each of those inputs have been separated out into separate typed dictionaries:
- query_params
- path_params
- header_params
- cookie_params

So when updating your code, you will need to pass endpoint parameters in using those
dictionaries.

### Endpoint responses
Endpoint responses have been enriched to now include more information.
Any response reom an endpoint will now include the following properties:
response: urllib3.HTTPResponse
body: typing.Union[Unset, Schema]
headers: typing.Union[Unset, TODO]
Note: response header deserialization has not yet been added


## Installation & Usage
### pip install

If the python package is hosted on a repository, you can install directly using:

```sh
pip install git+https://github.com/GIT_USER_ID/GIT_REPO_ID.git
```
(you may need to run `pip` with root permission: `sudo pip install git+https://github.com/GIT_USER_ID/GIT_REPO_ID.git`)

Then import the package:
```python
import reearthcmsapi
```

### Setuptools

Install via [Setuptools](http://pypi.python.org/pypi/setuptools).

```sh
python setup.py install --user
```
(or `sudo python setup.py install` to install the package for all users)

Then import the package:
```python
import reearthcmsapi
```

## Getting Started

Please follow the [installation procedure](#installation--usage) and then run the following:

```python

import time
import reearthcmsapi
from pprint import pprint
from reearthcmsapi.apis.tags import assets_api
from reearthcmsapi.model.asset import Asset
# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = reearthcmsapi.Configuration(
    host = "http://localhost"
)


# Enter a context with an instance of the API client
with reearthcmsapi.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = assets_api.AssetsApi(api_client)
    asset_id = "assetId_example" # str | ID of the selected asset

    try:
        api_response = api_instance.asset_delete(asset_id)
        pprint(api_response)
    except reearthcmsapi.ApiException as e:
        print("Exception when calling AssetsApi->asset_delete: %s\n" % e)
```

## Documentation for API Endpoints

All URIs are relative to *http://localhost*

Class | Method | HTTP request | Description
------------ | ------------- | ------------- | -------------
*AssetsApi* | [**asset_delete**](python/docs/apis/tags/AssetsApi.md#asset_delete) | **delete** /assets/{assetId} | 
*AssetsApi* | [**asset_get**](python/docs/apis/tags/AssetsApi.md#asset_get) | **get** /assets/{assetId} | 
*AssetsCommentsApi* | [**asset_comment_create**](python/docs/apis/tags/AssetsCommentsApi.md#asset_comment_create) | **post** /assets/{assetId}/comments | 
*AssetsCommentsApi* | [**asset_comment_delete**](python/docs/apis/tags/AssetsCommentsApi.md#asset_comment_delete) | **delete** /assets/{assetId}/comments/{commentId} | 
*AssetsCommentsApi* | [**asset_comment_list**](python/docs/apis/tags/AssetsCommentsApi.md#asset_comment_list) | **get** /assets/{assetId}/comments | 
*AssetsCommentsApi* | [**asset_comment_update**](python/docs/apis/tags/AssetsCommentsApi.md#asset_comment_update) | **patch** /assets/{assetId}/comments/{commentId} | Update AssetComment
*AssetsProjectApi* | [**asset_create**](python/docs/apis/tags/AssetsProjectApi.md#asset_create) | **post** /projects/{projectId}/assets | Create an new asset.
*AssetsProjectApi* | [**asset_filter**](python/docs/apis/tags/AssetsProjectApi.md#asset_filter) | **get** /projects/{projectId}/assets | Returns a list of assets.
*ItemsApi* | [**item_create**](python/docs/apis/tags/ItemsApi.md#item_create) | **post** /models/{modelId}/items | create an item
*ItemsApi* | [**item_delete**](python/docs/apis/tags/ItemsApi.md#item_delete) | **delete** /items/{itemId} | delete an item
*ItemsApi* | [**item_filter**](python/docs/apis/tags/ItemsApi.md#item_filter) | **get** /models/{modelId}/items | Returns a list of items.
*ItemsApi* | [**item_get**](python/docs/apis/tags/ItemsApi.md#item_get) | **get** /items/{itemId} | Returns an items.
*ItemsApi* | [**item_update**](python/docs/apis/tags/ItemsApi.md#item_update) | **patch** /items/{itemId} | Update an item.
*ItemsCommentsApi* | [**item_comment_create**](python/docs/apis/tags/ItemsCommentsApi.md#item_comment_create) | **post** /items/{itemId}/comments | 
*ItemsCommentsApi* | [**item_comment_delete**](python/docs/apis/tags/ItemsCommentsApi.md#item_comment_delete) | **delete** /items/{itemId}/comments/{commentId} | 
*ItemsCommentsApi* | [**item_comment_list**](python/docs/apis/tags/ItemsCommentsApi.md#item_comment_list) | **get** /items/{itemId}/comments | 
*ItemsCommentsApi* | [**item_comment_update**](python/docs/apis/tags/ItemsCommentsApi.md#item_comment_update) | **patch** /items/{itemId}/comments/{commentId} | Update Item Comment
*ItemsProjectApi* | [**item_create_with_project**](python/docs/apis/tags/ItemsProjectApi.md#item_create_with_project) | **post** /projects/{projectIdOrAlias}/models/{modelIdOrKey}/items | 
*ItemsProjectApi* | [**item_filter_with_project**](python/docs/apis/tags/ItemsProjectApi.md#item_filter_with_project) | **get** /projects/{projectIdOrAlias}/models/{modelIdOrKey}/items | Returns a list of items.
*ModelsApi* | [**model_get**](python/docs/apis/tags/ModelsApi.md#model_get) | **get** /models/{modelId} | Returns a model.
*ModelsApi* | [**model_get_with_project**](python/docs/apis/tags/ModelsApi.md#model_get_with_project) | **get** /projects/{projectIdOrAlias}/models/{modelIdOrKey} | Returns a model.

## Documentation For Models

 - [Asset](python/docs/models/Asset.md)
 - [AssetEmbedding](python/docs/models/AssetEmbedding.md)
 - [Comment](python/docs/models/Comment.md)
 - [Field](python/docs/models/Field.md)
 - [File](python/docs/models/File.md)
 - [Item](python/docs/models/Item.md)
 - [Model](python/docs/models/Model.md)
 - [RefOrVersion](python/docs/models/RefOrVersion.md)
 - [Schema](python/docs/models/Schema.md)
 - [SchemaField](python/docs/models/SchemaField.md)
 - [ValueType](python/docs/models/ValueType.md)
 - [Version](python/docs/models/Version.md)
 - [VersionedItem](python/docs/models/VersionedItem.md)

## Documentation For Authorization

Authentication schemes defined for the API:
<a id="bearerAuth"></a>
### bearerAuth

- **Type**: Bearer authentication


## Author

## Notes for Large OpenAPI documents
If the OpenAPI document is large, imports in reearthcmsapi.apis and reearthcmsapi.models may fail with a
RecursionError indicating the maximum recursion limit has been exceeded. In that case, there are a couple of solutions:

Solution 1:
Use specific imports for apis and models like:
- `from reearthcmsapi.apis.default_api import DefaultApi`
- `from reearthcmsapi.model.pet import Pet`

Solution 1:
Before importing the package, adjust the maximum recursion limit as shown below:
```
import sys
sys.setrecursionlimit(1500)
import reearthcmsapi
from reearthcmsapi.apis import *
from reearthcmsapi.models import *
```
## License

[MIT License](LICENSE)