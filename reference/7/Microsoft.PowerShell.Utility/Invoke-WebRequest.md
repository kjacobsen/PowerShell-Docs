---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
keywords: powershell,cmdlet
locale: en-us
Module Name: Microsoft.PowerShell.Utility
ms.date: 11/17/2017
online version: https://go.microsoft.com/fwlink/?linkid=821826
schema: 2.0.0
title: Invoke-WebRequest
---
# Invoke-WebRequest

## SYNOPSIS
Gets content from a web page on the Internet.

## SYNTAX

### StandardMethod (Default)

```
Invoke-WebRequest [-UseBasicParsing] [-Uri] <Uri> [-WebSession <WebRequestSession>]
 [-SessionVariable <String>] [-AllowUnencryptedAuthentication] [-Authentication <WebAuthenticationType>]
 [-Credential <PSCredential>] [-UseDefaultCredentials] [-CertificateThumbprint <String>]
 [-Certificate <X509Certificate>] [-SkipCertificateCheck] [-SslProtocol <WebSslProtocol>]
 [-Token <SecureString>] [-UserAgent <String>] [-DisableKeepAlive] [-TimeoutSec <Int32>]
 [-Headers <IDictionary>] [-MaximumRedirection <Int32>] [-MaximumRetryCount <Int32>]
 [-RetryIntervalSec <Int32>] [-Method <WebRequestMethod>] [-Proxy <Uri>] [-ProxyCredential <PSCredential>]
 [-ProxyUseDefaultCredentials] [-Body <Object>] [-Form <IDictionary>] [-ContentType <String>]
 [-TransferEncoding <String>] [-InFile <String>] [-OutFile <String>] [-PassThru] [-Resume]
 [-PreserveAuthorizationOnRedirect] [-SkipHeaderValidation]
```

### StandardMethodNoProxy

```
Invoke-WebRequest [-UseBasicParsing] [-Uri] <Uri> [-WebSession <WebRequestSession>]
 [-SessionVariable <String>] [-AllowUnencryptedAuthentication] [-Authentication <WebAuthenticationType>]
 [-Credential <PSCredential>] [-UseDefaultCredentials] [-CertificateThumbprint <String>]
 [-Certificate <X509Certificate>] [-SkipCertificateCheck] [-SslProtocol <WebSslProtocol>]
 [-Token <SecureString>] [-UserAgent <String>] [-DisableKeepAlive] [-TimeoutSec <Int32>]
 [-Headers <IDictionary>] [-MaximumRedirection <Int32>] [-MaximumRetryCount <Int32>]
 [-RetryIntervalSec <Int32>] [-Method <WebRequestMethod>] [-NoProxy] [-Body <Object>]
 [-Form <IDictionary>] [-ContentType <String>] [-TransferEncoding <String>] [-InFile <String>]
 [-OutFile <String>] [-PassThru] [-Resume] [-PreserveAuthorizationOnRedirect] [-SkipHeaderValidation]
```

### CustomMethod

```
Invoke-WebRequest [-UseBasicParsing] [-Uri] <Uri> [-WebSession <WebRequestSession>]
 [-SessionVariable <String>] [-AllowUnencryptedAuthentication] [-Authentication <WebAuthenticationType>]
 [-Credential <PSCredential>] [-UseDefaultCredentials] [-CertificateThumbprint <String>]
 [-Certificate <X509Certificate>] [-SkipCertificateCheck] [-SslProtocol <WebSslProtocol>]
 [-Token <SecureString>] [-UserAgent <String>] [-DisableKeepAlive] [-TimeoutSec <Int32>]
 [-Headers <IDictionary>] [-MaximumRedirection <Int32>] [-MaximumRetryCount <Int32>]
 [-RetryIntervalSec <Int32>] -CustomMethod <String> [-Proxy <Uri>] [-ProxyCredential <PSCredential>]
 [-ProxyUseDefaultCredentials] [-Body <Object>] [-Form <IDictionary>] [-ContentType <String>]
 [-TransferEncoding <String>] [-InFile <String>] [-OutFile <String>] [-PassThru] [-Resume]
 [-PreserveAuthorizationOnRedirect] [-SkipHeaderValidation]
```

### CustomMethodNoProxy

