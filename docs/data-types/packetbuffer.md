#PacketBuffer

##Overview

The [MicroBitRadio](../ubit/radio.md) class provides direct micro:bit to micro:bit communication.

It is often beneficial in both wired and wireless communication protocols to send
and receive data in a raw format, viewed as an ordered sequence of bytes.

This class provides a basic set of functions for the creation, manipulation and
accessing of a managed type for byte arrays used to hold network data packets.

!!! note
    This is a **managed type**. This means that it will automatically use and release memory as needed. There is no need for you to explicitly free or release memory when your done - the memory will be freed as soon as the last piece of code stops using the data.


## Creating PacketBuffers

`PacketBuffer`s are simple to create - just create them like a variable, and provide the size (in bytes) of the buffer you want to create.

```cpp
PacketBuffer b(16);
```

Alternatively, if you already have an array of bytes allocated, then you can simply refer to that using a slightly different form:

```cpp
uint8_t data[16];
PacketBuffer b(data,16);
```

## Manipulating PacketBuffers

Once created, the data inside `PacketBuffer`s can be freely changed at any time. The simplest way to do this is through the array operators `[` and `]`.

You can read or write bytes to the buffer by simply dereferencing it with square bracket.

For example: to create, set and send a PacketBuffer on the micro:bit radio, you could do the following:
```cpp
PacketBuffer b(2);
b[0] = 255;
b[1] = 10;

uBit.radio.datagram.send(b);
```

`PacketBuffer`s can also be assigned a value:
```cpp
PacketBuffer b;
b = uBit.radio.datagram.recv();
```

If you need more granular access, the `getBytes` function provides direct access to the memory buffer, presented as a byte array:
```cpp
PacketBuffer b(16);
uint8_t *buf = b.getBytes();

memcpy(buf, "HI", 2);
```

Finally, it is also possible to extract the received signal strength of a packet from a `PacketBuffer`.

This can provide both a rough indication of the reliability of the link, and a crude but moderately effective mechanism to estimate the distance between two micro:bits.

```cpp
PacketBuffer b;
b = uBit.radio.datagram.recv();

uBit.display.scroll(b.getRSSI());
```

#API
[comment]: <> ({"className":"ManagedString"})
##Constructor
<br/>
####ManagedString( <div style='color:#a71d5d; display:inline-block'>StringData  *</div> ptr)
#####Description
Constructor. Create a managed string from a specially prepared string literal.  

 


#####Parameters

>  <div style='color:#a71d5d; display:inline-block'>StringData  *</div> ptr - The literal - first two bytes should be 0xff, then the length in little endian, then the literal. The literal has to be 4-byte aligned.
#####Example
```cpp
 static const char hello[] __attribute__ ((aligned (4))) = "\xff\xff\x05\x00" "Hello"; 
 ManagedString s((StringData*)(void*)hello); 
```
##leakData
<br/>
####<div style='color:#a71d5d; display:inline-block'>StringData</div> <div style='color:#795da3; display:inline-block'>leakData</div>()
#####Description
Get current ptr, do not decr() it, and set the current instance to empty string.  

 This is to be used by specialized runtimes which pass  StringData  around.           


##Constructor
<br/>
####ManagedString( <div style='color:#a71d5d; display:inline-block'>const char *</div> str)
#####Description
Constructor.  

 Create a managed string from a pointer to an 8-bit character buffer.  

 The buffer is copied to ensure safe memory management (the supplied character buffer may be declared on the stack for instance).  

 


#####Parameters

>  <div style='color:#a71d5d; display:inline-block'>const char *</div> str - The character array on which to base the new  ManagedString .
#####Example
```cpp
 ManagedString s("abcdefg"); 
```
<br/>
####ManagedString( <div style='color:#a71d5d; display:inline-block'>const int</div> value)
#####Description
Constructor.  

 Create a managed string from a given integer.  

 


#####Parameters

>  <div style='color:#a71d5d; display:inline-block'>const int</div> value - The integer from which to create the  ManagedString .
#####Example
```cpp
 ManagedString s(20); 
```
<br/>
####ManagedString( <div style='color:#a71d5d; display:inline-block'>const char</div> value)
#####Description
Constructor. Create a managed string from a given char.  

 


#####Parameters

