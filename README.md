Download Link: https://assignmentchef.com/product/solved-cs-570-homework-assignment-4
<br>
<h1>2        Assignment</h1>




The aim of this assignment is to use recursion and <em>backtracking</em> to find a path through a maze. If you are attempting to walk through a maze, you will probably walk down a path as far as you can go. Eventually, you will reach your destination or you won’t be able to go any farther. If you can’t go any farther, you will need to consider alternative paths. Therefore we need to be able to systematically perform trial and error search. <em>Backtracking</em> is a way for doing just this. It is a systematic, nonrepetitive approach to trying alternative paths and eliminating them if they don’t work. Recursion allows you to implement backtracking in a relatively straightforward manner. Each activation frame is used to remember the choice that was made at that particular decision point. After returning from a recursive call you can perform other recursive calls to try out different paths.




<h2>2.1      The Maze</h2>




The maze itself will consist of a two dimensional grid of colored cells. The maze will start at the cell (0,0) and the exit point is the cell (getNCols()-1,getNRows()-1). The operations getNCols() and getNRows() are methods of the class TwoDimGrid described below. All cells that can be part of the path will be in the NON_BACKGROUND color. All the cells that represent barriers




1

(a)                                         (b)                                         (c)




<h3>Figure 1: Sample grids</h3>




and cannot be part of the path will be in the BACKGROUND color. To keep track of a cell that we have visited, we will set it to the TEMPORARY color. If we find a path, all cells on the path will be reset the PATH color. So there are a total of four possible colors. Initially, the maze has all cells set to color BACKGORUND, as depicted in Fig. 1. The values we will use for each of these constants is:




<ul>

 <li>Green for PATH;</li>

</ul>




<ul>

 <li>White for BACKGROUND;</li>

</ul>




<ul>

 <li>Red for NON_BACKGROUND; and</li>

</ul>




<ul>

 <li>Black for TEMPORARY.</li>

</ul>




This is specified in the interface GridColors described below.




<h2>2.2      Class Layout</h2>




You are provided with a stub which may be downloaded from Canvas. It consists of the following classes and interfaces:




<ul>

 <li>Class TwoDimGrid. This class implements the two dimensional grid. It is used by the class Maze, which has a data field of this type. You will not need to modify this class in your code.</li>

</ul>




<ul>

 <li>Interface GridColors. This interface simply assigns names to the various colors that a cell can have. Here is the code:</li>

</ul>

<table width="496">

 <tbody>

  <tr>

   <td width="496"><strong>i m p o r t                  </strong>java . awt . Color ; <strong>p u b l i c i n t e r f a c e</strong>       G r i d C o l o r s  {</td>

  </tr>

  <tr>

   <td width="496">Color  PATH  =  Color . green ;</td>

  </tr>

  <tr>

   <td width="496">Color  B A C K G R O U N D  =  Color . white ;</td>

  </tr>

 </tbody>

</table>




2




4




<table width="496">

 <tbody>

  <tr>

   <td width="29"></td>

   <td width="467">Color N O N _ B A C K G R O U N D = Color .</td>

  </tr>

  <tr>

   <td width="29">}</td>

   <td width="467">red ; Color TE MP O RA RY = Color . black ;</td>

  </tr>

 </tbody>

</table>

6

8




<ul>

 <li>Class Maze. This is the class which you have to modify by implementing your search algorithm. It has a private field maze of type TwoDimGrid, where the two dimensional grid            is stored.</li>

</ul>

The method you have to implement is <strong>public boolean</strong> findMazePath(<strong>int</strong> x, <strong>int</strong> y). Also, you shall be asked to implement some variations of this algorithm, each of which will result in a new method. The details of these methods are described in Sec. 3 together with a description of other methods of this class that you will need.




<ul>

 <li>Class MazeTest. This class provides a graphical interface to test your algorithm. This should be the main class of your project. The main method of this class:</li>

</ul>




<ol>

 <li>asks you for the size of the grid,</li>

</ol>




<ol start="2">

 <li>displays an initial grid where all cells have been set to color BACKGORUND (see Fig. 1(a) which depicts a 5 by 5 grid), and</li>

 <li>allows you to edit it in order to indicate which cells are potentially considered to be part of a path.</li>

</ol>




Regarding this last item, you may indicate which cells can be part of a path by clicking on them and changing their color to the color NON_BACKGROUND. It is the cells of color NON_BACKGROUND that can be part of a path. For example, in Fig 1(b) we see an example