```
Invoke-WebRequest [-UseBasicParsing] [-Uri] <Uri> [-WebSession <WebRequestSession>]
 [-SessionVariable <String>] [-AllowUnencryptedAuthentication] [-Authentication <WebAuthenticationType>]
 [-Credential <PSCredential>] [-UseDefaultCredentials] [-CertificateThumbprint <String>]
 [-Certificate <X509Certificate>] [-SkipCertificateCheck] [-SslProtocol <WebSslProtocol>]
 [-Token <SecureString>] [-UserAgent <String>] [-DisableKeepAlive] [-TimeoutSec <Int32>]
 [-Headers <IDictionary>] [-MaximumRedirection <Int32>] [-MaximumRetryCount <Int32>]
 [-RetryIntervalSec <Int32>] -CustomMethod <String> [-NoProxy] [-Body <Object>] [-Form <IDictionary>]
 [-ContentType <String>] [-TransferEncoding <String>] [-InFile <String>] [-OutFile <String>]
 [-PassThru] [-Resume] [-PreserveAuthorizationOnRedirect] [-SkipHeaderValidation]
```

## DESCRIPTION

The `Invoke-WebRequest` cmdlet sends HTTP and HTTPS requests to a web page or web service.
It parses the response and returns collections of links, images, and other significant HTML elements.

This cmdlet was introduced in PowerShell 3.0.

## EXAMPLES

### Example 1: Send a web request

This command uses the `Invoke-WebRequest` cmdlet to send a web request to the Bing.com site.

```powershell
$R = Invoke-WebRequest -URI http://www.bing.com?q=how+many+feet+in+a+mile
$R.InputFields | Where-Object {
    $_.name -like "* Value*"
} | Select-Object Name, Value
```

```Output
name       value
----       -----
From Value 1
To Value   5280
```

The first command issues the request and saves the response in the `$R` variable.

The second command gets any **InputField** where the **name** property is like "* Value". The
filtered results are piped to `Select-Object` to select the **name** and **value** properties.

### Example 2: Use a stateful web service

```powershell
$Body = @{
    User = 'jdoe'
    password = 'P@S$w0rd!'
}
$LoginResponse = Invoke-WebRequest 'http://www.contoso.com/login/' -SessionVariable 'Session' -Body $Body -Method 'POST'

$Session

$ProfileResponse = Invoke-WebRequest 'http://www.contoso.com/profile/' -WebSession $Session

$ProfileResponse
```

This example shows how to use the `Invoke-WebRequest` cmdlet with a stateful web service.

The first call to `Invoke-WebRequest` sends a sign-in request. The command specifies a value of "Session" for the value of the **-SessionVariable** parameter, and saves the result in the `$LoginResponse` variable. When the command completes, the `$LoginResponse` variable contains an `BasicHtmlWebResponseObject` and the `$Session` variable contains a `WebRequestSession` object. This logs the user into the site.

The call to `$Session` by itself shows the `WebRequestSession` object in the variable.

The second call to `Invoke-WebRequest` fetches the user's profile which requires that the user be logged into the site. The session data stored in the `$Session` variable is used to provide session cookies to the site created during the login. The result is saved in the `$ProfileResponse` variable.

The call to `$ProfileResponse` by itself shows the `BasicHtmlWebResponseObject` in the variable.

### Example 3: Get links from a web page

```powershell
(Invoke-WebRequest -Uri "https://aka.ms/pscore6-docs").Links.Href
```

This command gets the links in a web page.
It uses the `Invoke-WebRequest` cmdlet to get the web page content.
Then it users the **Links** property of the `BasicHtmlWebResponseObject` that `Invoke-WebRequest` returns, and the Href property of each link.

### Example 4: Writes the response content to a file using the encoding defined in the requested page.

```powershell
$Response = Invoke-WebRequest -Uri "https://aka.ms/pscore6-docs"
$Stream = [System.IO.StreamWriter]::new('.\docspage.html', $false, $Response.Encoding)
try {
    $Stream.Write($response.Content)
}
finally {
    $Stream.Dispose()
}
```

This command uses the `Invoke-WebRequest` cmdlet to retrieve the web page content of an msdn page.

The first command retrieves the page and saves the response object in a variable.

The second command creates a `StreamWriter` to use to write the response content to a file. The **Encoding** property of the response object is used to set the encoding for the file.

The final few commands write the **Content** property to the file then disposes the `StreamWriter`.

Note that the **Encoding** property will be null if the web request does not return text content.