>  <div style='color:#a71d5d; display:inline-block'>const char</div> value - The character from which to create the  ManagedString .
#####Example
```cpp
 ManagedString s('a'); 
```
<br/>
####ManagedString( <div style='color:#a71d5d; display:inline-block'>PacketBuffer</div> buffer)
#####Description
Constructor. Create a  ManagedString  from a  PacketBuffer . All bytes in the  PacketBuffer  are added to the  ManagedString .  

 


#####Parameters

>  <div style='color:#a71d5d; display:inline-block'>PacketBuffer</div> buffer - The PacktBuffer from which to create the  ManagedString .
#####Example
```cpp
 ManagedString s = radio.datagram.recv(); 
```
<br/>
####ManagedString( <div style='color:#a71d5d; display:inline-block'>const char *</div> str,  <div style='color:#a71d5d; display:inline-block'>const int16_t</div> length)
#####Description
Constructor. Create a  ManagedString  from a pointer to an 8-bit character buffer of a given length.  

 The buffer is copied to ensure sane memory management (the supplied character buffer may be declared on the stack for instance).  

 


#####Parameters

>  <div style='color:#a71d5d; display:inline-block'>const char *</div> str - The character array on which to base the new  ManagedString .

>  <div style='color:#a71d5d; display:inline-block'>const int16_t</div> length - The length of the character array
#####Example
```cpp
 ManagedString s("abcdefg",7); 
```
<br/>
####ManagedString( <div style='color:#a71d5d; display:inline-block'>const  ManagedString  &</div> s)
#####Description
Copy constructor. Makes a new  ManagedString  identical to the one supplied.  

 Shares the character buffer and reference count with the supplied  ManagedString .  

 


#####Parameters

>  <div style='color:#a71d5d; display:inline-block'>const  ManagedString  &</div> s - The  ManagedString  to copy.
#####Example
```cpp
 ManagedString s("abcdefg"); 
 ManagedString p(s); 
```
<br/>
####ManagedString()
#####Description
Default constructor.  

 Create an empty  ManagedString .  

 


#####Example
```cpp
 ManagedString s(); 
```
##operator=
<br/>
####<div style='color:#a71d5d; display:inline-block'>ManagedString</div> <div style='color:#795da3; display:inline-block'>operator=</div>( <div style='color:#a71d5d; display:inline-block'>const  ManagedString  &</div> s)
#####Description
Copy assign operation.  

 Called when one  ManagedString  is assigned the value of another.  

 If the  ManagedString  being assigned is already refering to a character buffer, decrement the reference count and free up the buffer as necessary.  

 Then, update our character buffer to refer to that of the supplied  ManagedString , and increase its reference count.  

 


#####Parameters

>  <div style='color:#a71d5d; display:inline-block'>const  ManagedString  &</div> s - The  ManagedString  to copy.
#####Example
```cpp
 ManagedString s("abcd"); 
 ManagedString p("efgh"); 
 p = s // p now points to s, s' ref is incremented 
```
##operator==
<br/>
####<div style='color:#a71d5d; display:inline-block'>bool</div> <div style='color:#795da3; display:inline-block'>operator==</div>( <div style='color:#a71d5d; display:inline-block'>const  ManagedString  &</div> s)
#####Description
Equality operation.  

 Called when one  ManagedString  is tested to be equal to another using the '==' operator.  

 


#####Parameters

>  <div style='color:#a71d5d; display:inline-block'>const  ManagedString  &</div> s - The  ManagedString  to test ourselves against.
#####Returns
true if this  ManagedString  is identical to the one supplied, false otherwise.
#####Example
```cpp
 MicroBitDisplay display; 
 ManagedString s("abcd"); 
 ManagedString p("efgh"); 
 
 if(p == s) 
 display.scroll("We are the same!"); 
 else 
 display.scroll("We are different!"); //p is not equal to s - this will be called 
```
##operator<
<br/>
####<div style='color:#a71d5d; display:inline-block'>bool</div> <div style='color:#795da3; display:inline-block'>operator<</div>( <div style='color:#a71d5d; display:inline-block'>const  ManagedString  &</div> s)
#####Description
Inequality operation.  

 Called when one  ManagedString  is tested to be less than another using the '<' operator.  

 


#####Parameters

