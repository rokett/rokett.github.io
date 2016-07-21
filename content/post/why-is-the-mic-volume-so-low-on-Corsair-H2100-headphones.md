+++
date = "2016-07-20T21:33:35+01:00"
title = "Why is the mic volume so low on Corsair H2100 headphones?"

+++

My new Corsair H2100 headset is amazing.  It's wireless and the sound is so immersive; so far above my previous cheap pair.   The only problem is that the microphone volume is really low.

It doesn't seem to matter how you have the headphones configured in the Corsair control panel, there is no way to boost the mic volume...or is there?

It's pretty simple really, I just needed to install an application called [Equalizer APO](http://sourceforge.net/projects/equalizerapo/). Once this application is installed, delete the contents of the *C:\Program Files\EqualizerAPO\config\config.txt* file and replace it with the following:

```
Device: Headset Microphone
Preamp: +10db
```

+10db worked for me but YMMV.

Save the file and the changes happen instantly.  Tweak to your hearts content.

Credit where credit is due, this fix comes courtesy of http://forum.corsair.com/forums/showthread.php?t=132227.