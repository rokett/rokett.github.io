---
title: "Accessing Websites Requiring TLS1.1 and above with PowerShell"
date: 2019-10-27T22:43:35+01:00

---

PowerShell, or at least Windows PowerShell, is somewhat behind the times, because it is based on .Net 4.x, in terms of supporting newer encryption protocols when access secure websites.  By default .Net 4.x only supports up to TLS 1.1, but PowerShell still seems to be locked to TLS 1.0.

So when attempting to either `Invoke-WebRequest` or `Invoke-RestMethod` you may receive an error message back saying `The request was aborted: Could not create SSL/TLS secure channel`.  This is likely because the website you are trying to send a secure request to requires you to use a protocol that your local environment is not enabled to use.

In order to see what protocol(s) are currently configured in your session, run the following command.

```powershell
[System.Net.ServicePointManager]::SecurityProtocol

# Output would be something like the below
Ssl3, Tls
```

This indicates that your PowerShell session is currently configured to use either SSL3 or TLS 1.0.  If the website you're trying to access does not support either of those protocols, you'll receive the error above.

#### What is wrong with SSL3 and TLS 1.0?
They're both pretty broken security protocols and were deprecated in 2015 and 2018 respectively.  In other words they really shouldn't be used unless you have to.

Of course you may find you're dealing with an application which is still using them, so other than sending a polite email asking the maintainers to update their security, you may need to allow PowerShell to use them.

#### How do I use something more secure?
You can do the following either in your session or at the top of whatever script you're writing.  If you are writing a script I'd recommend putting it in there so that you can be sure it can run on any host (assuming the host has support for the TLS versions you are enabling).  You can also put this into your profile.

```powershell
# Enable TLS 1.2 and TLS 1.1 as Security Protocols
[System.Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12, [Net.SecurityProtocolType]::Tls11

# Check which protocols are enabled
[System.Net.ServicePointManager]::SecurityProtocol

# Output would be something like the below
Tls11, Tls12
```

#### What about something even more secure?
TLS 1.3 is the latest and greatest but .Net, and therefore Windows, does not yet support it so it can't currently be used by PowerShell; maybe one day.....

As and when it is supported, I would assume you'd be able to do:

```powershell
# Enable TLS 1.2 and TLS 1.1 as Security Protocols
[System.Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls13, [Net.SecurityProtocolType]::Tls12, [Net.SecurityProtocolType]::Tls11

# Check which protocols are enabled
[System.Net.ServicePointManager]::SecurityProtocol

# Output would be something like the below
Tls11, Tls12, Tls13
```
