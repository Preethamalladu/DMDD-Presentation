---
layout: default
---


This is a short description of what we have done so far.

**Content**

*   Understanding Maya Scripting

*   The Image Generation Part

*   The Database Part



## Understanding Maya Scripting

In the first week of the project we have gone through the process on how we can automate tasks in Maya using python and mel, we acquired knowledge about scripting from, Proffersor Nick Brown and through various other sites.

### Things we learnt

> How to Translate and Rotate an object in 3D space

> How to reset the obects movemenets 

> How to create a script in python which can do the above

> How to render images using MayaHardware 2.0


## The Image Generation Part

After the first week we have have tried few diffrent ways to render images ,inorder to do so we have tried the follwoing method

### Just the object rotated in space

> In this method we just rotated the image using its pivot 

![Branching](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/chair_test_x0y0z7.jpg)


![Branching](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/chair_test_starting.jpg)

As we can see the images dont seem so refined, and also rotating the object does not make sense for what we want to achive, thus moving to a new method.

### Creating a camera and rotaing the camera around the object

> Since rotating the image was not an option , rotating the camera was the alternative, thus came up with a solution which looks like so,

![Branching](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/char_circle.PNG)

### Creating Environments to get dynamic shadows

> This was the part which took a lot of time, we had to come up with a solution which gave us dynamic shaodows meaning when the cameras angle changed the shadows have to change too.


**Problems faced and solutions we came up with**

Firstly, we had to make sure we do not want gradients in the background. Therefore I came up with diffrent iterations of environments


*   Iteration One :

![Branching](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/obj31.jpg)

It is clear what the problem is,  bad lights and gradients.

*   Iteration Two

![Branching](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/Save%20image%20faults.png)

This environment still had the same problems as the earlier versions .e, gradients


*   Iteration Three :

![Branching](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/objX0Y315Z0.jpg)

In this environment , instead of using a box environment like above we used a sphere environment and avoided getting gradients but the shadows were static. 


*   Iteration Four :

![Octocat](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/Final%202.png)

This environment was selected as final environment since it had minial noise and had dyanmic shadows.

## Choosing which texture to use

We have tried few diffrent textures and this is a sample of what we came up with , after few trails and tests we decided to go with "lambert" texture.

![Branching](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/tmpcd00148_thumb.png)

## Meshing and Rescaling

Most of the objects which we got online needed some preprocessing such as making the image smaller / larger , combining the mesh (most objects are a collection of smaller objects)

![Octocat](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/Capture.PNG)

![Octocat](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/Capture1.PNG)

## Naming convention

We decided to come up with a naming convention which allows us to retrive images based on the category and certain angle.

> CategoryName_ObjectName_X_degree_Y_degree_Z_degree


## Scripting to get Images


### Script to rotate images

```python
// python code to rotate the images.
def rotateImage(objName, deg):
    for x in range(0, 360/deg):
        l = 'x'+str(x) + 'y0' + 'z0'
        cmds.xform(objName, relative=True, rotation=(deg, 0, 0) )
        screenShot(objName, l) 
        for y in range(0, 360/deg):
            l = 'x'+str(x)+ 'Y' +str(y) + 'z0'
            cmds.xform(objName, relative=True, rotation=(0, deg, 0) ) 
            screenShot(objName, l) 
            for z in range (0, 360/deg):
                l = 'x'+str(x)+'y'+ str(y) +'Z' +str(z)
                cmds.xform(objName, relative=True, rotation=(0, 0, deg) )
                screenShot(objName, l)
```
### Script to take images

```python
def screenShot(objName, l):
    mel.eval('renderWindowRender redoPreviousRender renderView')
    editor =  'renderView'
    cmds.renderWindowEditor( editor, e=True,refresh = True,removeAllImages = True, writeImage=('path'+'awp'+str(l)), removeImage = True)
```

### Mesh smoothing 

Inorder to get smooth images without sharp edges, mesh smoothing was used.

![Branching](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/Image%20from%20iOS.jpg)


### Finalized Script to rotate and take images

```python
import maya.cmds
cmds.setAttr('defaultRenderGlobals.ren', 'mayaHardware2', type = 'string')
cmds.setAttr('defaultRenderGlobals.imageFormat', 32) 

cx = 0
i = 0
s = cmds.ls(selection = True)
camName = "camera1"
camPos = 0
obj = s[0]
deg = 45
cy = 0
cz = 0
def screenShot(i):
    mel.eval('renderWindowRender redoPreviousRender renderView')
    editor = 'renderView'
    cmds.renderWindowEditor(editor, e = True, refresh = True, writeImage = ('path' + str(i)))


while(cx<= 360):
    for a in s:
        x = a +"."+"rotate" +"X"
        cmds.setAttr(x,cx)
    cy=0
    while(cy<=360):
        for a in s:
            x = a +"."+"rotate" +"Y"
            cmds.setAttr(x,cy)
        cz=0
        while(cz<=360):
            for a in s:
                x = a +"."+"rotate" +"Z"
                cmds.setAttr(x,cz)
            l = "_X_"+str(cx) + "_Y_"+str(cy) + "_Z_"+str(cz)
            camPos = cmds.xform(camName, q=True, ws=True, rp=True)
            if( camPos[1] > 1.0):
                screenShot(l)

            cz = cz + deg
        cy=cy+deg
    cx=cx+deg

```


* * *


[The Database Part](./another-page.html).


## What work is left over

*   Update a website with more featues like filters, views (front,back, ..etc), option to download.

*   Improving cloud setup 



## What we are planning to contribute over the summer for the database as a part of SkunkWorks

*   Figure out a way to automate the process of setting the textures and combineing images

*   Pipeline the process of generating images

*   Add more objects 

*   Add an option website that allows users to view images based on user search

*   Add an option to the website with allows users to upload a 3d model and get images for that model



* * *





