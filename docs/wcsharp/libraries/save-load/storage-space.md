# Save/Load Storage Space

**Use of this package requires that the compilers IsExportMetadata property is set to true.**

In a nutshell, the save/load system uses some curious WarCraft III interactions to store a file on disk, and in the next game we can set tooltips to previously defined values using this file. This is a bit of a convoluted approach, but it's the best we have right now.

The use of ability tooltips raises an issue though, since it means we need to have a sufficient number of them to be able to load all the data. By default, WCSharp defines 10 of them, which allows us to save a JSON of around 1500 characters before we run out of space. This will be more than enough for most use cases, but in case you exceed 50% of that, the system will give out a warning. Furthermore, exceeding the limit will cause the system to refuse to save entirely.

Raising this limit is not hard, however. By default, the following IDs are defined:

```
1097690227, // Amls
1097359726, // Ahan
1098018659, // Aroc
1097689443, // Amic
1097689452, // Amil
1097034854, // Aclf
1097035111, // Acmg
1097098598, // Adef
1097099635, // Adis
1097228916, // Afbt
```

You can extend this list by calling `SaveSystem<MySaveable>.AddAbilityId(<integer>);`. You can use [this page](https://ubershmekel.github.io/fourcc-to-text/) to get the integer values of ability codes, or alternatively just use the built-in `FourCC` method.

The default list is a number of Human abilities, and for everyone's convenience I've included the remaining Human abilities below. Simply replace `<MySaveable>` with whatever Saveable implementation you are using. If you need even more, I recommend starting from Orc abilities and continuing in the same manner.

**Note: Only use abilities which have a tooltip! Abilities such as "Berserker Upgrade" (Sbsk) will NOT work!** WCSharp.SaveLoad 2.0.2 and later will attempt to detect when this occurs, but this detection will likely not work on non-English clients.

```
SaveSystem<MySaveable>.AddAbilityId(1097228907); // Afbk
SaveSystem<MySaveable>.AddAbilityId(1097231467); // Aflk
SaveSystem<MySaveable>.AddAbilityId(1097231457); // Afla
SaveSystem<MySaveable>.AddAbilityId(1097300322); // Agyb
SaveSystem<MySaveable>.AddAbilityId(1097233256); // Afsh
SaveSystem<MySaveable>.AddAbilityId(1097360737); // Ahea
SaveSystem<MySaveable>.AddAbilityId(1097362536); // Ahlh
SaveSystem<MySaveable>.AddAbilityId(1097428582); // Ainf
SaveSystem<MySaveable>.AddAbilityId(1097430643); // Aivs
SaveSystem<MySaveable>.AddAbilityId(1097364073); // Ahri
SaveSystem<MySaveable>.AddAbilityId(1097688166); // Amdf
SaveSystem<MySaveable>.AddAbilityId(1097102451); // Adts
SaveSystem<MySaveable>.AddAbilityId(1097363557); // Ahpe
SaveSystem<MySaveable>.AddAbilityId(1097889894); // Apxf
SaveSystem<MySaveable>.AddAbilityId(1097886841); // Aply
SaveSystem<MySaveable>.AddAbilityId(1097364080); // Ahrp
SaveSystem<MySaveable>.AddAbilityId(1095267425); // AHta
SaveSystem<MySaveable>.AddAbilityId(1098083439); // Aslo
SaveSystem<MySaveable>.AddAbilityId(1098084467); // Asps
SaveSystem<MySaveable>.AddAbilityId(1098085480); // Asth
SaveSystem<MySaveable>.AddAbilityId(1097364322); // Ahsb
SaveSystem<MySaveable>.AddAbilityId(1097300342); // Agyv
```