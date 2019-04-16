---
layout: default
---

# The Database Part

description: This is a report about the database work we have done


To get started with the database work first we had to figure out how do we collect the image data of so many images together in a database.


So initially, we came up with the python script to get just one image property. That anyone can look up for any 3D image shots captured using Maya from the database.
The challenge was to understand how python reads an image file from any file location. We went through few websites and youtube videos and came across this function numpy and cv2. 

The initial script we created got us the properties of just one image.

```python
import numpy as np 
import cv2
cv2.imread('.../Desktop/Capture.png')
print('Image shape is \n', img.shape)
print('Image shape is \n', img.size)
print('Image datatype is \n', img.dtype)
```

Then we came up with a script to get the properties of various images in just one folder imported to an excel sheet.


```python
import pandas as pd
import numpy as np
import cv2
import os
import os, os.path
photos = []
for filename in os.listdir('.../Desktop/photos'):
   photos.append(filename)
file_name = []
shape = []
size =[]
img_type = []
for i in range(0,len(photos)):
   fn = '.../Desktop/photos/{}'.format(photos[i])
   file_name.append(fn)
for i in file_name:
   img = cv2.imread(i)
   shape.append(img.shape)
   size.append(img.size)
   img_type.append(img.dtype)
   image_df = pd.DataFrame(np.column_stack([photos,shape,size,img_type]),                              columns=     ['file_name','shape_1','shape_2','shape_3','size','type'])
image_df.to_csv (r'C:/Users/harini/Desktop/image.csv', index = None, header=True)

```

And finally we came up with the final code of getting the image properties from various folders inside a parent folder and got the required data exported to an excel file.


```python
from PIL import Image
import os, os.path
import numpy as np
import cv2
import pandas as pd

dir = ".../Desktop/DMDD Project/Images"
r = []
n=0
valid_images = [".jpg",".jpeg",".png"]
for root, dirs, files in os.walk(dir):
 print(root)
 #print(dirs)
 #print(files)
 df= pd.DataFrame(columns = ['Shape', 'Size',
                          'Datatype'])


 for name in files:
     #print(dirs)
     print(name)
     ext = os.path.splitext(name)[1]
     if ext.lower() not in valid_images:
         continue
     else:
         img = cv2.imread(os.path.join(root,name))
        ## print(os.path.join(root,name))
         i4=name
         i1=img.shape
         i2=os.path.getsize(os.path.join(root,name))
         i3=img.dtype
         print("name",i4)
         print('Image shape is \n',img.shape)
         print('Image size is \n', os.path.getsize(os.path.join(root,name)))
         print ('Image datatype is \n',img.dtype)
         df.loc[n, 'Shape'] = i1
         df.loc[n, 'Size'] = i2
         df.loc[n, 'Datatype'] = i3
         n+=1

```


After which we did a dry run on few sample images to get the image properties imported in an excel sheet which got us successful in grabbing the data.

Post which we came up with the below database schema inorder to store all the object models and images with its properties.

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

And then we finally established connection to workbench from python using the below code.
Challenge faced during the connection establishment is regarding the password authentication with the local host for which the user has to be identified with mysql_native_password on workbench using the below query.

```sql
ALTER USER 'root'@'localhost'
 IDENTIFIED WITH mysql_native_password
 BY 'password';
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

# Few of the Usecases

1. Get all images with certain angles
2. Get all images of certain category
3. Get image properties of the selected image
4. Get 3D image model of particular category
5. Get images of objects within a range of angles

# The Cloud Part

We are using Google cloud platform as per our requirement, we chose GCP over AWS because of cost factors and also as we had credits for GCP

Cloud  Requirement Analysis for the Dry Run
The Analysis of cloud and its servies was analysed based on the following features:- 
1 Cloud SQL Instance for Database
 
<br> SQL Standard 
<br> M/C type : db-n1-standard-4
<br> RAM(GB) : 15
<br> Max Storage Capacity: 10,230 GB
<br> Max Connections: 4000
 
We will require Cloud type as : My SQL and Second Generation.
 
1 Cloud Storage
 
1 Compute Engine Instance
 
M/C type: n1-standard-8
Virtual CPU: 8
Memory: 30 GB

![Octocat](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/image1.png)
![Octocat](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/image2.png)
![Octocat](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/image3.png)
![Octocat](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/image4.png)
![Octocat](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/image5.png)
![Octocat](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/image6.png)
![Octocat](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/image7.png)
![Octocat](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/image8.png)
![Octocat](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/image9.png)
![Octocat](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/image10.png)
![Octocat](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/image11.png)
![Octocat](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/image12.png)
![Octocat](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/image13.png)
![Octocat](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/image14.png)



[back](./)
