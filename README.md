Joakim readme

clean_eeprom step never worked for me, so far so good without

Needed to run stm8flash with this patch: https://github.com/vdudouyt/stm8flash/pull/157
Otherwise flashing would just give SWIM error 0x05 / 0x04
I have my own branch of stm8flash for this on github

Final sequence to get it to flash was (after building with compile_and_flash_20.sh and objcopy):
echo "00 00 ff 20 df 00 ff 00 ff 00 ff 00 ff 00 ff" | xxd -r -p > option_bytes_pwm_n_channels_enabled.bin
stm8flash -c stlinkv2 -p "stm8s105?6" -s opt -w option_bytes_pwm_n_channels_enabled.bin
stm8flash -c stlinkv2 -p "stm8s105?6" -w releases/TSDZ2-20.1C.2-2-initialflash.hex.bin

This is using the binary format, converted hex to binary with objcopy according to wiki. Turns out this was not necessary and later flash was done directly with hex format

Speed sensor error check disabled in ebike_app.c by just not incrementing the error timer

----------------------------------------------------------------------------------------------------

TL;DR

Have stm8flash with patch installed. Since I did my first work stm8flash has merged the patch so an official release might work. Otherwise use my branch and do make && make install or something like that

To mess with settings, before flashing run and press the compile button to save (wont actually compile or flash anything)
java -jar JavaConfigurator.jar

To compile run the following and select no on backup and flash questions
./compile_and_flash_20.sh <name of build>

To then flash run 
stm8flash -c stlinkv2 -p "stm8s105?6" -w releases/TSDZ2-20.1C.2-2-immediateassist52V.hex
