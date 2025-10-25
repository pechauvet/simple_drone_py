# simple_drone_py
Application support for a student project in python to build an algorithm allowing a drone to explore a room to find an object (which we will call the target).

## Requirements
To use this project, you must install :  
- shapely : a Python package for manipulation and analysis of planar geometric objects (https://pypi.org/project/shapely/)  
- matplotlib  : a comprehensive library for creating static, animated, and interactive visualizations in Python (https://pypi.org/project/matplotlib/)
- pillow : Python Imaging Library (https://pypi.org/project/pillow/)  

## Drone and environment creation
The following four instructions create the environment and view for this drone search simulation:  
- createRoom(room_description: String, height: Integer): Creates the room to be explored by defining its dimensions. This is the first instruction that must appear in your algorithm!
- createTarget(): Creates the target and randomly positions it in the room. This instruction must be used after creating the room to be explored.
- createTargetIn(x1, y1, z1, x2, y2, z2): Creates the target and randomly positions it in the cube defined by the bottom-left point (x1, y1, z1) and the top-right point (x2, y2, z2). This instruction must be used after creating the room to be explored, and the cube must be included in the room.
- createDrone( drone_id: String, viewer_id: String): Creates the drone (virtual, or connected to a real drone), and creates the view of the drone and its simulated environment. This instruction must appear in your algorithm before the locate() instruction below, and of course before calling the drone commands above.
- locate( x: Integer, y: Integer, heading: Integer): Locates the drone's initial position – landed on the ground (z=0), in the orthonormal coordinate system.

## Drone commands
There are several commands available to control the drone's navigation. For this project, we're using Tello Edu drone's like commands. These commands are the following procedures (with or without parameters):  
- takeOff( ): the drone takes off and hovers approximately 80 cm above the ground; if the drone is already in flight, the command has no effect.  
- land( ): the drone lands directly below it. No effect if the drone has already landed.
- forward( n ): the drone moves straight ahead n cm, where n cannot be less than 20 cm and greater than 500 cm. No effect if the drone has not taken off.
- backward( n ): the drone moves backward n cm (in the opposite direction to forward), always with 20 ≤ n ≤ 500. No effect if the drone has not taken off.
- goUp( n ): The drone rises n cm from its current altitude, with 20≤n≤500. No effect if the drone has not taken off.
- goDown( n ): The drone descends n cm from its current altitude, with 20≤n≤500. No effect if the drone has not taken off.
- goLeft( n ): The drone moves laterally n cm to its left, with 20≤n≤500. No effect if the drone has not taken off.
- goRight( n ): The drone moves laterally n cm to its right, with 20≤n≤500. No effect if the drone has not taken off.
- rotateLeft( n ): The drone rotates n degrees to the left with 1 ≤ n ≤ 360, without changing its position. This has no effect if the drone has not taken off.
- rotateRight( n ): The drone rotates n degrees to the right with 1 ≤ n ≤ 360, without changing its position. This has no effect if the drone has not taken off.
- getPosition( ): A function that returns the drone's current position, as a Position object. Example: Position pos = getPosition( ) (pos.x contains the abscissa, pos.y the ordinate, and pos.z the altitude).
- getHeight( ): A function that returns the drone's (approximate) altitude, in cm.
- getHeading(unit='radian'): The drone's heading is in radians by default, otherwise unit='degree' to get it in degrees.  
- isTargetDetected(): Returns true if the drone has detected the target, false otherwise.

## A first example (basic view)
Here is an example Python program that uses some of these instructions:

from dronecmds import *  
if __name__ == '__main__':  
    &emsp;try :  
        &emsp;&emsp;createRoom( "(0 0, 500 0, 500 1000, 0 1000, 0 0)", 300)  
        &emsp;&emsp;createTarget()  
        &emsp;&emsp;createDrone(DRONE_VIRTUAL, VIEWER_BASICMPL)  
        &emsp;&emsp;locate(200,200,90)  
        &emsp;&emsp;takeOff()  
        &emsp;&emsp;forward(100)  
        &emsp;&emsp;goUp(50)  
        &emsp;&emsp;rotateLeft(90)  
        &emsp;&emsp;forward(100)  
        &emsp;&emsp;land()  
    &emsp;except Exception as err:  
        &emsp;&emsp;print(err)  
        &emsp;&emsp;display()  
  
**Program Description:**

1. createRoom(…): Creates a rectangular room with the polygon (0,0) – (500,0) – (500,1000) – (0,1000) – (0,0). The room is therefore 500 cm along the abscissa and 1000 cm along the ordinate, and 300 cm high (reminder: we work in centimeters). See the figure opposite.
2. createTarget(): Creates the target and places it at a random position in the room.
3. createDrone(DRONE_VIRTUAL, VIEWER_BASICMPL): Creates a virtual drone, and the viewer identified by VIEWER_BASICMPL – you can see the rendering in the figure on the next page.
4. locate(200,200,90): Positions the drone at x=200, y=200 and with a heading of 90°. The drone must start by landing on the ground (z=0).
5. takeOff(): The drone takes off – it positions itself 80 cm above the ground.
6. forward(100): The drone moves forward 100 cm along its heading.
7. goUp(50): The drone climbs 50 cm.
8. rotateLeft(90): The drone rotates 90° to the right.
9. forward(100): The drone moves forward 100 cm along its heading.
10. land(): The drone lands on the ground (the Tello Edu drone used as a model is equipped with a ventral sensor that allows it to assess its altitude relative to the floor of the room and land safely).

The figure below shows how the basic viewer (VIEWER_BASICMPL) displays the drone's trajectory.  
- Green arrow: Position and orientation of the drone when a command was executed.
- Red-purple diamond: The target to search for. Here, the drone hasn't found the target !
<img width="398" height="429" alt="Viewer_BasicMPL_light" src="https://github.com/user-attachments/assets/1eec8589-7294-4121-a234-6b470d34006d" />

## A second example (tkinter view)

You also have the following viewers:  
- VIEWER_CONSOLE: Views in text mode in the console.
- VIEWER_TKMPL: Views in a tkinter application with matplotlib, that we explain here.  

**Program example :**

from dronecmds import *  
import traceback  

def myprog() :  
    &emsp;"""  
    &emsp;Function that describes the drone program.  
    &emsp;"""  
    &emsp;try :  
        &emsp;&emsp;locate(100,100,90)  
        &emsp;&emsp;takeOff()  
        &emsp;&emsp;forward(100)  
        &emsp;&emsp;goUp(50)  
        &emsp;&emsp;rotateLeft(90)  
        &emsp;&emsp;forward(50)  
        &emsp;&emsp;forward(200)  
        &emsp;&emsp;land()  
    &emsp;except Exception as err:  
        &emsp;&emsp;print(err)  
        &emsp;&emsp;traceback.print_exc()  
        &emsp;&emsp;display()  

if __name__ == '__main__':  
        &emsp;createRoom( "(0 0, 500 0, 500 1000, 0 1000, 0 0)", 300)  
        &emsp;createTargetIn(200, 200, 200, 300, 300, 250)  
        &emsp;createDrone(DRONE_VIRTUAL, VIEWER_TKMPL, progfunc=myprog)  

This code produces this simulation window :  
<img width="347" height="361" alt="Viewer_TKMPL" src="https://github.com/user-attachments/assets/05edd4d5-8dfe-469b-8a34-833d885272c3" />

