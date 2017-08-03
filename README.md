# Creating Animations for the _Dynamic Wall_

Thanks for your interest in developing animations for the _Dynamic Wall_ at MoMath! This guide will help you get started and walk you through the process of submitting your creations for possible display in the Museum.

## Setting up the Development Environment

### Confirm that you have Java 8 (a.k.a. Java 1.8) installed

- To check your version number on Linux or OS X, open a terminal and enter `java -version`.
- To check your version number on Windows, follow [this page](https://www.java.com/en/download/help/version_manual.xml).
- If you don't have Java 8, download it from [here](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html).

### Install Processing 3.1.1

- Processing is "a flexible software sketchbook and a language for learning how to code within the context of the visual arts." You can find the download link [here](https://processing.org/download/?processing).
- If you want to pause here and try a simple introduction to Processing first, we recommend following [this](https://processing.org/tutorials/gettingstarted/) as a starting point. Since Processing builds off of Java, it will seem very familiar if you have a background in Java.

### Install ControlP5 and PeasyCam

1. Open Processing and go Sketch > Import Library ... > Add Library ...
2. Now search for and install "ControlP5" and "PeasyCam".

### Install MoMath’s DynamicWallLibrary developer kit

1. Download the DynamicWallLibrary folder from this Git repository.
2. Find your "Processing sketchbook directory" by going Processing > Preferences and looking under "Sketchbook location".
3. Go to that directory and you should see a directory called `libraries`.
4. Put the DynamicWallLibrary folder into `libraries`.

At this point, your directory structure inside your "Processing sketchbook directory" should look similar to the following:

```
- examples
- libraries
    - DynamicWallLibrary
        - examples
            - DynamicWallExample
        - library
            - DynamicWallLibrary.jar
            - base
            - gson
            - user
    - controlP5
        - library
            - controlP5.jar
    - peasycam
        - library
            - peasycam.jar
- modes
- templates
- tools
```
        
Don't worry if it doesn't match exactly. The important parts are the subfolders of `libraries`, and the `.jar` files in each `library` folder that matches the name of the parent folder it's in.

## Getting Started

1. Open Processing, or restart Processing if you just finished the set up section above.
2. Go File > Examples ... > Contributed Libraries > DynamicWallLibrary > double click DynamicWallExample
3. Go File > Save As ... > browse to your "Processing sketchbook directory" you found above, and save it with a name of your choice - this will be the name of your own sketch
4. Click the "Animation" tab that is to the right of the tab you are on.
5. Press the "Play" button (looks like a triangle).
6. If all went well, you should see a simulator of the _Dynamic Wall_ open! (If not, you should see an error message in red text get logged in the Processing app. This should put some light on the issue.)

You can now make modifications to the code, and press the Play button to see your changes!

**Important**: The animation of the _Dynamic Wall_ is controlled by the code written in the `Animation` tab. Only modify code in this tab! Changes to the other file will not be run in the MoMath environment. One exception for modifying the other tab is noted in [Controlling the Simulation](#controlling-the-simulation). 

## Structure of the Program

All code you write must appear in the class Animation. You may use inner classes if needed. The animation in the Animation class is structured around three methods: `setup()`, `update()`, and `exit()`. There are also metadata accessor functions.

The `setup()` method should be where you initialize your entire animation. For example, if you wanted all slats to start pushed back all the way, this would be specified in `setup()`. Note that this method can be empty. By default, the slats will start in the fully forward position. To be clear, the wall will instantiate your Animation class just once, and then call `setup()` each time it restarts the animation throughout the day.

The `update()` method will be run continuously through the course of the animation. Think of it as an infinite loop that will be terminated when the wall switches to a different animation. (For experienced Processing developers, `update()` should be compared to the `draw()` function.)

The `exit()` method must be included, but code included in it will not be run, so it is best to leave it empty.

At the beginning of the example program, you will see three variables with data about the animation: behaviorName, author, and description. 
**Note**: Please be sure to fill these in with information about your animation! These fields will be used to find your animation later on.

There are also three functions used to access these pieces of metadata: `getBehaviorName()`, `getAuthorName()`, `getDescription()`. Please do not change the contents of these functions!

## Controlling the Slats

There are 128 `DWSlat` objects, one for each of the slats making up the wall, which are preloaded into the array `wall.slats`. The slats are zero-indexed beginning with the leftmost slat.

Each slat has an individually controllable bottom and top depth, which can be set with using the methods `slat.setBottom()` or `slat.setTop()`, respectively, where `slat` is one of the slat objects in the `wall.slats` array. These functions take float values from 0.0 to 1.0, corresponding to fully back and fully forward, respectively. (Negative values are interpreted as 0.0, while values above 1.0 are interpreted as 1.0.)

For example, the following code will set the value of each of the slats to 1, i.e. bring all slats fully forward:

```
for (DWSlat slat : wall.slats) {
    slat.setBottom(1);
    slat.setTop(1);
}
```         

The following code will set the value of every even-indexed slat to 0:

```
for (int index = 0; index < wall.slats.length; index++) {
    if (index % 2 == 0) {
        DWSlat slat = wall.slats[index];
        slat.setBottom(0);
        slat.setTop(0);
    }
}
```         

There is a built-in variable, `deltaTime`, that corresponds to the amount of time, in microseconds, that has elapsed since the previous frame was rendered. You can also keep track of the time elapsed since starting your simulation with `pApplet.millis()`, which will return the time elapsed in milliseconds. These might be helpful for timing your animation.

## Controlling the Simulation

You can control the simulation in the following ways:

- Zoom in or out using your mouse's scroll wheel
- Rotate the simulation by clicking and dragging within the window
- View in 'block mode,' which changes the look of the slats in the simulation from lines to prisms, by pressing `b` on your keyboard. You can go back to 'line mode' by pressing `l`.
- Start and stop the animation as you wish by pressing `s` on your keyboard

The animation will run continuously until terminated by closing the simulation window, pressing the escape key, or clicking 'Stop' in the Processing window.

While testing your animation, take note of the speed at which the slats are moving. If they move too quickly, the wall will not respond as expected because the hardware limits the maximum speed.
To see the actual speed of the wall, you can edit the file in the other Processing tab (the one not `Animation`). This tab should be named the name of your sketch.
Change the framerate function in `setup()` from `framerate(30)` to `framerate(6)`, since the actual wall runs at ~6 FPS.
Note that though this will simulate the speed of the wall, it will also slow the zooming/panning effects of the simulator. Thus this change is only suggested for the final stages of development.

## Resources

- [Processing website](https://processing.org/) - Find the language reference, tutorials, examples, the Processing source code, and more.
- [Java 8 website](http://docs.oracle.com/javase/8/) - Note that you are not limited to just Processing's built in libraries. You can use any part of the Java standard library as well.

