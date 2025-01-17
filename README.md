CommanderProFix
==============

[![Build Status](https://github.com/dreamwhite/CommanderProFix/workflows/CI/badge.svg?branch=master)](https://github.com/dreamwhite/CommanderProFix/actions)

This is a forked version of the [Lilu](https://github.com/acidanthera/Lilu) plugin [RestrictEvents](https://github.com/acidanthera/RestrictEvents) that has been modified to only prevents `/usr/libexec/ioupsd` from running. This removes the "UPS battery backup" warning at the cost of disabling macOS's UPS support.

# Hey there's already a version of CommanderProFix!1!1!

Please note that this project isn't an idea of mine: [acleee](https://github.com/acleee) actually developed [CommanderProFix](https://github.com/acleee/CommanderProFix).

I simply decided to recreate it using latest [RestrictEvents](https://github.com/acidanthera/RestrictEvents), as well as smoothing it, since at the time of writing his project is more than 50 commits behind the original kext. You can check it [here](https://github.com/acleee/CommanderProFix/compare/master...acidanthera:RestrictEvents:master).

With this I do not wanna discredit anyone but rather point out that there can be issues with using his version, as it cannot support, macOS Ventura.

# Anyways, let's give a background

There exists a USB UPS that is called the Commander Pro. When plugged into an internal USB header the Corsair Commander Pro also appears with the name "Commander Pro". macOS (lazily) checks the USB name instead of the vendor/product IDs and believes there is a UPS plugged in when it detects the Corsair Commander Pro.

When you have a Corsair Commander Pro plugged in without any mapping/disabling, you're greeted with this wonderful message every time you boot into macOS:

![stupid annoying error](https://i.imgur.com/SndZNbA.png)

I got the idea for this after seeing [this thread](https://www.tonymacx86.com/threads/solved-catalina-thinks-my-corsair-commander-pro-is-a-ups.288458/) on tonymacx86, which showed how the fix shown (which basically overwrote `ioupsd` to sleep forever) did not work on Big Sur.

In my case, I have a custom water cooled PC, with my fans and a temperature probe running thru the Commander Pro. I wanted to be able to monitor the fan RPM and my coolant temperature, to verify the fan curves stayed applied in macOS but also because I like having those temps readily accessible so I can see what the computer is "doing". CommanderProFix _does_ work on Big Sur, and as of yet nobody has yelled at me telling me this is a horrible idea or horrible way to fix this ¯\\\_(ツ)_/¯

# Perks of CommanderProFix

- No need to disable the Commander Pro in your USB map, which means: 
  - You can still fully access USB headers connected to the Commander Pro
  - [liquidctl](https://github.com/liquidctl/liquidctl) remains usable for pulling fan and temperature sensor info / RGB control
  - [OpenRGB](https://gitlab.com/CalcProgrammer1/OpenRGB) might work (will need to test - my ASUS Aura device showed on the same USB port as the Commander Pro)
  - macOS no longer harrasses you about your `UPS` battery being dead

# Cons
- If you have a USB UPS, you probably can't poll it for data and have it integrate into macOS anymore.

# Boot arguments

The boot arguments have been modified from the original ones (`-rev*`) to make sure that users who use RestrictEvents (mainly AMD users with MP7,1 SMBIOS) and Corsair Commander Pro can disable only CommanderProFix and not also RestrictEvents 

- `-cpfoff` (or `-liluoff`) to disable
- `-cpfdbg` (or `-liludbgall`) to enable verbose logging (in DEBUG builds)
- `-cpfbeta` (or `-lilubetaall`) to enable on macOS older than 10.8 or newer than 11
- `-cpfproc` to enable verbose process logging (in DEBUG builds)

# Credits
- [Acidanthera](https://github.com/vit9696) for RestrictEvents  
- [Apple](https://www.apple.com) for macOS  
- [vit9696](https://github.com/vit9696) for [Lilu.kext](https://github.com/vit9696/Lilu) and great help in implementing some features 
[acleee](https://github.com/acleee) for [CommanderProFix.kext](https://github.com/acleee/CommanderProFix)