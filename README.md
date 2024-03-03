# Microplate-Reader-Protocol

The serial communication protocol used between the [Microplate-Reader-Software](https://github.com/UBC-MECH2024-Capstone-Team14/Microplate-Reader-Software) and [Microplate-Reader-Firmware](https://github.com/UBC-MECH2024-Capstone-Team14/Microplate-Reader-Hardware)

## Packet Structures

The communication follows a command-reply structure. The software sends a command packet to the firmware, and the firmware replies with the reply packet(s).

### Command Packet Structure

The command packet is an ASCII string that starts with a "/", followed by a command and optional data strings, and ends with "\n", separated by a single space " ".

```
╭────────── /
│ ╭──────── command
│ │    ╭─── [data...]
│╭┴─╮  │ ╭─ \n
/move_abs 123↵
```

The format of `[data]` varies depending on the command.

### Reply Packet Structure

The reply packet is an ASCII string that starts with a "@", followed by a command and optional data strings, and ends with "\n", separated by a single space " ".

```
╭────────── /
│ ╭──────── command
│ │    ╭─── [data...]
│╭┴─╮  │ ╭─ \n
@move_abs 123↵
```

The format of `[data]` varies depending on the command.

Unless otherwise specified, the reply packet is sent after the operation is complete.

## Commands

### /echo

Echo back the data.

```
/echo 123 456
@echo 123 456
```

### /home

```
/home
@home
```

### /move_abs

```
/move_abs 123
@move_abs 123
```

<!-- ### /move_rel

```
/move_rel -123
@move_rel -123
``` -->

### /set_row_pos

```
/set_row_pos 10 20 30 40 50 60 70 80 90 100 110 120
@set_row_pos 10 20 30 40 50 60 70 80 90 100 110 120
```

### /set_led_pwr

```
/set_led_pwr 10 20 30 40 50 60 70 80
@set_led_pwr 10 20 30 40 50 60 70 80
```

### /scan_well

```
/scan_well 0 1
@scan_well 0 1 123
```

The first two numbers are the row and column of the well, with the index starting at 0. The third number is the LED intensity of the well in the command packet and the intensity of the well in the reply packet.

### /scan_all

```
/scan_all
@scan_well 0 0 123
@scan_well 0 1 123
@scan_well 0 2 123
...
```

The reply will be a series of `@scan_well` reply packets, one for each well.
