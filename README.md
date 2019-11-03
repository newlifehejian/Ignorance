Ignorance 1.3
=============
[![Ko-Fi](https://img.shields.io/badge/Donate-Ko--Fi-red)](https://ko-fi.com/coburn) 
[![PayPal](https://img.shields.io/badge/Donate-PayPal-blue)](https://paypal.me/coburn64) 
[![GitHub issues](https://img.shields.io/github/issues/SoftwareGuy/Ignorance.svg)](https://github.com/SoftwareGuy/Ignorance/issues)
![GitHub last commit](https://img.shields.io/github/last-commit/SoftwareGuy/Ignorance.svg) ![MIT Licensed](https://img.shields.io/badge/license-MIT-green.svg)

*If you feel so inclined, I'd appreciate* [a coffee to keep me
caffeinated](https://ko-fi.com/coburn) *if you use this transport.*

Welcome to Ignorance, a plug-in Transport system for [Mirror
Networking](https://github.com/vis2k/Mirror) that allows you to harness reliable
UDP communications over the vast seas that is the internet. Harnessing the ENET
Backend, it provides reliable and unreliable UDP packet delivery with up to 255
individual channels and 4096 theortical maximum clients connected at any time.

Let's face the music: Unity's Low-level API networking sucks. Ignorance aims to
do what LLAPI did... and a little bit more.

Please read the FAQ (towards the bottom of this wall of text) before using the
Ignorance Transport.

Compatiblity
------------

### Desktop

-   Windows 7 x64 onwards, **however...**

    -   32bit Windows ENET Blobs are included, however they should be treated as
        experimental. I am doing this because I get quite a lot of people in
        countries where 32bit computers are more common than 64bit computers
        (Russia, Brazil, etc).

    -   If you have found a quirk in the 32bit blobs, you will be required to
        build a 64bit version of your game and test to see if the quirk exists.
        If it does, then you qualify for a bug report.

    -   If the quirk exists in 32bit blobs but does not exist in 64bit blobs,
        then you can file a bug report but it’ll be lower priority.

-   macOS 10.12 onwards. El Captain and below **ARE NOT** supported.

    -   Tested and confirmed working on High Sierra and Mojave. Catalina has not
        been tested due to bugs and/or version immaturity.

-   Linux

    -   x86_64 libraries included. Compiled on a Ubuntu 18.04 LTS instance
        (Bionic Beaver).

    -   Should *just work* on various Linux distros, as long as they aren’t too
        exotic. Main testing has been done on Debian/Ubuntu and CentOS.

    -   If your distro uses a different C library instead of GNU C then you’ll
        need to recompile the blobs manually. See the ENet-CSharp repository for
        more information.

### Console

-   Nintendo Switch: **Likely possible, however you’ll need to manually compile
    it.** [See this document from the ENet-CSharp
    repository.](https://github.com/SoftwareGuy/ENet-CSharp/blob/master/BUILD-FOR-SWITCH.txt)

-   Xbox One

    -   UWP: Unlikely, unless UWP applications can access C-language plugins.

    -   Native: Possibly, however I don’t have development kit or license to
        test.

-   PlayStation

    -   Vita: Possibly, however I’d need access to DevNet to build against their
        toolchain. And I don’t have said access.

    -   PS4: Falls into the same boat as Vita (above)

    -   Will probably require some patches in ENET to commodate the OS
        differences that Vita/PS4 have (they are \*BSD based)

### Mobile

-   Android 4.4 "KitKat" onwards.

    -   ARMv7, ARM64 and x86 Blobs are included.

    -   x86_64 Blobs are not included since Unity 2018.4 LTS does not support
        that platform yet.

-   Apple iOS

    -   System version 8.0 minimum

    -   Supports iPhone 4S to the latest iPhone. Tested and working on iPhone
        4S, iPhone 5s, iPad 2 WiFi + 3G and iPad (5th Gen).

Dependencies
------------

-   [Mirror Networking](https://github.com/vis2k/Mirror)

-   [ENet-CSharp](https://github.com/SoftwareGuy/ENet-CSharp) (custom fork, NOT
    compatible with upstream)

-   ENET Native C Library Blobs (included)

Mirror Compatibility
--------------------

Please use the Mirror Asset Store releases. The master branch of Mirror can also
be used but please beware that it's bleeding edge and massive breakage can be
expected on a bad day.

###  

When using Ignorance inside the MacOS environment, it will run in a
compatibility mode to ensure things work correctly. This is due to some Unity
Editor thread managed-to-native issues that were fixed in 2018.3+. Windows and
Linux do not suffer this problem.

Installation
------------

Download the Unity Package from Releases that is the latest one. Simply import
the Unity Package and Unity will do the rest. Follow the instructions below.

How to use
----------

As of Ignorance 1.3, I have included a pre-configured sample scene so you can
get started easily.

**Hard mode:**

If you haven't already, make a empty GameObject with the Mirror **Network
Manager**. It will automatically add Telepathy as the default transport. Add a
**new component** and look for **Ignorance**. You will have Ignorance added as a
transport, but it will not be activated. Now **drag the Ignorance script** into
the **Transport** field of the **Network Manager inspector**. If all goes well,
you should see **GameObjectName (Ignorance)** in that field.

FAQ (aka Please Read This First)
--------------------------------

### Why should I use Ignorance over Unity LLAPI?

Unity LLAPI is old, obsolete and no longer mantained by Unity Technologies. It
is currently on heavy life-support until they get their act together with the
new Unity Multiplay system. Plus, it was held together by bandaids and
bubble-gum. Depending on what you threw over the network, you'd get random
latency spikes and packets would go missing even in Reliable delivery mode.

Ignorance aims to replace it with a stable and high performance transport system
that allows you to forget about low-level networking stress and spend more time
focusing on the gameplay.

### Ignorance Classic vs Ignorance Threaded

As of the latest version of Ignorance, there are two versions included in
releases and this repository. They are as follows:

-   Classic: This one pumps the ENET Backend every LateUpdate tick, and if any
    events come in, they will be fired all in that tick. This is the "tried and
    true" transport pumping method.

-   Threaded: This one pumps the ENET Backend with a configurable timeout and
    uses seperate server and client incoming and outgoing queues. Network Events
    get placed into their respective queues and are pumped until the server or
    client is disconnected respectively. During every LateUpdate tick, the
    incoming queues are drained, ready for more network activity. This allows
    maximum performance, at the expense of some stability as Unity threading can
    be very tempermental at times.

If performance is essential for you, use the **Ignorance Threaded** version of
Ignorance. This will give you maximum networking performance. If stability is
essential, use the **Ignorance Classic** version of Ignorance.

### Important note

Since Mirror and all transports use LateUpdate to process their network code,
there might be a chance that at very low framerate (ie. you are really stressing
the server or creating lots of GameObjects in which Unity has to load from disk)
that the networking gets overwhelmed, regardless of classic or threaded versions
of Ignorance. It is essential to keep your server's frame rate running as high
as possible as this will reduce latency and in-game lag. You will be able to
tell when the server is under heavy stress when networked objects get very
choppy and/or the client starts hanging.

Until Mirror changes how they manage their transport code, we are stuck with
having to deal with this design issue.

### I have a bug!

[Check the current open bug reports and/or report a new one
here](https://github.com/SoftwareGuy/Ignorance/issues) and I also recommend you
be present in the Discord so I can ask for further info and/or you can test
fixes for your bug reports.

Bugs that do not have enough details will be closed with \#wontfix. Details like
your OS, Unity Editor version, any Ignorance errors, etc is essential for a good
bug report.

I have questions, I want answers.
---------------------------------

[You're welcome.](https://vis2k.github.io/Mirror/Transports/Ignorance)

### I am not satisfied with Ignorance.

Please let me know why as I can't improve my code if I don't get feedback.
*However, if you're just here to troll me and my code, then please move on.*

See also:
[LiteNetLib4Mirror](https://github.com/MichalPetryka/LiteNetLib4Mirror).

Credits
-------

-   **Coffee Donators**: Thank you so much.

-   [vis2k](https://github.com/vis2k) and [Paul](https://github.com/paulpach):
    Mirror lead developers.

-   [Katori](https://github.com/katori): Helped kickstart the threaded version
    that evolved into Ignorance 2.

-   [BigBoxVR](https://github.com/GabeBigBoxVR): Pull requests and found a race
    condition that threatened stability.

-   [c6burns](https://github.com/c6burns): Words cannot describe how much help
    he's been. Bloody oath mate.

-   [Petris](https://github.com/MichalPetryka): Code refactoring and tidy up
    (you rock man!)

-   [BigBoxVR](https://github.com/GabeBigBoxVR) and
    [Martin](https://github.com/martindevans): Pull requests.

-   **The folks at the Mirror discord**: Memes, Courage, laughs, drama and
    all-round awesome folks to chat with.

-   To all the others who I have missed. Thanks a lot, you know who you are.

-   You too can have your name here if you send in a PR. Don't delay, get your
    PR in today! \#\#\# Previous hall of fame:

-   [FSE_Vincenzo](https://github.com/Vincenz099): Providing insight into proper
    usage of ENET, rebuttal, improvements, etc.

-   [shiena](https://github.com/shiena): Pull requests for bug fixes and
    oversights.

-   [Draknith](https://github.com/FizzCube): Testing and mapping
    Reliable/Unreliable channels in Mirror to ENET Channels, testing.

### And last but not least...

-   [nxrighthere](https://github.com/nxrighthere): Helped debug some things in
    early versions of Ignorance, before going full rampage and blacklisting
    everyone on the Mirror team from his repos. RIP.
