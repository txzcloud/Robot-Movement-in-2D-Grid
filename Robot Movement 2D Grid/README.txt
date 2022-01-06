Explanation of Models-- Created by Tom Zheng / U98418371 
Note to Professor: //I do not have a team member for this project assignment





For Project 2 Requirements Undergraduate:

The submitted model demonstrates time constraints on the robot's behavior and the communication between the robot and the grid.
A lower limit bound of "x > 0" is established to show that robot cannot travel at infinite speed and will need a greater than zero to stay in the block.
An upper limit bound of "x <= 1" is established to show that the robot cannot stay in the same block/state indefinitely. 
Lastly, the communication between the block and the robot is assigned a clock of "x <= 5" to account for the communication delay caused by the computation.


Q: Does the composition of the robot and grid modelstillsatisfy all the correctness requirement as specified in Project 1?

Answer: Yes, with the time constraint implemented, the robot and grid model still satisfy all the correctness requirement as specified in project 1


Q: Find out theminimum amount timethat the robot uses to reach its goal.  Explainyour answer with sufficient details.Note that you need to use UPPAAL to answerthis question, not manually compute the answer.

According to the UPPAAL model of the robot and grid, the minimum time that the robot uses to reach its goal is: 995.458631 ticks/seconds

This is because I declared a third clock variable called "z" and have it evolved along side with the entire robot and grid model.
As the "z" clock evolves whilst the "x and y" clock resets overtime, it accumulates the total time it takes for the robot to reach its goal.
Furthermore, to find the minimum time it takes for the robot to reach its goal, I look for the shortest trace possible in UPPAAL by analyzing the trace simulation in the "Concrete simulator". 
According to the shortest trace that exist, it requires 995.458631 ticks/seconds seconds for the robot to reach its goal.
	
	


Information below is from Project 1:


Scenario 1

robot():

The SM model implemented in UPPAAL has the following states:

initial-- This is the initial starting point of the robot's coordinate. It is initalized to (x,y) = (0,0).
move_x-- This is the state that is responsible for all movements on the X axis, for example, left and right, x-1 and x+1.
move_y-- This is the state that is responsible for all movements on the Y axis, for example, up and down, y-1 and y+1.
move_(r,l,u,d)-- This is the state performs the movement in its corresponding direction and sends a REQUEST to the grid after setting the (bx,by) coordinates.

wait_(r,l,u,d)-- This is thes that waits for the grid to ACKNOWLEDGE the robot's REQUEST and to set ob to its corresponding boolean, depending if there is an existing obstacle at the specified coordinate.
receive_(r,l,u,d)-- This is the state that successfully receives the ACKNOWLEDGE from the grid and decides which action to take next using a guard depending on the value of ob.

goal-- This is the final state that the robot will be in and this is set to when the (x,y) coordinates are exactly equal to (dx,dy) coordinates. In this project, (dx,dy) = (4,4)



grid():

curr_position-- This is the current position of the robot on the grid. It is initialized to (x,y) = (0,0), which is the same as the robot. Because this is the starting point of the robot. 

blocked-- After the robot sends a request, this state checks whether or not an obstacle exist by calling the function "check(bx,by)". It then sets the value of ob after this state and return to the current position of the robot (x,y)



Correctness requirements
1. The robot does not hit any obstacles:
	A<>Map1.curr_position and ((x!=1 and y!=1) or (x!=1 and y!=2) or (x!=1 and y!=4) or (x!=3 and y!=3) or (x!=4 and y!=1))
	
	By having ob set to 1, this signals the robot that the requested cell has an obtacle, hence, preventing it from moving onto the next state that allows it to increment its coordinates to that blocked cell.

2. The robot does not cross any boundaries:
	A<>Map1.curr_position and ((x>=0 and x<=4) and (y>=0 and y<=4))
	
	By having the ob set to 1 whenever the robot's requested coordinate (bx,by) exceeds the boundaries of 0>=x<=4 and 0>=y>=4, hence, acting as an invisible obstacle, therefore, preventing the robot
	from taking the path that increments the robot's coordinates over or on the boundaries.	

3. The robot will eventually reach the destination:
	E<>Machine.goal

	Due to the above two correctness requirements, the robot will avoid all obstacles and avoid crossing any boundaries. With these two correctness requirements, the robot will eventually have to take the 
	cells that are free, hence, reaching the target cell eventually. As for the corners that results in stucking the robot and have it move back and forth repeatively, this is corrected by having these cornered cells
	marked after being visited by the robot. By marking these cells, the robot moves back by decrementing either x or y coordinate then immediately take the next type of movement.
	For example, robot reaches (4,0) and is stuck in the corner, it will decrement x-1 and become (3,0) then immediately check the next direction (which is the y axis) for obstacle, and move towards the y axis instead.
	Therefore, the resulting coordinate in this case would be (3,1).

verification/analysis results

Based on the computation tree produced by UPPAAL and its verifier's results.
1. Property is satisfied
2. Property is satisfied
3. Property is satisfied


Does  the  composition  of  the  robot  and  grid  model  satisfy  all  the  correctnessrequirement?   
In  case  your  model  violates  any  of  the  correctness  requirements, give a detailed explanation on why such violations occur, and your proposal to fix these violations.

Yes, my composition of the robot and grid model satisfy all the correctness requirement.

====================================================================================================================================================================================================================================

