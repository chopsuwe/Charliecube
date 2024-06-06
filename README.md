Current version: Unmodified copy of Asher Glick's master branch. 

Coming soon: 
The charliecube is a 4x4x4 RGB LED cube that uses Charlie Plexing to eliminate the need for additional shift regesters, buffers and the like. This dramatically simplifies construction and reduces parts count. The cube only requires a single Arduino board with 16 digital output pins. 

I modified Asher Glick's code to include
- Bug fix where the cube fails to progress to the next animation.
- Vertical layer offset to fix my wiring mistake. 
- Additional animations by Fishofgold https://github.com/Fishofgold/4x4x4-RGB-LED-Cube-  

- Randomly select next animation
- Randomly select animation colour
- Randomly select animation speed
- Randomly select max animation time (animationMax).
- Adjust the speed of individual animations (animationSpeed) for a more consistent visual experience

https://www.youtube.com/watch?v=0nbVw5vNdXA

**Bug fix - animation fails to advance**
A bug in the original software occasionally prevents the cube from advancing to the next animation. This is most likely to occur when a complex animation is running and the workload is high. Two bug fixes have been posted in on Asher Glick's GitHub, I haven't done any testing to determine which solution is best.

My Solution: The original code checks to see if animationTimer is *equal* to animationMax and sets the flag to move to the next animation. High workload can cause the processor to miss the point when the variables are equal. From that point onward the the value of animationTimer will always be greater than (not equal to) animationMax so the flag will never be set.

The solution is to change the line of code to evaluate if animationTimer is *greater than or equal to* animationMax.

The corrected code is:

ISR(TIMER1_OVF_vect) {
  animationTimer++;
  if (animationTimer >= animationMax) {   // change the equal == to greater than or equal to >= 
    continuePattern = false;
    animationTimer=0;
  }
}

Alternative solution from simeon2020: The compiler treats continuePattern as a normal variable when it should be volatile. Volatile causes the compiler to generate code that always reads the variable from RAM and does not "cache" the last read value in a register. Volatile should ALWAYS be used on any variable that can be modified an interrupt, or any external source. In our case it is the TIMER1_OVF_vect() ISR that is updating the 

Change the line in cubeplex.h that reads:

bool continuePattern = false;

should be changed to:

volatile bool continuePattern = false;



**Wiring fix - Vertical layer offset**
I made a mistake in the wiring resulted in the animation being offset downwards by two layers. 
Fix curtesy of WaLkeR in some long forgotten comment train. 

In function drawLed of cubeplex.h look for the line:

void drawLed(int color, int brightness, int x, int y, int z) {

Add the following code (uncomment as required)
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