<sub> </sub>of cell selection, the cells in red may form part of a path, but the others do not participate. Once you have selected the cells that can be part of a path, you can press the “solve” button. This will call your Maze.findMazePath method and report the results. Fig. 1(c) shows the result of solving the maze. The cells in green form the path. The cells in black are ones which were visited while exploring for a solution. The ones in red were      not visited.

You can always start over by clicking on the “reset” button.




<h1>3        Problems</h1>




<h2>3.1      Problem 1</h2>




Implement a recursive algorithm

<strong>public boolean </strong>findMazePath(<strong>int </strong>x,<strong> int </strong>y)




that returns true if a path is found. When you click the button “solve”, the method MazeTest.solve calls the wrapper method Maze.findMazePath(), that in turn calls your algo-rithm with parameters x and y set both to 0. In order to implement your algorithm, take the following into consideration:







3

<ul>

 <li>If the current cell being analyzed falls outside the grid, then <strong>false</strong> should be returned since there is no possible path through that cell.</li>

</ul>




<ul>

 <li>If the current cell being analyzed does not have NON_BACKGROUND, then <strong>false</strong> should be returned since there is no possible path through that cell. To determine the color of a cell use getColor(<strong>int</strong> x, <strong>int</strong> y) of the class TwoDimGrid, which is the type of the data field maze.</li>

</ul>




<ul>

 <li>If the current cell being analyzed is the exit cell, then</li>

</ul>




<ul>

 <li>The cell must be painted color PATH. For that you may use the recolor(<strong>int </strong>x, <strong>int </strong>y, Color c)</li>

</ul>

<sub>                           </sub>method from the class TwoDimGrid, which is the type of the data field maze.

<ul>

 <li>You should return <strong>true</strong>.</li>

</ul>




<ul>

 <li>Otherwise the current cell may be assumed to be part of the path and hence</li>

</ul>




<ul>

 <li>It should be painted color PATH.</li>

 <li>Each of the four neighboring cells must be explored to see if they too are part of a path (i.e. they are the cells where the path “continues”). This is where</li>

</ul>

recursion takes place. In fact, multiple recursive calls, one for each neighbor. <strong>– </strong>If the result of exploring at least one of the neighbors is successful, then a path has been found. Otherwise, the cell is not part of a path: it is a dead end. Hence it must be marked so that it is not visited again. For that it must be colored to color TEMPORARY.




<h2>3.2      Problem 2</h2>




Implement a recursive algorithm

<strong>public </strong>ArrayList&lt;ArrayList&lt;PairInt&gt;&gt; findAllMazePaths(<strong>int </strong>x,<strong> int </strong>y)

by modifying the solution of Problem 1 so that a list of all the solutions to the maze is returned. Each solution may be represented as a list of coordinates. If there are no solutions, then the resulting list should have the empty list as only element. Some examples follow.




<strong>3.2.1    Examples </strong>




As an example, for the grid in Figure 2(a), it should report the following output since there are two paths to the exit:




<table width="528">

 <tbody>

  <tr>

   <td width="73">[[(0 ,0) ,</td>

   <td width="48">(0 ,1) ,</td>

   <td width="47">(1 ,1) ,</td>

   <td width="48">(2 ,1) ,</td>

   <td width="48">(3 ,1) ,</td>

   <td width="47">(4 ,1) ,</td>

   <td width="48">(4 ,2) ,</td>

   <td width="52">(4 ,3) ,</td>

   <td width="118">(4 ,4)] ,</td>

  </tr>

  <tr>

   <td width="73">[(0 ,0) ,</td>

   <td width="48">(0 ,1) ,</td>

   <td width="47">(1 ,1) ,</td>

   <td width="48">(2 ,1) ,</td>

   <td width="48">(2 ,2) ,</td>

   <td width="47">(2 ,3) ,</td>

   <td width="48">(3 ,3) ,</td>

   <td width="52">(4 ,3) ,</td>

   <td width="118">(4 ,4)]]</td>

  </tr>

 </tbody>

</table>




2

For the grid in Figure 2(b), it should report the following output:




