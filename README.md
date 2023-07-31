# snowman-
#include <stdlib.h>
#include <math.h>
#include <gl/glut.h>

static float angle = 0.0, ratio;
static float x = 0.0f, y = 1.75f, z = 5.0f;
static float lx = 0.0f, ly = 0.0f, lz = -1.0f;
static GLint snowman_Display_list, sun_list;
float deltaAngle = 0.0f;
int deltaMove = 0;
GLfloat red = 1.0, blue = 0.0, green = 1.0;

void ChangeSize(int w, int h)
{
	if (h == 0)h = 1;
	ratio = 1.0f + w / h;
	glMatrixMode(GL_PROJECTION);
	glLoadIdentity();
	glViewport(0, 0, w, h);
	gluPerspective(45, ratio, 1, 1000);
	glMatrixMode(GL_MODELVIEW);
	gluLookAt(x, y, z, x + lx, y, z + lz, 0.0f, 1.0f, 0.0f);
}

void Drawsun()
{
	glPushMatrix();
	glTranslatef(0, 13, -50);
	glColor3f(0.99, 0.99, 0.20);
	glutSolidSphere(4, 20, 20);
	glPopMatrix();

}

void DrawSnowman()
{
	glColor3f(1.0f, 1.0f, 1.0f);
	glTranslatef(0.0f, 0.5f, 0.0f);
	glutSolidSphere(0.75f, 20, 20);
	//glutSolidCube(1.0);
	glPushMatrix();

	glColor3f(0.2f, 0.7f, 0.3f);
	glTranslatef(0.0f, 0.0f, 0.50f);
	glutSolidSphere(0.03, 20, 20);
	glTranslatef(0.0f, -0.25f, 0.0f);
	glutSolidSphere(0.03, 20, 20);
	glTranslatef(0.0f, 0.5f, 0.0f);
	glutSolidSphere(0.03, 20, 20);



	glPopMatrix();


	glColor3f(1.0f, 1.0f, 1.0f);
	glTranslatef(0.0f, 0.75f, 0.0f);
	glutSolidSphere(0.25f, 20, 20);

	glColor3b(90, 50, 100);
	glTranslatef(0.27f, 0.0f, 0.0f);
	glutSolidSphere(0.05f, 20, 20);
	glTranslatef(-0.54f, 0.0f, 0.0f);
	glutSolidSphere(0.05f, 20, 20);
	glTranslatef(0.27f, 0.0f, 0.0f);


	glPushMatrix();
	glColor3f(0.0f, 0.0f, 0.0f);
	glTranslatef(0.05f, 0.10f, 0.18f);
	glutSolidSphere(0.05f, 10, 10);
	glTranslatef(-0.1f, 0.0f, 0.0f);
	glutSolidSphere(0.05f, 10, 10);
	glPopMatrix();
	glColor3f(1.0f, 0.5f, 0.2f);
	glRotatef(0.0f, 1.0f, 0.0f, 0.0f);
	glutSolidCone(0.08, 0.5f, 10, 2);
}

void OrientMe(float ang)
{
	lx = sin(ang);
	lz = -cos(ang);
	glLoadIdentity();
	gluLookAt(x, y, z, x + lx, y, z + lz, 0.0f, 1.0f, 0.0f);
}

void MoveMeFlat(int i)
{
	x = x + i*(lx)*0.1;
	z = z + i*(lz)*0.1;
	glLoadIdentity();
	gluLookAt(x, y, z, x + lx, y, z + lz, 0.0f, 1.0f, 0.0f);
}

void ProcessSpecialkeys(int key, int x, int y)
{
	if (key == GLUT_KEY_F2)
	{
		red = 0.0;
		green = 1.0;
		blue = 0.0;
	}
	if (key == GLUT_KEY_F3)
	{
		red = 0.0;
		green = 0.0;
		blue = 1.0;
	}
	int mod;
	if (key == GLUT_KEY_F1)
	{
		mod = glutGetModifiers();
		if (mod & GLUT_ACTIVE_CTRL && mod & GLUT_ACTIVE_ALT)
		{
			red = 0.1;
			green = 0.60;
			blue = 0.80;
		}
		else
		{
			red = 1.0;
			green = 0.0;
			blue = 0.0;
		}
	}
	x = x + x - x;
	y = y;
}

