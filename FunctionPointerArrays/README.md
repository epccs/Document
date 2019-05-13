# Function Pointer Arrays in C

## Overview

Arrays of function pointers in C are a good option to replace jump tables.


## Check size of static table befor using

```C
typedef void (* const PFV_U8)(uint8_t); //pointer to function that takes 8 bit unsigned integer

void test(uint8_t const ump_index)
{
    static PFV_U8 pf[] = {fna, fnb, fnc, ..., fnz};

    if (jump_index < sizeof(pf) / sizeof(*pf))
    {
        /* Call the function specified by jump_index */
        pf[jump_index]();
    }
}
```

The function table is private inside the scope of the test function, but it also needs to be kept off the stack. The static keyword does what is needed. An unsigned index keeps the bounds check simple. Testing the index befor using it ensures that only valid calls are made. This approach is just as secure as a switch stagement (which may or not be turned into a jump table.) The const keyword allows the table to be placed in flash (e.g. ROM). 


## I2C commands

I want to use this method for my bus manager I2C slave command interface. I think it needs to be done in a few groups so I can have boundaries between some of the ideas. The IC2 command is received by an asynchronous interrupt-driven I2C state machine that after an I2C bus stop (or repeated start) condition passes control to a user-defined callback where a function table is desired.

The I2C slave needs to do the command's work during the receive-event. The receive-event callback is called from the context of an interrupt service routine and will clock stretch the I2C bus; clock stretching can affect an I2C master (e.g., on an OS like Linux SMbus techniques can help deal with the issue). The slave will echo back the bytes given, it will replace the bytes related to the command but the master has to send enough, or the reply may be incomplete. 

https://github.com/epccs/RPUpi/blob/master/Remote/i2c_cmds.c

```C
// The whole thing can be seen at https://github.com/epccs/RPUpi/tree/master/Remote

uint8_t i2c0Buffer[TWI0_BUFFER_LENGTH]; // def is in my lib/twi0.h
uint8_t i2c0BufferLength = 0;

#define GROUP  4
#define MGR_CMDS  8

// Prototypes for point 2 multipoint commands
void fnRdMgrAddr(uint8_t*, int);
void fnWtMgrAddr(uint8_t*, int);
void fnRdBootldAddr(uint8_t*, int);
void fnWtBootldAddr(uint8_t*, int);
void fnRdShtdnDtct(uint8_t*, int);
void fnWtShtdnDtct(uint8_t*, int);
void fnRdStatus(uint8_t*, int);
void fnWtStatus(uint8_t*, int);

// Prototypes for point 2 point commands
// TBD

// Prototypes for reserved (e.g. power management?) commands
// TBD

// Prototypes for test mode commands
// TBD

void receive_i2c_event(uint8_t* inBytes, int numBytes) 
{
    static void (*pf[GROUP][MGR_CMDS])(uint8_t*, int) = 
    {
        {fnRdMgrAddr, fnWtMgrAddr, fnRdBootldAddr, fnWtBootldAddr, fnRdShtdnDtct, fnWtShtdnDtct, fnRdStatus, fnWtStatus},
        {fnNull, fnNull, fnNull, fnNull, fnNull, fnNull, fnNull, fnNull},
        {fnNull, fnNull, fnNull, fnNull, fnNull, fnNull, fnNull, fnNull},
        {fnNull, fnNull, fnNull, fnNull, fnNull, fnNull, fnNull, fnNull}
    };

    // i2c will echo's back what was sent (plus modifications) with transmit event
    for(uint8_t i = 0; i < numBytes; ++i)
    {
        i2c0Buffer[i] = inBytes[i];    
    }
    i2c0BufferLength = numBytes;

    // mask the group bits (6 and 7) so they are alone then roll those bits to the left so they can be used as an index.
    uint8_t group;
    group = (i2c0Buffer[0] & 0xc0) >> 6;
    // if(group >= GROUP) return; // it can not happen

    // mask the command bits (0..5) so they can be used as an index.
    uint8_t command;
    command = i2c0Buffer[0] &3F;
    if(command >= MGR_CMDS) return; // not valid, do nothing but the echo.

    /* Call the function and return */
    (* pf[group][command])();
    return;	
}

/* Dummy function */
void fnNull(uint8_t* inBytes, int numBytes)
{
    return; 
}

// I2C_COMMAND_TO_READ_RPU_ADDRESS
void fnRdMgrAddr(uint8_t* inBytes, int numBytes)
{
    i2c0Buffer[1] = rpu_address; // '1' is 0x31
    local_mcu_is_rpu_aware =1; 
    
    // end the local mcu lockout. 
    if (localhost_active) 
    {
        // If the local host is active then broadcast on DTR pair
        uart_started_at = millis();
        uart_output = RPU_NORMAL_MODE;
        printf("%c", uart_output); 
        uart_has_TTL = 1; // causes host_is_foreign to be false
    }
    else 
        if (bootloader_started)
        {
            // If the bootloader_started has not timed out yet broadcast on DTR pair
            uart_started_at = millis();
            uart_output = RPU_NORMAL_MODE;
            printf("%c", uart_output); 
            uart_has_TTL = 0; // causes host_is_foreign to be true, so local DTR/RTS is not accepted
        } 
        else
        {
            lockout_started_at = millis() - LOCKOUT_DELAY;
            bootloader_started_at = millis() - BOOTLOADER_ACTIVE;
        }
}
```

Prototye to defend against using wrong parameters and returning worng type. Replaced run-time test with compile time test (i.e., assert).

## More

see

https://barrgroup.com/Embedded-Systems/How-To/C-Function-Pointers
