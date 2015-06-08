# WS2812 Class

This class allows the imp to drive WS2812 and WS2812B LEDs. The WS2812 is an all-in-one RGB LED with integrated shift register and constant-current driver. The parts are daisy-chained, and a proprietary one-wire protocol is used to send data to the chain of LEDs. Each pixel is individually addressable and this allows the part to be used for a wide range of effects animations.

Some example hardware that uses the WS2812 or WS2812B:

* [40 NeoPixel Matrix](http://www.adafruit.com/products/1430)
* [60 LED - 1m strip](http://www.adafruit.com/products/1138)
* [30 LED - 1m strip](http://www.adafruit.com/products/1376)
* [NeoPixel Stick](http://www.adafruit.com/products/1426)

**To add this library to your project, add** `#require "WS2812.class.nut:2.0.0"` **to the top of your device code**

## Hardware

WS2812s require a 5V power supply and logic, and each pixel can draw up to 60mA when displaying white in full brightness, so be sure to size your power supply appropriatly. Undersized power supplies (lower voltages and/or insufficent current) can cause glitches and/or failure to produce and light at all.

Because WS2812s require 5V logic, you will need to shift your logic level to 5V. A sample circuit can be found below using Adafruit’s [4-channel Bi-directional Logic Level Converter](http://www.adafruit.com/products/757):

![WS2812 Circuit](./circuit.png)

## Class Usage

### Constructor

Instantiate the class with a pre-configured SPI object and the number of pixels that are connected. The SPI object must be configured at 7500kHz and have the *MSB_FIRST* flag set:

```squirrel
#require "ws2812.class.nut:2.0.0"

// Configure the SPI bus
spi <- hardware.spi257;
spi.configure(MSB_FIRST, 7500);

pixels <- WS2812(spi, 5);
```

### Class Methods

The WS2812 class keeps an internal frame that is only output to the pixel array when the **writeFrame()** method is called. As a result, changing the pixel strip takes two steps: writing values to the frame, and writing the frame to the SPI bus.

### set(*pixelAddress, color*)

The *set* method changes the colour of a particular pixel in the frame buffer. The color is passed as as an arry of three integers between 0 and 255 representing `[red, green, blue]`.

NOTE: The set method does not output the changes to the pixel strip. After setting up the frame, you must call `draw` (see below) to output the frame to the strip.

```squirrel
// Set each pixel individually
pixels.set(0, [127,0,0]);
pixels.set(1, [0,127,0]);
pixels.set(2, [0,0,127]);
pixels.set(3, [0,127,0]);
pixels.set(4, [127,0,0]);

// Output the frame
pixels.draw();
```

### fill(*color, [start], [end]*)

The *fill* methods sets all pixels in the specified range to the desired colour. If no range is selected, the entire frame will be filled with the specified colour:

```squirrel
// Turn all LEDs off
pixels.fill([0,0,0]);

// Output the frame
pixels.draw();
```

```squirrel
// Set half the array red
pixels.fill([255,0,0], 0, 2);

// Set the other half blue
pixels.fill([0,0,255], 3, 4);

// Output the frame
pixels.draw();
```

### draw()

The *draw* method draws writes the current frame to the pixel array.

## License

The WS2812 class is licensed under the [MIT License](./LICENSE).
