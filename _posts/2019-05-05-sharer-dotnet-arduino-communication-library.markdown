---
layout: post
title:  Sharer is an Arduino and .NET communication library
date:   2019-05-05
image:  '/images/Sharer-NET-Arduino-communication-library.jpg'
tags:   Arduino .NET
hn_id: 
hn_link: 
reddit_id: 1589817094
reddit_link: https://www.reddit.com/r/arduino/comments/fr7khr/arduino_net_serial_communication_library_to/
repo: Sharer
---
Sharer is both a <b>.NET and an Arduino Library</b>. It allows a desktop application to <b>read/write variables</b> and <b>remote call functions on Arduino</b>, using the Sharer protocole accross a serial communication.
Sharer has initially been developped for the [Ballcuber project](https://ballcuber.github.io){:target="_blank"}, but it is now a standalone library ;).

<p align="center">
	<a href="https://github.com/Rufus31415/Sharer/releases">
		<img alt="Download arduino library and examples" src="https://img.shields.io/badge/▼-Download%20Arduino%20library%20and%20examples-blue?logo=arduino&style=for-the-badge" />
	</a>
	<a href="https://github.com/Rufus31415/Sharer.NET/releases">
		<img alt="Download .NET library and example" src="https://img.shields.io/badge/▼-Download%20.NET%20Library%20and%20examples-green?logo=c%20sharp&style=for-the-badge" />
	</a>
	<br/>
	<a href="https://www.nuget.org/packages/Sharer">
		<img alt="Download Nuget library" src="https://img.shields.io/badge/▼-Download%20Nuget%20.NET%20Library-20038d?logo=nuget&style=for-the-badge" />
	</a>
</p>

# Overview 🧐
### How to connect

``` cs
// C#
var connection = new SharerConnection("COM3", 115200);
connection.Connect();
```

### How to call a function
``` cs
// C# - My Arduino code has a function : int Sum(int a, byte b);
var result = connection.Call("Sum", 10, 12);
// result.Status : OK
// result.Type : int
// result.Value : 22
```

``` cs
// C# - Read all digital pins : digitalRead(0) to digitalRead(13)
for(int i=0; i <= 13; i++){
	var digitalValue = connection.Call("digitalRead", i);
	// digitalValue.Status : OK
	// digitalValue.Type : bool
	// digitalValue.Value : true or false
}
```
``` cs
// C# - Definition of all functions
var functions = connection.Functions;
// functions[0].Name : function name
// functions[0].ReturnType : enum void, int, long, ...
// functions[0].Arguments[0].Name // Name of first argument
// functions[0].Arguments[0].Type // enum void, int, long, ...
```

### How to write a variables
``` cs
// C#
connection.WriteVariable("myVar", 15); // returns true if writting OK
```
``` cs
// C# - Write simultaneously several variables by passing a List<WriteVariable>()
connection.WriteVariables(listOfVariablesToWrite);
```
``` cs
// C# - Definition of all variables
var variables = connection.Variables;
// variables[0].Name : variable name
// variables[0].Type : enum int, long, ...
```

### How to read variables
``` cs
// C#
var value = connection.ReadVariable("myVar");
// value.Status : OK
// value.Value : 12
```
``` cs
// C# - Read simultaneously several variables
var values = connection.ReadVariables(new string[] {"myVar", "anotherVar", "yetAnother"});
```

### Get board information
``` cs
// C#
var info = connection.GetInfos();
// info.Board : Arduino UNO
// info.CPUFrequency : 16000000 (Hz)
// a lot more : info.CPlusPlusVersion, info.FunctionMaxCount, info.VariableMaxCount, ...
```

### Receive and send custom messages
``` cs
// C#
connection.WriteUserData("Hello!");
connection.WriteUserData(12.2);
connection.WriteUserData(new byte[] {0x12, 0x25, 0xFF});

// Event raised when new user data sent by Arduino
connection.UserDataReceived += UserDataReceived;
```


### Arduino code
``` cpp
// C++ Arduino
#include <Sharer.h>

// A simple function that sums an integer and a byte and return an integer
int Sum(int a, byte b) {
	return a + b;
}

// a simple integer
int myVar = 25;

void setup() {
	Sharer.init(115200); // Init Serial communication with 115200 bauds

	// Expose this function to Sharer : int Sum(int a, byte b) 
	Sharer_ShareFunction(int, Sum, int, a, byte, b);
	
	// Share system functions
	Sharer_ShareFunction(bool, digitalRead, uint8_t, pin);
	Sharer_ShareVoid(pinMode, uint8_t, pin);

	// Expose this variable to Sharer so that the desktop application can read/write it
	Sharer_ShareVariable(int, myVar);
}

// Run Sharer engine in the main Loop
void loop() {
	Sharer.run();
}
```

# Get Started 💪
## Sources
Sharer is divided into 2 repositories, one for the Arduino sources and the other for .NET sources
- Arduino : [https://github.com/Rufus31415/Sharer](https://github.com/Rufus31415/Sharer){:target="_blank"}
- .NET : [https://github.com/Rufus31415/Sharer.NET](https://github.com/Rufus31415/Sharer.NET){:target="_blank"}

## Download and install Arduino library
### Library manager
Sharer is available in the library manager available in the menu ```Tools/Manage Libraries...```. Just look for Sharer and install the latest version.

![Sharer in library manager](https://raw.githubusercontent.com/Rufus31415/Sharer/master/Resources/arduino-package-manager.png)


### Direct download
Please download the Sharer library archive : https://github.com/Rufus31415/Sharer/releases/latest/download/Sharer.zip

Sharer has been tested with Arduino UNO, NANO, MEGA and DUE. It may work with other boards.
Extract so that you get a Sharer directory in your Arduino "libraries" directory : ```C:\Program Files (x86)\Arduino\libraries\Sharer```

You can then enjoy the examples by restarting your Arduino IDE and go to menu ```File / examples / Sharer```.

## Sharer .NET
### Nuget
Sharer.NET is available on nuget : [https://www.nuget.org/packages/Sharer/](https://www.nuget.org/packages/Sharer/){:target="_blank"}

Use this command line to install it with you package manager :

```Install-Package Sharer```

### Download .NET assembly
Sharer.dll assembly can be downloaded here : [https://github.com/Rufus31415/Sharer.net/releases/latest/download/SharerAsssemblies.zip](https://github.com/Rufus31415/Sharer.net/releases/latest/download/SharerAsssemblies.zip)

This archive contains the nuget package Sharer.nupkg and Sharer.dll compiled in AnyCPU Release for the following targets:
- NET Framework 3.5, 4.0, 4.5.1, 4.5.2, 4.6, 4.6.1, 4.6.2, 4.7, 4.7.1, 4.7.2, 4.8
- NET Core 2.0, 2.1, 2.2, 3.0, 3.1
- NET Standard 2.1

### Try examples
#### Windows Forms
Windows Forms example requires .NET Framework 3.5. It can be downloaded here : [https://github.com/Rufus31415/Sharer.net/releases/latest/download/SharerWindowsFormsExample.zip](https://github.com/Rufus31415/Sharer.net/releases/latest/download/SharerWindowsFormsExample.zip)

![Winforms example](https://raw.githubusercontent.com/Rufus31415/Sharer.NET/master/Resources/winforms.png)

#### Console cross-plateform example
The console example run with .NET Core 3.0. But you don't need any runtime to execute it. The standalone console examples are available here :
- [Windows 64 bits](https://github.com/Rufus31415/Sharer.net/releases/latest/download/SharerConsoleExample_win-x64.zip)
- [Windows 32 bits](https://github.com/Rufus31415/Sharer.net/releases/latest/download/SharerConsoleExample_win-x86.zip)
- [Windows ARM (for example Windows IOT for Raspberry PI)](https://github.com/Rufus31415/Sharer.net/releases/latest/download/SharerConsoleExample_win-arm.zip)
- [Linux ARM (for example Raspbian for Raspberry PI)](https://github.com/Rufus31415/Sharer.net/releases/latest/download/SharerConsoleExample_linux-arm.zip)
- [Linux 64 bits](https://github.com/Rufus31415/Sharer.net/releases/latest/download/SharerConsoleExample_linux-x64.zip)
- [Mac OS X](https://github.com/Rufus31415/Sharer.net/releases/latest/download/SharerConsoleExample_osx-x64.zip)

![Console example](https://raw.githubusercontent.com/Rufus31415/Sharer.NET/master/Resources/console.gif)

# License: MIT 😘
© Rufus31415
See the [license file](https://github.com/Rufus31415/Sharer.NET/blob/master/LICENSE) for details.


# Usage 🛠️
## Arduino Usage
### Initialization
The header file <Sharer.h> should be included. 

In function ```setup()```, Sharer is initialized by passing a baudrate to function ```Sharer.init(...)```. It internally call ```Serial.init()``` with the same baudrate.

In function ```loop()```, ```Sharer.run()``` should be called. It runs the internal kernel of Sharer that decodes commands received.
``` cpp
#include <Sharer.h>

void setup() {
	Sharer.init(115200);
}

void loop() {
	Sharer.run();
}
```

You also can initialize Sharer with another stream, such as the Serial2 of Arduino DUE if you use Serial2 to communication with the desktop application :
``` cpp
void setup() {
	// Initialize with another Serial interface
	Serial2.begin(9600);
	Sharer.init(Serial2);
}
```

### Share variables
To add a variable in the shared variable list, you should call the macro ```Sharer_ShareVariable```. Its first argument is the type of the variable, and the second is its name. This macro allows Sharer to have a pointer to the variable, its name, its type and its memory size.
``` cpp
byte myByteVar;
long myLongVar;
int myIntArray[2];

void setup() {
	Sharer.init(115200);
	
	Sharer_ShareVariable(byte, myByteVar);
	Sharer_ShareVariable(long, myLongVar);
	Sharer_ShareVariable(int, myIntArray[0]);
	Sharer_ShareVariable(int, myIntArray[1]);
}
```

### Share functions with a return type
To add a variable in the shared function list, you should call the macro ```Sharer_ShareFunction```. Its first argument is the returned type, and the second is its name. Following arguments of the macro describe the argument of the shared function by its type and its name. There is no limit in the number of argument you can share, but all arguments should be shared.

You can share your own functions, but all system functions like analogRead, digitalRead, digitalWrite, millis, ...

``` cpp
int Sum(int a, byte b) {
	return a + b;
}

void setup() {
	Sharer.init(115200);

	// Share your function to Sharer : int Sum(int a, byte b) 
	Sharer_ShareFunction(int, Sum, int, a, byte, b);
	
	// Sharer system functions
	Sharer_ShareFunction(int, analogRead, uint8_t, pin);
	Sharer_ShareFunction(bool, digitalRead, uint8_t, pin);
}
```

### Share void functions
Void functions are functions without returned type. You should call the macro ```Sharer_ShareVoid``` to share a void function. The first argument is its name, followed by type and name of each arguments.

``` cpp
void TurnLEDOn(void) {
	pinMode(13, OUTPUT);
	digitalWrite(13, true);
}

void SetLEDState(bool state) {
	pinMode(13, OUTPUT);
	digitalWrite(13, state);
}

void setup() {
	Sharer.init(115200);
	
	// Share your void fuctions
	Sharer_ShareVoid(TurnLEDOn);
	Sharer_ShareVoid(SetLEDState, bool, state);
	
	// Sharer system void functions
	Sharer_ShareVoid(digitalWrite, uint8_t, pin);
}
```

### Receive and send regular serial messages
Sharer class inherits from [Stream](https://www.arduino.cc/reference/en/language/functions/communication/stream/), so you can use the following functions. Please, never use Sharer.print(), Sharer.println() or Sharer.write() inside a shared function, writting woud be ignored.

``` cpp
void loop() {
	Sharer.run();
	
	 //gets the number of bytes available in the stream
	int available = Sharer.available();
	
	// reads last reveided byte (-1 if no data to read)
	int lastByte = Sharer.read();
	
	// Empty the stream
	Sharer.flush();
	
	// reads data from the serial buffer until the target is found
	// returns, true if data is found
	bool found = Sharer.find("string to find");
	
	//Returns the next byte of incoming serial data without removing it from the internal serial buffer
	// Successive calls to peek() will return the same character
	int nextByte = Sharer.peek();

	// reads characters from the serial buffer into a String
	String str = Sharer.readString();

	// Looks for the next valid integer in the incoming serial
	int lastInt = Sharer.parseInt();
	lastInt = Sharer.parseInt(SKIP_ALL); // all characters other than digits or a minus sign are ignored when scanning the stream for an integer. This is the default mode
	lastInt = Sharer.parseInt(SKIP_NONE); // nothing is skipped, and the stream is not touched unless the first waiting character is valid.
	lastInt = Sharer.parseInt(SKIP_WHITESPACE); // only tabs, spaces, line feeds, and carriage returns are skipped.
	
	// returns the first valid floating point number from the Serial buffer
	float lastFloat = Sharer.parseFloat();
	lastFloat = Sharer.parseFloat(SKIP_ALL); // all characters other than a minus sign, decimal point, or digits are ignored when scanning the stream for a floating point number. This is the default mode.
	lastFloat = Sharer.parseFloat(SKIP_NONE); // Nothing is skipped, and the stream is not touched unless the first waiting character is valid.
	lastFloat = Sharer.parseFloat(SKIP_WHITESPACE); // only tabs, spaces, line feeds, and carriage returns are skipped.
	
	// Prints ASCII encoded string
	Sharer.print(85); // sends the string "85"
	Sharer.print(1.23456); // sends the string "1.23"
	Sharer.print('N'); // sends the string "N"
	Sharer.print("Hello world."); // sends the string "Hello world."
	Sharer.print(78, BIN); // sends the string "1001110"
	Sharer.print(78, OCT); // sends the string "116"
	Sharer.print(78, DEC); // sends the string "78"
	Sharer.print(78, HEX); // sends the string "4E"
	Sharer.print(1.23456, 0); // sends the string "1"
	Sharer.print(1.23456, 2); // sends the string "1.23"
	Sharer.print(1.23456, 4); // sends the string "1.2345"
	Sharer.println("Hello ;)"); // send the string and a new line "Hello ;)\n"
	Sharer.println(); // just sends a new line
	
	// Write a single byte
	Sharer.write(0x12);
}
```

### Capabilities
You can change the limits of Sharer by editing the constants of file ```C:\Program Files (x86)\Arduino\libraries\Sharer\src\SharerConfig.h```.
``` cpp
// maximum number of shared functions
#define _SHARER_MAX_FUNCTION_COUNT		16

// maximum number of shared variables
#define _SHARER_MAX_VARIABLE_COUNT		32

// circle buffer size for user serial message
#define _SHARER_USER_RECEIVE_BUFFER_SIZE	64

// maximum number of call to Serial.read() each time Sharer.Run() is called
#define _SHARER_MAX_BYTE_READ_PER_RUN		1000
```

## .NET usage
You can find here the full documentation of Sharer.NET : [/Sharer.NET/Sharer.NET.Documentation.md](https://github.com/Rufus31415/Sharer.NET/blob/master/Sharer.NET/Sharer.NET.Documentation.md).

# Performances 🚀
A command takes less than 10ms to be called on a Arduino Uno (command + response) at 115200 bauds. The protocole is optimized in order not to have strings to decode, just a binary stream to interprete, byte by byte.
Memory footprint has been optimized by using F() macro and PROGMEM to store names of variable, function and arguments in flash.

# How it works 🤯
Sharer uses a unique protocole called the Sharer Protocole. Every serial commands received by the Arduino are interprated.

![Sharer](https://raw.githubusercontent.com/Rufus31415/Sharer.NET/master/Resources/RemoteFunctionCall.png)


# Improvement ideas and contributions 🧠
I have some ideas to extend Sharer features like :
- Develop Sharer client for other languages like Python and Node.js
- Add LabView and Matlab examples for research purposes
- Add other transport layers like TCP and UDP
- Add a HTTP REST API based on Sharer that exposes endpoints to call functions, and read/write variables.

If you are interested in helping me with Sharer development, I will be happy to receive feature requests. Fork is also welcome ;)
