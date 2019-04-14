---
layout: default
---

Text can be **bold**, _italic_, or ~~strikethrough~~.

[Link to another page](./another-page.html).

This is a short description of what we have done so far.

**Index**

•   Understanding Maya Scripting

•   The Image Generation Part

    1)  Challenges Faced
    2)  Failed vs Sucessfull Ideas
    3)  Final Images
    
    
•   The Database Part

    1)  
    2)
    3)



## Understanding Maya Scripting

In the first week of the project we have gone through the process on how we can automate tasks in Maya using python and mel, we acquired knowledge about scripting from, Proffersor Nick Brown and through various other sites.

### Things we learnt
> How to Translate and Rotate an object in 3D space
> How to reset the obects movemenets 
> How to create a script in python which can do the above
> How to render images using MayaHardware 2.0


## The Image Generation Part

After the first week we have have tried few diffrent ways to render images ,inorder to do so we have tried the follwoing method

### Just The Object rotated in space

    In this method I just rotated the image using its pivot 

![Branching](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/chair_test_x0y0z7.jpg)


![Branching](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/chair_test_starting.jpg)

As we can see the images dont seem so refined, and also rotating the object does not make sense for what we want to achive, thus moving to a new method.

### Creating a camera and rotaing the camera around the object

> Since rotating the image was not an option , rotating the camera was the alternative, thus came up with a solution which looks like so,

![Branching](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/char_circle.PNG)

### Creating Environments to get dynamic shadows

> This was the part which took a lot of time, we had to come up with a solution which gave us dynamic shaodows meaning when the cameras angle changed the shadows have to change too.


**Problems I faced and solutions I came up with**

Firstly, we had to make sure we do not want gradients in the background. Therefore I came up with diffrent iterations of environments


•   Iteration One :

![Branching](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/obj31.jpg)

It is clear what the problem is,  bad lights and gradients.


•   Iteration Two :

![Branching](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/objX0Y315Z0.jpg)

In this environment , instead of using a box environment like above I used a sphere environment and avoided getting gradients but the shadows were static, 

•   Iteration Three :

![Branching](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/Telescope_X_0_Y_45_Z_90.png)

This environment was done by Akash Srivatsava, and was selected as final environment since it had minial noise and had dyanmic shadows.


### Header 3

```js
// Javascript code with syntax highlighting.
var fun = function lang(l) {
  dateformat.i18n = require('./lang/' + l)
  return true;
}
```

```ruby
# Ruby code with syntax highlighting
GitHubPages::Dependencies.gems.each do |gem, version|
  s.add_dependency(gem, "= #{version}")
end
```

#### Header 4

*   This is an unordered list following a header.
*   This is an unordered list following a header.
*   This is an unordered list following a header.

##### Header 5

1.  This is an ordered list following a header.
2.  This is an ordered list following a header.
3.  This is an ordered list following a header.

###### Header 6

| head1        | head two          | three |
|:-------------|:------------------|:------|
| ok           | good swedish fish | nice  |
| out of stock | good and plenty   | nice  |
| ok           | good `oreos`      | hmm   |
| ok           | good `zoute` drop | yumm  |

### There's a horizontal rule below this.

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
