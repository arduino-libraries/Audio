# Audio Library

## Methods

### `begin()`

#### Description

Initializes the Audio library by specifying the target sample rate and size of the audio buffer.


#### Syntax

Audio.begin(rate, size);


#### Parameters

rate (int) : the sample rate of the sound file. If stereo, double the rate (ex. 44100 Hz stereo = 88200).

size (int) : the size of the audio buffer in milliseconds.


#### Returns

nothing


#### Example

```
/*

 Demonstrates the use of the Audio library for the Arduino Due

 Hardware required :
 *Arduino shield with a SD card on CS 4 (the Ethernet shield will work)
 *Audio amplifier circuit with speaker attached to DAC0

 Original by Massimo Banzi September 20, 2012
 Modified by Scott Fitzgerald October 19, 2012

*/

#include <SD.h>
#include <SPI.h>
#include <Audio.h>

void setup()
{
  // debug output at 9600 baud
  Serial.begin(9600);

  // setup SD-card
  Serial.print("Initializing SD card...");
  if (!SD.begin(4)) {
    Serial.println(" failed!");
    return;
  }
  Serial.println(" done.");
  // hi-speed SPI transfers
  SPI.setClockDivider(4);

  // 44100 Hz stereo => 88200 sample rate
  // 100 mSec of prebuffering.
  Audio.begin(88200, 100);
}

void loop()
{
  int count=0;

  // open wave file from sdcard
  File myFile = SD.open("test.wav");
  if (!myFile) {
    // if the file didn't open, print an error and stop
    Serial.println("error opening test.wav");
    while (true);
  }

  const int S=1024; // Number of samples to read in block
  short buffer[S];

  Serial.print("Playing");
  // until the file is not finished
  while (myFile.available()) {
    // read from the file into buffer
    myFile.read(buffer, sizeof(buffer));

    // Prepare samples
    int volume = 1024;
    Audio.prepare(buffer, S, volume);
    // Feed samples to audio
    Audio.write(buffer, S);

    // Every 100 block print a '.'
    count++;
    if (count == 100) {
      Serial.print(".");
      count = 0;
    }
  }
  myFile.close();

  Serial.println("End of file. Thank you for listening!");
  while (true) ;
}
```

### `prepare()`

#### Description

Prepares audio samples from the named file to the audio buffer, and sets the volume.


#### Syntax

Audio.prepare(buffer, samples, volume);


#### Parameters

buffer (short) : the named buffer holding the audio file.

samples (int) : number of samples to write

volume (int) : a 10-bit number representing the volume of the audio being played. 0 turns the sound off, 1023 is the maximum volume.


#### Returns

nothing


#### Example

```
/*

 Demonstrates the use of the Audio library for the Arduino Due

 Hardware required :
 *Arduino shield with a SD card on CS 4 (the Ethernet shield will work)
 *Audio amplifier circuit with speaker attached to DAC0

 Original by Massimo Banzi September 20, 2012
 Modified by Scott Fitzgerald October 19, 2012

*/

#include <SD.h>
#include <SPI.h>
#include <Audio.h>

void setup()
{
  // debug output at 9600 baud
  Serial.begin(9600);

  // setup SD-card
  Serial.print("Initializing SD card...");
  if (!SD.begin(4)) {
    Serial.println(" failed!");
    return;
  }
  Serial.println(" done.");
  // hi-speed SPI transfers
  SPI.setClockDivider(4);

  // 44100 Hz stereo => 88200 sample rate
  // 100 mSec of prebuffering.
  Audio.begin(88200, 100);
}

void loop()
{
  int count=0;

  // open wave file from sdcard
  File myFile = SD.open("test.wav");
  if (!myFile) {
    // if the file didn't open, print an error and stop
    Serial.println("error opening test.wav");
    while (true);
  }

  const int S=1024; // Number of samples to read in block
  short buffer[S];

  Serial.print("Playing");
  // until the file is not finished
  while (myFile.available()) {
    // read from the file into buffer
    myFile.read(buffer, sizeof(buffer));

    // Prepare samples
    int volume = 1024;
    Audio.prepare(buffer, S, volume);
    // Feed samples to audio
    Audio.write(buffer, S);

    // Every 100 block print a '.'
    count++;
    if (count == 100) {
      Serial.print(".");
      count = 0;
    }
  }
  myFile.close();

  Serial.println("End of file. Thank you for listening!");
  while (true) ;
}
```

### `write()`

#### Description

Writes an audio signal from a buffer to DAC0 and DAC1.


#### Syntax

Audio.write(buffer, length);


#### Parameters

buffer (short) : the named buffer with the audio file.

length (int) : number of samples to write


#### Returns

nothing


#### Example

```
/*

 Demonstrates the use of the Audio library for the Arduino Due

 Hardware required :
 *Arduino shield with a SD card on CS 4 (the Ethernet shield will work)
 *Audio amplifier circuit with speaker attached to DAC0

 Original by Massimo Banzi September 20, 2012
 Modified by Scott Fitzgerald October 19, 2012

*/

#include <SD.h>
#include <SPI.h>
#include <Audio.h>

void setup()
{
  // debug output at 9600 baud
  Serial.begin(9600);

  // setup SD-card
  Serial.print("Initializing SD card...");
  if (!SD.begin(4)) {
    Serial.println(" failed!");
    return;
  }
  Serial.println(" done.");
  // hi-speed SPI transfers
  SPI.setClockDivider(4);

  // 44100 Hz stereo => 88200 sample rate
  // 100 mSec of prebuffering.
  Audio.begin(88200, 100);
}

void loop()
{
  int count=0;

  // open wave file from sdcard
  File myFile = SD.open("test.wav");
  if (!myFile) {
    // if the file didn't open, print an error and stop
    Serial.println("error opening test.wav");
    while (true);
  }

  const int S=1024; // Number of samples to read in block
  short buffer[S];

  Serial.print("Playing");
  // until the file is not finished
  while (myFile.available()) {
    // read from the file into buffer
    myFile.read(buffer, sizeof(buffer));

    // Prepare samples
    int volume = 1024;
    Audio.prepare(buffer, S, volume);
    // Feed samples to audio
    Audio.write(buffer, S);

    // Every 100 block print a '.'
    count++;
    if (count == 100) {
      Serial.print(".");
      count = 0;
    }
  }
  myFile.close();

  Serial.println("End of file. Thank you for listening!");
  while (true) ;
}
```
