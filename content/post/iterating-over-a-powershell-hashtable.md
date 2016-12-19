+++
date = "2016-12-19T22:23:46Z"
title = "Iterating over a PowerShell hashtable"

+++

Iterating over a PowerShell hashtable....it's not as easy as a simple ForEach loop; a hashtable isn't like an array.

So how do you iterate over a hashtable?  It's actually really easy as there is a method on the hashtable object called `GetEnumerator()`.  Iterating a hashtable using the `GetEnumerator()` method sends each key/value pair to the pipeline.

```powershell
$hashtable = @{
    'key1' = 'value1'
    'key2' = 'value2'
    'key3' = 'value3'
}

$hashtable.GetEnumerator() | ForEach-Object {
    Write-Host "$_.Key : $_.Value"
}
```

or if you prefer a ForEach loop instead of the pipeline (see https://rokett.github.io/post/powershell-performance-and-foreach-loops/)

```powershell
$hashtable = @{
    'key1' = 'value1'
    'key2' = 'value2'
    'key3' = 'value3'
}

$hashtable2 = $hashtable.GetEnumerator()

ForEach ($item in $hashtable2) {
    Write-Host "$($item.Key) : $($item.Value)"
}
```

The above code will output the following:

```powershell
key3 : value3
key1 : value1
key2 : value2
```

Or something similar; iterating over a hashtable does not necessarily do it in the same order that it was created.