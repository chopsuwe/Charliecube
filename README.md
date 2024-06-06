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



