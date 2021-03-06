# cam-track.py  - Camera Movement Tracker Demo
#### A Raspberry Pi Camera Pan-Tilt Tracker using openCV2 & Video Stream Thread

YouTube Video Demo https://youtu.be/yjA3UtwbD80   
YouTube Video Code Walkthrough https://youtu.be/lkh3YbbNdYg   
RPI Forum Post https://www.raspberrypi.org/forums/viewtopic.php?p=1027463#p1027463  
Github Repo https://github.com/pageauc/motion-track/tree/master/cam-track   

###Release History
* ver 0.60 15-Aug-2016 - Initial Release
* ver 0.70 16-Aug-2016 - Added extra comments and move np import  
* ver 0.80 01-Sep-2016 - Updated and added resistance to quick movements  
* ver 0.85 05-Sep-2016 - More functions and better resistance to moving objects  

### Program Description
This is a raspberry pi computer openCV2 program that tracks camera (pan/tilt)
 movements. It requires a RPI camera module installed and working. The program is 
written in python2 and uses openCV2.  

It captures a search rectangle from the center of a video stream tread image. 
It then locates the rectangle in subsequent images based on a score value and
returns the x y location in the image based on a threshold accuracy.  
If movement gets too close to the sides of the image or
a suitable image search match cannot be found, then another search rectangle
is selected. This data is processed to track a cumulative pixel location based on
an initial camera image center value of 0,0.    
This code could be used for a simple robotics application, movement stabilization, 
searching for an object image in the video stream rather than taking a search
rectangle from the stream itself.  Eg look for a dog.
where the camera is mounted on a moving platform or object, Etc. 
I will be working to implement Robot (without wheel encoders) Navigation
Test using this camera tracking.

####Project - Accurate Robot Navigation without wheel encoders using camera tracking
I am looking at saving high value search rectangles that
are spaced out around the full xy range of the camera movement and use those
to correct any tracking errors. These check point rectangles will also need to
be updated if a better check point rectangle (higher maxVal) is found in the same region. 
I was thinking approx every half image spacing in xy cam position. 
This would allow it to self correct position drift (self calibrating). 
I am hoping to test this on a robot that does not have wheel encoders. 
Camera tracking could allow the robot to more accurately navigate and rotate.
I am pleased with the current FPS with ver 0.85. The multi version is not very
stable due to segment faults so I will stick with only the video stream being
threaded. 
If you decide to try this as well, let me know.
Claude ...
 

Note: This application is a demo and is currently still in development, but I 
thought it could still be useful, since I was not able to find a similar
RPI application that does this.  Will try to implement an object searcher based
on this demo.

Thanks to Adrian Rosebrock jrosebr1 at http://www.pyimagesearch.com 
for the PiVideoStream Class code available on github at
https://github.com/jrosebr1/imutils/blob/master/imutils/video/pivideostream.py

### Setup
Requires a Raspberry Pi computer with a RPI camera module installed, configured
and tested to verify it is working. I used a RPI model B3 but a 2, B+ or 
earlier will work OK. A quad core processor will greatly improve performance
due to threading

From logged in RPI SSH session or console terminal perform the following.  

    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get install -y python-opencv python-picamera
    mkdir ~/cam-track
    cd ~/cam-track
    wget https://raw.github.com/pageauc/motion-track/master/cam-track/cam-track.py
    chmod +x cam-track.py  
    ./cam-track.py       # defaults to run from RPI desktop terminal session
                         # Set window_on=False if running in SSH session
                         
### Tuning
You may have to experiment with some settings to optimize performance.
If there are plain backgrounds or random motions in camera view then the
tracking values may get out of sync.

The main variables are

#### MAX_SEARCH_THRESHOLD - default is .97
This variable sets the value for the highest accuracy for maintaining a 
lock on the search rectangle found in the stream images.  Otherwise another similar block will be returned.  
Setting this higher will force a closer match to the original search rectangle. 
If you have a unique background features then set this higher eg .98, .99 
or for a background with fewer unique features set it lower since the match criteria
will not be able to be met.  Review debug data for your environment.

#### cam_move_x and cam_move_y - defaults 10 and 8
These variables set the maximum x and y pixel movement allowed in one loop cycle.
This reduces unexpected cam position changes when objects move through the 
camera image view quickly.  The search_rect can lock onto the moving objects
pixel pattern and track it. When this happens, the cam position
will get out of sync since it is not tracking the image background properly.
Balance the setting with the normal expected cam movement speed.  
defaults are 10 and 8


Use a text editor to review code for other variable settings.  Eg. 

    nano cam-track.py
    
nano editor is just a suggestion.  You can use whatever editor you are
comforable with

Have Fun Claude Pageau

YouTube Channel https://www.youtube.com/user/pageaucp   
GitHub https://github.com/pageauc   


