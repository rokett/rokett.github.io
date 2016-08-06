+++
date = "2016-08-06T23:05:51+01:00"
title = "Powershell performance and ForEach loops"

+++

PowerShell is a powerful beast; the pipeline is crazy clever in terms of acting on many objects at once with a single command, but unless you're really trying to do a one-liner there are other faster ways of doing things.

Take the humble ForEach loop for example.  The pipeline way of doing things is actually the slowest, whilst the more traditional programming way of doing things is much faster.  How much faster?  Let's take a looksee.

#### ForEach-Object
```powershell
Measure-Command {
    (1..5000) | ForEach-Object {
        (1..100) | ForEach-Object {
            $_ * $_
        }
    }
}
```
This loop takes 11.6 seconds to complete.

What about another option..How about working directly on the array.

#### arr.ForEach
```powershell
Measure-Command {
    (1..5000).ForEach{
        (1..100).ForEach{
            $_ * $_
        }
    }
}
```
This one takes 3.5 seconds to complete.  That's a 69% increase in speed over the `ForEach-Object` option.

Funny fact: the lack of a space after the `ForEach` command is not a mistake.  PowerShell throws up an error if there is a space.  Go figure!

Lastly the final, and quickest, option.

#### ForEach ($var in $arr)
```powershell
Measure-Command {
    $1 = (1..5000)
    ForEach ($num1 in $1) {
        $2 = (1..100)
        ForEach ($num2 in $2) {
            $num1 * $num2
        }
    }
}
```
834 ms!  That's a 92% increase in speed over the `ForEach-Object` option and a 76% increase in speed over the `arr.ForEach` option.

It may not be as pretty as the other options, and yes it's more verbose, but to be honest I prefer that in a script (the verbosity not the lack of prettiness).

If you're not fussed about speed and you're not doing many iterations maybe it doesn't matter that you use a slower option for the prettiness factor. On the other hand if you need to squeeze some speed out, definitely do it the more verbose way.