---
title: "Projects"
date: 2020-11-12T12:12:20+08:00
---
# Unity game demos
Available at [bilibili](https://www.bilibili.com/video/BV1gA411x7wL/) and [youtube](https://youtu.be/w6G9uX6GwV0).

# VR programming serious game demo
Available at [bilibili](https://www.bilibili.com/video/BV17a411w7rB/) and [youtube](https://youtu.be/oT43W-Sd_do).



# OpenGL Scene

## Introduction
When I was a child, I usually snuck out from my apartment and take a bus to go to a mall for playing arcade games, the scene try to restore the view of the mall and the objects surrounding it.
The scene consists a house with two arcades in the left, a security guard wandering in front of it, a tree next to the house, a moving car on the other side and a galaxy in the top of the scene. The scene can switch from day to night by a right-click menu and the camera and view port can be moved around with keyboard and mouse input. 
![](https://raw.githubusercontent.com/invictuslh/Image_Hosting/master/uPic/20200520_125733.png)

## Modelling
There are eight models in the scene, which are: a skybox, a house, a human, a car, a tree, a galaxy and two arcades.

### Skybox (Stage.cpp & Stage.h)
Night skybox
![](https://raw.githubusercontent.com/invictuslh/Image_Hosting/master/uPic/20200518_150519.png)
Day skybox
![](https://raw.githubusercontent.com/invictuslh/Image_Hosting/master/uPic/20200518_150742.png)
The skybox consists six textured quads that is visible from the inside. Example codes are in the following chapter of texture.
The size of the skybox is determined by the window height:
`stage->size((Scene::GetWindowHeight()) / tan(M_PI / 8.f));`
Besides, when the size of the skybox changes, the clipping plane of the camera shall changes accordingly.
`gluPerspective(60.0, aspect, 1.0, 2200.0);`

### Human (Human.cpp & Human.h)
![](https://raw.githubusercontent.com/invictuslh/Image_Hosting/master/uPic/20200504_151228.png)
The model uses hierarchical modelling:
The process is moving upward to draw the head and hat then moving downward step by step to draw other parts of the human through saving and restoring the matrix.
The orientation, the first translation, the angle of legs and arms are controlled by variables in order to enable animation.
The model consists:
- a hat: a cylinder (made of quads) and solid circles (made of quads)
- a head: a solid sphere
- a body, two arms, two hands, ten fingers and two legs: solid cubes
The model also has a walking animation, which will be illustrated in the following part.
Example code:
```cpp
// Draw the hand of the human
void Human::DrawHand()
{
    // color grey
    glColor3f(0.1f, 0.1f, 0.1f);
    // Draw hand
    glTranslatef(0.f, -0.3f, 0.f);
    Box(0.7f, 0.6f, 0.5f);
    glTranslatef(0.f, -0.3f, 0.f);
    // Draw Fingers
    // thumb
    glPushMatrix();
        glTranslatef(-0.3f, 0.f, 0.f);
        glRotatef(-30.f, 0.f, 0.f, 1.f);
        glTranslatef(0.f, -0.15f, 0.f);
        Box(0.1f, 0.3f, 0.5f);
    glPopMatrix();
    // index finger
    glPushMatrix();
        glTranslatef(-0.1f, 0.f, 0.f);
        glRotatef(-15.f, 0.f, 0.f, 1.f);
        glTranslatef(0.f, -0.3f, 0.f);
        Box(0.1f, 0.6f, 0.5f);
    glPopMatrix();

    // the same paradigm for middle finger, ring finger and little finger is omitted
}
```

The human is resized and repositioned appropriately in the scene.
```cpp
    Human* human = new Human();
    human->position(-500,50,100);
    human->size(0.7f,0.7f,0.7f);
    human->orientation(0,90,0);
    AddObjectToScene(human);
```

### Tree (Tree.cpp & Tree.h)
![](https://raw.githubusercontent.com/invictuslh/Image_Hosting/master/uPic/20200520_131911.png)
The tree model uses hierarchical modelling and turtle graphics.
By binding specific function with a character, the model of tree is generated with a string and a character replacement system (L system).
Example codes:
```cpp
    // tree next to the house
    Tree* tree = new Tree();
    objects["tree"] = tree;
    tree->SetReplaceString('f', "ff-[-& f < f < + << + f] + [+>f--f-f]");
    AddObjectToScene(tree);
```

### Car (Car.cpp & Car.h)
![](https://raw.githubusercontent.com/invictuslh/Image_Hosting/master/uPic/20200518_144508.png)

The car model also uses hierarchical modelling:
First move upwards to draw the top of the car and windows, then move downwards to draw body, lights and wheels successively.
The angle of line strip and the first translation is controlled by variables in order to enable animation.

The model consists:
- a top body: quads with material
- a bottom body: quads with material
- four windows: textured quads
- two headlights object: solid cube
- two actual headlights (Light1 and Light2)
- four wheels: line strip and solid torus
The size of the arcade is controlled by variables `height, length and width`, the vertices are based on these variable, and the quads are based on those vertices.
The model also has a spinning wheel and moving animation, which will be illustrated in the following part.
Example codes
```cpp
// Draw a wheel with wheel hub
void Car::DrawWheel() // total height: 0.6 length
{
    float res = 0.1*M_PI; // eqivalent to 0.1 * 180 degree
    float radius = highth; // the radius of the whell
    float x = 0.f;
    float y = 0.f;
    glColor3f(0.9,0.9,0.9);
    glBegin(GL_LINE_STRIP);
        for(float radians=0.f; radians<=2*M_PI; radians+=res)
          {
              glVertex3f(0,0,0);
              x = radius*cos(radians+angle/M_PI);
              y = radius*sin(radians+angle/M_PI);
              glVertex3f(x, y, 0);
          }
    glEnd();
    glColor3f(0,0,0);
    glutSolidTorus(0.2f*radius,radius,10,25);
}
```

### Galaxy (Galaxy.cpp & Galaxy.h)
![](https://raw.githubusercontent.com/invictuslh/Image_Hosting/master/uPic/20200518_144651.png)
The galaxy model uses hierarchical modelling:
The sun (a solid sphere)  is drew first, and then record the matrix status, rotate a certain degree, translate a certain distance and draw a planet (solid sphere), then restore the matrix status, then repeat this process for five times.
The rotate angle is controlled by a variable in order to add an orbiting animation.
Example Codes:
```cpp
    // Draw sun
    glColor3f(1.f, 1.f, 0.f);
    DrawStar(2);
    
    // Draw planet, center to center distance: 6, radius: 1
    glPushMatrix();
        glRotatef(30.f + angle, 0.f, 1.f, 0.f);
        glTranslatef(6.f, 0.f, 0.f);
        glColor3f(1.f, 0.f, 0.f);
        DrawStar(1);
    glPopMatrix();
```

### Arcade (Arcade.cpp & Arcade.h)
![](https://raw.githubusercontent.com/invictuslh/Image_Hosting/master/uPic/20200518_140919.png)
The arcade uses hierarchical modelling:
The top part and the screen is drew first, then move downwards where the buttons and stick are drawn (same position on z axis and y axis), then the bottom part of the arcade is drawn.

The model consists:
- a top body: textured quads
- a bottom body: textured quads
- a stick: textured cylinder, textured solid circle, solid sphere
- three buttons: textured cylinder and textured solid circle
- a display screen: quads with animated texture
The size of the arcade is controlled by variables `height, length and width`, the vertices are based on these variable, and the quads are based on those vertices.
The top, left and right face of the top part consists two quads in order to make it visible both from in side and outside.
### House (House.cpp & House.h)
![](https://raw.githubusercontent.com/invictuslh/Image_Hosting/master/uPic/20200518_145446.png)
- Walls and doors: textured quads
The house is built through a key function `DrawWall()`, by rotating and scaling the wall, the house is assembled.
Example codes:
```cpp
	// draw  roof
    glPushMatrix();
        glTranslatef(0.f, height/2.f, 0.f);
        glRotatef(90.f, 1.f, 0.f, 0.f);
        glScalef(1.f,1.2f,1.f);
        DrawWall();
    glPopMatrix();
```
The model has a trigger animation, when the camera is near, the door will automatically open.

## Lighting
### Outdoor lighting
`GL_LIGHT0` in the frame work
The light will be turn off when the night mode is selected

### Indoor lighting
There is a directional light inside of the house and will not be turned off when the night mode is selected.

### Car lighting
The car has a mixture of lighting `GL_LIGHT1` and `GL_LIGHT2`, which provide a light shapes like a cone aiming to the front of the car.
![](https://raw.githubusercontent.com/invictuslh/Image_Hosting/master/uPic/20200518_135457.png)
Example codes:
```cpp
	GLfloat light_ambient1[] = { 1.0, 1.0, 1.0, 1.0 };
    GLfloat light_diffuse1[] = { 1.0, 1.0, 1.0, 1.0 };
    GLfloat light_specular1[] = { 1.0, 1.0, 1.0, 1.0 };
    GLfloat light_position1[] = { 0.0, 0, 0, 1 };
    GLfloat spot_direction1[] = { -10, 0, 0 };

    glLightfv(GL_LIGHT1, GL_AMBIENT, light_ambient1);
    glLightfv(GL_LIGHT1, GL_DIFFUSE, light_diffuse1);
    glLightfv(GL_LIGHT1, GL_SPECULAR, light_specular1);
    glLightfv(GL_LIGHT1, GL_POSITION, light_position1);

    glLightf(GL_LIGHT1, GL_CONSTANT_ATTENUATION, 1);
    glLightf(GL_LIGHT1, GL_LINEAR_ATTENUATION, 0.01);
    glLightf(GL_LIGHT1, GL_QUADRATIC_ATTENUATION, 0.0001);

    glLightfv(GL_LIGHT1, GL_SPOT_DIRECTION, spot_direction1);
    glLightf(GL_LIGHT1, GL_SPOT_EXPONENT, 20.f);
    glLightf(GL_LIGHT1, GL_SPOT_CUTOFF, 20.f);
```

## Texture & Material
### Skybox
The six faces of the skybox are bound with different textures in order to build a realistic environment.
The class `Stage` has a special function that intake a pointer that points at an array of images and apply them to the skybox. To switch from day to night, the array is in a size of 12, when the night mode is selected, the latter 6 pictures will be chosen to display as the skybox.
Example codes:
```cpp
void Stage::setTextures(GLuint* _texids)
{
    texids = _texids;                       // Store texture references in pointer array
    toTexture = true;                       // Assume all loaded correctly
    for (int i = 0; i < 11; i++)             // Check if any textures failed to load (NULL)
        if (!texids[i]) toTexture = false;   // If one texture failed, do not display any
}

void Stage::Update(const double &deltaTime)
{
    // ReturnDay() is a function in Menu that returns the status of day or night
    if(ReturnDay() == true)
    {
        imageStartNumber = 0;
    }
    else
    {
        imageStartNumber = 6;
    }
}
```

### Car
The top and body of the car is set with material, each face is bound with a normal. Each face can display different colour under different lighting conditions. Besides, the window of the car is bound with a glass-like texture to provide a realistic look.
Example codes:
```cpp
    // colour reflected by diffuse light, light grey
    float mat_colour[] = { 0.8f, 0.8f, 0.8f, 0.f };
    // colour reflected by ambient light, dark grey
    float mat_ambient[] = { 0.5f, 0.5f, 0.5f, 0.f };
    // no reflection of specular colour, set the colour dark.
    float mat_spec[] = { 0.f, 0.f, 0.f, 0.f };

    glMaterialfv(GL_FRONT_AND_BACK, GL_AMBIENT, mat_ambient); // set colour for ambient reflectance
    glMaterialfv(GL_FRONT_AND_BACK, GL_DIFFUSE, mat_colour);  // set colour for diffuse reflectance
    glMaterialfv(GL_FRONT_AND_BACK, GL_SPECULAR, mat_spec);   // set colour for specular reflectance

	// front face
    glBegin(GL_POLYGON);
        glNormal3f(0.f, 0.f, 1.f);
        glVertex3f(-2*length, -highth, width/2);
        glVertex3f(2*length, -highth, width/2);
        glVertex3f(length, highth, width/2);
        glVertex3f(-length, highth, width/2);
    glEnd();
```


### House
The walls of the house is bound with a brick image to provide a realistic look.
The walls are made of six quads, which are bound with the same image.
Example codes:
```cpp
    if (toTexture)
    {
        glEnable(GL_TEXTURE_2D);                // enable texturing
        glBindTexture(GL_TEXTURE_2D, texid);    // bind 2D texture to shape
    }
    // front face
    glBegin(GL_POLYGON);
        if (toTexture) glTexCoord2f(1, 0);
        glVertex3f(length/2, -height/2, width/2.f);
        if (toTexture) glTexCoord2f(1, 1);
        glVertex3f(length/2, height/2, width/2.f);
        if (toTexture) glTexCoord2f(0, 1);
        glVertex3f(-length/2, height/2, width/2.f);
        if (toTexture) glTexCoord2f(0, 0);
        glVertex3f(-length/2, -height/2, width/2.f);
    glEnd();
```

### Arcade
The arcade uses a specific function to intake images `void Arcade::SetTextures(GLuint* _texids)`.
The pointer points at an array of three images, the first image is the arcade screen, the second image is the material for the top part of the arcade and the third image is the material of the bottom part of the arcade.
The top part of the arcade and the buttons and stick user a reflecting image to provide a metallic material while the bottom part of the arcade uses an images like a computer host to make the model more realistic.
The body of the arcade are quads the binds with image like the wall of the house, the button and the body of the stick are cylinders and solid circle binds with image.
![](https://raw.githubusercontent.com/invictuslh/Image_Hosting/master/uPic/20200520_133014.png)
Example codes of button and body of the stick:
```cs
// draw a texture-bound cylinder and a solid circle at top as a lid
void Arcade::DrawCylinderWithTop(float r, float h)
{
    float res = 0.1*M_PI; // a quad for every 18 degrees
    float x = r, z = 0.f; // the x and z position is affected by the angle
    float angle = 2.f*M_PI; // rotate a full circle
    float imageProgress = 0.f; // keep track of the progress in order to bind texture
    
    if (toTexture)
    {
        glEnable(GL_TEXTURE_2D);
        glBindTexture(GL_TEXTURE_2D, texids[1]); // bind the second image of the array to the cylinder
    }
    
    do // for a full circle (2 * M_PI)
    {
        glBegin(GL_QUADS);
            // first two points along the y axis
            // top
            if (toTexture) glTexCoord2d(imageProgress, 1);
            glVertex3f(x, h, z);
            // bottom
            if (toTexture) glTexCoord2d(imageProgress, 0);
            glVertex3f(x, 0.f, z);
            // Iterate around circle
            angle -= res;
            // update x and z position
            x = r*cos(angle);
            z = r*sin(angle);
            // other two points of the quad, counter clockwise
            // bottom
            if (toTexture) glTexCoord2d(imageProgress + res / 2.f*M_PI, 0);
            glVertex3f(x, 0.f, z);
            // top
            if (toTexture) glTexCoord2d(imageProgress + res / 2.f*M_PI, 1);
            glVertex3f(x, h, z);
            imageProgress = (2.f*M_PI - angle) / 2.f*M_PI;
        glEnd();
    } while (angle >= 0);
    if (toTexture) glDisable(GL_TEXTURE_2D);
    
    // Top face, visible from the top
    // reset the parameter
    x = r;
    z = 0.f;
    angle = 2.f*M_PI;
    // draw a solid circle with texture bound
    if (toTexture)
    {
        glEnable(GL_TEXTURE_2D);
        glBindTexture(GL_TEXTURE_2D, texids[1]);
    }
    glBegin(GL_POLYGON);
        do
        {
            angle -= res;
            x = r*cos(angle);
            z = r*sin(angle);
            if (toTexture) glTexCoord2d(0.5f * cos(angle), 0.5f * sin(angle));
            glVertex3f(x, h, z);
        } while (angle >= 0);
    glEnd();
    if (toTexture) glDisable(GL_TEXTURE_2D);
    
}
```


## Animation
### Car
- The wheel is spinning to make the moving more realistic. The animation is achieved though changing the angle when drawing the line strip. The angle is controlled by the variable `angle`

- The car can also move in the scene. This is achieved through the `distance` variable in `DrawCar()`, by updating variable `distance` in accordance to time, the car will translate same distance in the same period of time.
Example codes:
```cpp
void Car::Update(const double &dT)
{
	// update time
	aT = fmod(aT + dT, animationTime);
	// total animation stages of 36
	float aS = 36.f*aT / animationTime;
	// use angle to display the spin of the wheel
	angle = 10.f * aS;
	// use distance to control the movement of the car in the scene
	distance = aS * 4.f;
}

void Car::DrawWheel() // total height: 0.6 length
{
	float res = 0.1*M_PI; // eqivalent to 0.1 * 180 degree
	float radius = highth; // the radius of the whell
	float x = 0.f;
	float y = 0.f;
	glColor3f(0.9,0.9,0.9);
	glBegin(GL_LINE_STRIP);
    	for(float radians=0.f; radians<=2*M_PI; radians+=res)
    	  {
        	  glVertex3f(0,0,0);
        	  x = radius*cos(radians+angle/M_PI);
        	  y = radius*sin(radians+angle/M_PI);
        	  glVertex3f(x, y, 0);
    	  }
	glEnd();
	glColor3f(0,0,0);
	glutSolidTorus(0.2f*radius,radius,10,25);
}
```

### Human
The animation of walking consists the animation of legs and arms, as well as a movement in the scene.
The legs and arms can swing by updating the angle when drawing them, and the movement in the scene is achieved by the `distance` variable to translate the whole model in the scene. In addition, there are fourteen stages of the animation in order to set a repeatedly swing animation of arms and legs.
Example codes:
```cpp
void Human::Update(const double &dT)
{
    aT = fmod(aT + dT, animationTime);          // update time in animation cycle
    float aS = 14.f*aT / animationTime;         // 14 animation stages in total

    // wandering back and forward
    if (aS < 7)
    {
        distance = 3.f * aS;
        turn = 0.f;
    }
    else
    {
        distance = 3.f * (14.f - aS);
        turn = 180.f;
    }
    
    // the animation cycle (the movement of arms and legs)
    // 1st or final stage
    if (aS < 1.f || aS > 14.f)
    {
        angle = -30.f;
    }
    // 2nd or 13th stage
    else if (aS < 2.f || aS > 13.f)
    {
        angle = -20.f;
    }
    // 3rd or 12th stage
    else if (aS < 3.f || aS > 12.f)
    {
        angle = -10.f;
    }
    // 4rd or 11th stage
    else if (aS < 4.f || aS > 11.f)
    {
        angle = 0.f;
    }
    // 5rd or 10th stage
    else if (aS < 5.f || aS > 10.f)
    {
        angle = 10.f;
    }
    // 6rd or 9th stage
    else if (aS < 6.f || aS > 9.f)
    {
        angle = 20.f;
    }
    // 7th and 8th stage
    else
    {
        angle = 30.f;
    }
}
```

### Galaxy
The animation of six stars orbiting around a sun is achieved by updating the angle when drawing the solid sphere.
Example codes:
```cpp
    // Draw planet, center to center distance: 6, radius: 1
    glPushMatrix();
        glRotatef(30.f + angle, 0.f, 1.f, 0.f);
        glTranslatef(6.f, 0.f, 0.f);
        glColor3f(1.f, 0.f, 0.f);
        DrawStar(1);
    glPopMatrix();
```

### House
Trigger animation, when the camera is close enough to the front gate, the gate will automatically open. Achieved through a Boolean variable `doorOpen` and by detecting static variable `eyePosition` in class `MyCamera`.
Example codes:
```cpp
void House::Update(const double &dT)
{
    if (MyCamera::eyePosition[0] < -250 && MyCamera::eyePosition[2] < 300)
    {
        doorOpen = true;
    }
    else
    {
        doorOpen = false;
    }
    DrawHouse();
}
```

## Interaction
### Camera
The camera class `MyClass` used in the scene is inherited from the class `Camera` in the framework. The key detection and mouse detection function (`Update` and `HandleKey`)are changed in order to provide more flexibility when viewing the scene.
1. Movement:
	Use WASD to move around the scene.
	Use Q to move upwards, and use E to move downwards
	Use R to reset the camera to its default position and the place it looks at
2. Look at:
	Drag the mouse to look around the scene.
The variable `eyePostion` is set to be public static in order to enable other class obtain the current position of the camera.
Example codes:
```cpp
void MyCamera::Update(const double& deltaTime)
{
    float speed = 4.0f;
    // move the camera rightwards
    if (aKey)
        sub(eyePosition, right, speed);
    // move the camera leftwards
    if (dKey)
        add(eyePosition, right, speed);
    // move the camera forwards
    if (wKey)
        add(eyePosition, forward, speed);
    // move the camera backwards
    if (sKey)
        sub(eyePosition, forward, speed);
    // move the camera upwards
    if (qKey)
        add(eyePosition, up, speed);
    // move the camera downwards
    if (eKey)
        sub(eyePosition, up, speed);
    // reset the camera
    if (rKey)
        Reset();

    SetupCamera();
}
```

### Right click menu
![](https://raw.githubusercontent.com/invictuslh/Image_Hosting/master/uPic/20200520_133134.png)
The program establishes a right click menu to provide the ability to change the scene from day to night, the light of the scene will be turn off and the skybox will be changed to the according one.
Example code:
```cpp
void processMenuEvents(int option)
{
    switch (option)
    {
        case DAY:
            day = true;
            glEnable(GL_LIGHT0);
            glEnable(GL_LIGHT3);
            break;
        case NIGHT:
            day = false;
            //glDisable(GL_LIGHT3);
            glDisable(GL_LIGHT0);
            break;
    }
    glutPostRedisplay();
}


void createGLUTMenus() {
    int menu;
    // establish the menu and use processMenuEvents to cope with menu events
    menu = glutCreateMenu(processMenuEvents);
    // add entrys to the menu
    glutAddMenuEntry("DAY", DAY);
    glutAddMenuEntry("NIGHT", NIGHT);

//    glutAddMenuEntry("Move", MOVE);

    // connect the menu to right mouse button
    glutAttachMenu(GLUT_RIGHT_BUTTON);
}
```

### Trigger animation
If the camera is close enough to the gate of the house, the door of the house will open automatically.
Example codes:
```cpp
void House::Update(const double &dT)
{
    if (MyCamera::eyePosition[0] < -250 && MyCamera::eyePosition[2] < 300)
    {
        doorOpen = true;
    }
    else
    {
        doorOpen = false;
    }
    DrawHouse();
}
```

## Conclusion & Future works
In conclusion, this project establishes an urban scene with a house, a car, a human and other decorating objects. Some of the models could be more complicated such as the house and the car in order to make the scene more realistic. The model of arcade is well-designed and bound with appropriate textures, it is the most satisfactory model in the scene in my opinion. The scene also provides some interactiveness, the user can view the scene from every perspective and the trigger to open the door of the house makes the scene less rigid.
As future work, more complex models will be added to make the scene more realistic and more texture will be used in order to achieve this goal.
