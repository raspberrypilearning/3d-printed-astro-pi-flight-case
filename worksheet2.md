# 3D printed Astro Pi flight case

- [Back to Worksheet 1](worksheet.md)

When you're happy with the 3D printed parts you can proceed with installing the hardware.

## Fold the camera ribbon

Getting the camera into the case can be a little tricky but here is how we did it for the flight units. Firstly, disconnect the ribbon cable from the camera; the two tabs lift up on either side to release the cable in the same way as they do on the Raspberry Pi itself. Then fold the ribbon similar to how it's shown below. These folds allow the camera cable to come up the side of the Raspberry Pi with enough flex to insert into the CSI camera port (see further down).

![](images/ribbon1.png)

You don't need to get this perfect for it to work, but try to get it as close as possible. The kapton tape is also optional; any kind of tape will work fine if you want to prevent the ribbon from unfurling.

![](images/ribbon2.png)

## Install the camera

Reconnect the `Cam` end of the ribbon cable to the camera module. Make sure that the tin connectors are facing the front and the blue tab is on the back.

Before proceeding, line up the camera module with the support pillar pilot holes and have a look through the aperture in the base to check the alignment of the lens. The lens block of the camera module is actually glued in position at the factory, and therefore its alignment can vary slightly from camera to camera. If you find you've got an alignment issue, you should be able to manipulate the lens block between finger and thumb before you install it into the case permanently.

When you're happy, the camera module should be installed into the case as shown below. With light finger pressure, the M2 cross head screws will cut their own thread in the support pillar pilot holes. After just a few turns you'll probably need a small screwdriver to continue.

![](images/install_camera.png)

Stop turning as soon as the head of the screw touches the camera module; if you tighten the screw too much it can cause the support pillar to split horizontally along the grain of the print. Furthermore, try to avoid removing and re-threading the screws as this will cut a brand new thread and, if done repeatedly, will erode the inside of the support pillar so that the screws will not hold.

## Install the Raspberry Pi

Firstly, ensure there's no residual scaffolding material around the SD card slot or LED holes that might prevent the Raspberry Pi from lining up with the mounting pillars. Once you're happy, line up the Raspberry Pi and do a fit check. Verify that it doesn't touch the camera module below it. Don't insert the camera ribbon cable just yet, as this will make the next job awkward.

![](images/install_pi1.png)

Next take the M2.5 - 11 mm stand offs; with some light finger pressure they will cut their own thread in the support pillar pilot holes. After a few turns you'll need to use a small pair of pliers to continue turning them. It's possible to get these to go in at a slight angle, which can lead to alignment issues with the Sense HAT later on, so do your best to make sure these go into the support pillars as straight as possible. Be careful while you do this and, again, stop turning as soon as the stand off touches the Raspberry Pi to avoid spliting the pillars horizontally along the grain of the print.

![](images/install_pi2.png)

Now you can insert the camera ribbon cable into the CSI port of the Raspberry Pi. Try to make sure the tin connectors are all level to ensure a good connection.

![](images/install_pi3.png)

## Install the Sense HAT

This is where we're going to deviate from what's inside the Astro Pi flight unit. The flight units have another circuit board in between the Raspberry Pi and Sense HAT which holds a real-time clock, oscillator crystal and backup battery. This RTC board also has some pins that the six push buttons connect to. Unfortunately, this is not available to the public.

Our goal was to keep the 3D printed flight case as *faithful* to the original as possible, so the decision was taken to *not* alter it to accommodate the absence of this board. It may be possible for us to release the Gerber files for it in the future so that people can make their own.

So we're going to use a hex nut or washer of the same depth as the RTC board to compensate for its absence. The RTC board is 1.6 mm thick, so we need a nut or washer of the same thickness. There are many ways you could achieve this, for example with two washers of 0.8 mm thickness. Perhaps you could 3D print one.

Take an 8 mm M2.5 stand off and put the hex nut or washer onto its thread before screwing it into the hole of the 11 mm stand off, as shown below. Do the same for the remaining three stand offs.

![](images/install_sense1.png)

