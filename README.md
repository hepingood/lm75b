<div align=center>
<img src="/doc/image/logo.png"/>
</div>

## LibDriver LM75B

[English](/README.md) | [ 简体中文](/README_CN.md)

The LM75B is a temperature-to-digital converter using an on-chip band gap temperature sensor and Sigma-Delta A-to-D conversion technique with an overtemperature detection output. The LM75B contains a number of data registers: Configuration register (Conf) to store the device settings such as device operation mode, OS operation mode, OS polarity and OS fault queue as described in the datasheet.Temperature register (Temp) to store the digital temp reading, and set-point registers (Tos and Thyst) to store programmable overtemperature shutdown and hysteresis limits, that can be communicated by a controller via the 2-wire serial I2C-bus interface. The device also includes an open-drain output (OS) which becomes active when the temperature exceeds the programmed limits. There are three selectable logic address pins so that eight devices can be connected on the same bus without address conflict.The LM75B can be configured for different operation conditions. It can be set in normal mode to periodically monitor the ambient temperature, or in shutdown mode to minimize power consumption. The OS output operates in either of two selectable modes:OS comparator mode or OS interrupt mode. Its active state can be selected as either HIGH or LOW. The fault queue that defines the number of consecutive faults in order to activate the OS output is programmable as well as the set-point limits. The temperature register always stores an 11-bit two’s complement data giving a temperature resolution of 0.125 C. This high temperature resolution is particularly useful in applications of measuring precisely the thermal drift or runaway. When the LM75B is accessed the conversion in process is not interrupted (that is, the I2C-bus section is totally independent of the Sigma-Delta converter section) and accessing the LM75B continuously without waiting at least one conversion time between communications will not prevent the device from updating the Temp register with a new conversion result. The new conversion result will be available immediately after the Temp register is updated. The LM75B powers up in the normal operation mode with the OS in comparator mode,temperature threshold of 80 C and hysteresis of 75 C, so that it can be used as a stand-alone thermostat with those pre-defined temperature set points.LM75B is used in system thermal management, personal computer, electronic equipment and industrial controller.

LibDriver LM75B is the full function driver of LM75B launched by LibDriver.It provides functions such as temperature reading and interrupt detection.

### Table of Contents

  - [Instruction](#Instruction)
  - [Install](#Install)
  - [Usage](#Usage)
    - [example basic](#example-basic)
    - [example interrupt](#example-interrupt)
  - [Document](#Document)
  - [Contributing](#Contributing)
  - [License](#License)
  - [Contact Us](#Contact-Us)

### Instruction

/src includes LibDriver LM75B source files.

/interface includes LibDriver LM75B IIC platform independent template。

/test includes LibDriver LM75B driver test code and this code can test the chip necessary function simply。

/example includes LibDriver LM75B sample code.

/doc includes LibDriver LM75B offline document.

/datasheet includes LM75B datasheet。

/project includes the common Linux and MCU development board sample code. All projects use the shell script to debug the driver and the detail instruction can be found in each project's README.md.

### Install

Reference /interface IIC platform independent template and finish your platform IIC driver.

Add /src, /interface and /example to your project.

### Usage

#### example basic

```C
uint8_t res;
uint8_t i;
float t;

res = lm75b_basic_init(LM75B_ADDRESS_A000);
if (res)
{
    return 1;
}

...

for (i = 0; i < 3; i++)
{
    lm75b_interface_delay_ms(1000);
    res = lm75b_basic_read((float *)&t);
    if (res)
    {
        lm75b_basic_deinit();

        return 1;
    }
    lm75b_interface_debug_print("lm75b: temperature is %0.3fC.\n", t);

    ...
    
}

...

lm75b_basic_deinit();

return 0;
```

#### example interrupt

```C
uint8_t res;
uint8_t i;
float t;
uint8_t g_flag;

res = gpio_interrupt_init();
if (res)
{
    return 1;
}
res = lm75b_interrupt_init(LM75B_ADDRESS_A000, LM75B_OS_OPERATION_INTERRUPT, 22.5, 32.1);
if (res)
{
    gpio_interrupt_deinit();

    return 1;
}

...
    
for (i = 0; i < 3; i++)
{
    lm75b_interface_delay_ms(1000);
    res = lm75b_interrupt_read((float *)&t);
    if (res)
    {
        gpio_interrupt_deinit();
        lm75b_interrupt_deinit();

        return 1;
    }
    lm75b_interface_debug_print("lm75b: read is %0.3fC.\n", t);
    
    ...
    
    if (g_flag)
    {
        lm75b_interface_debug_print("lm75b: find interrupt.\n");

        break;
    }
    
    ...
    
}

...

gpio_interrupt_deinit();
lm75b_interrupt_deinit();

return 0;
```

### Document

Online documents: https://www.libdriver.com/docs/lm75b/index.html

Offline documents: /doc/html/index.html

### Contributing

Please sent an e-mail to lishifenging@outlook.com

### License

Copyright (C) LibDriver 2015-2021 All rights reserved 



The MIT License (MIT) 



Permission is hereby granted, free of charge, to any person obtaining a copy

of this software and associated documentation files (the "Software"), to deal

in the Software without restriction, including without limitation the rights

to use, copy, modify, merge, publish, distribute, sublicense, and/or sell

copies of the Software, and to permit persons to whom the Software is

furnished to do so, subject to the following conditions: 



The above copyright notice and this permission notice shall be included in all

copies or substantial portions of the Software. 



THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR

IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,

FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE

AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER

LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,

OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE

SOFTWARE. 

### Contact Us

Please sent an e-mail to lishifenging@outlook.com