void ProcessNormalKeys(unsigned char key, int x, int y)
{
	int mod;
	if (key == 'q')exit(0);
	else if (key == 'r')
	{
		mod = glutGetModifiers();
		if (mod == GLUT_ACTIVE_ALT)
		{
			red = 0.12;
			green = 0.65;
			blue = 0.35;
		}
		else
		{
			red = 0.88;
			green = 0.35;
			blue = 0.65;
		}
	}
	x = x;
	y = y;
}


void renderscene()
{
	if (deltaMove != 0)
	{
		MoveMeFlat(deltaMove);
	}
	if (abs(deltaAngle)>0.008)
	{
		angle += deltaAngle;
		OrientMe(angle);
	}
	//glLoadIdentity();
	//gluLookAt (1,1.75,5,0,1,0,0,1,0);

	glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
	glColor3f(0.9f, 0.9f, 0.9f);
	glBegin(GL_QUADS);
	glVertex3f(-100.0f, 0.0f, -100.0f);
	glVertex3f(-100.0f, 0.0f, 100.0f);
	glVertex3f(100.0f, 0.0f, 100.0f);
	glVertex3f(100.0f, 0.0f, -100.0f);
	glEnd();
	// Draw House
	glColor3f(0.45, 0.4, 0.54);
	glBegin(GL_QUADS);
	glVertex3f(0.0f, 7.0f, -33.0f);
	glVertex3f(0.0f, 0.0f, -33.0f);
	glVertex3f(10.0f, 0.0f, -33.0f);
	glVertex3f(10.0f, 7.0f, -33.0f);
	glEnd();

	glColor3f(0.5, 0.3, 0.2);
	glBegin(GL_TRIANGLES);
	glVertex3f(-2.0f, 7.0f, -33.0f);
	glVertex3f(12.0f, 7.0f, -33.0f);
	glVertex3f(5.0f, 10.0f, -33.0f);
	//glVertex3f(10.0f,7.0f, -33.0f);
	glEnd();


	glColor3f(0.5, 0.3, 0.2);
	glBegin(GL_QUADS);
	glVertex3f(-9.0f, 7.0f, -33.0f);
	glVertex3f(-9.0f, 0.0f, -33.0f);
	glVertex3f(-8.0f, 0.0f, -33.0f);
	glVertex3f(-8.0f, 7.0f, -33.0f);
	glEnd();

	glColor3f(0, 0.8, 0);
	glBegin(GL_TRIANGLES);
	glVertex3f(-11.0f, 7.0f, -33.0f);
	glVertex3f(-6.0f, 7.0f, -33.0f);
	glVertex3f(-8.5f, 13.0f, -33.0f);
	//glVertex3f(10.0f,7.0f, -33.0f);
	glEnd();
	glBegin(GL_TRIANGLES);
	glVertex3f(-11.0f, 8.5f, -33.0f);
	glVertex3f(-6.0f, 8.5f, -33.0f);
	glVertex3f(-8.5f, 13.0f, -33.0f);
	//glVertex3f(10.0f,7.0f, -33.0f);
	glEnd();

	glBegin(GL_TRIANGLES);
	glVertex3f(-11.0f, 10.0f, -33.0f);
	glVertex3f(-6.0f, 10.0f, -33.0f);
	glVertex3f(-8.5f, 13.0f, -33.0f);
	//glVertex3f(10.0f,7.0f, -33.0f);
	glEnd();



	glColor3f(0.5, 0.3, 0.2);
	glBegin(GL_QUADS);
	glVertex3f(13.0f, 7.0f, -33.0f);
	glVertex3f(13.0f, 0.0f, -33.0f);
	glVertex3f(14.0f, 0.0f, -33.0f);
	glVertex3f(14.0f, 7.0f, -33.0f);
	glEnd();

	glColor3f(0, 0.8, 0);
	glBegin(GL_TRIANGLES);
	glVertex3f(11.0f, 7.0f, -33.0f);
	glVertex3f(16.0f, 7.0f, -33.0f);
	glVertex3f(13.5f, 13.0f, -33.0f);
	//glVertex3f(10.0f,7.0f, -33.0f);
	glEnd();
	glBegin(GL_TRIANGLES);
	glVertex3f(11.0f, 8.5f, -33.0f);
	glVertex3f(16.0f, 8.5f, -33.0f);
	glVertex3f(13.5f, 13.0f, -33.0f);
	//glVertex3f(10.0f,7.0f, -33.0f);
	glEnd();

	glBegin(GL_TRIANGLES);
	glVertex3f(11.0f, 10.0f, -33.0f);
	glVertex3f(16.0f, 10.0f, -33.0f);
	glVertex3f(13.5f, 13.0f, -33.0f);
	//glVertex3f(10.0f,7.0f, -33.0f);
	glEnd();

	glCallList(sun_list);
	/*glPushMatrix();
	glTranslatef(0,3,0);
	//glColor3f (1,0.5,0.6);
	//glutSolidOctahedron ();
	//glutSolidDodecahedron ();
	//glutSolidIcosahedron();
	//glutSolidTetrahedron();
	glPopMatrix(); */

	for (int i = -3; i<2; i++)
	{
		for (int j = -3; j<2; j++)
		{
			glPushMatrix();
			glTranslatef(i*10.0, 0, j*10.0);
			glCallList(snowman_Display_list);
			//DrawSnowman();
			glPopMatrix();
		}
	}
	//glutPostRedisplay();
	glutSwapBuffers();
	//glFlush();
}

