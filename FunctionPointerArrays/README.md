# Function Pointer Arrays in C

## Overview

Arrays of function pointers in C are a good option to replace jump tables.


## Check size of static table befor using

```C
void test(uint8_t const ump_index)
{
    static void (*pf[])(void) = {fna, fnb, fnc, ..., fnz};

    if (jump_index < sizeof(pf) / sizeof(*pf))
    {
        /* Call the function specified by jump_index */
        pf[jump_index]();
    }
}
```

Static keeps jump table private inside the function. An unsigned index keeps the bounds check simple. Testing the index befor using it ensures that only valid calls are made. This approach is just as secure as a switch stagement (which may or not be turned into a jump table.)

Now to place the table in flash (e.g. ROM). Read complex declarations like this backwards (i.e., from right to left).

```C
static void (* const pf[])(void) = {fna, fnb, fnc, ..., fnz};
```


## Keypads on Screens

```C
#define N_SCREENS  16
#define N_KEYS     6

/* Prototypes for functions that appear in the jump table */
INT fnUp(void);
INT fnDown(void);
...
INT fnMenu(void);
INT fnNull(void);

INT keypress(uint8_t key, uint8_t screen)
{
    static INT (* const pf[N_SCREENS][N_KEYS])(void) = 
    {
        {fnUp, fnDown, fnNull, ..., fnMenu},
        {fnMenu, fnNull, ..., fnHome},
        ...
        {fnF0, fnF1, ..., fnF5} 
    };

    assert (key < N_KEYS);
    assert( screen < N_SCREENS);

    /* Call the function and return result */
    return (*pf[screen][key])();	
}

/* Dummy function - used as an array filler */
INT fnNull(void)
{
    return 0; 
}
```

Prototye to defend against using wrong parameters and returning worng type. Replaced run-time test with compile time test (i.e., assert).

## More

see

https://barrgroup.com/Embedded-Systems/How-To/C-Function-Pointers