Remove the GPIO connector that comes with the Sense HAT; wiggle it from side to side and it will come off without too much force. The Sense HAT can then be inserted onto the GPIO connector with the *long* pins (see checklist above). Note that these pins should not protrude through the top of the Sense HAT. If they do, then the height is not correct.

![](images/install_sense2.png)

Finally, use the M2.5 cross head screws to secure the Sense HAT to the stand offs below.

![](images/install_sense3.png)

## Install the push buttons

If you are using the lid with the pilot holes then you will need to check the datasheet of your chosen button type to find the *threaded bushing diameter*. Once you know this you can select a drill bit with the this diameter, plus 1 mm for clearance, and proceed to drill out all six holes. We recommend using a vice or g clamp to hold the lid in place while you drill. You can then install the buttons as per their requirements.

The guidance below assumes you're using the APEM buttons that we used in the flight unit. These have a thread which goes through the lid from below, along with a friction washer to stop it rotating, a nut to adjust the thread depth (red star below) and then a collar to secure the button from the top. The red line below indicates where the lid should be.

![](images/buttons1.png)

Insert the thread along with the nut and friction washer from the underside of the lid, and then adjust the nut to the approximate position shown below. This will minimise the thread showing on top.

![](images/buttons2.png)

Place the collar over the threading and tighten securely. If too much thread is showing, adjust the nut on the underside again.

![](images/buttons3.png)

When you're done, it should look like this. Do the same for the remaining buttons.

![](images/buttons4.png)

## Wire up the buttons

Without the RTC board in the middle, you won't have a convenient way to wire the buttons to the GPIO pins of the Raspberry Pi. Because there are so many ways this can be solved, you should decide how you will do this yourself.

![](images/jumper_wiring.png)

To match the flight unit, you should wire the buttons to the last eight GPIO pins at the bottom of the header. **These pins do not need to be connected to the Sense HAT, so it's fine if you want to cut or bend them on the 2x20 pin PCB header (GPIO connector) as part of your solution.** You can also chop off the last 4 rows of the PCB header and put the jumper cables directly onto the Raspberry Pi pins (as shown above). Turn the rough edge to where the red star is, and this will leave the good edge for the jumper wires to fit against.

![](images/buttons_GPIO.png)

Note the orientation of the pin diagram is with the Ethernet and USB ports facing downwards, and the row of pins on the right-hand side of the Pi. They need to be wired in a **pull up** configuration in order to match the flight unit. Fortunately, the Raspberry Pi has all that circuitry built-in, so you can get away with just using a single wire between the button and GPIO pin.

There is also a Device Tree overlay that causes these buttons to type `u`, `d`, `l`, `r`, `a` and `b` when you press them. This allows you to easily write code that uses button and joystick events at the same time.

Here are the GPIO pin assignments:

- Top quad
    - UP: GPIO 26, pin 37
    - DOWN: GPIO 13, pin 33
    - LEFT: GPIO 20, pin 38
    - RIGHT: GPIO 19, pin 35
- Bottom pair
    - A (left): GPIO 16, pin 36
    - B (right): GPIO: 21, pin 40

The APEM buttons have three poles numbered 1 to 3 (note that pole 3 is in the middle):

![](images/apem_wiring.jpg)

1. GPIO pin
1. Do not connect
1. GND

We recommend that you strip back some jumper wire and solder directly onto the button contacts; however, you could also use crimped wire terminals that friction fit onto the contacts (we felt these were not reliable enough for flight).

The picture below is of one of the flight units that went into space. On the right, you can see the base of the RTC board with the connector pins for the buttons. If you look at the button contacts on the left, you'll see we used only one black ground wire that went from button to button - it's fine to do this.

![](images/flight_unit_wiring.jpg)

## Test the buttons

Once you have all the buttons wired up, start up your Astro Pi with a monitor, keyboard and mouse connected. We need to download some files and change a few configuration settings. Firstly, download the Device Tree overlay that maps the push buttons to corresponding keyboard keys. Open a terminal and enter these commands:

