This is a set of programs that receive and emit IR signals to be used for the creation of custom remotes
I personally used it to turn on/off both a television and a soundbar with one button press

The receiver program allows an IR receiver to be plugged in to GPIO pin 4 and listen for IR signals
The program then displays these signals in the serial monitor with the raw signal, the code, the protocol, and the length in bits

The emitter program went through several iterations

irEmitter
This version set a button to GPIO pin 23 and a voltage splitter to GPIO pin 35, with the receiver on pin 4 being replaced by an IR LED
The ESP32 would sit in idle until the button was pressed, at that point it would send the two power signals via the IR LED on pin 4 with a small delay between signals
The voltage splitter on pin 35 was set up to be used to detect the remaining voltage of the battery and flash the onboard LED if the battery was low
The low battery detection was not yet implemented in this version

irEmitter_v2
This version was made in an effort to extend battery life as well as slightly refactoring the cicuit itself
The button was moved to GPIO pin 15 and a transistor was added to check the battery on GPIO pin 26
When not in use, the ESP would be in deep sleep, reducing the amount of current draw
When the button was pressed, it would read the battery voltage and flash the onboard LED if it was below a certain threshold
The ESP would then send the signals as before and return to deep sleep

irEmitter_v3
This version was a radical change to further extend battery life and another refactoring of the circuit for a much simpler approach
The button was moved from the GPIO pin to interrupt the positive lead of the power supply
This allowed the whole circuit to only be powered while the button was pressed, drastically increasing battery life
A capacitor was also added between the 3v3 pin and the GND pin to help stabilize current
Everything was removed except for the barebones code to send the IR signals
The button must now be held down for half a second for the ESP to power on and send the signal, but that is a small price to pay for batteries to last several years