### Example 5: Submit a multipart/form-data file

```powershell
$FilePath = 'c:\document.txt'
$FieldName = 'document'
$ContentType = 'text/plain'

$FileStream = [System.IO.FileStream]::new($filePath, [System.IO.FileMode]::Open)
$FileHeader = [System.Net.Http.Headers.ContentDispositionHeaderValue]::new('form-data')
$FileHeader.Name = $FieldName
$FileHeader.FileName = Split-Path -leaf $FilePath
$FileContent = [System.Net.Http.StreamContent]::new($FileStream)
$FileContent.Headers.ContentDisposition = $FileHeader
$FileContent.Headers.ContentType = [System.Net.Http.Headers.MediaTypeHeaderValue]::Parse($ContentType)

$MultipartContent = [System.Net.Http.MultipartFormDataContent]::new()
$MultipartContent.Add($FileContent)

$Response = Invoke-WebRequest -Body $MultipartContent -Method 'POST' -Uri 'https://api.contoso.com/upload'
```

This example uses the `Invoke-WebRequest` cmdlet upload a file as a `multipart/form-data` submission. The file `c:\document.txt` will be submitted as the form field `document` with the `Content-Type` of `text/plain`.

### Example 6: Simplified Multipart/Form-Data Submission

```powershell
$Uri = 'https://api.contoso.com/v2/profile'
$Form = @{
    firstName  = 'John'
    lastName   = 'Doe'
    email      = 'john.doe@contoso.com'
    avatar     = Get-Item -Path 'c:\Pictures\jdoe.png'
    birthday   = '1980-10-15'
    hobbies    = 'Hiking','Fishing','Jogging'
}
$Result = Invoke-RestMethod -Uri $Uri -Method Post -Form $Form
```

Some APIs require `multipart/form-data` submissions to upload files and mixed content.
This example demonstrates updating a user profile.
The profile form requires these fields:
`firstName`, `lastName`, `email`, `avatar`, `birthday`, and `hobbies`.
The API is expecting an image for the user profile pic to be supplied in the `avatar` field.
The API will also accept multiple `hobbies` entries to be submitted in the same form.

When creating the `$Form` HashTable, the key names are used as form field names.
By default, the values of the HashTable will be converted to strings.
If a `System.IO.FileInfo` value is present, the file contents will be submitted.
If a collection such as arrays or lists are present,
the form field will be submitted will be submitted multiple times.

By using `Get-Item` on the `avatar` key, the `FileInfo` object will be set as the value.
The result is that the image data for `jdoe.png` will be submitted.

By supplying a list to the `hobbies` key,
the `hobbies` field will be present in the submissions
once for each list item.

### Example 7: Catch non success messages from Invoke-WebRequest

When `Invoke-WebRequest` encounters a non-success HTTP message (404, 500, etc.), it returns no
output and throws a terminating error. To catch the error and view the **StatusCode** you can
enclose execution in a `try/catch` block. The following example shows how to accomplish this.

```powershell
try
{
    $response = Invoke-WebRequest -Uri "www.microsoft.com/unkownhost" -ErrorAction Stop
    # This will only execute if the Invoke-WebRequest is successful.
    $StatusCode = $Response.StatusCode
}
catch
{
    $StatusCode = $_.Exception.Response.StatusCode.value__
}
$StatusCode
```

```Output
404
```

The first command calls `Invoke-WebRequest` with an **ErrorAction** of **Stop**, which forces
`Invoke-WebRequest` to throw a terminating error on any failed requests. The terminating error is
caught by the `catch` block which retrieves the **StatusCode** from the **Exception** object.

## PARAMETERS

### -AllowUnencryptedAuthentication

Allows sending of credentials and secrets over unencrypted connections. By default, supplying **-Credential** or any **-Authentication** option with a **-Uri** that does not begin with `https://` will result in an error and the request will abort to prevent unintentionally communicating secrets in plain text over unencrypted connections. To override this behavior at your own risk, supply the **-AllowUnencryptedAuthentication** parameter.

> [!WARNING]
> Using this parameter is not secure and is not recommended. It is provided only
> for compatibility with legacy systems that cannot provide encrypted
> connections. Use at your own risk.

This feature was added in PowerShell 6.0.0.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Authentication

Specifies the explicit authentication type to use for the request. The default is **None**. **-Authentication** cannot be used with **-UseDefaultCredentials**.

