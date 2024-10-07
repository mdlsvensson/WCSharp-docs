System is a big word, unlike this "system".

This system exists due to an unfortunate quirk in Reforged. Many effects do not display properly if you immediately destroy them, as you would before Reforged.  
In order to address this, you can use `EffectSystem.Add(effect, duration)` to make the effect automatically be cleaned up after the given duration (in seconds).  
You may leave out the duration, in which case it defaults to 0.03125 seconds. For the vast majority of effects, they will work fine with the default duration.