>  <div style='color:#a71d5d; display:inline-block'>const  ManagedString  &</div> s - The  ManagedString  to test ourselves against.
#####Returns
true if this  ManagedString  is alphabetically less than to the one supplied, false otherwise.
#####Example
```cpp
 MicroBitDisplay display; 
 ManagedString s("a"); 
 ManagedString p("b"); 
 
 if(s < p) 
 display.scroll("a is before b!"); //a is before b 
 else 
 display.scroll("b is before a!"); 
```
##operator>
<br/>
####<div style='color:#a71d5d; display:inline-block'>bool</div> <div style='color:#795da3; display:inline-block'>operator></div>( <div style='color:#a71d5d; display:inline-block'>const  ManagedString  &</div> s)
#####Description
Inequality operation.  

 Called when one  ManagedString  is tested to be greater than another using the '>' operator.  

 


#####Parameters

>  <div style='color:#a71d5d; display:inline-block'>const  ManagedString  &</div> s - The  ManagedString  to test ourselves against.
#####Returns
true if this  ManagedString  is alphabetically greater than to the one supplied, false otherwise.
#####Example
```cpp
 MicroBitDisplay display; 
 ManagedString s("a"); 
 ManagedString p("b"); 
 
 if(p>a) 
 display.scroll("b is after a!"); //b is after a 
 else 
 display.scroll("a is after b!"); 
```
##substring
<br/>
####<div style='color:#a71d5d; display:inline-block'>ManagedString</div> <div style='color:#795da3; display:inline-block'>substring</div>( <div style='color:#a71d5d; display:inline-block'>int16_t</div> start,  <div style='color:#a71d5d; display:inline-block'>int16_t</div> length)
#####Description
Extracts a  ManagedString  from this string, at the position provided.  

 


#####Parameters

>  <div style='color:#a71d5d; display:inline-block'>int16_t</div> start - The index of the first character to extract, indexed from zero.

>  <div style='color:#a71d5d; display:inline-block'>int16_t</div> length - The number of characters to extract from the start position
#####Returns
a  ManagedString  representing the requested substring.
#####Example
```cpp
 MicroBitDisplay display; 
 ManagedString s("abcdefg"); 
 
 display.scroll(s.substring(0,2)) // displays "ab" 
```
##operator+
<br/>
####<div style='color:#a71d5d; display:inline-block'>ManagedString</div> <div style='color:#795da3; display:inline-block'>operator+</div>( <div style='color:#a71d5d; display:inline-block'>ManagedString  &</div> s)
#####Description
Concatenates this string with the one provided.  

 


#####Parameters

>  <div style='color:#a71d5d; display:inline-block'>ManagedString  &</div> s - The  ManagedString  to concatenate.
#####Returns
a new  ManagedString  representing the joined strings.
#####Example
```cpp
 MicroBitDisplay display; 
 ManagedString s("abcd"); 
 ManagedString p("efgh") 
 
 display.scroll(s + p) // scrolls "abcdefgh" 
```
##charAt
<br/>
####<div style='color:#a71d5d; display:inline-block'>char</div> <div style='color:#795da3; display:inline-block'>charAt</div>( <div style='color:#a71d5d; display:inline-block'>int16_t</div> index)
#####Description
Provides a character value at a given position in the string, indexed from zero.  

 


#####Parameters

>  <div style='color:#a71d5d; display:inline-block'>int16_t</div> index - The position of the character to return.
#####Returns
the character at posisiton index, zero if index is invalid.
#####Example
```cpp
 MicroBitDisplay display; 
 ManagedString s("abcd"); 
 
 display.scroll(s.charAt(1)) // scrolls "b" 
```
##toCharArray
<br/>
####<div style='color:#a71d5d; display:inline-block'>const char *</div> <div style='color:#795da3; display:inline-block'>toCharArray</div>()
#####Description
Provides an immutable 8 bit wide character buffer representing this string.  

 


#####Returns
a pointer to the character buffer. 
##length
<br/>
####<div style='color:#a71d5d; display:inline-block'>int16_t</div> <div style='color:#795da3; display:inline-block'>length</div>()
#####Description
Determines the length of this  ManagedString  in characters.  

 


#####Returns
the length of the string in characters.
#####Example
```cpp
 MicroBitDisplay display; 
 ManagedString s("abcd"); 
 
 display.scroll(s.length()) // scrolls "4" 
```
____
[comment]: <> ({"end":"ManagedString"})
