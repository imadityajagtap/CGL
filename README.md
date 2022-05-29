# CGL
#include <stdio.h>
#include <math.h>
//#include<windows.h>
#include <GL/glut.h>
double X1, Y1, X2, Y2;
float round_value(float v)
{
return floor(v + 0.5);
}
void LineDDA(void)
{
double dx=(X2-X1);
double dy=(Y2-Y1);
double steps;
float xInc,yInc,x=X1,y=Y1;
/* Find out whether to increment x or y */
steps=(abs(dx)>abs(dy))?(abs(dx)):(abs(dy));
xInc=dx/(float)steps;
yInc=dy/(float)steps;
/* Clears buffers to preset values */
glClear(GL_COLOR_BUFFER_BIT);
/* Plot the points */
glBegin(GL_POINTS);
/* Plot the first point */
glVertex2d(x,y);
int k;
/* For every step, find an intermediate vertex */
for(k=0;k<steps;k++)
{
x+=xInc;
y+=yInc;
/* printf("%0.6lf %0.6lf\n",floor(x), floor(y)); */
glVertex2d(round_value(x), round_value(y));
}
glEnd();
glFlush();
}
void Init()
{
/* Set clear color to white */
glClearColor(1.0,1.0,1.0,0);
/* Set fill color to black */
glColor3f(0.0,0.0,0.0);
/* glViewport(0 , 0 , 640 , 480); */
/* glMatrixMode(GL_PROJECTION); */
/* glLoadIdentity(); */
gluOrtho2D(0 , 640 , 0 , 480);
}
int main(int argc, char **argv)
{
printf("Enter two end points of the line to be drawn:\n");
printf("\n************************************");
printf("\nEnter Point1( X1 , Y1):\n");
scanf("%lf%lf",&X1,&Y1);
printf("\n************************************");
printf("\nEnter Point1( X2 , Y2):\n");
scanf("%lf%lf",&X2,&Y2);
/* Initialise GLUT library */
glutInit(&argc,argv);
/* Set the initial display mode */
glutInitDisplayMode(GLUT_SINGLE | GLUT_RGB);
/* Set the initial window position and size */
glutInitWindowPosition(0,0);
glutInitWindowSize(640,480);
/* Create the window with title "DDA_Line" */
glutCreateWindow("DDA_Line");
/* Initialize drawing colors */
Init();
/* Call the displaying function */
glutDisplayFunc(LineDDA);
/* Keep displaying untill the program is closed */
glutMainLoop();
}


/*****************************************************BRESHAM LINE*******************************************************/
#include <GL/glut.h>
#include<math.h>
#include<stdlib.h>
#include<iostream>
#include<stdio.h>
using namespace std;
int sign(float arg)
{
if(arg<0)
return -1;
else if(arg==0)
return 0;
else
return 1;
}
void Bre(int X1,int Y1,int X2,int Y2)
{
float dx=abs(X2-X1);
float dy=abs(Y2-Y1);
int s1,s2,exc,y,x,i;
float g,temp;
x=X1;
y=Y1;
s1=sign(X2-X1);
s2=sign(Y2-Y1);
glBegin(GL_POINTS);
if(dy>dx)
{
temp=dx;
dx=dy;
dy=temp;
exc=1;
}
else
{
exc=0;
}
g=2*dy-dx;
i=1;
while(i<=dx)
{
glVertex2d(x,y);
while(g>=0)
{
if(exc==1)
x=x+s1;
else
y=y+s2;
g=g-2*dx;
}
if(exc==1)
y=y+s2;
else
x=x+s1;
g=g+2*dy;
i++;
}
glEnd();
glFlush();
}
void display()
{
Bre(40,40,200,200);
Bre(250,4000,100,100);
}
void myInit(void)
{ glClearColor(1.0,1.0,1.0,0.0);
glColor3f(1,1,1);
glPointSize(1.0);
glMatrixMode(GL_PROJECTION);
glLoadIdentity();
gluOrtho2D(0.0,640.0,0.0,480.0);
}
int main(int argc,char **argv)
{
glutInit(&argc,argv);
glutInitDisplayMode(GLUT_SINGLE|GLUT_RGB);
glutInitWindowSize(640,480);
glutInitWindowPosition(100,150);
glutCreateWindow("Bresenham Line ");
glutDisplayFunc(display);
myInit();
glutMainLoop();
return 0;
}
  
 /**************************************************** Circle **************************************************************/
  #include <GL/glut.h>
#include<math.h>
#include<stdlib.h>
#include<iostream>
#include<stdio.h>
using namespace std;
void symmetry(double xc,double yc,double x,double y)
{
glBegin(GL_POINTS);
glVertex2i(xc+x, yc+y);
glVertex2i(xc+x, yc-y);
glVertex2i(xc+y, yc+x);
glVertex2i(xc+y, yc-x);
glVertex2i(xc-x, yc-y);
glVertex2i(xc-y, yc-x);
glVertex2i(xc-x, yc+y);
glVertex2i(xc-y, yc+x);
glEnd();
}
void circle(double x1,double y1,double r)
{
int x=0,y=r;
float pk=(5.0/4.0)-r;
/* Plot the points */
/* Plot the first point */
symmetry(x1,y1,x,y);
int k;
/* Find all vertices till x=y */
while(x < y)
{
 x = x + 1;
 if(pk < 0)
pk = pk + 2*x+1;
 else
 {
y = y - 1;
pk = pk + 2*(x - y) + 1;
 }
 symmetry(x1,y1,x,y);
}
glFlush();
}
void display()
{
circle(200,200,120);
}
void myInit(void)
{ glClearColor(1.0,1.0,1.0,0.0);
glColor3f(1,1,1);
glPointSize(2.0);
glMatrixMode(GL_PROJECTION);
glLoadIdentity();
gluOrtho2D(0.0,640.0,0.0,480.0);
}
int main(int argc,char **argv)
{
glutInit(&argc,argv);
glutInitDisplayMode(GLUT_SINGLE|GLUT_RGB);
glutInitWindowSize(640,480);
glutInitWindowPosition(100,150);
glutCreateWindow("Circle");
glutDisplayFunc(display);
myInit();
glutMainLoop();
return 0;
}

  /**************************************************POLYGON*************************************************************/
  
  #include<iostream>
