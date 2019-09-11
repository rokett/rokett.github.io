+++
date = "2019-09-11T21:14:32Z"
title = "Access the internet with PowerShell behind NTLM proxy"

+++

When you're behind a corporate proxy server which uses NTLM, you will probably find that PowerShell is unable to connect to the internet.  Sometimes this will just silently fail, which sucks, and other times you'll receive an error.

In order to prove there is a problem, try the following request.

```powershell
Invoke-WebRequest -Uri https://www.google.com/
```

The problem happens because an NTLM proxy server expects that user credentials will be passed for authentication purposes.  You can fix this by adding the following code to your script, or in your session.  The configuration will last for the duration of the session.

```powershell
$proxyUri = New-Object System.Uri(([System.Net.WebProxy]::GetDefaultProxy()).Address.AbsoluteUri)
[System.Net.WebRequest]::DefaultWebProxy = New-Object System.Net.WebProxy($proxyUri, $true)
[System.Net.WebRequest]::DefaultWebProxy.Credentials = [System.Net.CredentialCache]::DefaultCredentials
```

I would recommend adding this to your PowerShell profile for ease of use as well.
