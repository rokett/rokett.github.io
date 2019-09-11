+++
date = "2016-07-20T22:13:34+01:00"
title = "Creating PowerShell modules"

+++

What is the best way to structure a PowerShell module?  Realistically there is no such thing, however I have a structure which works for me; it provides a standard which is followed within my organisation and covers any modules I create.  Standard practices are good...doing things differently each and every time makes understanding how something is working very difficult.

So what's the structure?

![](/images/PowerShell-Module-Structure.png)

Let's look at that structure a bit more.

#### Root folder
The name of the root folder is the name of the module.  Module naming?  Whatever you like really as there aren't any standards.  Generally it's helpful to name it something sensible and reflective of its purpose.

#### Private folder
Any supporting functions or files that do not form part of your public API go in here.  Things like configuration files (I like to use YAML but how you do that is up to you) or functions which are used internally but never externally.

#### Public folder
This is where all of the public functions go.  The naming of these functions should follow PowerShell naming standards and use one of the [approved verbs](https://msdn.microsoft.com/en-us/library/ms714428(v=vs.85).aspx).

#### Tests folder
Module tests are here.  By default I always have a test file named after the module itself which validates the module manifest.  You should, and I'm not always great at it, add additional tests for each function you create.  This will help you in the long run, but there is a small learning curve.

Tests are run by running [Pester](https://github.com/pester/Pester) whilst in the module root folder.

```powershell
Invoke-Pester
```

#### Module-Name.psd1
This is the manifest file.  It's created automatically when I run a script to create a new module. You will need to edit this file though in order to expose your public functions (there are also many other reasons you may want to edit it, but they are outside the scope of this article).

```powershell
# Functions to export from this module
FunctionsToExport = @(
    'Public-Function1'
    'Public-Function2'
)
```

This will expose `Public-Function1` and `Public-Function2` once the module has been imported.

#### Module-Name.psm1
This is the module loader file.  When the module is imported, it will ensure that all of the scripts in the Public folder are available to the user.

### Scripting the creation of a new module
My [New-PSModule.ps1](https://github.com/rokett/Powershell-Module-Template) script is used to create a new module; it creates the folder structure described above.

There is an expectation that the manifest and loader files are in a folder called `_Templates` in whatever path you intend to use to create the new module in.  For example if you are creating a new module in `C:\Program Files\WindowsPowerShell\Modules` there should be a folder in there called `_Templates` which contains the two files.  Grab these from my [GitHub repo](https://github.com/rokett/Powershell-Module-Template).