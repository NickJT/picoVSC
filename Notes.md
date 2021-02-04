
Find the udev rules file in the pico/openocd folder. It should be here:

/home/<your path>/pico/openocd/contrib/60-openocd.rules

scroll to the bottom and make sure it has these lines:

# Raspberry Pi Picoprobe
ATTRS{idVendor}=="2e8a", ATTRS{idProduct}=="0004", MODE="660", GROUP="plugdev", TAG+="uaccess"

This is immediately before the last line of the file which is:
LABEL="openocd_rules_end"

If this line is present, copy the 60-openocd.rules file to /etc/udev/rules.d/ folder

sudo cp 60-openocd.rules /etc/udev/rules.d/

To reload the rules, you can try 

sudo udevadm control --reload

or to be safe, just restart.


Now we have permission to access the serial port for picoprobe, we can change the launch.json file

{
  "version": "0.2.0",
  "configurations": [
      {
        "name": "Debug", 
        "searchDir": [
         "/home/<your path>/pico/openocd/tcl"
        ],
        "cwd": "${workspaceRoot}",
        "executable": "${command:cmake.launchTargetPath}",
        "request": "launch",
        "type": "cortex-debug",
        "servertype": "openocd",
        "gdbPath" : "gdb-multiarch",
        "device": "RP2040",
        "configFiles": [
            "/interface/picoprobe.cfg",
            "/target/rp2040.cfg"
        ],
        "svdFile": "${env:PICO_SDK_PATH}/src/rp2040/hardware_regs/rp2040.svd",
        "runToMain": true,
        "postRestartCommands": [
            "break main",
            "continue"
        ]
    }
  ]
}

Check that the CMakeLists.txt file includes:
pico_enable_stdio_uart(<exe name> 1)