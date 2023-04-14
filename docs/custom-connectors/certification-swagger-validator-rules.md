---
title: Fix Swagger Validator errors in Power Platform connectors| Microsoft Docs
description: Use this topic during the certification process to find your Swagger Validation errors and fix it in the the connector files you submit.
author: Amjed-Ayoub
services: connectors
documentationcenter:
ms.service: connectors
ms.workload: connectors
ms.topic: article
ms.date: 09/14/2022
ms.author: v-amjedayoub
ms.reviewer: angieandrews
---

# Fix Swagger Validator errors

The Swagger Validator tool validates the connector files you submit in the [GitHub open-source repository](https://github.com/Microsoft/PowerPlatformConnectors) and the [ISV portal](https://isvstudio.powerapps.com/home). It checks the connector files to ensure they're proper, and adhere to our connector requirements and guidelines.

Use the tables in this topic to help you find and fix errors. Each table lists the errors that the tool might generate. For each error, there's a corresponding issue and solution. The solutions listed might help you avoid getting errors *before* or *during* submission.

If you need further guidance, send an email to [ConnectorPartnerMgmtTeam@service.microsoft.com](mailto:connectorpartnermgmtteam@service.microsoft.com).

## Operations

| Error 	| Issue 	| Solution 	|
|---	|---	|---	|
|  ApiAnnotationExtensionReplacementInfo<br/>ForNonDeprecatedOperations 	| `The 'x-ms-api-annotation'   extension for operation '{0}' is invalid. The replacement property should be specified for deprecated operations only.` 	| The operation {0} isn't marked deprecated. To use this property, the operation should have the ’deprecated’ property set to true. Otherwise, if this isn't the case, remove the ‘x-ms-api-annotation' property. 	|
|  ApiAnnotationExtensionReplacementInfo<br/>MissingApiProperty 	| `The 'x-ms-api-annotation' extension for operation '{0}' is invalid. The replacement property must specify 'api' property.` 	| The ‘x-ms-api-annotation' has the ‘replacement’ property specified but the ‘api’ property is missing. In order to use the ‘replacement’ property, both the ‘api’ and ‘operationId’ property must be set. 	|
|  ApiAnnotationExtensionReplacementInfo<br/>MissingOperationIdProperty 	| `The 'x-ms-api-annotation' extension for operation '{0}' is invalid. The replacement property must specify 'operationId' property.` 	| The ‘x-ms-api-annotation' has the ‘replacement’ property specified but the ‘operationId’ property is missing. In order to use the ‘replacement’ property, both the ‘api’ and ‘operationId’ property must be set. 	|
|  ArrayDuplicateValue 	| `The array contains duplicate values, values must be unique.` 	| Arrays must have unique values, but your swagger contains duplicates. Remove the duplicates. 	|
|  BodyOrFormDataParameterInFetch<br/>OperationNotAllowed 	| `Body or form data parameters are not supported in a fetch operation.` 	| Operations using the “GET” HTTP method can't have a body or form data. 	|
|  ConnectionIdParameterNotAllowed 	| `A parameter cannot be named as 'connectionId'.` 	|  Rename the parameter to a value that isn't ‘connectionId’. 	|
|  ConsumesMissing 	| `At least one supported MIME type must be provided in 'consumes' for operation '{0}'.` 	|  Your swagger should specify one supported MIME type in consumes for operation. 	|
|  DuplicateEnumValuesInExtension 	| `The 'x-ms-enum-values' extension has duplicate values. You can only have one display name mapped to a value.` 	| Enum values should be unique and not duplicated. 	|
|  DuplicateOperationPath 	| `The operations '{0}' and '{1}' have the path templates '{2}' and '{3}' which are duplicate and indistinguishable. They need to have a difference in static path segments for a deterministic routing.` 	| {0} is the operation id, {1} is   the duplicate operation id, {2} is the operation path, {3} is the duplicate   operation path. Remove the duplicate operation id from the duplicate   operation path. 	|
|  DynamicExtensionDefinitionNotAllowed<br/>InParameter 	| `Dynamic extensions are not allowed to be defined on the body parameter. It should be in the schema.` 	| The dynamic extensions are defined on a body parameter and must be removed. It's only allowed to be defined in the schema. 	|
|  DynamicListExtensionRequiredFor<br/>AmbiguousReferences 	| `The 'x-ms-dynamic-values'   extension references ambiguous parameter references. You need to define the   'x-ms-dynamic-list' extension.` 	| ‘x-ms-dynamic-list' must be defined. More information: [How to use dynamic values](openapi-extensions.md#how-to-use-dynamic-values) 	|
|  DynamicListExtensionRequiredFor<br/>NonParameterReferences 	| `The 'x-ms-dynamic-values'   extension references properties within parameters. You need to define the   'x-ms-dynamic-list' extension.` 	| ‘x-ms-dynamic-list' must be defined. More information: [How to use dynamic values](openapi-extensions.md#how-to-use-dynamic-values) 	|
|  DynamicPropertiesExtensionRequiredFor<br/>AmbiguousReferences 	| `The 'x-ms-dynamic-schema'   extension references ambiguous parameter references. You need to define the   'x-ms-dynamic-properties' extension.` 	| ‘x-ms-dynamic- properties' must be defined. More information: [How to use dynamic values](openapi-extensions.md#how-to-use-dynamic-values) 	|
|  DynamicPropertiesExtensionRequiredFor<br/>NonParameterReferences 	| `The 'x-ms-dynamic-schema'   extension references properties within parameters. You need to define the   'x-ms-dynamic-properties' extension.` 	| x-ms-dynamic- properties' must be defined. More information: [How to use dynamic values](openapi-extensions.md#how-to-use-dynamic-values) 	|
|  DynamicPropertiesExtensionRequiredFor<br/>PropertiesValuePath 	| `The 'x-ms-dynamic-schema'   extension property 'value-path' points to schema properties. You need to   define the 'x-ms-dynamic-properties' extension, which points to schema   object.` 	| Your swagger file is missing   ‘x-ms-dynamic-properties', which is required since it defines an   ‘x-ms-dynamic-schema'. 	|
|  DynamicPropertiesItemValue<br/>PathMismatch 	| `The 'x-ms-dynamic-properties'   extension should have the same item value path as 'x-ms-dynamic-schema'   extension without 'properties' ending. The expected path is '{0}'.` 	| The dynamic properties extension   item value path does not match the expected path based on the schema. 	|
|  DynamicTreeExtensionMissing 	| `The 'x-ms-dynamic-tree'   extension must be defined to enable file picker support.` 	| The property ‘x-ms-dynamic-tree'   is missing and must be defined to support file picker. 	|
|  InvalidEnumValue 	| `The type of the enum value is   '{0}' but it should be '{1}' as specified in the schema.` 	| The value must be replaced to be   of the type specified. 	|
|  InvalidEnumValuesExtensionValue 	| `The 'value' field must match   one of the enums as defined in the 'enum' property of the schema.` 	|  The enum that is selected has not been   defined in the list of enums. Either define the value in the enum property or   use a value defined in the enum property. 	|
|  InvalidFormDataParameterName 	| `The form data parameter name   '{0}' is invalid. All characters must be part of the US-ASCII character set.` 	| Rename the form data parameter   name to include only US-ASCII characters only. 	|
|  InvalidNextLinkNameValue 	| `The nextLinkName value for   operation '{0}' is invalid. Supported values are 'nextLink' or   '@odata.nextLink'.` 	| Fix the value of property ‘nextLinkName’ so it is either ‘nextLink’ or   ‘@odata.nextLink’. Any other values aren't allowed. 	|
| InvokedOperationShouldProduceArray 	| `The dynamic operation expects   an array on the specified path '{0}' which should be defined in the   successful response of the invoked operation '{1}'.` 	| {0} is the path to a response   property, {1} is the operation id. Define within the response of the   operation a return of type array. 	|
| InvokedOperationShouldProduceProperty 	| `The dynamic operation expects a   property on the specified path '{0}' which should be defined in the   successful response of the invoked operation '{1}'.` 	| {0} is the path to a response   property, {1} is the operation id. Define within the response of the   operation a specific property 	|
| MimeTypeNotCompatibleWithRequest<br/>ContentSchemaType 	| `The MIME type(s) are not   compatible with the request content type '{0}' for operation '{1}'.` 	| The MIME type that you are trying to use isn't compatible with the content of your request in the specified operation. 	|
| NotificationExtensionNotSupportedIn<br/>WebhookOperation 	| `The 'x-ms-notification'   extension must not be defined in a webhook operation.` 	| Using “x-ms-notification"   isn't allowed in a webhook operation. 	|
| NotificationExtensionSupportedFor<br/>TriggerOnly 	| `The 'x-ms-notification' is   supported for trigger operations only.` 	| The “x-ms-notification"   extension can only be used for trigger operations. 	|
| NotificationOperationMustBeWebhook 	| `The 'operationId' property in   'x-ms-notification' extension must be for a webhook operation.` 	| When using   “x-ms-notification", the operation ID must refer to a webhook operation. 	|
|  OperationFamilyHasDuplicateRevisions 	| `The operation family '{0}' has   operations with duplicate revision.` 	| Your swagger defines an   operation family with the same version/revision number. Each operation in an   operation family should have a unique version number. 	|
|  OperationFamilyHasManyActive<br/>Operations 	| `The operation family '{0}' has   more than two active operations. Extra operations must be deprecated.` 	| Operation families are limited to two non-deprecated operations. Any others should be marked as deprecated to reduce confusion for the user of your connector. 	|
|  OperationFamilyHasManyActive<br/>ProductionOperations 	| `The operation family '{0}' has   several active operations in Production status. Previous revisions should be   deprecated.` 	| An operation family has multiple   operations in production. To reduce confusion, limit the number of production   operations in a family and deprecate the rest. 	|
|  OperationFamilyHasManyEmptyRevisions 	| `The operation family '{0}' has   several operations with empty revision. Only one operation can have empty   revision.` 	| Operation families can only have   one operation without a revision. All others in the family should have a   revision. 	|
|  OperationHasManyResponsesWithSchema 	| `The operation '{0}' has more   than one response with specified schema, only one schema will be used in the   designer.` 	| Limitations in the designer   prevent multiple response schemas. Your operation specifies multiple, but   should only specify one. 	|
|  OperationIdNotFound 	| `The 'operationId' could not be   found in this swagger document.` 	|  The operationId can't be found in your swagger. 	|
|  OperationIdNotUnique 	| `The operationId '{0}' is used   multiple times. Operation identifiers must be unique.` 	| The same operationId appears   more than once. Remove one of the instances or fix one of the instances to be   different. 	|
| OperationIdRequired 	| `The operation 'operationId'   property is required.` 	| The property ‘operationId’ is   missing, include the property ‘operationId’ for your operation. 	|
|  OperationIdSanitized 	| `The 'operationId' property   value is different from its sanitized value '{0}'. Please avoid the useage of   non-alphanumeric characters to have matching values.` 	| The provided operation ID does   not match the sanitized version. Update the operation ID to match. 	|
|  OperationIdShouldEndWithRevision 	| `The operation Id '{0}' should   end with revision suffix '{1}'.` 	| Operation IDs with a revision   should have a name with the revision suffix. 	|
|  OperationIdTooLong 	| `The operationId length must be   less than '{0}'.` 	| Reduce the value of the property   ‘operationId’ so the character length is less than {0}. 	|
| OperationMissingPathParameter 	| `The operation '{0}' does not define the '{1}' parameter which is required by the path.` 	| Your swagger has an operation which doesn't define a parameter that seems to be required by its path template. 	|
|  OperationMissingRequiredProperty 	| `The target operation requires   parameter '{0}'.` 	| The operation has parameter ‘{0}’ defined as required but this isn't defined in the parameters supplied. Add this parameter to fix the issue. 	|
|  OperationMustHaveNotification<br/>ContentExtension 	| `The operation '{0}' is missing   notification content extension as it has '{1}' properties marked as notification URL.` 	|  Your swagger contains an operation that has   no notification content extension. (Need more context) 	|
| OperationMustHaveResponse 	| `The operation is not valid, it must contain at least one response definition.` 	| Your swagger has an operation that doesn't have at least one response definition. 	|
|  OperationParameterNameIsAmbiguous 	| `The parameter name is ambiguous as it matches both a parameter name and a path to a body property.` 	| The name of the parameter is   used as a parameter name and a parameter body path name. Rename one of the   instances to fix this issue. 	|
|  OperationParameterNameNotFound 	| `The parameter could not be   found in operation '{0}' in the swagger document.` 	| The operation parameter name can't be found in your swagger. 	|
| OperationParameterRequiredMismatch 	| `The parameter is required, but   an optional value is provided by operation '{0}'. The request cannot be made   until a value for this parameter is provided.` 	|  The parameter is required but the value for   the operation states that it is optional causing this mismatch. 	|
|  OperationParametersAreNotUnique 	| `The parameters in the operation   '{0}' are not unique.` 	|  There seems to be a duplicate within the   operation parameters. Your swagger needs to remove operation parameter   duplicates. 	|
|  OperationParametersContain<br/>DuplicateParameter 	| `The operation '{0}' contains   duplicate parameters with name '{1}', location '{2}'.` 	| There is an operational   parameter that is being listed twice. {0} is the operation name, {1} is the   duplicated parameter name, {2} is the duplicated parameter location ('in'   property in swagger). Remove the reoccurrence of the property. 	|
|  OperationParametersContain<br/>MultipleBodyParameters 	| `The operation '{0}' contains multiple parameters in the location 'body', at most one is allowed.` 	| The operation parameters contain a duplicated body parameters. Remove one of the body parameters. 	|
|  OperationParameterStaticTypeMismatch 	| `The parameter has type '{0}',   but a static value is provided of type '{1}'.` 	|  The parameter has static type provided when that isn't expected for this operation parameter. 	|
|  OperationParameterTypeMismatch 	| `The parameter has type '{0}',   but operation '{1}' is providing a parameter of type '{2}'.` 	|  The parameter and the operation have different types. Make the type of your parameter and your operation match. 	|
|  OperationPathContainsUnsupportedCharacters 	| `The operation path cannot   contain wildcard character '*'.` 	| One of your operations specifies a URL path which contains unsupported characters (like the wildcard symbol). 	|
|  OperationPathMayNotContainQueryOrFragment 	| `The path is not valid, it must   not contain a query part or fragment identifier. To include '?' or '#'   characters in the path, they must be URL encoded.` 	| The path of the operation can't   contain ‘?’ and ‘#’ unless they are URL encoded (translated to their %xx equivalents). 	|
|  OperationPathMustStartWithSlash 	| `The path is not valid, it must   start with a '/' character.` 	| If you use an operation path   that you are trying to achieve requires it to start with the '/' character.   Include the  '/' character at the   beginning of the path to fix this issue. 	|
|  OperationPathTemplateNotValid 	| `The path is not valid, each   path template variable enclosed by '{{' and '}}' characters must be contained   within a single URL segment. Individual '{{' or '}}' characters which are not   enclosing a path template variable must be URL encoded.` 	| Your swagger contains an   operation path with an unclosed “{“ or “}” characters. Those characters are used to refer to a variable in path, so they can't exist in the path when unclosed. 	|
|  OperationPathVariablesContainDefaults 	| `The operation path template   variables cannot set default values with character '='.` 	| One of your operations is specifying a default value for a parameter which contains “=”. That parameter is also specified as being in the URL path. This combination isn't allowed. 	|
|  OperationRevisionEmpty 	| `The operation '{0}' does not   specify a revision in api annotation.` 	| The operation is marked as being   part of a family, but does not have a revision. 	|
|  OperationSchemaCountExceedsMaxAllowed 	| `The total number of schemas in   the operation has exceeded the maximum allowed value of '{0}'. Please remove   any unnecessary property or item definitions.` 	| There are too many schemas in   the operation. Remove some of them so the total is back under the limit. 	|
|  OperationShouldHaveSuccessfulResponse 	| `The operation '{0}' should have   at least one successful response definition.` 	| Your swagger has an operation   that does not have at least one successful response definition. 	|
|  OperationStatusAheadOfConnector 	| `The operation '{0}' cannot be in a Production status ahead of the connector.` 	| Operations can't be marked as   “Production” while the entire connector is still in “Preview”. 	|
|  OperationSummaryTooLong 	| `The operation 'summary' should   be fewer than {0} characters for improved readablity.` 	| Reduce the operation ‘summary’   property to be fewer than 120 characters. 	|
|  PartialDuplicateOperationPath 	| `The operation '{0}' has the   path template '{1}' which is similar to other operation path templates, this   can lead to an unexpected routing to a different operation in case an input   path parameter value matches to a static value segment. It is better to have   unconditional routing in all cases. Partially duplicated paths are: {2}` 	| {0} is the operation id, {1} is   the operation path, {2} is the set of partially duplicated operation paths.   Change the operation path to create a bigger difference between the operation   paths that share the similarity. 	|
|  PathParameterMustBeRequired 	| `The parameter must be marked as   required as it is used in the operation path. Include the 'required'   property, with the value 'true'.` 	| The location of the parameter is   set to ‘Path’ therefore it needs to have the property ‘required’ set to   ‘true’. 	|
|  PathParameterNotDefinedInTemplate 	| `The 'path' parameter '{0}' is   not defined in the URI path template. To include the parameter as a variable   in the path, use '{{' and '}}' characters to enclose the name of the   parameter in the path.` 	| There is a parameter in one of your operations which is marked as being in the operation path but does not   exist in the path template. 	|
|  PathParametersContain<br/>DuplicateParameter 	| `The path '{0}' contains   duplicate parameters with name '{1}', location '{2}'.` 	| {0} is the operation path. {1}   is the duplicated parameter name. {2} is the location (the ‘in’ property) of   the duplicated parameter. Remove the duplicated path parameter. 	|
|  PathParametersContainMultiple<br/>BodyParameters 	| `The path '{0}' contains   multiple parameters in the location 'body', at most one is allowed.` 	| There can't be multiple parameters for an operation that are both in the body. Remove one of the parameters to fix this error. 	|
|  PathRedefined 	| `The path '{0}' is defined   multiple times with different capitalization. Paths must be unique when   compared case-insensitively.` 	| The same path is defined more   than once. Remove or rename the instances so they are unique. 	|
|  RestrictedCharactersInSummary 	| `Summary string contains any of   the restricted characters [{0}]` 	| The summary of one of your   operations contains restricted characters. Remove the characters or rephrase   the summary to fix this error. 	|
|  SelectedItemValuePathNotAllowedIn<br/>ThisPosition 	| `The 'selectedItemValuePath'   property is not allowed in this position. It is allowed only in 'browse'   operation of 'x-ms-dynamic-tree' extension.` 	|  Wherever you placed the ‘selectedItemValuePath’ isn't allowed. Remove it or place it within a valid   location. 	|
|  StringBinaryNotSupportedFor<br/>FormUrlEncoded 	| `String/binary parameters are   not supported for operation '{0}' with 'application/x-www-form-urlencoded'   consumes type.` 	| The operation has the   ‘application/x-www-form-urlencoded' value defined in the ‘consumes’ property.   The parameters for this operation can't be of type ‘string’ or ‘binary’. 	|
|  TotalOperationsCountExceedsMaxAllowed 	| `The total number of operations   in the swagger exceeds the maximum allowed value of '{0}'.` 	| There are too many operations in   the swagger. 	|
|  UnrecognizedTypeFormat 	| `The type/format '{0}' is not   recognized.` 	| The given type/format ‘{0}’ is   unknown. Check for typos or documentation to confirm the supported type   formats. 	|
|  WebhookOperationsMissingNotification<br/>UrlProperty 	| `The webhook operation '{0}' is   missing notification URL property.` 	|  Your swagger contains a webhook operation   that does not have a missing notification URL property. Adding this property   within the webhook operation fixes the operation. 	|

## Arrays

|Error  |Issue  |Solution  |
|---------|---------|---------|
| ArrayHasNoItems 	| `The array should have items   specified.` 	| The array is empty, but an empty array isn't allowed. 	|
| RequiredPropertyArrayEmpty 	| `The 'required' array requires   at least one value. If no properties are required in the schema, remove the   'required' property.` 	| Your swagger has an empty array   for a property named ‘required’. It should be removed if there are no   required parameters. 	|
| SchemesArrayEmpty 	| `The 'schemes' array requires at least one value.` 	| Schemas can't be empty and must have at least one value. 	|

## Not supported

|Error  |Issue  |Solution  |
|---------|---------|---------|
| CollectionFormatValueNotSupported 	| `The 'collectionFormat' keyword   value '{0}' is not supported.` 	| Only certain collection formats   are supported. Your swagger contains an unsupported format. 	|
| MimeTypeNotSupported 	| `The MIME type '{0}' is not   supported. The supported mime types are: '{1}'.` 	| The MIME type that you're trying to use in your swagger isn't supported. 	|
| PathItemRefNotSuppored 	| `The '$ref' property in a path   is not supported.` 	| A path can't have a ‘$ref’   property defined. Remove this property. 	|
| SwaggerKeywordNotSupported 	| `The '{0}' keyword is not   supported.` 	| {0} is a swagger keyword such as   'uniqueItems' and isn't allowed in the context where it is being used. 	|

## Parameters

|Error  |Issue  |Solution  |
|---------|---------|---------|
| FileParameterMustBeInFormData 	| `Parameters of type 'file' are   not valid in locations other than 'formData'.` 	| Your swagger has a parameter   with a ‘file’ type and it can only be ‘in’ the ‘formData’. 	|
| ParameterListContainsFormDataAndBody 	| `The 'parameters' list defines   both a 'body' parameter, and a 'formData' parameter. Only one of these   location types can be used in an operation.` 	| Your swagger is defining both a   body parameter and formData which isn't allowed. Remove one of these parameter types. 	|
| ParameterReferenceNotFound 	| `The parameter reference '{0}'   could not be found in the swagger document.` 	| Your swagger has a parameter reference that can't be found in the swagger document. Remove the reference to this parameter. 	|
| ParameterReferenceNotValid 	| `The parameter reference '{0}'   is not valid. Parameter references must start with '#/parameters/' and be   valid JSON Pointers.` 	| Your swagger has a parameter reference (“#/parameters/”) which is invalid based on the JSON pointer RFC   specification. 	|
| ParameterRefLoopNotAllowed 	| `The parameter definition may   not use a '$ref' to point to another definition.` 	| Your swagger has a parameter   definition that is incorrectly using ‘$ref’ to point to another definition. 	|
| ParamtersWithSameNameMustHave<br/>SameTypeAndVisibility 	| `Parameters of the same name in   fetch and subscribe operations must be of the same type and visibility.` 	|  If your swagger has parameters of the same   name in fetch and subscribe operations, they should have the same type and   visibility. 	|
| PathParameterMissingEncoding 	| `Encoding for path parameter   '{0}' is missing, which might lead to invalid requests. Use   "x-ms-url-encoding": "single" to ensure the value is URL   encoded.` 	| {0} is the parameter name that   is missing the encoding. 	|
| QueryParameterEncodingNotSupported 	| `Encoding for query parameter   '{0}' is not supported. All query parameters are single encoded by default.` 	| {0} is the parameter name of   that has the unsupported encoding. Fix this encoding. 	|
| ReservedParameterIsUsed 	| `The parameter '{0}' is reserved   and cannot be used in '{1}'.` 	| The parameter that's stated in this error is being used, whereas it can't be used because it's reserved for that section or in general. 	|
| ResponseRefLoopNotAllowed 	| `The response definition may not   use a '$ref' to point to another definition.` 	| Your swagger has a response definition that is incorrectly using ‘$ref’ to point to another definition. 	|
| SourcePropertyInternal 	| `The parameter reference '{0}'   in the source operation '{1}' is internal or one of its parent(s) are   internal. Either make the property visible or update the parameter reference   to a static value.` 	| The parameter reference is   marked as internal, but the operation where the parameter reference is used isn't internal. Update the parameter or the operation to fix this. 	|

## Properties

| Error 	| Issue 	| Solution 	|
|---	|---	|---	|
| AdditionalPropertyNotAllowed 	| `The property '{0}' is not allowed and must be removed.` 	| There is an extra property in   your swagger that must be removed. 	|
| AdditionalPropertyNotAllowedAdjacent<br/>ToRef 	| `When '$ref' is defined, no   other properties may be specified.` 	| When using ‘$ref’ in the   swagger, no other properties should be included in that JSON object. 	|
| BasePathNotValid 	| `The 'basePath' property, if   present, must start with a '/' character.` 	| If you use a base path, it must   start with “/”. Invalid base paths look like “abc/def/ghi” or   “microsoft.com/abc”. 	|
| BodyParameterPropertyNotValid 	| `The 'body' parameter cannot use   property '{0}', instead, the 'schema' property should be used to define the   type used by the parameter.` 	| One of the body parameters in your swagger is using an invalid property that can't be used in a body parameter. 	|
| ConnectorMetadataPropertiesMissing 	| `The connector metadata property '{0}' is required.` 	| Certain metadata is required for the connector. 	|
| ContactEmailNotValid 	| `The contact 'email' property, if present, must be a valid email address.` 	| The email address provided is an incorrect format or contains non-ASCII characters. 	|
| DefaultValueMustMatchType 	| `The 'default' value is of type '{0}', but should match the given type '{1}'.` 	| The value for your default value isn't the same type as the given type. Change the value for the default value to that of the given type to fix this issue. 	|
| EnumMustContainAtLeastOneElement 	| `The 'enum' array requires at least one value.` 	| If your property type is an enum, it should specify an array with at least one of the enum values. 	|
| ExtensionNameNotValid 	| `The property '{0}' is not valid. If this property is an extension, its name must begin with "x-".` 	| Properties that are extensions   must begin with the "x-" prefix. 	|
| FilterFunctionsPropertyEmpty 	| `The 'filterFunctions' property   must have at least one element.` 	| The ‘filterFunctions’ property can't be empty if specified. 	|
| FilterFunctionsPropertyHasDuplicates 	| `The 'filterFunctions' property cannot contain duplicates.` 	| The ‘filterFunctions’ property can't contain duplicates. 	|
| FilterFunctionsPropertyInInputSchema 	| `The 'filterFunctions' property   has to be in the response schema.` 	| The ‘filterFunctions’ property   can only be in the response schema. 	|
| FilterFunctionsPropertyInvalidValue 	| `The 'filterFunctions' property   supports only these values: {0}".` 	| The ‘filterFunctions’ property can't specify an unsupported value. 	|
| HostNameNotValid 	| `The 'host' property, if present, must be a valid URI host without a scheme or path.` 	| A host URI without a scheme or   path looks like “www.microsoft.com”. An invalid URI for the host looks like “https://www.microsoft.com/” or “www.microsoft.com/hello”. 	|
| InternalPropertyWithDefaultOptional 	| `The internal property has a   default value but is optional. Only required properties are guaranteed to be   included in requests.` 	| A property is marked internal, has a default value specified, and isn't required. If you want to ensure this is included in the requests, mark it as required; else if this is intended, it may be left as is. 	|
| InvalidConnectorCategory 	| `The connector category is   invalid. Available categories are: [{0}].` 	| Categories can only come from the list of allowed category values. 	|
| InvalidStringBinaryProperty 	| `A schema with type/format   'string/binary' can only be at the top level of a body or formData   parameter.` 	| The value of property “type” and “format” of “string” and “binary” isn't allowed at this schema location. 	|
| JsonTypeIncorrect 	| `The type of the property is   incorrect. Expected type '{0}', but value is of type '{1}'.` 	| A property in your swagger has one type, but the actual value is a different type. Change the value type to match the expected type. 	|
| JsonValueNotInEnum 	| `The property value must be one   of the following: {0}.` 	| The property value of the enum   in does not match the current available options. Change the value to one of   the following stated in the error message. 	|
| MimeTypeNotValid 	| `The MIME type '{0}' is not   valid.` 	| The user-provided MIME 'content type' that you selected isn't valid. The valid mime types are: "application/json", "text/plain", "multipart/form-data", "application/x-www-form-urlencoded" 	|
| MissingEnumValuesDisplayName 	| `The 'x-ms-enum-values'   extension is invalid. Please specify the property 'displayName'.` 	|  The x-ms-enum-values did not specify the   ‘displayName’ property. 	|
| MultipleOfMustBeGreaterThanZero 	| `The 'multipleOf' value must be   greater than zero.` 	| The value of the “multipleOf”   property should be a value greater than zero. 	|
| OneOfPropertiesRequired 	| `At least one of the properties   is required: [{0}].` 	| {0} is a set of properties. This   error displays all the properties that are required to be present in the   swagger. 	|
| ProductionStatusOnBetaService 	| `The connector cannot be in Production status on a beta service.` 	| The connector can't be marked as ‘Production’ when the backend service is in Beta. It must be set to ‘Preview’. 	|
| PropertyMustBeRequired 	| `The property is internal and has a default value, it must be required. Optional internal fields are ignored.` 	| Property isn't being set although it is required. 	|
| RequiredPropertyDefinitionMissing 	| `The required property '{0}' is   not defined in the object schema.` 	| A schema property is listed as   under “required” and therefore must be defined in the “properties” 	|
| RequiredPropertyMissing 	| `The '{0}' property is   required.` 	| A property in your swagger is   required, but it isn't currently in your swagger. Include this property to   fix this error. 	|
| RequiredPropertyNotOnObject 	| `The property 'required' is   applicable to schemas with 'object' type only and should be removed.` 	| The ‘required’ property is only   supported for the ‘object’ type. Remove the property for all other types. 	|
| RequiredReadOnlyPropertyNotAllowed<br/>AsInput 	| `An input property cannot be   'readonly' and required.` 	|  Your swagger contains a property which is marked as readonly and required. A readonly property means the user can't modify the value. A required property requires a value for modification. 	|
| SchemeNotValid 	| `The 'schemes' property must   only contain transfer protocols from the list: {0}.` 	| The scheme property should be   one of “http” or “https”. 	|
| SecuritySchemePropertyNotValid 	| `The security definition property '{0}' is not valid for definitions of type '{1}'.` 	| There is a property name and value that aren't valid that you have included within the securityDefinitions dictionary. Change this property to a valid property name to fix this issue. 	|
| SecuritySchemePropertyRequired 	| `The security definition property '{0}' is required for definitions of type '{1}'.` 	| There is a property name and value that are required that aren't included within the securityDefinitions dictionary. Add this property and value to fix this issue. 	|
| SpecificationVersionIncorrect 	| `The 'swagger' property must   have the string value "2.0".` 	| Only version “2.0” is supported,   so fix the swagger version property to be “2.0”. 	|
| TagRedefined 	| `The 'tags' property defines tag   '{0}' multiple times.` 	| Tags defined in the ‘tags’   property must be unique. Remove or replace any multiple occurrences. 	|
| TooManyConnectorCategories 	| `The maximum allowed number of   categories for the connector is '{0}'.` 	| There is a limit to the number   of categories that can be set for the connector. 	|
| UrlNotValid 	| `The 'url' property must be a   valid absolute URL.` 	| Your swagger has an invalid URL. This can be due to incorrectly copying the URL from somewhere. 	|

## Response

| Error 	| Issue 	| Solution 	|
|---	|---	|---	|
|  BodyParameterSchemaRequired 	| `The 'body' parameter must   define a 'schema' property.` 	| In the body parameter, the   schema property must be defined. Define the schema property to fix this   issue. 	|
|  BodySchemaCountExceedsMaxAllowed 	| `The total number of schemas in   the object exceeds the max schema count allowed value of '{0}'. Please remove   any unnecessary property or item definitions.` 	| There are too many schemas in   the body object. Remove some of them so the total is back under the limit. 	|
|  DefaultResponseHasSchema 	| `The 'default' response should   not have schema definition. Schemas should be defined on expected responses   only.` 	|  Your swagger contains a schema definition   for the default response. Removing this schema definition for the default   response fixes this error. 	|
|  InvalidCustomEditorExtension<br/>DictionaryValue 	| `The 'x-ms-editor' extension can   be specified as 'dictionary' only for schema with type object and additional   properties allowed.` 	|  Check to see if the x-ms-editor is only set   to dictionary when the schema is of type object and set to additional   properties allowed. 	|
|  JsonPointerNotValid 	| `The JSON Pointer path is not   valid.` 	| A reference (#ref) being used in   your swagger isn't valid. 	|
|  NonBodyParameterSchemaNotValid 	| `The parameter cannot define a   schema as it is not in the 'body'.` 	| Your swagger has a parameter   that uses ‘schema’, but only body parameters can use ‘schema’. 	|
|  RequiredLoopInSchemaNotAllowed 	| `Loops of required schema   properties are not allowed.` 	| Specification of required properties can't use reference loops. 	|
|  ResponseExampleMustBeProduced<br/>ByOperation 	| `The response example '{0}' is   not produced by the operation '{1}'.` 	| Your swagger has an operation   with a response example, but that same operation does not produce a response   with the indicated MIME type. 	|
|  ResponseHeadersAreNotUnique 	| `The response headers are not   unique.` 	| There is a duplicate response   header. Response headers should be unique. 	|
|  ResponseMustBeDefaultOrHttpStatusCode 	| `The response is not valid, the   response must be a valid HTTP status code, or the string 'default'.` 	|  Your swagger does not have a default HTTP   status code or a valid HTTP status code. Add a status code to your response. 	|
|  ResponseReferenceNotFound 	| `The response reference '{0}'   could not be found in the swagger document.` 	| Your swagger has a response reference that can't be found in the swagger document. Remove the reference to this response. 	|
|  ResponseReferenceNotValid 	| `The response reference '{0}' is   not valid. Response references must start with '#/responses/' and be valid   JSON Pointers.` 	| Your swagger has a response   reference (“#/responses/”) which is invalid based on the JSON pointer RFC   specification. 	|
| SchemaDefinitionSetIncorrectly 	| `The operation '{0}' should   define an output schema on the 200 or 201 response code or in   'x-ms-notification-content' extension for a webhook to be visible in the   designer.` 	| Your operation is missing a   response schema for the 200 or 201 response code. For webhooks, the response   schema should be specified as part of the notification content extension. 	|
| SchemaReferenceNotFound 	| `The schema reference '{0}'   could not be found in the swagger document.` 	| Your swagger has a schema   reference that could not be found in the swagger document. 	|
| SchemaReferenceNotValid 	| `The schema reference '{0}' is   not valid. Schema references must start with '#/definitions/' and be valid   JSON Pointers.` 	| Your swagger has a schema   reference(‘#/definitions/’), which is invalid based on the JSON pointer RFC   specification. 	|
| SchemaRefLoopNotAllowed 	| `The schema definition may not   use a '$ref' to point to another definition.` 	| There is a reference loop in   your schema (A points to B, which points back to A). 	|
| SchemaTooDeep 	| `The schema exceeds the maximum   allowed depth. Schemas must not exceed a depth of '{0}', including referenced   ('$ref') schemas. ` 	| Schemas are limited to a   specific JSON depth. Use a shallower schema. 	|
| SchemaTypeFileNotValid 	| `The schema type 'file' is not valid. Only parameters in 'formData' and the root response schema may be of type 'file'.` 	| Using the schema type ‘file’ is   only supported in form data and must be at the root of the response schema   for the response definition. 	|
| SchemeNotSupported 	| `The WebSocket transfer protocol   schemes are not supported.` 	| This scheme isn't supported. The supported schemes are Https and Http. 	|
| TotalSchemaCountExceedsMaxAllowed 	| `The maximum allowed number of   schemas in the swagger is '{0}'. Please remove any unnecessary property or   item definitions.` 	| There are too many schemas in   the swagger. 	|

## Security

| Error 	| Issue 	| Solution 	|
|---	|---	|---	|
| OAuthSecurityRequirementValueMustBe<br/>ScopeName 	| `The OAuth security requirement   '{0}' refers to scope name '{1}', but this is not defined in the   'securityDefinitions' object.` 	| Security requirement names must   be a scope name that is defined in the security definitions object. 	|
| SecurityRequirementMustBeDefined 	| `The security requirement '{0}'   is not defined in the 'securityDefinitions' object.` 	| A security requirement is used,   but not defined in the security definitions dictionary. 	|
| SecurityRequirementMustHaveEmptyValue 	| `The security requirement '{0}'   cannot specify any scopes, an empty array should be provided.` 	| Based on the current   configuration, the security requirements with name ‘{0}’ must be an empty   array. 	|
| SecurityRequirementMustHaveValue 	| `The security requirement '{0}' must have have a value. Provide an empty array if no OAuth scopes are required.` 	| The security requirement is missing or is null. If there are no OAuth scopes required, specify an empty array instead. 	|

## Webhooks

| Error 	| Issue 	| Solution 	|
|---	|---	|---	|
| NotificationUrlNotAllowedInThisPosition 	| `The notification URL property is not allowed in this position. It is allowed only in input body.` 	| The “x-ms-notification-url"   property should only be defined in an input. 	|
| NotificationUrlNotInternal 	| `The notification URL property   itself or at least one of its parents must be internal.` 	|  Your swagger currently isn't setting the notification URL property to internal, which should be the case. 	|
| NotificationUrlNotRequired 	| `The notification URL property   must be required all the way through the schema chain.` 	|  Your swagger currently isn't requiring notification URL property, which should be required. 	|
| NotificationUrlNotString 	| `The notification URL property   must be of type string.` 	|  The notification URL property has a value that isn't of type string. 	|

## Metadata

| Error 	| Issue 	| Solution 	|
|---	|---	|---	|
| AllowEmptyValueOnlyValidForQuery<br/>FormData 	| `The parameter cannot use the   'allowEmptyValue' keyword as it is not in the 'query' or 'formData'   location.` 	| The property ‘allowEmptyValue’ can only be set to ‘true’ in a parameter that has a ‘location’ property that is ‘query’ or ‘formData’. The value of the ‘location’ property isn't either case. 	|
| DiscriminatorMustBeARequiredStringProperty 	| `The 'discriminator' value must   be the name of a required string property.` 	| The ' discriminator ' value   should be the name of a required string property in your swagger. 	|
| ExclusiveMaximumRequiresMaximum 	| `The 'exclusiveMaximum' keyword   can only be specified if the 'maximum' keyword is also specified.` 	| You can't specify a   “exclusiveMaximum” without also specifying a “maximum”. 	|
| ExclusiveMinimumRequiresMinimum 	| `The 'exclusiveMinimum' keyword   can only be specified if the 'minimum' keyword is also specified.` 	| You can't specify a   “exclusiveMinimum” without also specifying a “minimum”. 	|
| FieldLengthExceeded 	| `The '{0}' value goes over the   character limit '{1}'` 	| A field with the name specified   in {0} is over the character limit specified in {1}. 	|
| FieldLengthNotInRange 	| `The '{0}' value must be between   '{1}' and '{2}' characters.` 	| {0} is the field name, {1} is   the minimum length, {2} is the limit. Decrease the length of the characters   in this field. 	|
| MaxItemsMustBeGreaterThanOrEqual<br/>ToZero 	| `The 'maxItems' value must be   greater than or equal to zero.` 	| The 'maxItems' value currently   is below zero, change this value to greater than or equal to zero. 	|
| MaxLengthMustBeGreaterThanOrEqual<br/>ToZero 	| `The 'maxLength' value must be   greater than or equal to zero.` 	| The value of the “maxLength”   property should be a non-negative number. 	|
| MaxPropertiesMustBeGreaterThanOrEqual<br/>ToZero 	| `The 'maxProperties' value must   be greater than or equal to zero.` 	| The 'maxProperties' value currently is below zero, change this value to greater than or equal to zero. 	|
| MinimumShouldBeLessThanOrEqual<br/>ToMaximum 	| `The 'minimum' value should be   less than or equal to the 'maximum' value.` 	| When specifying a minimum value,   the value should be less than or equal to the maximum value. 	|
| MinItemsMustBeGreaterThanOrEqual<br/>ToZero 	| `The 'minItems' value must be   greater than or equal to zero.` 	| The 'minItems' value currently   is below zero, change this value to greater than or equal to zero. 	|
| MinItemsShouldBeLessThanOrEqual<br/>ToMaxItems 	| `The 'minItems' value should be   less than or equal to the 'maxItems' value.` 	| When specifying minimum items,   the value should be less than or equal to the maximum items. 	|
| MinLengthMustBeGreaterThanOrEqual<br/>ToZero 	| `The 'minLength' value must be   greater than or equal to zero.` 	| The value of the “minLength”   property should be a non-negative number. 	|
| MinLengthShouldBeLessThanOrEqual<br/>ToMaxLength 	| `The 'minLength' value should be   less than or equal to the 'maxLength' value.` 	| When specifying a minimum   length, the value should be less than or equal to the maximum length. 	|
| MinPropertiesMustBeGreaterThanOrEqual<br/>ToZero 	| `The 'minProperties' value must   be greater than or equal to zero.` 	| The 'minProperties' value   currently is below zero, change this value to greater than or equal to zero. 	|
| PatternShouldBeValidRegularExpression 	| `The 'pattern' value should be a   valid regular expression.` 	| The 'pattern' value isn't a valid regular expression, fixing the regular expression will fix this error. 	|
| RequiredValueEmpty 	| `The '{0}' property must have not empty value.` 	| Your swagger contains an empty   value for a  property that is required.   Setting the value of this required property fixes this issue. 	|
| ValueContainsRestrictedWords 	| `The value '{0}' contains at   least one of the restricted words: '{1}'.` 	| One of the fields in the swagger   uses a restricted word. 	|
| ValueMustBeInEnglish 	| `The '{0}' value must be written   in English.` 	| Replace the value with a value   containing only alphanumeric characters. 	|
| ValueMustEndWithAlphanumeric 	| `The '{0}' value must end with   alphanumeric character.` 	| Replace the last character with   an alphanumeric character 	|

