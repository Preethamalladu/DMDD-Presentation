---
layout: default
---

# The Database Part

description: This is a report about the database work we have done


To get started with the database work first we had to figure out how do we collect the image data of so many images together in a database.


So initially, we came up with the python script to get just one image property. That anyone can look up for any 3D image shots captured using Maya from the database.
The challenge was to understand how python reads an image file from any file location. I went through few websites and youtube videos and came across this function numpy and cv2. 


Numpy is a library that is used for computing scientific/mathematical data. The arrays in numpy are faster than python lists. 


OpenCV, is an image and video processing library in python. OpenCV is used for all sorts of image and video analysis, like facial recognition and detection, license plate reading, photo exditing, advanced robotic vision, optical character recognition, and a whole lot more. Using CV2 we can use the opencv package in python to read an image.


Next is to get the library cv2 installed in my machine.


So I installed the library:
pip install opencv-python in the anaconda prompt window.
Initially I came up with the below script:

```python
import numpy as np 
import cv2
cv2.imread('/Users/Anindita/Desktop/Capture.png')
print('Image shape is \n', img.shape)
print('Image shape is \n', img.size)
print('Image datatype is \n', img.dtype)
```

 
Later we  worked with **Harini Grandhi** to get all the image properties of a 3d model imported in an excel using for loop as stated below:


```python
import pandas as pd
import numpy as np
import cv2
import os
import os, os.path
photos = []
for filename in os.listdir('C:/Users/harini/Desktop/photos'):
   photos.append(filename)
file_name = []
shape = []
size =[]
img_type = []
for i in range(0,len(photos)):
   fn = 'C:/Users/harini/Desktop/photos/{}'.format(photos[i])
   file_name.append(fn)
for i in file_name:
   img = cv2.imread(i)
   shape.append(img.shape)
   size.append(img.size)
   img_type.append(img.dtype)
   image_df = pd.DataFrame(np.column_stack([photos,shape,size,img_type]),                              columns=     ['file_name','shape_1','shape_2','shape_3','size','type'])
image_df.to_csv (r'C:/Users/harini/Desktop/image.csv', index = None, header=True)

```

After which we did a dry run on few sample images to get the image properties imported in an excel sheet which got us successful in grabbing the data.

Post which I along with Sindhura Reddy and Harini Grandhi came up with the below database schema inorder to store all the object models and images with its properties.

The below is the database schema:
![Octocat](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/hiii.png)

The above schema can be explained with an example below:


<br> category - table 
<br> category_id: C01 (PK) 
<br> category_name: Animals 


<br> Category_object - table
<br> category_id: C01 (FK)
<br> object_model_id: C01_01 (PK)
<br> Object_model_name: Dog


<br> Sub_object - table
<br> object_model_id: C01_01 (FK)
<br> sub_object_id: C01_01_01 (PK)
<br> sub_object_name: labrador


<br> sub_object_imageid - table
<br> sub_object_id: C01_01_01 (FK)
<br> image_id: I01 (PK)


<br> image_data - table
<br> image_id: I01 (FK)
<br> image_path: link or the path
<br> image_size: in kb
<br> image_resolution: ...
<br> image_type: .png


Later, We created a physical database on MYSQL Workbench.
Sindhura established connection to workbench from python using the below code.
Challenge faced during the connection establishment is regarding the password authentication with the local host for which the user has to be identified with mysql_native_password on workbench using the below query.

```sql
ALTER USER ‘root’@‘localhost’
 IDENTIFIED WITH mysql_native_password
 BY ‘password’;
```

```python
import mysql.connector
from mysql.connector import errorcode
try:
  cnx = mysql.connector.connect(host= 'localhost',
                                user='root',
                                password='password',
                                auth_plugin='mysql_native_password',
                                database='images')
  if cnx.is_connected():
            print('Connected to MySQL database')
except mysql.connector.Error as err:
  if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
    print("Something is wrong with your user name or password")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist")
  else:
    print(err)
else:
  cnx.close()

```
[back](./)
