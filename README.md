# Linux Port
This is a Linux port of OpenVR Space Calibrator that I plan to maintain going forward.

## Credits
- Original code by [pushrax](https://github.com/pushrax)
- Base of this repo is the Linux fork by [gyroninja](https://github.com/gyroninja)
- Hook logic from [AngriestSCV](https://github.com/AngriestSCV)'s fork

Please thank them as I'm just adding some very minor fixes.

## Building

Required packages: `cmake` `glfw` `eigen` `openvr`

On Arch-based distros: `pacman -S cmake glfw-x11 eigen openvr`

```
mkdir build
cd build
../install.sh
```

## Running

**If using ALVR**: Launch ALVR, then on the `Installation` tab, click `Register ALVR Driver`.

**From now on you _must_ start SteamVR from Steam instead of starting ALVR!** SteamVR will start ALVR for you.

If you used the `install.sh` script, the driver will be loaded by SteamVR, but the companion software must be started separately every time.

For best results, I recommend doing the calibration in SteamVR void/home, before launching any titles, especially if you have high latency or low frame rate.

### Step-by-step Guide to Calibrate using ALVR

Verify settings:
 - Settings/Headset tab -> `Show advanced options` -> `Use Headset Tracking System`: on
 - Installation tab: ALVR should be listed under `Registered Drivers`. If not, press `Register ALVR driver`

If you want to use Lighthouse-tracked controllers with ALVR: (not needed if you just want trackers)
 - Settings/Headset tab -> `Controllers`: off

Pico users might want to increase the screen / sleep timeouts to 5 minutes or higher, as the Pico will lose calibration when the screen is off: [Link](https://www.reddit.com/r/PICO_VR/comments/zmspi9/i_managed_to_turn_off_the_pico_4s_sleep_mode_by/)

If you have ALVR open at this point, close it as well as SteamVR.

Steps:
- Launch SteamVR. It should start ALVR for you.
- Start the `openvr-spacecalibrator` companion software.
- Connect your headset via ALVR.
- Turn on one lighthouse-tracked device.
- Select these settings on the Space Calibrator window (in VR or on destkop)
  - Calibration Speed: `Slow` or `Very Slow`
  - Reference Space: The top option, which should be the headset
  - Target space: Select the lighthouse device that you'll be using
- Click `Start Calibration` while holding the lighthouse device firmly to your headset.
  - The lighthouse device shouldn't wiggle, change position or orientation relative to the headset while calibration.
- Walk around your play area while still holding the lighthouse device firmly.
- Once calibration is done, power on your other lighthouse devices, they should be good to go.

Base stations may show up in the wrong places, this issue can be ignored.


## Support

Feel free to reach out on Discord:

https://discord.gg/gHwJ2vwSWV

## To do

- Include the Continuous Calibration by bd_ 

# OpenVR Space Calibrator

This helps you use tracked VR devices from one company with any other. It aligns multiple tracking systems with a quick calibration step. It may not work for your setup, but there are many cases that work to a degree, and some work very well.

- Rift CV1 x Vive devices: works very well with the v2 (blue logo) trackers, v1 trackers (grey logo, not in production) have major interference issues in the IR spectrum, controller wands (both gen) and Index controllers work very well.
- Rift S, Quest, Windows MR, other SLAM inside-out tracked HMDs x Vive devices: works very well when you aren't moving around the room far (e.g. Beat Saber) but a lot of walking around causes a nontrivial amount of drift between systems. Your results may vary depending on your space. It's possible some of this can be fixed in software with a better calibration algorithm.
- Quest wireless streaming is particularly bad right now and requires frequent recalibration, but it does work for a short time until one of many factors causes it to drift. With wireless devices, moving slowly when calibrating and using the Slow or Very Slow calibration modes is effective at reducing the initial error.
- Any non-Rift HMD x Touch controllers: does not work, the Oculus driver requires the HMD is a Rift. It's theoretically possible to work around this in software but as far as I know it hasn't been done as it would require a fair amount of reverse engineering effort.

There is a community of a few thousand on [**Discord**](https://discord.gg/m7g2Wyj) and a newer community on [**Reddit**](https://www.reddit.com/r/MixedVR/). You may find the answer to your question in [the **wiki**](https://github.com/pushrax/OpenVR-SpaceCalibrator/wiki).

A quick video of how this works using an old version (~v0.3) is available at https://www.youtube.com/watch?v=W3TnQd9JMl4. The user interface has been upgraded since then; the calibration is now done via a SteamVR dashboard menu, and there's much more configurability.

### Usage

Once Space Calibrator has a calibration, it works in the background to keep your devices configured correctly. Since v0.8, everything aside from creating the calibration is automated.

### Calibration

As part of first time setup, or when you make a change to your space (e.g. move a sensor), and occasionally as the calibration drifts over time (consumer VR tracking isn't perfectly stable), you'll need to run a calibration:

1. Copy the chaperone/guardian bounds from your HMD's play space. This doesn't need to be run if your HMD's play space hasn't changed since last time you copied it. __Example:__ if you're using the Rift with Vive trackers and you bump a Vive lighthouse, or if the calibration has just drifted a little, you likely don't need to run this step, but if you bump an Oculus sensor you will (after running Oculus guardian setup again).
    1. Run SteamVR, with only devices from your HMD's tracking system powered on. __Example:__ for Rift with Vive trackers, don't turn on the trackers yet.
    2. Confirm your chaperone/guardian is set up with the walls in the right place. If you change it later, you need to run step again.
    3. Open SPACE CAL in the SteamVR dashboard overlay.
    4. Click `Copy Chaperone Bounds to profile`

2. Calibrate devices.
    1. Open SteamVR if you haven't already. Turn on some or all your devices.
    2. Open SPACE CAL in the SteamVR dashboard overlay.
    3. Select one device from the reference space on the left and one device from the target space on the right. If you turned on multiple devices from one space and can't tell which one is selected, click "Identify selected devices" to blink an LED or vibrate it. __Example:__ for Rift with Vive trackers, you'll see the Touch controllers on the left, and Vive trackers on the right. __Pro tip:__ if you turn on just one Vive tracker, you don't have to figure out which one is selected.
    4. Hold these two devices in one hand, like they're glued together. If they slip, calibration won't work as well.
    5. Click `Start Calibration`
    6. Move and rotate your hand around slowly a few times, like you're calibrating the compass on your phone. You want to sample as many orientations as possible.
    7. Done! A profile will be saved automatically. If you haven't already, turn on all your devices. Space Calibrator will automatically apply the calibration to devices as they turn on.

### Calibration outside VR

You can calibrate without using the dashboard overlay by unminimizing Space Calibrator after opening SteamVR. This is required if you're calibrating for a lone HMD without any devices in its tracking system.

### The math

See [math.pdf](https://github.com/pushrax/OpenVR-SpaceCalibrator/blob/master/math.pdf) for details.
If you have some ideas for how to improve the calibration process, let me know!