#include<GL/glut.h> 


void DrawPolygon(int x1, int z1, int x2, int y2) { 
float x,y,dx,dy,xin,yin,k,step,i;
dx=(x2-x1);
dy=(y2-z1);
if(abs(dx)>abs(dy))
{
step=abs(dx);
}
else{
step=abs(dy);
}
xin=dx/step;
yin=dy/step;
x=x1;
y=z1;

glBegin(GL_POINTS);
glVertex2i(x,y);
glEnd();
for(k=1;k<=step;k++)
{
x=x+xin;
y=y+yin;
glBegin(GL_POINTS);
glVertex2i(x,y);
glEnd();
}
glFlush();
}



void putpixel(int c, int d, float* fillColor) { 
glColor3f(fillColor[0], fillColor[1], fillColor[2]);  
  glBegin(GL_POINTS);
  glVertex2i(c, d); 
  glEnd(); 
  glFlush();
}

 void boundaryFill4(int x, int y, float* fillColor, float* boundarycolor) { 
  float color[3];
  glReadPixels(x, y, 1.0, 1.0, GL_RGB, GL_FLOAT, color);
  if ((color[0] != boundarycolor[0] || color[1] != boundarycolor[1] ||     color[2] != boundarycolor[2]) && 
(color[0] != fillColor[0] || color[1] != fillColor[1] || color[2] != fillColor[2])) 
{ 
 putpixel(x, y, fillColor);
 boundaryFill4(x + 2, y, fillColor, boundarycolor); 
 boundaryFill4(x - 2, y, fillColor, boundarycolor);
 boundaryFill4(x, y + 2, fillColor, boundarycolor); 
 boundaryFill4(x, y - 2, fillColor, boundarycolor);
 }
}

void Floodfill4(int x, int y, float* fillColor, float* interiorColor) 
 {
   float color[3];
   glReadPixels(x, y, 1.0, 1.0, GL_RGB, GL_FLOAT, color);
   if (color[0] == interiorColor[0] && color[1] == interiorColor[1] && color[2] == interiorColor[2]) 
 {
 putpixel(x, y, fillColor);
 Floodfill4(x + 2, y, fillColor, interiorColor); 
 Floodfill4(x - 2, y, fillColor, interiorColor);
 Floodfill4(x, y + 2, fillColor, interiorColor);  
 Floodfill4(x, y - 2, fillColor, interiorColor);
  }
 }
 
 void myMouse(int button, int state, int x, int y) 
 { 
  y = 480 - y;
  if (button == GLUT_LEFT_BUTTON)
  {
   if (state == GLUT_DOWN)
   {
     float boundaryCol[] = { 0,1,0 };
     float fillcolor[] = { 1,0,1 }; 
     boundaryFill4(x, y, fillcolor, boundaryCol);
   }
  } 
 }

 void myMouse1(int button, int state, int x, int y) { 
  y = 480 - y;
  if (button == GLUT_LEFT_BUTTON && state == GLUT_DOWN) { 
    float interiorcolor[] = { 1,1,1 };
    float fillcolor[] = { 1,1,0 };  
    Floodfill4(x, y, fillcolor, interiorcolor);
  }
}

 void myDisplay() { 
   glPointSize(2.0);
   glClear(GL_COLOR_BUFFER_BIT); 
   glColor3f(0, 1, 0);
   DrawPolygon(100, 100, 200, 200);
   DrawPolygon(200, 200, 400, 200);
   DrawPolygon(400, 200, 400, 100);
   DrawPolygon(400, 100, 100, 100); 
   glEnd();
   glFlush(); 
  }
  
 void myMenu(int item) { 
  if (item == 1) {
    glutMouseFunc(myMouse);
  }
  else if (item == 2) {
    glutMouseFunc(myMouse1);
  }
 }
 
 void Init() 
{
 glClearColor(1.0, 1.0, 1.0, 0.0); 
 glMatrixMode(GL_PROJECTION); 
 gluOrtho2D(0.0, 640.0, 0.0, 480.0);
 }
 
 int main(int argc, char** argv) { 
    glutInit(&argc, argv);
    glutInitDisplayMode(GLUT_SINGLE | GLUT_RGB); 
    glutInitWindowSize(640, 480);
    glutInitWindowPosition(200, 200); 
    glutCreateWindow("Polygon Filling"); 
    glutDisplayFunc(myDisplay); 
    glutCreateMenu(myMenu); 
    glutAttachMenu(GLUT_RIGHT_BUTTON); 
    glutAddMenuEntry("Boundary fill",1); 
    glutAddMenuEntry("Flood fill",2);
    Init();
    glutMainLoop(); 
    return 0;
}