<table width="576">

 <tbody>

  <tr>

   <td width="73">[[(0 ,0) ,</td>

   <td width="48">(1 ,0) ,</td>

   <td width="47">(2 ,0) ,</td>

   <td width="48">(3 ,0) ,</td>

   <td width="48">(4 ,0) ,</td>

   <td width="47">(4 ,1) ,</td>

   <td width="48">(4 ,2) ,</td>

   <td width="218">(4 ,3) , (4 ,4)] ,</td>

  </tr>

  <tr>

   <td width="73">[(0 ,0) ,</td>

   <td width="48">(1 ,0) ,</td>

   <td width="47">(2 ,0) ,</td>

   <td width="48">(2 ,1) ,</td>

   <td width="48">(2 ,2) ,</td>

   <td width="47">(3 ,2) ,</td>

   <td width="48">(4 ,2) ,</td>

   <td width="218">(4 ,3) , (4 ,4)] ,</td>

  </tr>

  <tr>

   <td width="73">[(0 ,0) ,</td>

   <td width="48">(0 ,1) ,</td>

   <td width="47">(0 ,2) ,</td>

   <td width="48">(1 ,2) ,</td>

   <td width="48">(2 ,2) ,</td>

   <td width="47">(3 ,2) ,</td>

   <td width="48">(4 ,2) ,</td>

   <td width="218">(4 ,3) , (4 ,4)] ,</td>

  </tr>

  <tr>

   <td width="73">[(0 ,0) ,</td>

   <td width="48">(0 ,1) ,</td>

   <td width="47">(0 ,2) ,</td>

   <td width="48">(1 ,2) ,</td>

   <td width="48">(2 ,2) ,</td>

   <td width="47">(2 ,1) ,</td>

   <td width="48">(2 ,0) ,</td>

   <td width="218">(3 ,0) , (4 ,0) ,  (4 ,1) ,  (4 ,2) , (4 ,3) ,  (4 ,4)]]</td>

  </tr>

 </tbody>

</table>




2




4

For the grid in Figure 2(c), it should report the following output since there are no paths to the exit:

(a)                                               (b)                                                (c)




<h3>Figure 2: More sample grids</h3>







<sup> </sup><sub>            </sub>[[]]

The class PairInt is a class you should define and which encodes pairs of integers that represent coordinates. The operations it should support are described below (copy should   return a new copy of a PairInt)

<strong>PairInt </strong>




<sub>                                                         </sub>private int x

private int y




<sub>                                                         </sub>public PairInt(int x, int y)

public int getX()   public int getY()   public void setX(int x)        public void setY(int y)        public boolean equals(Object p)   public String toString()

public PairInt copy()




<strong>3.2.2    Hints </strong>




<ul>

 <li>Think about implementing the following helper method</li>

</ul>

<strong>public void </strong>findMazePathStackBased(<strong>int </strong>x,<strong> int </strong>y, ArrayList&lt;ArrayList&lt;PairInt&gt;&gt; result, Stack&lt;PairInt&gt; trace)

where:




<ul>

 <li>(x,y) are the coordinates currently being visited</li>

</ul>




5

<ul>

 <li>result is the list of successful paths recorded up to now</li>

 <li>trace is the trace of the current path being explored</li>

</ul>

and then defining findAllMazePaths as:




<table width="496">

 <tbody>

  <tr>

   <td width="22"></td>

   <td width="474"><strong>p u b l i c</strong>ArrayList &lt; ArrayList &lt; PairInt &gt; &gt;  f i n d A l l M a z e P a t h s ( <strong>int</strong>  x ,  <strong>int</strong>  y )  { ArrayList &lt; ArrayList &lt; PairInt &gt; &gt;  result  =  <strong>new</strong>  ArrayList &lt; &gt;();</td>

  </tr>

  <tr>

   <td width="22"></td>

   <td width="474">        Stack &lt; PairInt &gt;  trace  =  <strong>new</strong>             Stack &lt; &gt;();f i n d M a z e P a t h S t a c k B a s e d (0 ,0 , result , trace</td>

  </tr>

  <tr>

   <td width="22">}</td>

   <td width="474">); <strong>r e t u r n </strong>result ;</td>

  </tr>

 </tbody>

</table>




2




4

6




<ul>

 <li>When the exit has been reached the contents of the stack should be copied into a list (for that you may use the addAll method of lists which also accepts stacks – any Collection in fact) and placed in result.</li>

</ul>




<ul>

 <li>The colors may be ignored in this exercise <em>except</em> for their use to avoid visiting nodes that have already been visited. For that only one color is necessary to mark a cell (choose PATH to mark cells that have been visited). In the recursive call you should mark the cell with color PATH before calling your helper function recursively on neighboring cells.</li>

</ul>




<ul>

 <li>In the recursive call, <em>after</em> returning from visiting the neighbors, you should <em>remove</em> the mark (by coloring the cell with the NON_BACKGROUND color) so that this cell may be visited again after backtracking.</li>

</ul>




<h2>3.3      Problem 3</h2>




Adapt <strong>boolean</strong> Maze.findMazePath() so that it returns the shortest path in the list of paths. The resulting method should be called

<strong>public </strong>ArrayList&lt;PairInt&gt; findMazePathMin(<strong>int </strong>x,<strong> int </strong>y)





