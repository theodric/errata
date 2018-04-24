Following the instructions at https://github.com/adafruit/esp8266-micropython-vagrant does not result in a working configuration.

1. just bloody increase the amount of RAM allocated to the VM straight out of the hole. If the compile fails because it runs out of RAM,
it will add an hour to the amount of time you have to spend fighting it.

2. "make STANDALONE=y" is going to fail anyway because it's looking for some headers that aren't in the esp8266 directory.
Go find them in another architecture's directory one level up and copy them to esp8266

TODO: actual useful detail about what the missing files are and how to fix it before you have to watch your build fail