```bash
cd /boot/overlays
sudo wget https://github.com/raspberrypilearning/3d-printed-astro-pi-flight-case/raw/master/dtb/astropi-keys.dtb --no-check-certificate
ls
```

Check that the file `astropi-keys.dtb` is now showing in the list of files.

Next, we need to configure `config.txt` to load this overlay:

```bash
sudo nano /boot/config.txt
```

Go to the bottom of the file and enter the line below:

```bash
dtoverlay=astropi-keys
```

Press `Ctrl - O` then `Enter` to save, followed by `Ctrl - X` to quit.

Now reboot the Astro Pi.

```bash
sudo reboot
```

Now let's download and run a Python test program to check everything is working. The test code uses [Pygame](http://pygame.org/wiki/tutorials), so please do this on the Astro Pi's own screen and not via remote access. Open a terminal and enter these commands:

```bash
cd ~
wget https://github.com/raspberrypilearning/3d-printed-astro-pi-flight-case/raw/master/test_code/pygame_test.py --no-check-certificate
chmod +x pygame_test.py
./pygame_test.py
```

Waggle the joystick and press all the push buttons. If everything is working, the joystick should give a direction indication and the buttons will show the corresponding letter on the LED matrix. Press `Escape` to exit.

The flight unit uses hardware pull ups on the GPIO pins; however, this test code enables the Raspberry Pi's own internal pull up resistors so your button wiring can be nice and simple. Because of this, you will also need to set the internal pull ups in any Astro Pi code that you write. The block of code below will do this for you; just make sure you have this at the top of each program.

```python
import RPi.GPIO as GPIO

UP = 26
DOWN = 13
LEFT = 20
RIGHT = 19
A = 16
B = 21

GPIO.setmode(GPIO.BCM)

for pin in [UP, DOWN, LEFT, RIGHT, A, B]:
    GPIO.setup(pin, GPIO.IN, GPIO.PUD_UP)
```

## Assemble the case

Once you're happy that the internals of the case are complete, you can proceed to the final assembly stage. If you haven't done so already, now is the time to use the epoxy adhesive to join the heat sink to the base and the lid to the middle.

When joining the lid to the middle, you have an opportunity to locate the lid so as to mitigate any alignment issues with the LED matrix and joystick that may have occurred from the stand offs not being quite straight. After the case is assembled, insert an M4 bolt into each corner bolt enclosure and tighten up the hex nuts on the other side.

![](images/install_fit_check.png)

Your Astro Pi is almost complete; the last thing to do is install the joystick hat. The flight units use a track point cap from a Lenovo ThinkPad laptop (part no 73P2698). There are plenty of cheaper alternatives available online that will work just as well.

To mount the track point cap onto the Sense HAT joystick, you should chop up an empty ink tube from a BIC biro - yes, this is what's used in the Space Station unit. Cut off a short length of the tube and, with some gentle force, the tube will fit over the stump of the joystick. It will also pop off again if too much force is used, which protects the cheap joystick component inside.

Fill the cavity on the base of the track point cap with some hot melt adhesive from a glue gun, and then insert the BIC tube into the soft adhesive. Allow this to cool, and then your joystick is ready for use.

![](images/track_point_cap.png)

## What next?

We've deliberately not shown a really polished gorgeous case, because we're hoping you'll go the extra mile and blow our socks off. Please show us your cases by tweeting pictures of them to `@astro_pi` and `@raspberry_pi`!

Here are some further ideas:

- Use metallic grey spray paint
- Use sandpaper to create the matt finish that the bead-blasted aluminium flight cases have
- Engrave decals into the case
- Use different colours of filament for each part

The STL files are released under the Creative Commons attribution [licence](http://creativecommons.org/licenses/by-sa/4.0/) so you are welcome to modify them. Please note that GitHub has a great [STL viewer](https://github.com/blog/1465-stl-file-viewing) and also has a [3D file diff](https://github.com/blog/1633-3d-file-diffs), which could be useful for tracking your changes.

But by far the greatest benefit of owning an Astro Pi flight unit is the ability to prototype and test code that could be run on the International Space Station. Head over to the [Astro Pi website](https://astro-pi.org/) now to get involved!
