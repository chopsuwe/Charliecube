Current version: Unmodified copy of Asher Glick's master branch. 

Coming soon: 

# The charliecube is a minimal parts count 4x4x4 RGB LED cube. 
It uses Charlie Plexing to eliminate the need for the shift registers, buffers and other parts in traditional designs. This dramatically simplifies construction and reduces parts count. The only parts required are a single Arduino board with 16 digital output pins and the LEDS. Original code and build instructions curtsey of Asher Glick at https://aglick.com/charliecube.html. 

# Changes and Improvements to the original code

- Bug fix - failure to progress to the next animation.
- Wiring fix - Vertical layer offset to fix my wiring mistake (optional).
- Additional animations - by Fishofgold at https://github.com/Fishofgold/4x4x4-RGB-LED-Cube-

**Visual improvements** (See comments in the code for more detail).
- Randomly select the animation colour.
- Randomly select the animation speed.
- Randomly select the next animation.
- Randomly select the max animation time before changing to another pattern.
- Better speed consistency across all animations.

See comments in the code for more detail.

Short video of my cube in action https://www.youtube.com/watch?v=0nbVw5vNdXA


## Bug fix - Failure to advance to the next anumation
Occasionally the cube fails to advance to the next animation. This is most likely to occur when a complex animation is running and the processor workload is high. Two bug fixes have been posted in on Asher Glick's GitHub, I haven't done any testing to determine which solution is best, although I suspect mine tackles the root of the the problem.

### My Solution - greater than or equal to:
The original code checks to see if animationTimer is *equal* to animationMax and sets the flag to move to the next animation. High workload can cause the processor to miss the point when the variables are equal. From that point onward the value of animationTimer will always be greater than (not equal to) animationMax so the flag will never be set.

The solution is to change the line of code to evaluate if animationTimer is *greater than or equal to* animationMax.

The corrected code is:

```
ISR(TIMER1_OVF_vect) {
  animationTimer++;
  if (animationTimer >= animationMax) {   // change the equal == to greater than or equal to >= 
    continuePattern = false;
    animationTimer=0;
  }
}
```

### simeon2020's alternative solution - volatile variable: 
The compiler treats continuePattern as a normal variable when it should be volatile. Volatile causes the compiler to generate code that always reads the variable from RAM and does not "cache" the last read value in a register. Volatile should ALWAYS be used on any variable that can be modified an interrupt, or any external source. In our case it is the TIMER1_OVF_vect() ISR that is updating the 

Find the line in cubeplex.h that reads:

`bool continuePattern = false;`

Change it to:

`volatile bool continuePattern = false;`



## Wiring fix - Vertical layer offset
I made a mistake in the wiring resulted in the animation being offset downwards by two layers. 
This fix curtsey of WaLkeR from a long forgotten comment train. 

In function drawLed of cubeplex.h, find the line:

`void drawLed(int color, int brightness, int x, int y, int z) {`

Directly below that line, add the following code (uncomment as required)

```
  /********************* Layer offset fix *********************/
  /******** Move the layers upward one space - WaLkeR  ********/
  // z++;
  // If (z==4) z = 0;

  /****** Move the layers upward two spaces - chopsuwe ********/
  /********** Fix for an error in my wiring *******************/  
  // z++;
  // z++;
  // if (z==4) z = 0;
  // if (z==5) z = 1;
  /****************** Layer offset fix - end ******************/
```


