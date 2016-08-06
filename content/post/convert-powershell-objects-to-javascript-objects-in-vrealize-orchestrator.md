+++
date = "2016-07-20T22:13:03+01:00"
title = "Convert powershell objects to javascript objects in vRealize Orchestrator"

+++

Question: What do PowerShell and vRealize Orchestrator have in common?

Answer: They're both amazing tools for automation and orchestration.

PowerShell is fantastic for scripting tasks on Microsoft based products and it's moving into the Configuration Management world with DSC; although the tooling leaves a lot to be desired at the moment.

vRO is a gem of a product, given away for free with vCenter, from VMware; I know...who would have thought VMware would give something so great away for free.  The documentation is pretty woeful and some of the inbuilt workflows are suspect in terms of their functionality, but with it you can orchestrate pretty much anything.

You'd think that the combination of PowerShell and vRO would be a match made in heaven.  Right?  Yeah not quite.

vRO has prebuilt workflows for working with PowerShell, and they do work once you have your PowerShell host setup properly, but dealing with the object returned from a PowerShell script isn't that simple; vRO uses Javascript and therefore doesn't natively understand PowerShell objects.

There are ways around this...vRO has methods for dealing with PowerShell objects, but by and large it's much easier to deal with Javascript objects in vRO.  So the following handy dandy action will convert a PowerShell object into a Javascript object allowing you to work with it easily in vRO.

#### Create a new action
I'm going to assume you already know how to create an action, but for the sake of this article let's call it `convertPSObjectToArray`.

#### Set the input parameters
We need two parameters.

The first is called `PSObject` and is of type `Powershell:PowershellRemotePSObject`.  This is the PowerShell object you want to convert.

The second is called `properties` and is of type `Array`.  This is an array of properties that the PowerShell object has which you wish to be present in the final Javascript object.

#### Code the action
The following code block is the meat of the action.  It takes in the PowerShell object, loops over it and assigns each property of each item to a JS object which is then pushed to the output array.  Hey presto, you have an array of JS objects.

```javascript
var root = PSObject.getRootObject();

var output = [];

for each (item in root) {
	var temp = {};

	for each (property in properties) {
		temp[property] = item.getProperty(property);
	}

	output.push(temp);
}

return output;
```

#### Use the action in a workflow
Now that the action is created, you can use it in a workflow.  Let's assume that we are retrieving a list of user accounts from AD using PowerShell.  From the returning object you want to get the username and email address for later processing.  Here's the full code.

```javascript
var properties = [
    "SamAccountName",
    "Mail",
];

users = System.getModule("xxx.library.powershell").convertPSObjectToArray(outPSObject, properties);
```

What we've just done is create an array containing the names of the object properties you want to retrieve.  The returned Javascript object will have properties named the same.

We're then passing the PowerShell object and the array of properties to the action, which will do the business and spit out an array of objects to the `users` array.

The users array looks something like the below:

```javascript
users = [
    {
        "SamAccountName": "username1",
        "Mail": "user1@something.com"
    },
    {
        "SamAccountName": "username2",
        "Mail": "user2@somethihng.com"
    }
]
```

And we're done....making PowerShell and vRO happier together one step at a time!