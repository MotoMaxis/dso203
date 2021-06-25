Since a while my DS203 v2.72 8MB was not able to reliably save files, only CFG were working, also filesystem get corrupted very often.
I tried all combination of:

SYS_B152

SYS_B160

SYS_B162

SYS_B164

JPA Alterbios by patching all the above

formatting the flash with any kind of old dos and windows devices, virtual machines

v113 firmware, B251 firmware and any older release of Wildcat firmware.

Nothing worked, I was close to throw away the scope thinking the flash got broken.

Then I fixed a bug in the Alterbios that used to write/read using 512B as block size regardless the flash disk size.
Now it uses 512B on 2M and 4096B on 8M.

Every issue disappeared, I realized it was partially working with CFG because those files were 512B (now 4096B), with other filesizes the filesystem got messed.

Here https://github.com/Turro75/AlterBIOS/tree/test You can find my fork of Alterbios (not sure JPA can work on it anymore) where I put a Compiled folder with all binaries ready to flash.

Installation is easy:

load HEX file on DFU or

load ADR+BIN of ALTBIOS and ALT_B164 on DFU or

load BIN with stm32flash (following debrick instructions)

I use these commands on my linux desktop:

stm32flash /dev/ttyUSB0 -S 0x8044000:0x1fff -w ALTBIOS.BIN

stm32flash /dev/ttyUSB0 -S 0x8004000:0x7fff -w ALT_B164.BIN

ALTBIOS is common to all ALT_B1XX, no need to flash it every time after the first load.
Flashing SYS_B164.HEX/BIN restores the stock SYS
