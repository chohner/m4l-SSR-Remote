# SSR Remote for Max for Live
Max-for-Live implementation for interfacing the SoundScapeRenderer network adaptor.
Allows the user to place a source in 2D space and let the SSR renderer (http://spatialaudio.net/ssr/) deal with the reproduction setup. It implements following playback modes:

* Wave Field Synthesis (WFS)
* Vector Base Amplitude Panning (VBAP)
* Ambisonics Amplitude Panning
* Near-field-corrected Higher-Order Ambisonics (NFC-HOA)
* (Dynamic) Binaural Synthesis
* (Dynamic) Binaural Room Synthesis (BRS)

## Installation
### Adding to Ableton Live user library
To easily access the m4l devices from Live, add both files `m4l_ssr_remote_master.amxd` and `m4l_ssr_remote_object.amxd` to the Live user library. The default location is `~/Music/Ableton/User Library/Presets/Audio Effects/Max Audio Effect`.

### Correct line ending
To satisfy SSR's line ending (binary zero, `\0`), the default has to be changed in the `mxj net.tcp.send` object. This has to be done only once.

You can either do this yourself by inserting a single line and recompiling or simply using the files provided in the `modified mxj net.tcp.send` folder (recommended). Read the provided README.txt for correct file locations and please back up your original files!

#### DYI 
If you don't want to use the modified java class, follow the step-by-step instructions below to do it yourself.

* Open `m4l_ssr_remote_master.amxd`, disable presentation mode, double click the `p tcpClientMXJ` object, and hit the `viewsource` message box.
* Inside the `send(Atom args[]) {}` declaration, you need to add the following line: 


```java
System.setProperty("line.separator", "\0");
```


* Go to `Java > Open Compile Window...` and hit `Compile`.
* Ideally you should receive no errors and can safely close all windows.

Below a snippet of the modified `mxj net.tcp.send` java source:

```java
send(Atom args[]) {
    	// Force binary 0 linebreak for SSR's TCP interface
		System.setProperty("line.separator", "\0");
		
		// Unmodified code below
		
    	declareIO(1, 3);
    	setInletAssist(0, "(anything) message to send to group");
    	setOutletAssist(0, "(anything) successfully sent data");
    	setOutletAssist(1, "(anything) data unsuccessfully sent");
    	setOutletAssist(2, "(int) number of active packets");
    	declareAttribute("address", "getAddress", "setAddress");
    	declareAttribute("port", "getPort", "setPort");
    	ts = new TcpSender();
    	ts.setDebugString("net.tcp.send");
    	ts.setSuccessCallback(this, "success");
    	ts.setFailureCallback(this, "failure");
}
```
#### Warning!
Please be aware that this does change the default behaviour of `net.tcp.send`. This may cause unexpected behaviour when using it in other patches.

#### Removing modifications
To reverse the line ending to the systems default, simply remove the pre-compiled files and restore the original files or remove the inserted line and recompile. A restart of Max might be required.

### Enabling audio
To route audio from Live to SSR, you need to add live input sources to SSR. A possible source declaration may look like this:
```XML
<source name="Source 1" model="point">
      <port>1</port>
      <position x="1" y="1"/>
</source>
```
You can then use Jack (see http://jackaudio.org or http://brewformulas.org/Jack) to easily send audio through virtual connections from Ableton Live into the virtual sources in SSR. Please note that SSR automatically enumerates the sources with increasing IDs, starting from 1.

## Usage
1. Place the master plugin (`m4l_ssr_remote_master.amxd`) on any channel - master channel is a solid choice.
2. Set the IP and port of SSR, as well as the number of sources (may be changed later).
3. Use the remote object plugin (`m4l_ssr_remote_object.amxd`) on any channel you want to control. Set the ID to the corresponding SSR object and start automating!

You can use both automation in the timeline as well as m4l devices such as the LFO to modulate the source position.

### Have fun!
Christoph Hohnerlein

mail@chrisclock.com

chohner@ccrma.stanford.edu