Available Authentication Options:

- **None**: This is the default option when **-Authentication** is not supplied. No explicit authentication will be used.
- **Basic**: Requires **-Credential**. The credentials will be used to send an RFC 7617 Basic Authentication `Authorization: Basic` header in the format of `base64(user:password)`.
- **Bearer**: Requires **-Token**. Will send and RFC 6750 `Authorization: Bearer` header with the supplied token. This is an alias for **OAuth**
- **OAuth**: Requires **-Token**. Will send and RFC 6750 `Authorization: Bearer` header with the supplied token. This is an alias for **Bearer**

Supplying **-Authentication** will override any `Authorization` headers supplied to **-Headers** or included in **-WebSession**.

This feature was added in PowerShell 6.0.0.

```yaml
Type: WebAuthenticationType
Parameter Sets: (All)
Aliases:
Accepted values: None, Basic, Bearer, OAuth

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Body

Specifies the body of the request.
The body is the content of the request that follows the headers.
You can also pipe a body value to `Invoke-WebRequest`.

The **-Body** parameter can be used to specify a list of query parameters or specify the content of the response.

When the input is a GET request and the body is an `IDictionary` (typically, a hash table), the body is added to the URI as query parameters.
For other request types (such as POST), the body is set as the value of the request body in the standard name=value format.

The **-Body** parameter may also accept a `System.Net.Http.MultipartFormDataContent` object. This will facilitate `multipart/form-data` requests. When a `MultipartFormDataContent` object is supplied for **-Body**, any Content related headers supplied to the **-ContentType**, **-Headers**, or **-WebSession** parameters will be overridden by the Content headers of the `MultipartFormDataContent` object. This feature was added in PowerShell 6.0.0.

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -Certificate

Specifies the client certificate that is used for a secure web request.
Enter a variable that contains a certificate or a command or expression that gets the certificate.

To find a certificate, use `Get-PfxCertificate` or use the `Get-ChildItem` cmdlet in the Certificate (`Cert:`) drive.
If the certificate is not valid or does not have sufficient authority, the command fails.

```yaml
Type: X509Certificate
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -CertificateThumbprint

Specifies the digital public key certificate (X509) of a user account that has permission to send the request.
Enter the certificate thumbprint of the certificate.

Certificates are used in client certificate-based authentication.
They can be mapped only to local user accounts; they do not work with domain accounts.

To get a certificate thumbprint, use the `Get-Item` or `Get-ChildItem` command in the PowerShell `Cert:` drive.

> [!NOTE]
> This feature is currently only supported on Windows OS platforms.

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ContentType

Specifies the content type of the web request.

If this parameter is omitted and the request method is POST, `Invoke-WebRequest` sets the content type to `application/x-www-form-urlencoded`.
Otherwise, the content type is not specified in the call.

**-ContentType** will be overridden when a `MultipartFormDataContent` object is supplied for **-Body**.

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Credential

Specifies a user account that has permission to send the request.
The default is the current user.

Type a user name, such as "User01", "Domain01\User01", "User01@Domain.com", or enter a `PSCredential` object, such as one generated by the `Get-Credential` cmdlet.

**-Credential** can be used alone or in conjunction with certain **-Authentication** options. When used alone, it will only supply credentials to the remote server if the remote server
sends an authentication challenge request. When used with **-Authentication** options, the credentials will be explicitly sent.

```yaml
Type: PSCredential
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -CustomMethod

Specifies custom method used for the web request. This can be used with the Request Method required by the endpoint is not an available option on the **-Method**. **-Method** and **-CustomMethod** cannot be used together.

Example:



Invoke-WebRequest -uri 'https://api.contoso.com/widget/' -CustomMethod 'TEST'

This makes a `TEST` HTTP request to the API.

This feature was added in PowerShell 6.0.0.

```yaml
Type: String
Parameter Sets: CustomMethod, CustomMethodNoProxy
Aliases: CM

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -DisableKeepAlive

Indicates that the cmdlet sets the **KeepAlive** value in the HTTP header to False.
By default, **KeepAlive** is True.
**KeepAlive** establishes a persistent connection to the server to facilitate subsequent requests.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Form

Converts a dictionary to a `multipart/form-data` submission.
`-Form` may not be used with `-Body`.
If `-ContentType` will be ignored.

