# WS2812B SPI DMA example

Single-file-header for SPI-DMA to output WS2812B LEDs.  By chunking the outputs using the center-interrupt, it is possible to double-buffer the WS2812B output data while only storing a few LEDs worth of data in memory at once.

This outputs the LED data on the MOSI (PC6) pin of the CH32V003.

Additionally, this demo only uses 6% CPU while it's outputting LEDs and free while not and it doesn't require precise interrupt timing, increasing or decreasing `DMALEDS` will adjust how lineant the timing is on LED catpures.

The timing on the SPI bus is not terribly variable, the best I was able to find was:

* Ton0 = 324ns
* Ton1 = 990ns
* Toff0 = 990ns
* Toff1 = 324ns
* Treset = 68us

## Usage

If you are including this in main, simply 
```c
#define WS2812DMA_IMPLEMENTATION
#include "ws2812b_dma_spi_led_driver.h"
```

You will need to implement the following two functions, as callbacks from the ISR.
```c
uint32_t CallbackWS2812BLED( int ledno );
```

You willalso need to call
```c
InitWS2812DMA();
```

Then, whenyou want to update the LEDs, call:
```c
WS2812BStart( int num_leds );
```

If you want to see if it's done sending, examine `WS2812BLEDInUse`.
