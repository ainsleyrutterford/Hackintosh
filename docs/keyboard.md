# Keyboard Remapping

A bit of tinkering is necessary to get the Vortex Race 3 working nicely on macOS and Windows. Unfortunately,
in the process of soldering on new switches, I broke the `=` key... so that has to be fixed with remapping
too. `F11` is the closest key and is used less than `F12`, so `F11` will be remapped to `=`.

## macOS

Initially, the ``` ` ``` and `§` keys were switched with the Vortex Race 3. I had to go to **System Preferences > Keyboard > Change Keyboard Type...** and re-run the keyboard assistant. Then had to go to **Input Sources**, add the "US International - PC" layout, press the key next to shift, and remove the layout. It seems to be a bug in macOS as I had to do the same process on my MacBook.

When plugging in a Razer mouse, it messed up the macOS keyboard preferences and I could no longer change the keyboard type for the Vortex Race 3, only the Razer mouse. To fix this I unplugged the Razer mouse, typed the following into the terminal, and restarted:

```
sudo rm /Library/Preferences/com.apple.keyboardtype.plist
```

After restarting, the keyboard setup assistant ran again and I could setup the Vortex Race 3 properly. The Razer mouse could then be plugged back in.

Used [Karabiner Elements](https://karabiner-elements.pqrs.org/) to remap `F11` to `=`. The Vortex Race
3 is set to macOS mode so most of the keys are mapped correctly by default. The `Command` and `Alt` keys
are swapped using macOS's System Preferences Application.

## macOS Sonoma

When I got a new work laptop with Sonoma installed, the above steps would no longer work for me (I also tried [these steps](https://gist.github.com/ainsleyrutterford/1860376d26256e28f310b86265b1aa51) which involved editing the `plist` directly and they didn't work for me). In the end I had to unplug my security key and Razer mouse (basically any USB that seems to trigger the Keyboard Setup Assistant), and re-run the assistant. After this, the only thing I could get working is to install [Krabiner Elements](https://karabiner-elements.pqrs.org/). It then re-ran the assistant again, and now the Vortex 3 (which now has the [`bear_face` board](https://github.com/qmk/qmk_firmware/tree/c035a37249ed064edbd51379bb45057e88e7ef11/keyboards/bear_face) by [`chemicalwill`](https://www.reddit.com/r/mechmarket/comments/p6cxfj/bulk_bear_face_v21_pcbs_qmk_replacement_for/)) has the same layout as the native keyboard. All I had to do then was remap `left_command` to `left_option` (and the same for the right keys) and I was good to go.

**Note:** this has changed my native keyboard so that I have to press `option + 3` for `£` rather than `shift + 3`, but I've spent so long on this at this point that I don't really care, and `shift + 3` is nicer than `option + 3` anyway.

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
