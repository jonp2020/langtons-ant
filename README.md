# Langton's Ant

Langton's ant is simulation developed by Chris Langton in 1986. [Link to Wikipedia](https://en.wikipedia.org/wiki/Langton%27s_ant). The simulation follows some basic rules:
- An ant moves across a grid of squares one step at a time, either up, down, left or right.
- At a white square, turn 90° clockwise, flip the color of the square, move forward one unit.
- At a black square, turn 90° counter-clockwise, flip the color of the square, move forward one unit.
- In this version, if the ant attempts to move outside of the existing grid, the grid will expand to accomodate the movements of the ant.

This simulation was built using VueJS and set up using Vite.
     
## To get started

- Clone this repo to your machine
- cd into the directory where you cloned this repo on your machine e.g. ```cd langtons-ant```
- Run ```npm install``` in your terminal
- Run ```npm run dev``` in your terminal to start a local server
- Open localhost in your browser e.g. the http address stated in the terminal (or your can press 'h' for help and then 'o')
- Once the page has loaded, click the 'Start Simulation' button to start the Langton's Ant simulation and 'Pause Simulation' to pause it

You can change the size of the grid by increasing the 'gridProportionFromCenter' property that is returned in the data object. 'gridProportionFromCenter' represents the number of squares there all in all directions from the square in the centre of the grid. E.g. if 'gridProportionFromCenter' is set to 1, there will be 1 square to the left, right, above and below of a central square in the middle of the grid.  

Similarly, you can change the starting position of the ant by updating the 'antStartingPosition' property in the returned data object. Currently it is hardcoded it to "0_0", which represents the square in the centre of the grid. 
