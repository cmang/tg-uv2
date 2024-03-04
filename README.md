This is a copy of the C code required to read and write to TG-UV2.

The original was found here: https://chirp.danplanet.com/issues/2311

This is the original documentation from the qs-tg-uv2.c source file by Mike Nix:

----

   Quansheng TG-UV2 Utility by Mike Nix <mnix@wanm.com.au>

   Currently only reads the device memory, and displays the contents nicely formatted.
   
   
### Supports:

All channel config, except CTSS/DCS settings (we know where they are, just haven't
bothered to interpret them yet)

All the settings configurable via Options in the original windows utility.

Both of the known keypad dances for enabling/disabling bands are detected/displayed.
and yes, we can do the same thing by updating the config.

Saves the config read from the device to a file (tguv2.dat)
**** THIS IS NOT THE SAME FORMAT USED BY THE WINDOWS UTILITY ****
The windows utility saves in some proprietry format... we just dump
a copy of the radio memory to disk :)

Reads a file saved above, and uploads it to the radio.      
      
### TODO:

There are at least 5 memory locations that do stuff I can't figure out (config->unk*)
      
I never found a way to enable transmit on the 470-520 band
(Australian UHF CB is at 476-477 Mhz).  Probably need a firmware hack for this :(

Command-line options for settings so we can change the config easily.

Find a copy of the firmware, and the update utility so we can really open it up.


There is a command line option to manually set any byte in the config area for
testing purposes, but this is not the way to configure your radio normally :)

There is another command line option for sending arbitrary data to the radio in 
programming mode...
   

### Other Info:
   
There are positive responses (ACK) to the following commands if followed
by any other byte:

    G, O, P, Q  but have no idea what they do....
   
The known commands for programming and getting the model number are:

    \002PnOGdAM	Enter Programming Mode
    M\002	Get Model Number? (Returns 'P5555' followed by F4 00 00)
    R		Read config memory
    W		Write config memory
   
Any 1-byte after entering program mode followed by 0x02 will return the model number

Other commands will not work until after you get the model number.

R command is followed by addr_hi, addr_lo, length

W command is followed by addr_hi, addr_lo, length then data bytes

A null byte is sent by the radio every time the display changes... no idea what use that is.
   
Further testing reveals that any 8 bytes written will do to start programming mode.
** maybe there is a special sequence for firmware mode?
Actually, any sequence of more than 1 byte containing "M" seems to hit program mode.
