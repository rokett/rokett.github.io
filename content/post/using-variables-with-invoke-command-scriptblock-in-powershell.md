+++
date = "2016-07-26T21:25:07+01:00"
title = "Using variables with Invoke-Command script block in PowerShell"

+++

The `Invoke-Command` PowerShell cmdlet is pretty handy.  It allows you to run commands from your local system as if they were being run on a remote system.  Here's a simple example:

```powershell
Get-Service -DisplayName 'Task Scheduler'
```

This retrieves the Task Scheduler service.  It runs locally on whichever computer you used the command from.  But what if you wanted to get the Task Scheduler service on a remote computer?

Sure you could do this (which would run the command against the computer called `server01`):

```powershell
Get-Service -DisplayName 'Task Scheduler' -ComputerName 'server01'
```

But you could also do it like this using `Invoke-Command`:

```powershell
Invoke-Command -ComputerName 'server01' -ScriptBlock {
    Get-Service -DisplayName 'Task Scheduler'
}
```

It effectively does the same thing as the previous example, but it runs the command locally on `server01` using `WinRM`.  Generally speaking `WinRM` requires less setup than ensuring a remote call works.  It is enabled by default on Windows 2012 R2 and above, and is simple to enable on other Windows versions assuming they are running at least PowerShell 2.

#### Getting to the point

So this article is about passing variables into the `Invoke-Command` script block.  Can't you just use variables directly?  Nope...any variables defined outside of the script block do not exist in the scope of the script block itself.  You therefore need to pass them in somehow.

We're going to focus on the method of doing this from PowerShell 3 upwards.  There are other methods for PowerShell 2 and below, but those versions are old now and we need to move on.  So how does it work with PowerShell 3?

Taking our simple example, what if this was a function and we wanted to allow the user to specify the service name?  We'd probably want them to pass it in as a parameter, in which case how do we pass it to the script block?  This is how:

```powershell
Function Get-RemoteServiceDetails() {
    [CmdletBinding()]
    Param(
        [Parameter(Mandatory=$True)]
        [string]$ServiceDisplayName
    }

    Invoke-Command -ComputerName 'server01' -ScriptBlock {
        Get-Service -DisplayName $Using:ServiceDisplayName
    }
}
```

The magic there is the `$Using:` command.  This tells the script block that we want to use the `$ServiceDisplayName` parameter from the parent scope.

And that's all there is to it!