The keys of the dictionary will be used as the form field names.
By default, form values will be converted to string values.

If the value is a `System.IO.FileInfo` object,
then the binary file contents will be submitted.
The name of the file will be submitted as the `filename`.
The MIME will be set as `application/octet-stream`.
`Get-Item` can be used to simplify supplying the `System.IO.FileInfo` object.

```powershell
$Form = @{
    resume = Get-Item 'c:\Users\jdoe\Documents\John Doe.pdf'
}
```

If the value is a collection type,
such Arrays or Lists,
the for field will be submitted multiple times.
The values of the list will be treated as strings by default.
If the value is a `System.IO.FileInfo` object,
then the binary file contents will be submitted.
Nested collections are not supported.

```powershell
$Form = @{
    tags     = 'Vacation', 'Italy', '2017'
    pictures = Get-ChildItem 'c:\Users\jdoe\Pictures\2017-Italy\'
}
```

In the above example the `tags` field will be supplied 3 times in the form,
once for each of `Vacation`, `Italy`, and `2017`.
The `pictures` field will also be submitted once for each file in the `2017-Italy` folder.
The binary contents of the files in that folder will be submitted as the values.

This feature was added in PowerShell 6.1.0.

```yaml
Type: IDictionary
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Headers

Specifies the headers of the web request.
Enter a hash table or dictionary.

To set UserAgent headers, use the **-UserAgent** parameter.
You cannot use this parameter to specify `User-Agent` or cookie headers.

Content related headers, such as `Content-Type` will be overridden when a `MultipartFormDataContent` object is supplied for **-Body**.

```yaml
Type: IDictionary
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -InFile

Gets the content of the web request from a file.

Enter a path and file name.
If you omit the path, the default is the current location.

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -MaximumRedirection

Specifies how many times PowerShell redirects a connection to an alternate Uniform Resource Identifier (URI) before the connection fails.
The default value is 5.
A value of 0 (zero) prevents all redirection.

```yaml
Type: Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -MaximumRetryCount

Specifies how many times PowerShell retries a connection when a failure code between 400 and 599, inclusive or 304 is received.
Also see `-RetryIntervalSec` parameter for specifying number of retries.

```yaml
Type: Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Method

Specifies the method used for the web request.
The acceptable values for this parameter are:

- Default
- Delete
- Get
- Head
- Merge
- Options
- Patch
- Post
- Put
- Trace

The **-CustomMethod** parameter can be used for Request Methods not listed above.

```yaml
Type: WebRequestMethod
Parameter Sets: StandardMethod, StandardMethodNoProxy
Aliases:
Accepted values: Default, Get, Head, Post, Put, Delete, Trace, Options, Merge, Patch

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -NoProxy

Indicates that the cmdlet will not use a proxy to reach the destination.

When you need to bypass the proxy configured in the environment, use this switch.

This feature was added in PowerShell 6.0.0.

```yaml
Type: SwitchParameter
Parameter Sets: StandardMethodNoProxy, CustomMethodNoProxy
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -OutFile

Specifies the output file for which this cmdlet saves the response body.
Enter a path and file name.
If you omit the path, the default is the current location.

By default, `Invoke-WebRequest` returns the results to the pipeline.
To send the results to a file and to the pipeline, use the **-Passthru** parameter.

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PassThru

Indicates that the cmdlet returns the results, in addition to writing them to a file.
This parameter is valid only when the **-OutFile** parameter is also used in the command.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PreserveAuthorizationOnRedirect

Indicates the cmdlet should preserve the `Authorization` header, when present, across redirections.

By default, the cmdlet strips the `Authorization` header before redirecting. Specifying this parameter disables this logic for cases where the header needs to be sent to the redirection location.

This feature was added in PowerShell 6.0.0.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Proxy

Specifies a proxy server for the request, rather than connecting directly to the Internet resource.
Enter the URI of a network proxy server.

```yaml
Type: Uri
Parameter Sets: StandardMethod, CustomMethod
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ProxyCredential

Specifies a user account that has permission to use the proxy server that is specified by the **-Proxy** parameter.
The default is the current user.

Type a user name, such as "User01" or "Domain01\User01", "User@Domain.Com", or enter a `PSCredential` object, such as one generated by the `Get-Credential` cmdlet.

This parameter is valid only when the **-Proxy** parameter is also used in the command.
You cannot use the **-ProxyCredential** and **-ProxyUseDefaultCredentials** parameters in the same command.

```yaml
Type: PSCredential
Parameter Sets: StandardMethod, CustomMethod
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ProxyUseDefaultCredentials

