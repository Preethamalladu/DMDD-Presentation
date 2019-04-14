---
layout: default
---

Text can be **bold**, _italic_, or ~~strikethrough~~.

[Link to another page](./another-page.html).

This is a short description of what we have done so far.

**Index**

*   Understanding Maya Scripting

*   The Image Generation Part

    1.  Challenges Faced
    1.  Failed vs Sucessfull Ideas
    1.  Final Images
    
    
*   The Database Part

    1.  
    1.
    1.



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

> In this method I just rotated the image using its pivot 

![Branching](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/chair_test_x0y0z7.jpg)


![Branching](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/chair_test_starting.jpg)

As we can see the images dont seem so refined, and also rotating the object does not make sense for what we want to achive, thus moving to a new method.

### Creating a camera and rotaing the camera around the object

> Since rotating the image was not an option , rotating the camera was the alternative, thus came up with a solution which looks like so,

![Branching](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/char_circle.PNG)

### Creating Environments to get dynamic shadows

> This was the part which took a lot of time, we had to come up with a solution which gave us dynamic shaodows meaning when the cameras angle changed the shadows have to change too.


**Problems faced and solutions I came up with**

Firstly, we had to make sure we do not want gradients in the background. Therefore I came up with diffrent iterations of environments


*   Iteration One :

![Branching](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/obj31.jpg)

It is clear what the problem is,  bad lights and gradients.


*   Iteration Two :

![Branching](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/objX0Y315Z0.jpg)

In this environment , instead of using a box environment like above I used a sphere environment and avoided getting gradients but the shadows were static, 

*   Iteration Three :

![Octocat](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/Telescope_X_0_Y_45_Z_90.png)

This environment was done by **Akash Srivatsava**, and was selected as final environment since it had minial noise and had dyanmic shadows.


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

### My contribution towards the final code

Though the logic for rotating images is similar we chose while loop over for loop for simplicity

**This snippet automatically sets the renderer to Maya Hardware 2.0 and saves images as .PNG files**

```python
cmds.setAttr('defaultRenderGlobals.ren', 'mayaHardware2', type = 'string')
cmds.setAttr('defaultRenderGlobals.imageFormat', 32)')
```

**This snippet makes sure to take images which are only above the ground thus avoiding redundent images**

```python
camPos = cmds.xform(camName, q=True, ws=True, rp=True)
if( camPos[1] > 1.0):
    screenShot(l)
```
**Naming convention**

```python
name = "_X_"+str(cx) + "_Y_"+str(cy) + "_Z_"+str(cz)
```

* * *

## Stored Procedures to get Front, Back, Top, Left, Right views of the object from the data base

**Top View**
```sql
create procedure TopView @ObjName varchar(30) 
as
select images from image_table where images like
              '%' + @ObjName` + '_X_90_Y_0_Z_180' 
go;
```

**Front View**
```sql
create procedure FrontView @ObjName varchar(30) 
as
select images from image_table where images like
              '%' + @ObjName` + '_X_0_Y_0_Z_0' 
go;
```

**Right View**
```sql
create procedure RightView @ObjName varchar(30) 
as
select images from image_table where images like
              '%' + @ObjName` + '_X_0_Y_90_Z_0' 
go;
```

**Back View**
```sql
create procedure BacKView @ObjName varchar(30) 
as
select images from image_table where images like
              '%' + @ObjName` + '_X_0_Y_180_Z_0' 
go;
```

**Left View**
```sql
create procedure LeftView @ObjName varchar(30) 
as
select images from image_table where images like
              '%' + @ObjName` + '_X_0_Y_270_Z_0' 
go;
```


* * *

### Here is an unordered list:

*   Item foo
*   Item bar
*   Item baz
*   Item zip

### And an ordered list:

1.  Item one
1.  Item two
1.  Item three
1.  Item four

### And a nested list:

- level 1 item
  - level 2 item
  - level 2 item
    - level 3 item
    - level 3 item
- level 1 item
  - level 2 item
  - level 2 item
  - level 2 item
- level 1 item
  - level 2 item
  - level 2 item
- level 1 item

### Small image

![Octocat](https://assets-cdn.github.com/images/icons/emoji/octocat.png)

### Large image

![Branching](https://guides.github.com/activities/hello-world/branching.png)


### Definition lists can be used with HTML syntax.

<dl>
<dt>Name</dt>
<dd>Godzilla</dd>
<dt>Born</dt>
<dd>1952</dd>
<dt>Birthplace</dt>
<dd>Japan</dd>
<dt>Color</dt>
<dd>Green</dd>
</dl>

```
Long, single-line code blocks should not wrap. They should horizontally scroll if they are too long. This line should be long enough to demonstrate this.
```

```
The final element.
```
