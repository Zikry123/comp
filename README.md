

## Your task

You **must build and submit your solution** using the sample code we provide you in this repository, which is different from the original [UC Berlkley code base](https://inst.eecs.berkeley.edu/~cs188/fa18/project1.html). If you want to provide a report with your submission (e.g., reflections, acknowledgments, etc.), please do so in file [REPORT.md](REPORT.md).

* Please remember to complete the [STUDENT.md](STUDENT.md) file with your individual submission details (so we can identify you when it comes time to submit). 

* You should **only work and modify** files [search.py](search.py) and [searchAgents.py](searchAgents.py) in doing your solution. Do not change the other Python files in this distribution.

* Your code **must run _error-free_ on Python 3.6**. Staff will not debug/fix any code. Using a different version will risk your program not running with the Pacman infrastructure or autograder and may risk losing (all) marks. 
   * You can install Python 3.6 from the [official site](https://www.python.org/dev/peps/pep-0494/), or set up a [Conda environment](https://www.freecodecamp.org/news/why-you-need-python-environments-and-how-to-manage-them-with-conda-85f155f4353c/) or an environment with [PIP+virtualenv](https://uoa-eresearch.github.io/eresearch-cookbook/recipe/2014/11/26/python-virtual-env/). 

* Your code **must not have any personal information**, like your student number or your name. That info should go in the [STUDENT.md](STUDENT.md) file, as per instructions above. If you use an IDE that inserts your name, student number, or username, you should disable that.

* **Assignment 1 FAQ** is available to answer common questions you might have about the Assignment 1 on ED at https://edstem.org/au/courses/8178/discussion/716336

* **Getting started on GitHub** - the video below explains how to **clone**, **git add**, **commit** and **push** while developing your solution for this assignment:

[![How to work with github teams](img/loom_video.png)](https://www.loom.com/share/ae7e93ab8bec40be96b638c49081e3d9)



### Practice Task (0 marks)

To familiarize yourself with basic search algorithms and the Pacman environment, it is a good start to implement the tasks at https://inst.eecs.berkeley.edu/~cs188/fa18/project1.html, especially the first four tasks; however, there is no requirement to do so.

You should code your implementations *only* at the locations in the template code indicated by ```***YOUR CODE HERE***``` in files [search.py](search.py) and [searchAgents.py](searchAgents.py), please do not change code at any other locations or in any other files.

### Part 1 (3 marks)

Implement the **Enforced Hill Climbing algorithm** discussed in lectures, using Manhattan Distance as the heuristic, by inserting your code into the template indicated by comment ```***YOUR CODE HERE FOR TASK 1***```, you can view the location at this link: [search.py#L166](search.py#L166).

Note that you don't have to implement Manhattan Distance, as this has already been implemented for you in the template code, although you will need to call the heuristic from inside your search. You should be able to test the algorithm using the following command:

```
python pacman.py -l mediumMaze -p SearchAgent -a fn=ehc,heuristic=manhattanHeuristic
```

Other layouts are available in the layouts directory, and you can easily create you own. The `autograder` will try to validate your solution by looking for the exact output match.

### Part 2 (3 marks)

In this part we will prove that you have all the ingredients to pick up many new search algorithms, tapping to knowledge you acquired in the lectures and tutorials.

**Iterative Deepening A\* (IDA\*)** is a search algorithm that has not been introduced in the lectures but it relies on ingredients which you already know: 
*  **Iterative Deepening**, and 
* the evaluation function <img src="https://latex.codecogs.com/svg.image?f(n)&space;=&space;g(n)&space;&plus;&space;h(n)"/> used by **A\***. 

**IDA\*** is similar to Iterative Deepening, it does a limited depth first search but:
1. instead of initializing the limit <img src="https://latex.codecogs.com/svg.image?l_0=0"/>, it initialises the limit <img src="https://latex.codecogs.com/svg.image?l_0=f(s_0)"/> using the evaluation function of the initial state <img src="https://latex.codecogs.com/svg.image?s_0"/>, and
2. instead of updating the next limit <img src="https://latex.codecogs.com/svg.image?l_{i&plus;1}&space;=&space;l_{i}&space;&plus;&space;1"/> by one, it updates the next limit <img src="https://latex.codecogs.com/svg.image?l_{i&plus;1}"/> by looking at the minimum <img src="https://latex.codecogs.com/svg.image?f"/> value of a node that was pruned by the current limit <img src="https://latex.codecogs.com/svg.image?l_i"/>. Formally, lets define <img src="https://latex.codecogs.com/svg.image?P_i&space;=&space;\{&space;n&space;\&space;|&space;f(n)&space;>&space;l_i&space;\}"/> as the set of nodes pruned by limit <img src="https://latex.codecogs.com/svg.image?l_i"/>, then the update <img src="https://latex.codecogs.com/svg.image?l_{i&plus;1}&space;=&space;\min\limits_{n&space;\in&space;P_i}&space;f(n)"/>, where <img src="https://latex.codecogs.com/svg.image?f(n)&space;=&space;g(n)&space;&plus;&space;h(n)"/> is the same evaluation function as described in **A\***.

This allows IDA* to use the accumulated cost + heuristic  function in order to minimise the number of iterations running the limited depth first search. **IDA\*** is often used to find optimal solutions for memory-constrained problems.

Implement the **Iterative Deepening A\* algorithm** discussed above, using Manhattan Distance as the heuristic, by inserting your code into the template indicated by comment ```***YOUR CODE HERE FOR TASK 2***```, you can view the location at this link: [search.py#L179](search.py#L179). You should be able to test the algorithm using the following command:
```
python pacman.py -l mediumMaze2 -p SearchAgent -a fn=ida,heuristic=manhattanHeuristic
```
For this exercise, you can assume you must **not** check for cycles in the problem. Other layouts are available in the layouts directory, and you can easily create your own. The `autograder` will seek for exact match of the solution and the number node expansions. The successors list are expected to be visited in the original order given by the API. If you implement **IDA\*** using a recursive method, please **reverse** it. An example is given as follows:
```
succs = problem.getSuccessors(state)
succs.reverse()
```

### Part 3 (0 marks)

*This part worths 0 marks. It is now an example how we could generate the heuristics*

This part involves solving a more complicated problem. You will be able to model the problem, using the search algorithm (both Astar, which is provided and Iterative Deepening A* algorithm implemented by yourself in part 2) and design a heuristic function that can guide the search algorithm. 

Just like in Q7 of the Berkerley Pacman framework, you will be required to create an agent that will eat all of the food (dots) in a maze. However, to make it more interesting, the Capsule can provide energy to the Pacman. That is, when the Pacman enters a cell that contains a capsule, it will consume the capsule and gain energy, **causing this move to have 0 cost**. The capsule will disappear after being consumed by a Pacman.

In order to implement this, you should create a new problem called `CapsuleSearchProblem`. Some of the variables are listed in the comments and the initialization. You will need to design a way to represent the states of the problem. You will need to return the initial state through `getStartState` function. You will also need to have `isGoalState` to return true if the given state belongs to one of the goal states. In addition, you will need to implement your Applicability and transition function in `getSuccessors` function, which means to return a list of tuples that contains (`next state`, `action`, `cost`).

You will also need to implement a suitable `capsuleProblemHeuristic`. Your heuristic function needs to be **admissible**. You may choose to implement other helper classes/functions. You should insert your code into the template indicated by the comments ```***YOUR CODE HERE FOR TASK 3***```, you can view the locations at these links: [search.py#L117](search.py#L117) and 4 positions in [searchAgents.py](searchAgents.py).

You should be able to test your program by running the following commands (in one line):

```
python pacman.py -l capsuleSearch -p CapsuleSearchAgent
   -a fn=astar,prob=CapsuleSearchProblem,heuristic=capsuleProblemHeuristic
```

```
python pacman.py -l capsuleSearch -p CapsuleSearchAgent
   -a fn=ida,prob=CapsuleSearchProblem,heuristic=capsuleProblemHeuristic
```

The `autograder` seeks an optimal solution and the number of nodes expended (Please make sure you **do not** remove the code we use to count nodes expansion  in the `getSussessors` function). In addition, please make sure your heuristic is **admissible**, otherwise you might get 0 marks for this part due to either failed to find the optimal plan, or expanding more nodes than our baselines (zero heuristic).

#### Indicative marks IDA*

They are shown below for node expansions with **IDA\*** on the testing files [capsuleIDA1.test](test_cases_assignment1/part3-2/capsuleIDA1.test) and [capsuleIDA2.test](test_cases_assignment1/part3-2/capsuleIDA2.test) which are used by the autograder for checking your submission. Note that we will test your submission on different layouts, however this will give you a good guide for comparing the expected performance of your solution:

| capsuleIDA1 (node expansions)     | Potential Marks | running time | capsuleIDA2 (node expansions) | Potential Marks | running time |
| ----------- | ----------- | ----------- | ----------- |  ----------- |  ----------- |
| <= 515    | 1.00 | 17s  | <= 8458   | 1.00 | 317.9s |
| > 515     | 0.75 | 0.2s | > 8458    | 0.75 | 6s |
| > 1150    | 0.50 | 0.2s | > 30820   | 0.50 | 6.4s |
| > 3754    | 0.25 | 2.5s | > 132449  | 0.25 | 120s |
| >= 55938  | 0.00 |  -   | >= 2429729 | 0.00 |  - |

The runtimes give you a rough idea about how long did it take for the program to run with different sample heuristics we implemented. 

The **timeout** for this part will be **three times** the runtime of the **slowest** heuristic above. Due to the nature of **IDA\***, the program takes significantly longer when the solution length is increased. In addition, the program runtime changes if other layouts are used (different branching) and tie-breaking rules. 


####  Indicative marks A*

They are shown below for node expansions with **A\*** on the testing files [capsuleA1.test](test_cases_assignment1/part3-1/capsuleA1.test) and [capsuleA2.test](test_cases_assignment1/part3-2/capsuleA2.test) which are used by the autograder for checking your submission. Note that we will test your submission on different layouts, however this will give you a good guide for comparing the expected performance of your solution:

| capsuleA1 (node expansions)     | Potential Marks | running time | capsuleA2 (node expansions) | Potential Marks | running time |
| ----------- | ----------- | ----------- | ----------- |  ----------- |  ----------- |
| <= 704    | 1.00 | 146s | <= 648   | 1.00 | 111.5s |
| > 704     | 0.75 | 0.3s | > 648    | 0.75 | 0.5s |
| > 908     | 0.50 | 0.4s | > 1444   | 0.50 | 0.7s |
| > 2980    | 0.25 | 0.6s | > 4194   | 0.25 | 1.0s |
| >= 4688   | 0.00 |  -   | >= 6527   | 0.00 |  - |

The runtime gives you a rough idea about how long did it take for the program to run with the our heuristics. The **timeout** for this part will be **three times**  the runtime of the slowest heuristic above.

You will see in first person the balance between 1) how informed you make your heuristic (it should expand less nodes in general), and 2) the overall runtime. As you can see, sometimes it may be preferable to have a cheaper less informed heuristic, even if you end up expanding more nodes.

### Part 4 (4 marks)

This part involves solving a more complicated problem. You will be able to model the problem, using the search algorithm (both Astar, which is provided and Iterative Deepening A* algorithm implemented by yourself in part 2) and design a heuristic function that can guide the search algorithm. 

Just like in Q7 of the Berkerley Pacman framework, you will be required to create an agent that will eat all of the food (dots) in a maze. However, to make it more interesting, the Capsule is harder to be consumed. That is, when the Pacman enters a cell that contains a capsule, it will consume the capsule with costing more energy, **causing this move to have a cost of 2**. The capsule will disappear after being consumed by a Pacman.

In order to implement this, you should create a new problem called `CapsuleAvoidSearchProblem`. Some of the variables are listed in the comments and the initialization. You will need to design a way to represent the states of the problem. You will need to return the initial state through `getStartState` function. You will also need to have `isGoalState` to return true if the given state belongs to one of the goal states. In addition, you will need to implement your Applicability and transition function in `getSuccessors` function, which means to return a list of tuples that contains (`next state`, `action`, `cost`).

You will also need to implement a suitable `capsuleAvoidProblemHeuristic`. Your heuristic function needs to be **admissible**. You may choose to implement other helper classes/functions. You should insert your code into the template indicated by the comments ```***YOUR CODE HERE FOR TASK 3***```, you can view the locations at these links: [search.py#L117](search.py#L117) and 4 positions in [searchAgents.py](searchAgents.py).

You should be able to test your program by running the following commands (in one line):

```
python pacman.py -l capsuleAvoidA1 -p CapsuleAvoidSearchAgent
   -a fn=astar,prob=CapsuleAvoidSearchProblem,heuristic=capsuleAvoidProblemHeuristic
```

```
python pacman.py -l capsuleAvoidA1 -p CapsuleAvoidSearchAgent
   -a fn=ida,prob=CapsuleAvoidSearchProblem,heuristic=capsuleAvoidProblemHeuristic
```

The `autograder` seeks an optimal solution and the number of nodes expended (Please make sure you **do not** remove the code we use to count nodes expansion  in the `getSussessors` function). In addition, please make sure your heuristic is **admissible**, otherwise you might get 0 marks for this part due to either failed to find the optimal plan, or expanding more nodes than our baselines (zero heuristic).

#### Indicative marks IDA*

They are shown below for node expansions with **IDA\*** on the testing files [capsuleAvoidIDA1.test](test_cases_assignment1/part4-2/capsuleAvoidIDA1.test) and [capsuleAvoidIDA2.test](test_cases_assignment1/part4-2/capsuleAvoidIDA2.test) which are used by the autograder for checking your submission. Note that we will test your submission on different layouts, however this will give you a good guide for comparing the expected performance of your solution:

| capsuleIDA1 (node expansions)     | Potential Marks | running time | capsuleIDA2 (node expansions) | Potential Marks | running time |
| ----------- | ----------- | ----------- | ----------- |  ----------- |  ----------- |
| <= 414     | 1.00 | 6.6s  | <= 8818    | 1.00 | 101s |
| > 414      | 0.75 | 0.7s  | > 8818     | 0.75 | 1.8s |
| > 7300     | 0.50 | 8.6s  | > 21314    | 0.50 | 34.7s |
| > 120317   | 0.25 | 29.8s | > 400872   | 0.25 | 56.7s |
| >= 673473  | 0.00 |  -    | >= 1116517 | 0.00 |  - |

The runtimes give you a rough idea about how long did it take for the program to run with different sample heuristics we implemented. 

The **timeout** for this part will be **three times** the runtime of the **slowest** heuristic above. Due to the nature of **IDA\***, the program takes significantly longer when the solution length is increased. In addition, the program runtime changes if other layouts are used (different branching) and tie-breaking rules. 


####  Indicative marks A*

They are shown below for node expansions with **A\*** on the testing files [capsuleAvoidA1.test](test_cases_assignment1/part4-1/capsuleAvoidA1.test) and [capsuleAvoidA2.test](test_cases_assignment1/part4-2/capsuleAvoidA2.test) which are used by the autograder for checking your submission. Note that we will test your submission on different layouts, however this will give you a good guide for comparing the expected performance of your solution:

| capsuleA1 (node expansions)     | Potential Marks | running time | capsuleA2 (node expansions) | Potential Marks | running time |
| ----------- | ----------- | ----------- | ----------- |  ----------- |  ----------- |
| <= 45    | 1.00 | 1s   | <= 188   | 1.00 | 8s |
| > 45     | 0.75 | 0.1s | > 188    | 0.75 | 0.1s |
| > 68     | 0.50 | 0.1s | > 243    | 0.50 | 0.2s |
| > 349    | 0.25 | 0.2s | > 1034   | 0.25 | 0.5s |
| >= 1890  | 0.00 |  -   | >= 3051  | 0.00 |  - |

The runtime gives you a rough idea about how long did it take for the program to run with the our heuristics. The **timeout** for this part will be **three times**  the runtime of the slowest heuristic above.

You will see in first person the balance between 1) how informed you make your heuristic (it should expand less nodes in general), and 2) the overall runtime. As you can see, sometimes it may be preferable to have a cheaper less informed heuristic, even if you end up expanding more nodes.





## Acknowledgements

This is [Project 1 - Search](http://ai.berkeley.edu/search.html) from the set of [UC Pacman Projects](http://ai.berkeley.edu/project_overview.html).  We are very grateful to UC Berkeley CS188 for developing and sharing their system with us for teaching and learning purposes.
