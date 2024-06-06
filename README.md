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


**Vertical layer offset fix**
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