Indicates that the cmdlet uses the credentials of the current user to access the proxy server that is specified by the **-Proxy** parameter.

This parameter is valid only when the **-Proxy** parameter is also used in the command.
You cannot use the **-ProxyCredential** and **-ProxyUseDefaultCredentials** parameters in the same command.

```yaml
Type: SwitchParameter
Parameter Sets: StandardMethod, CustomMethod
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Resume

Performs a best effort attempt to resume downloading a partial file.
`-Resume` requires `-OutFile`.

`-Resume` only operates on the size of the local file and remote file
and performs no other validation that the local file and the remote file are the same.

If the local file size is smaller than the remote file size,
then the cmdlet will attempt to resume downloading the file
and append the remaining bytes to the end of the file.

If the local file size is the same as the remote file size,
then no action is taken and the cmdlet assumes the download already complete.

If the local file size is larger than the remote file size,
then the local file will be overwritten and the entire remote file will be completely re-downloaded.
This behavior is the same as using `-OutFile` without `-Resume`.

If the remote server does not support download resuming,
then the local file will be overwritten and the entire remote file will be completely re-downloaded.
This behavior is the same as using `-OutFile` without `-Resume`.

If the local file does not exist,
then the local file will be created and the entire remote file will be completely downloaded.
This behavior is the same as using `-OutFile` without `-Resume`.

This feature was added in PowerShell 6.1.0.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -RetryIntervalSec

Specifies the interval between retries for the connection when a failure code between 400 and 599, inclusive or 304 is received.
Also see `-MaximumRetryCount` parameter for specifying number of retries.

```yaml
Type: Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SessionVariable

Specifies a variable for which this cmdlet creates a web request session and saves it in the value.
Enter a variable name without the dollar sign (`$`) symbol.

When you specify a session variable, `Invoke-WebRequest` creates a web request session object and assigns it to a variable with the specified name in your PowerShell session.
You can use the variable in your session as soon as the command completes.

Unlike a remote session, the web request session is not a persistent connection.
It is an object that contains information about the connection and the request, including cookies, credentials, the maximum redirection value, and the user agent string.
You can use it to share state and data among web requests.

To use the web request session in subsequent web requests, specify the session variable in the value of the **-WebSession** parameter.
PowerShell uses the data in the web request session object when establishing the new connection.
To override a value in the web request session, use a cmdlet parameter, such as **-UserAgent** or **-Credential**.
Parameter values take precedence over values in the web request session.

You cannot use the **-SessionVariable** and **-WebSession** parameters in the same command.

```yaml
Type: String
Parameter Sets: (All)
Aliases: SV

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SkipCertificateCheck

Skips certificate validation checks. This includes all validations such as expiration, revocation, trusted root authority, etc.

> [!WARNING]
> Using this parameter is not secure and is not recommended. This switch is only
> intended to be used against known hosts using a self-signed certificate for
> testing purposes. Use at your own risk.

This feature was added in PowerShell 6.0.0.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SkipHeaderValidation

Indicates the cmdlet should add headers to the request without validation.

This switch should be used for sites that require header values that do not conform to standards. Specifying this switch disables validation to allow the value to be passed unchecked.  When specified, all headers are added without validation.

This will disable validation for values passed to the **-ContentType**, **-Headers** and **-UserAgent** parameters.

This feature was added in PowerShell 6.0.0.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SslProtocol

Sets the SSL/TLS protocols that are permissible for the web request. By default all, SSL/TLS protocols supported by the system are allowed. **-SslProtocol** allows for limiting to specific protocols for compliance purposes.

**-SslProtocol** uses the `WebSslProtocol` Flag Enum. It is possible to supply more than one protocol using flag notation or combining multiple `WebSslProtocol` options with `-bor`, however supplying multiple protocols is not supported on all platforms.

> [!NOTE]
> On non-Windows platforms it may not be possible to supply `'Tls, Tls12'` as an option.

This feature was added in PowerShell 6.0.0.

```yaml
Type: WebSslProtocol
Parameter Sets: (All)
Aliases:
Accepted values: Default, Tls, Tls11, Tls12

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -TimeoutSec

