---
title: Write code in a custom connector | Microsoft Docs
description: Learn about custom code and get samples to help you create a connector for Power Automate and Power Apps.
author: shgogna
contributors:
  - shgogna
  - v-aangie
services: connectors
documentationcenter: 
ms.assetid: 
ms.service: connectors
ms.workload: connectors
ms.topic: conceptual
ms.date: 12/06/2022
ms.author: shgogna
ms.reviewer: angieandrews
---

# Write code in a custom connector

Custom code transforms request and response payloads beyond the scope of existing policy templates. When code is used, it will take precedence over the codeless definition.

For more information, go to [Create a custom connector from scratch](define-blank.md#step-4-optional-use-custom-code-support).

## Script class

Your code needs to implement a method called ExecuteAsync, which is called during runtime. You can create other methods in this class as needed, and call them from ExecuteAsync method. The class name must be **Script** and it must implement **ScriptBase**.

```csharp
public class Script : ScriptBase
{
    public override Task<HttpResponseMessage> ExecuteAsync()
    {
        // Your code here
    }
}
```

## Definition of supporting classes and interfaces

The following classes and interfaces are referenced by the Script class. They can be used for local testing and compilation.

```csharp
public abstract class ScriptBase
{
    // Context object
    public IScriptContext Context { get; }

    // CancellationToken for the execution
    public CancellationToken CancellationToken { get; }

    // Helper: Creates a StringContent object from the serialized JSON
    public static StringContent CreateJsonContent(string serializedJson);

    // Abstract method for your code
    public abstract Task<HttpResponseMessage> ExecuteAsync();
}

public interface IScriptContext
{
    // Correlation Id
    string CorrelationId { get; }

    // Connector Operation Id
    string OperationId { get; }

    // Incoming request
    HttpRequestMessage Request { get; }

    // Logger instance
    ILogger Logger { get; }

    // Used to send an HTTP request
    // Use this method to send requests instead of HttpClient.SendAsync
    Task<HttpResponseMessage> SendAsync(
        HttpRequestMessage request,
        CancellationToken cancellationToken);
}
```

## Samples

### Hello World script

This sample script will always return Hello World as response for all requests.

```csharp
public override async Task<HttpResponseMessage> ExecuteAsync()
{
    // Create a new response
    var response = new HttpResponseMessage();

    // Set the content
    // Initialize a new JObject and call .ToString() to get the serialized JSON
    response.Content = CreateJsonContent(new JObject
    {
        ["greeting"] = "Hello World!",
    }.ToString());

    return response;
}
```

### Regex script

The following sample takes some text to match and the regex expression and returns the result of the match in the response.

```csharp
public override async Task<HttpResponseMessage> ExecuteAsync()
{
    // Check if the operation ID matches what is specified in the OpenAPI definition of the connector
    if (this.Context.OperationId == "RegexIsMatch")
    {
        return await this.HandleRegexIsMatchOperation().ConfigureAwait(false);
    }

    // Handle an invalid operation ID
    HttpResponseMessage response = new HttpResponseMessage(HttpStatusCode.BadRequest);
    response.Content = CreateJsonContent($"Unknown operation ID '{this.Context.OperationId}'");
    return response;
}

private async Task<HttpResponseMessage> HandleRegexIsMatchOperation()
{
    HttpResponseMessage response;

    // We assume the body of the incoming request looks like this:
    // {
    //   "textToCheck": "<some text>",
    //   "regex": "<some regex pattern>"
    // }
    var contentAsString = await this.Context.Request.Content.ReadAsStringAsync().ConfigureAwait(false);

    // Parse as JSON object
    var contentAsJson = JObject.Parse(contentAsString);

    // Get the value of text to check
    var textToCheck = (string)contentAsJson["textToCheck"];

    // Create a regex based on the request content
    var regexInput = (string)contentAsJson["regex"];
    var rx = new Regex(regexInput);

    JObject output = new JObject
    {
        ["textToCheck"] = textToCheck,
        ["isMatch"] = rx.IsMatch(textToCheck),
    };

    response = new HttpResponseMessage(HttpStatusCode.OK);
    response.Content = CreateJsonContent(output.ToString());
    return response;
}
```

### Forwarding script

The following sample forwards the incoming request to the backend.

```csharp
public override async Task<HttpResponseMessage> ExecuteAsync()
{
    // Check if the operation ID matches what is specified in the OpenAPI definition of the connector
    if (this.Context.OperationId == "ForwardAsPostRequest")
    {
        return await this.HandleForwardOperation().ConfigureAwait(false);
    }

    // Handle an invalid operation ID
    HttpResponseMessage response = new HttpResponseMessage(HttpStatusCode.BadRequest);
    response.Content = CreateJsonContent($"Unknown operation ID '{this.Context.OperationId}'");
    return response;
}

private async Task<HttpResponseMessage> HandleForwardOperation()
{
    // Example case: If your OpenAPI definition defines the operation as 'GET', but the backend API expects a 'POST',
    // use this script to change the HTTP method.
    this.Context.Request.Method = HttpMethod.Post;

    // Use the context to forward/send an HTTP request
    HttpResponseMessage response = await this.Context.SendAsync(this.Context.Request, this.CancellationToken).ConfigureAwait(continueOnCapturedContext: false);
    return response;
}
```

### Forwarding and transform script

The following sample forwards the incoming request and transforms the response returned from the backend.

```csharp
public override async Task<HttpResponseMessage> ExecuteAsync()
{
    // Check if the operation ID matches what is specified in the OpenAPI definition of the connector
    if (this.Context.OperationId == "ForwardAndTransformRequest")
    {
        return await this.HandleForwardAndTransformOperation().ConfigureAwait(false);
    }

    // Handle an invalid operation ID
    HttpResponseMessage response = new HttpResponseMessage(HttpStatusCode.BadRequest);
    response.Content = CreateJsonContent($"Unknown operation ID '{this.Context.OperationId}'");
    return response;
}

private async Task<HttpResponseMessage> HandleForwardAndTransformOperation()
{
    // Use the context to forward/send an HTTP request
    HttpResponseMessage response = await this.Context.SendAsync(this.Context.Request, this.CancellationToken).ConfigureAwait(continueOnCapturedContext: false);

    // Do the transformation if the response was successful, otherwise return error responses as-is
    if (response.IsSuccessStatusCode)
    {
        var responseString = await response.Content.ReadAsStringAsync().ConfigureAwait(continueOnCapturedContext: false);
        
        // Example case: response string is some JSON object
        var result = JObject.Parse(responseString);
        
        // Wrap the original JSON object into a new JSON object with just one key ('wrapped')
        var newResult = new JObject
        {
            ["wrapped"] = result,
        };
        
        response.Content = CreateJsonContent(newResult.ToString());
    }

    return response;
}
```

## Supported namespaces

Not all C# namespaces are supported. Currently, you can use functions from the following namespaces only.
 
```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Diagnostics;
using System.IO;
using System.IO.Compression;
using System.Linq;
using System.Net;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Net.Security;
using System.Security.Authentication;
using System.Security.Cryptography;
using System.Text;
using System.Text.RegularExpressions;
using System.Threading;
using System.Threading.Tasks;
using System.Web;
using System.Xml;
using System.Xml.Linq;
using System.Drawing;
using System.Drawing.Drawing2D;
using System.Drawing.Imaging;
using Microsoft.Extensions.Logging;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
```

### GitHub samples

For examples in the DocuSign connector, go to [Power Platform Connectors](https://github.com/microsoft/PowerPlatformConnectors/tree/dev/certified-connectors/DocuSign) in GitHub.

## Custom code FAQ

To learn more about custom code, go to [Step 4: (Optional) Use custom code support](define-blank.md#step-4-optional-use-custom-code-support).

**Q: Is it possible to use multiple scripts per custom connector?**<br/>
**A:** No, only one script file per custom connector is supported.

**Q: I'm getting an internal server error when updating my custom connector. What could be the issue?**<br/>
**A:** Most likely this is an issue compiling your code. In the future, we'll display the full list of compilation errors to improve this experience. We recommend using the [supporting classes](#definition-of-supporting-classes-and-interfaces) to test compilation errors locally for now as a workaround.


**Q: Can I add logging to my code and get a trace for debugging?**<br/>
**A:** Not currently, but support for this will be added in the future.

**Q: How can I test my code in the meantime?**<br/>
**A:** Test it locally, and make sure you can compile code using only the namespaces provided in [supported namespaces](write-code.md#supported-namespaces). For information on local testing, go to [Write code in a custom connector](write-code.md).

**Q: Are there any limits?**<br/>
**A:** Yes. Your script must finish execution within 5 seconds and the size of your script file can’t be more than 1 MB.

**Q: Can I create my own http client in script code?**<br/>
**A:** Currently yes, but we'll block this in the future. The recommended way is to use **this.Context.SendAsync** method.

**Q: Can I use custom code with the on-premises data gateway?**<br/>
**A:** Not currently, no.

## Next step

[Create a custom connector from scratch](define-blank.md)


[!INCLUDE[feedback](../includes/feedback.md)]
