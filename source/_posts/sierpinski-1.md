title: 'Sierpinski三角形'
tags:
  - 计算机图形学
  - 分形
id: 943
categories:
  - 算法/数据结构
date: 2013-06-26 21:09:47
---

大自然一切都是随机的，但是很多事物又反应了它自身的规律。
在学习分形的过程中，再一次见到了这种随机中的规律，一切都那么的美妙。

谢宾斯基(Sierpinski)分形三角形,这里使用画点构图的方法。

按照其生成算法：
1.在一个平面里，取三个点ABC，组成一个三角形 
2.在这个三角形附近，任选一点T作为第一个点 
3.在ABC三点中任意选一个点P，画出P与上一个点Q的中点，并画出
4.达到迭代次数退出，否则回第3步 

<!--more-->

## 基于OpenGL的实现代码

```
#include"stdafx.h"
#include<cstdlib>
#include<cmath>
#include<ctime>
#include<gl/glut.h>

#define MY_INIT_RAND() (srand((unsigned int)time(NULL)))
#define MY_RANDOM(A) (rand()%A)

typedef struct
{
	GLint x,
		  y;
}GLintPoint;

void myInit(void)
{
	glClearColor(1.0,1.0,1.0,0.0);
	glColor3f(0.0f,0.0f,0.0f);
	glPointSize(4.0);
	glMatrixMode(GL_PROJECTION);
	glLoadIdentity();
	gluOrtho2D(0.0,640.0,0.0,480.0);
}

void drawDot(GLint x,GLint y)
{
	glBegin(GL_POINTS);
		glVertex2i(x,y);
	glEnd();
}

void sierpinski_render(void)
{
	glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
	GLintPoint T[3] = {(0,0),(640,480),(480,640)};
	MY_INIT_RAND();
	int index = MY_RANDOM(3);
	GLintPoint point = T[index];
	drawDot(point.x,point.y);
	for(int i=0; i<150000; i++)
	{
		index = MY_RANDOM(3);
		point.x = (point.x + T[index].x) / 2;
		point.y = (point.y + T[index].y) / 2;
		drawDot(point.x,point.y);
	}
	glutSwapBuffers(); 
}

int _tmain(int argc, _TCHAR* argv[])
{
	glutInit(&argc, (char**) argv);
	glutInitDisplayMode(GLUT_DEPTH | GLUT_DOUBLE | GLUT_RGBA);
	glutInitWindowPosition(100,100);
	glutInitWindowSize(800,600);
	glutCreateWindow("Hello OpenGL");
	myInit();
	glutDisplayFunc(sierpinski_render);
	glutMainLoop();
	return 0;
}
```