Specifies how long the request can be pending before it times out.
Enter a value in seconds.
The default value, 0, specifies an indefinite time-out.

A Domain Name System (DNS) query can take up to 15 seconds to return or time out.
If your request contains a host name that requires resolution, and you set **-TimeoutSec** to a value greater than zero, but less than 15 seconds, it can take 15 seconds or more before a WebException is thrown, and your request times out.

```yaml
Type: Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Token

The OAuth or Bearer token to include in the request. **-Token** is required by certain **-Authentication** options. It cannot be used independently.

**-Token** takes a `SecureString` containing the token. To supply the token manually use the following:

`Invoke-WebRequest -Uri $uri -Authentication OAuth -Token (Read-Host -AsSecureString)`

```yaml
Type: SecureString
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -TransferEncoding

Specifies a value for the transfer-encoding HTTP response header.
The acceptable values for this parameter are:

- Chunked
- Compress
- Deflate
- GZip
- Identity

```yaml
Type: String
Parameter Sets: (All)
Aliases:
Accepted values: chunked, compress, deflate, gzip, identity

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Uri

Specifies the Uniform Resource Identifier (URI) of the Internet resource to which the web request is sent.
Enter a URI.
This parameter supports HTTP or HTTPS only.

This parameter is required.
The parameter name (**-Uri**) is optional.

```yaml
Type: Uri
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -UseBasicParsing

This parameter has been deprecated. Beginning with PowerShell 6.0.0, all Web requests use basic parsing only. This parameter is included for backwards compatibility only and any use of it will have no effect on the operation of the cmdlet.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -UseDefaultCredentials

Indicates that the cmdlet uses the credentials of the current user to send the web request. This cannot be used with **-Authentication** or **-Credential** and may not be supported on all platforms.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -UserAgent

Specifies a user agent string for the web request.

The default user agent is similar to `Mozilla/5.0 (Windows NT 10.0; Microsoft Windows 10.0.15063; en-US) PowerShell/6.0.0` with slight variations for each operating system and platform.

To test a website with the standard user agent string that is used by most Internet browsers, use the properties of the [PSUserAgent](/dotnet/api/microsoft.powershell.commands.psuseragent) class, such as Chrome, FireFox, InternetExplorer, Opera, and Safari.

For example, the following command uses the user agent string for Internet Explorer

```powershell
Invoke-WebRequest -Uri http://website.com/ -UserAgent ([Microsoft.PowerShell.Commands.PSUserAgent]::InternetExplorer)
```

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -WebSession

Specifies a web request session.
Enter the variable name, including the dollar sign (`$`).

To override a value in the web request session, use a cmdlet parameter, such as **-UserAgent** or **-Credential**.
Parameter values take precedence over values in the web request session. Content related headers, such as `Content-Type`, will be also be overridden when a `MultipartFormDataContent` object is supplied for **-Body**.

Unlike a remote session, the web request session is not a persistent connection.
It is an object that contains information about the connection and the request, including cookies, credentials, the maximum redirection value, and the user agent string.
You can use it to share state and data among web requests.

To create a web request session, enter a variable name (without a dollar sign) in the value of the **-SessionVariable** parameter of an `Invoke-WebRequest` command.
`Invoke-WebRequest` creates the session and saves it in the variable.
In subsequent commands, use the variable as the value of the **-WebSession** parameter.

You cannot use the **-SessionVariable** and **-WebSession** parameters in the same command.

```yaml
Type: WebRequestSession
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### CommonParameters

This cmdlet supports the common parameters: **-Debug**, **-ErrorAction**, **-ErrorVariable**, **-InformationAction**, **-InformationVariable**, **-OutVariable**, **-OutBuffer**, **-PipelineVariable**, **-Verbose**, **-WarningAction**, and **-WarningVariable**. For more information, see [about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216).

## INPUTS

### System.Object

You can pipe the body of a web request to `Invoke-WebRequest`.

## OUTPUTS

### Microsoft.PowerShell.Commands.BasicHtmlWebResponseObject

## NOTES

Beginning with PowerShell 6.0.0 `Invoke-WebRequest` supports basic parsing only.

## RELATED LINKS

[Invoke-RestMethod](Invoke-RestMethod.md)

[ConvertFrom-Json](ConvertFrom-Json.md)

[ConvertTo-Json](ConvertTo-Json.md)