## SpectrumLab V1


## JUMPER
JP1 ... VBUS = 5V jumper (Default: not connected): Connecting this disables the protective SBD between USB and external power supply. To eliminate the voltage drop due to SBD and output the voltage input from USB as it is to 5V, connect this jumper. Note that shorting this pin will increase current consumption by about 0.1mA due to the pull-down resistor on the CC pin.

JP2 ... 5V -> 3.3V jumper (Default: connected): Connection of the external power supply terminal (5V pin) to the power input terminal to the ESP32-C3. Disconnect when suppressing regulator quiescent current.

