# FairinoDH-Modbus-Communication
Instructions for controlling DH grippers via Modbus through the Fairino control box

### Required Items
 - Fairino Robot
 - DH end effector
 - Pigtail adapter cable (included with gripper)

To begin, you will go to the Fairino WebApp interface and navigate to "Application -> Tool App -> Peripheral Protocol". Then select "Modbus master" from the dropdown menu and hit "Set".
The next step is to connect the 485A wire of the pigtail cable to the 485-A0 port of the control box for the Fairino. You will do the same for the 485B wire to the 485-B0 port.

Next is to connect the 24V and GND wires of the pigtail cable to either an external power supply, or connect the 24V to a DO port on the control box and the GND wire to a GND or E-0V port.

Once the gripper has power, you can control it using the functions ModbusRegRead(), ModbusRegGetData(), and ModbusRegWrite().

### ModbusRegRead(fun_code, reg_add, reg_num, 1, 0)
This function is to poll the gripper; it does NOT return the values read (this is done by GetData)
- fun_code: The function code; 0x01 (coil), 0x02 (discrete input), 0x03 (holding register), 0x04 (input register)
- reg_add: Register start address; obtained from gripper manual (available on DH website)
- reg_num: Number of consecutive registers to read from, starting from reg_add (i.e. to just read reg_add, reg_num = 1)

### ModbusRegGetData(reg_num, 0)
This function returns the values obtained by ModbusRegRead() in an array
- reg_num: Number of register values to return from most recent ModbusRegRead() call, starting from reg_add

### ModbusRegWrite(fun_code, reg_add, reg_num, reg_value, 1, 0)
This function writes values to desired addresses on the gripper
- fun_code: The function code; 0x05 (single coil), 0x06 (single register), 0x0F (multiple coils), 0x10 (multiple registers)
- reg_add: Register start address
- reg_num: Number of consecutive addresses to write to
- reg_value: Array containing values to write to registers
