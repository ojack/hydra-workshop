# Workshop: Code and Democratic Tools
Intersections, Feminism, Technology & Digital Humanities network (IFTe http://ifte.network/)

13.11.2020

led by [Olivia Jack](https://ojack.xyz)

## Intro to hydra
Hydra is a browser-based platform for live coding visuals, in which each connected browser window can be used as a node of a modular and distributed video synthesizer.

Built using WebRTC (peer-to-peer web streaming) and WegGL, hydra allows each connected browser/device/person to output a video signal or stream, and receive and modify streams from other browsers/devices/people. The API is inspired by analog modular synthesis, in which multiple visual sources (oscillators, cameras, application windows, other connected windows) can be transformed, modulated, and composited via combining sequences of functions.

## Getting started

Go to [https://hydra.ojack.xyz](https://hydra.ojack.xyz) (works best using Chrome or Chromium) and follow the instructions on screen.

Other editor commands:
* CTRL-Enter: run a line of code
* CTRL-Shift-Enter: run all code on screen (saves code to the URL)
* ALT-Enter: run a block
* CTRL-Shift-H: hide or show code
* CTRL-Shift-F: format code using [Prettier](https://prettier.io/)
* CTRL-Shift-S: Save screenshot and download as local file
* CTRL-Shift-G: Share to the user gallery (if available). Shares to [@hydra_patterns](https://twitter.com/hydra_patterns)


### Basic functions
render an oscillator with parameters frequency, sync, and rgb offset. Type the following into the editor and then press 'CTRL-shift-Enter'
```
osc(2, 0.1, 0.8).out()
```
osc() renders the oscillator, and .out() tells the computer to send the output to the screen.

rotate the oscillator 0.8 radians:
```
osc(20, 0.1, 0.8).rotate(0.8).out()
```
pixelate the output of the above function:
```
osc(20, 0.1, 0.8).rotate(0.8).pixelate(20, 30).out()
```

Try experimenting with any of the functions under the 'source', 'geometry' or 'color' categories [here](https://ojack.xyz/hydra-functions/).
Each chain of functions must always start with a 'source' function, and then can be followed by any number of 'geometry' and 'color' functions.
```
gradient().rotate(0, 2.6).repeat(3, 3, 0.5).out()
```

#### Using external sources
In addition to internal sources for , such as osc(), shape(), gradient(),
hydra can use external sources such as a webcam, window of another application, video,

There are four source buffers that can be used to connect to outside visuals, named s0, s1, s2, s3.

init webcam in source buffer 's0':
```
s0.initCam() // initialize a webcam in source buffer s0
```

render the output of s0 to the screen
```
src(s0).out()
```

webcam kaleidoscope:
```
s0.initCam() // initialize a webcam in source buffer s0
src(s0).kaleid(4).out() // render the webcam to a kaleidoscope
```
note that 's0.initCam()' only needs to be called once in order to initialize the camera

You can also use a browser tab or application window as an input texture (chrome only):
```
s1.initScreen()
src(s1).out()
```

render screen tab:
```
s0.initScreen()
src(s0).out()
```


### Blending sources together

By default, the environment contains four separate output buffers that can each render different graphics.  The outputs are accessed by the variables o0, o1, o2, and o3.

render to output buffer o1:
```
osc(8, 0.1, 0.3).out(o1)
render(o1) // render the contents of o1
```
If no output is specified in out(), the graphics are rendered to buffer o0.
to show all render buffers at once:
```
render()
```

Render the camera to output buffer o0, and an oscillator to output buffer o1:
```
s0.initCam() // initialize a webcam in source buffer s0
src(s0).out(o0) // render the webcam in buffer o0

osc(8, 0.1, 0.3).out(o1)

render() // show all four outputs
```

The output buffers can then be mixed and composited to produce what is shown on the screen.
Start with buffer o0, blend with o1, and show the result at o2:
```
src(o0).blend(o1).out(o2)
```

The composite functions blend(), diff(), mult(), and add() perform arithmetic operations to combine the input texture color with the base texture color, similar to photoshop blend modes. See 'blend' functions at: https://ojack.xyz/hydra-functions/

You can also composite multiple sources together within the same output (no need to render to a separate buffer):
```
osc(10)
  .rotate(0.5)
  .diff(osc(200))
  .out()
```

### Modulation

While blending functions combine the colors of two sources, modulation functions use the colors of one texture to affect the coordinates of the other texture.
This creates a sort of displacement or warping effect.

modulate(texture, amount) uses the red and green channels of the input texture to modify the x and y coordinates of the base texture. More about modulation at: https://lumen-app.com/guide/modulation/
```
osc(21, 0).modulate(o1).out(o0)
osc(40).rotate(1.57).out(o1)
```

You can also composite multiple sources together:
```
osc(10)
  .rotate(0.5)
  .diff(osc(200))
  .out()
```

## Connecting to remote streams
Any hydra instance can use other instances/windows containing hydra as input sources, as long as they are connected to the internet and not blocked by a firewall. Hydra uses webrtc (real time webstreaming) under the hood to share video streams between open windows. The included module rtc-patch-bay manages connections between connected windows, and can also be used as a standalone module to convert any website into a source within hydra.

To begin, open hydra simultaneously in two separate windows.
In one of the windows, set a name for the given patch-bay source:
```
pb.setName("myGraphics")
```
The title of the window should change to the name entered in setName().

From the other window, initiate "myGraphics" as a source stream.
```
s0.initStream("myGraphics")
```
render to screen:
```
s0.initStream("myGraphics")
src(s0).out()
```
The connections sometimes take a few seconds to be established; open the browser console to see progress.
To list available sources, type the following in the console:
```
pb.list()
```

## Links and resources

#### Hydra resources:
* [list of hydra functions](https://ojack.xyz/hydra-functions/)
* [documentation on github](https://github.com/ojack/hydra)
* [tutorials and examples](https://github.com/ojack/hydra/tree/master/examples)
* [gallery of user-generated sketches](https://twitter.com/hydra_patterns?lang=es)
* [Hydra Book](https://hydra-book.naotohieda.com/#/)
* [a talk about the motivations for creating hydra](https://www.youtube.com/watch?v=cw7tPDrFIQg).

 #### Libraries and tools used:
 * [Regl: functional webgl](http://regl.party/)
 * glitch.io: hosting for sandbox signalling server
 * codemirror: browser-based text editor
 * simple-peer

 ### Inspiration:
 * Space-Time Dynamics in Video Feedback (1984). [video](https://www.youtube.com/watch?v=B4Kn3djJMCE) and [paper](http://csc.ucdavis.edu/~cmg/papers/Crutchfield.PhysicaD1984.pdf) by Jim Crutchfield about using analog video feedback to model complex systems.
 * [Satellite Arts Project (1977) - Kit Galloway and Sherrie Rabinowitz](http://www.ecafe.com/getty/SA/)
 * [Sandin Image Processor](http://www.audiovisualizers.com/toolshak/vidsynth/sandin/sandin.htm)
 * [kynd - reactive buffers experiment](https://kynd.github.io/reactive_buffers_experiment/)

 #### Related projects:
 * [Lumen app (osx application)](https://lumen-app.com/)
 * [Vsynth (package for MaxMSP)](https://cycling74.com/forums/vsynth-package)
 * [VEDA (VJ system within atom)](https://veda.gl/)
 * [The Force](https://videodromm.com/The_Force/)
