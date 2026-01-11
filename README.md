# ESPHome Clock with a Vorne GY2200 Display
The overarching goal of this project is to leverage a 10 character Vorne GY2200 LED display to display accurate time integrated with Home Assistant via ESPHome.  

## Listed goals:
- Make the power cable be the only cable to the display
- Have time accurate to the second, and keep it that way
- Control the following parameters from Home Assistant through an ESPHome device
  - Turn the output ON or OFF; blank the display when OFF
  - Control the brightness of the display; values 1-9
  - Sync to a local SNTP time server at boot and frequently enough to keep the ESP32 from drifting
  - Choose between 4 time formats: “10:25:56”, “10:25 PM”, “10:25:56 P”, and “10:25”
  - Switch between 12 and 24 hour formats when appropriate: “22:25:56”, “10:25 PM”, “10:25:56 P”, and “22:25”

## Background 
The GY2200 was discontinued by Vorne some time back, but the display is rock solid and can run for decades.  Inside the back cover of the GY2200 are DIP switches for controlling baud, data bits, stop bits, parity, and display ID as well as a small changeable circuit board that can be one of a few variants such as RS485 or RS232, for example.  

This project uses a GY2200 with an RS232 board installed.

The GY2200 actually uses 5v TTL signalling, so if you are smarter than me, you can probably tie the TTL output of the ESP32 directly to the GY2200 if you know where to tie-in.  In my case, I did not want to work inside the case of the GY2200 by soldering in lines and such in favor of making my solution compact and external to the display.  I often build something and then later want to repurpose it.  Keeping everything external helps with that.

Fortunately (in a non-standard kinda way), the GY2200 male DB9 connector has RXD on Pin1, GND on Pin7, and 5V out on Pin5.  5V on Pin5 is a rather odd choice as it is GND on a normal PC Serial DB9 connector.  Because of this, I was able to power the ESP32 from the GY2200 itself.
  
Below is a parts list and wiring diagram for the project.  From left to right:
- A 2.4Ghz Wi-Fi antenna with pigtail.  I clipped off the end of the pigtail and soldered the core and the shield to the ESP32 after having scraped away solder points as shown and separated them by removing some of the line trace.  This is well documented at: [How to add an external antenna to an ESP board - ESPHome - Home Assistant Community](https://community.home-assistant.io/t/how-to-add-an-external-antenna-to-an-esp-board/131601).
- An ESP32 DevKit v1.  Note that basically any ESP32 should work as long as you pay attention to what pins you use for the UARTs.
  - The side-by-side VIN and GND pins are wired to Pin5 and Pin7 respectively on the GY2200.
  - 3.3v is wired to VCC on the TTL-to-RS232 adapter
  - GND is wired to GND on the TTL-to-RS232 adapter
  - D4 is wired to RXD on the TTL-to-RS232 adapter
- A TTL to RS232 DB9 female adapter was combined with a male DB9 connector to wire Pin2 of the adapter to Pin1 of the GY2200.  
- A PVC project box was used to hold the ESP32 and adapter with only the antenna and RS232 cable to the display exiting the box.  After the components were inside, it was hot-glued to the back of the GY2200.
- And of course, the Vorne GY2200 with RS232 board installed.  The GY2200 was configured for 9600 baud, 8 data bits, 1 stop bit, CR on, LF off, no parity, address 00.
  ![DIP Switches](https://github.com/jeparkspr/clock-display/blob/main/GY2200%20DIP%20Switches.png)
  ## Components and Wiring Diagram
  ![Wiring Diagram](https://github.com/jeparkspr/clock-display/blob/main/Wiring%20Diagram.png)
  ![Project Front](https://github.com/jeparkspr/clock-display/blob/main/Project%20Front.jpg)
  ![Project Back](https://github.com/jeparkspr/clock-display/blob/main/Project%20Back.jpg)