GLuint CreateDi()
{
	GLuint snowmanDL;
	snowmanDL = glGenLists(1);
	glNewList(snowmanDL, GL_COMPILE);
	DrawSnowman();
	glEndList();
	return snowmanDL;
}

GLuint Create_SunDi()
{
	GLuint sunDl;
	sunDl = glGenLists(1);
	glNewList(sunDl, GL_COMPILE);
	Drawsun();
	glEndList();
	return sunDl;
}
void InitScene()
{
	glEnable(GL_DEPTH_TEST);
	snowman_Display_list = CreateDi();
	sun_list = Create_SunDi();
}

void PressKey(int key, int x, int y)
{
	switch (key)
	{
	case GLUT_KEY_LEFT:
		deltaAngle = -0.01f;
		break;
	case GLUT_KEY_RIGHT:
		deltaAngle = 0.01f;
		break;
	case GLUT_KEY_UP:
		deltaMove = 1;
		break;
	case GLUT_KEY_DOWN:
		deltaMove = -1;
		break;
	}
	x = x;
	y = y;
}

void ReleaseKey(int key, int x, int y)
{
	switch (key)
	{
	case GLUT_KEY_LEFT:
		deltaAngle = 0.0f;
		break;
	case GLUT_KEY_RIGHT:
		deltaAngle = 0.0f;
		break;
	case GLUT_KEY_UP:
		deltaMove = 0;
		break;
	case GLUT_KEY_DOWN:
		deltaMove = 0;
		break;
	}
	x = x;
	y = y;
}

int main(int argc, char** argv)
{
	glutInit(&argc, argv);
	glutInitDisplayMode(GLUT_DEPTH | GLUT_DOUBLE | GLUT_RGBA);
	glutInitWindowPosition(100, 100);
	glutInitWindowSize(640, 360);
	glutCreateWindow("Snowman");
	InitScene();
	glutIgnoreKeyRepeat(1);

	glutSpecialFunc(PressKey);
	glutSpecialUpFunc(ReleaseKey);
	glutDisplayFunc(renderscene);
	glutIdleFunc(renderscene);
	glutReshapeFunc(ChangeSize);

	glutMainLoop();
	return(0);
}
