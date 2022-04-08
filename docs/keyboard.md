# Keyboard Remapping

A bit of tinkering is necessary to get the Vortex Race 3 working nicely on macOS and Windows. Unfortunately,
in the process of soldering on new switches, I broke the `=` key... so that has to be fixed with remapping
too. `F11` is the closest key and is used less than `F12`, so `F11` will be remapped to `=`.

## macOS

Initially, the ``` ` ``` and `§` keys were switched with the Vortex Race 3. I had to go to **System Preferences > Keyboard > Change Keyboard Type...** and re-run the keyboard assistant. Then had to go to **Input Sources**, add the "US International - PC" layout, press the key next to shift, and remove the layout. It seems to be a bug in macOS as I had to do the same process on my MacBook.

Used [Karabiner Elements](https://karabiner-elements.pqrs.org/) to remap `F11` to `=`. The Vortex Race
3 is set to macOS mode so most of the keys are mapped correctly by default. The `Command` and `Alt` keys
are swapped using macOS's System Preferences Application.

## Windows

Went to language preferences, pressed options on the `English (United Kingdom)` language, added a `US QWERTY`
keyboard, and removed the default `United Kingdom QWERTY` keyboard. This swapped `@` with `"` and fixed the `|`
key. To remap `F11` to `=` and fix the `Win` and `Alt` keys, [AutoHotkey](https://www.autohotkey.com/) was
used. The following script was placed in the Windows startup folder:

```
F11::=
LWin::LAlt
LAlt::LWin
RAlt::RWin
return
```
