# portmidi
Want to output to an MIDI device or listen your MIDI device as an input? This
package contains Go bindings for PortMidi. `libportmidi` is required as a
dependency. In order to start, go get this repository:
~~~ go
go get github.com/rakyll/portmidi
~~~
~~~ go
import "github.com/rakyll/portmidi"
~~~

## Usage

### Initialize
~~~ go
portmidi.Initialize()
~~~

### About MIDI Devices

~~~ go
portmidi.CountDevices() // returns the number of MIDI devices
portmidi.GetDeviceInfo(deviceId) // returns info about a MIDI device
portmidi.GetDefaultInputDeviceId() // returns the ID of the system default input
portmidi.GetDefaultOutputDeviceId() // returns the ID of the system default output
~~~

### Write to a MIDI Device

~~~ go
out, err := portmidi.NewOutputStream(deviceId, 1024, 0)

// note on events to play C# minor chord
out.WriteShort(0x90, 60, 100)
out.WriteShort(0x90, 64, 100)
out.WriteShort(0x90, 67, 100)

// notes will be sustained for 2 seconds
time.Sleep(2 * time.Second)

// note off events
out.WriteShort(0x80, 60, 100)
out.WriteShort(0x80, 64, 100)
out.WriteShort(0x80, 67, 100)

out.Close()
~~~

### Read from a MIDI Device
~~~ go
in, err := portmidi.NewInputStream(deviceId, 1024)
events, err := in.Read(1024)

// alternatively you can filter the input to listen
// only a particular set of channels
in.SetChannelMask(portmidi.Channel(1) | portmidi.Channel.(2))
in.Read(1024) // will retrieve events from channel 1 and 2

in.Close()
~~~

### Cleanup
Cleanup your input and output streams once you're done. Likely to be called on graceful termination.
~~~ go
portmidi.Terminate()
~~~
    
## License
    Copyright 2013 Google Inc. All Rights Reserved.
    
    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at
    
         http://www.apache.org/licenses/LICENSE-2.0
    
    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
