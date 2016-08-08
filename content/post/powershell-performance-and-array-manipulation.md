+++
date = "2016-08-06T23:24:36+01:00"
title = "PowerShell performance and array manipulation"

+++

Let's talk about array manipulation in PowerShell; specifically adding to an array within a loop.

You're probably used to initialising the array like so; `$arr = @()` and then adding new entries to it from within a loop by `$arr += $newEntry`.  That works but holy hell is it slow!  Not only is it slow but it eats CPU and memory resources.  Why?  Well I'm no expert on the inner workings of PowerShell but it's because arrays are not able to be changed in PowerShell.  So what happens is a new array has to be created in memory, the contents of the old array and the new entry added to it, and then the old array destroyed, before the new array becomes the old array.  Ummmmm..ok.

How bad is it?

```powershell
Measure-Command {
    $arr = @()
    $1 = (1..50000)
    ForEach ($num in $1) {
        $arr += $num
    }
}
```
That takes 94 seconds to complete and uses around 40% of the CPU resources on my machine (quad core i5) and fluctuates between 90MB and 110MB of RAM.  The fluctuation is where goes through the process of using the temporary array before destroying the old one and so on.

Is there a better way?  Of course.  You need to delve into a tiny bit of .Net for this.

```powershell
Measure-Command {
    $arr = New-Object System.Collections.ArrayList
    $1 = (1..50000)
    ForEach ($num in $1) {
        [void]$arr.Add($num)
    }
}
```
First we create a new `ArrayList`.  This is a .Net construct and crucially we are allowed to change the size on the fly.  Then we use `[void]$arr.Add($var)` to add entries.  Personally I much prefer the `.Add` method to using `+=`; it just looks better and is more intuitive.

Why the `[void]`?  Glad you asked...when you add entries to an `ArrayList` it will output out the row count every time.  That's just annoying and messes up your output, so casting it to `[void]` means that nothing is output.  There are other options, but that's a different article.

OK so the important bit, how much better is it?

109 milliseconds, 1% CPU and 60MB RAM utilisation!  Yes you did read that right...**109 ms!**  That's a 99.8% improvement.

I'll leave that with you to sink in, but suffice to say just use the `ArrayList` option from now on.