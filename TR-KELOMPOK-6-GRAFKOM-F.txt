#include <GL/glut.h>
#include <math.h>
#include <iostream>
using namespace std;

#define FPS 120
#define TO_RADIANS 3.14/180.0

//  Anggota Kelompok 6
//  Dhamar Dwy Arief Wibawa(672019080)
//  Anatasya Lingkanwene Ering(672020165)
//  Sarah Larasati(672020174)
//  Jessica Patricia Rahardjo(672020179)
//  Victor Angky Metekohy(672020216)

const int width = 1280;
const int height = 720;
int i;
float sudut;
double x_geser, y_geser, z_geser;

float pitch = 0.0, yaw = 0.0;
float camX = 0.0, camZ = 2000.0, terbang = 50.0;

void display();
void reshape(int w, int h);
void timer(int);
void passive_motion(int, int);
void camera();
void keyboard(unsigned char key, int x, int y);
void keyboard_up(unsigned char key, int x, int y);

struct Motion {
	bool Forward, Backward, Left, Right, Naik, Turun;
};
Motion motion = { false,false,false,false,false,false };

void init() {
	glClearColor(0.529, 0.807, 0.921, 0.0);
	glutSetCursor(GLUT_CURSOR_NONE);
	glEnable(GL_DEPTH_TEST);
	glEnable(GL_BLEND);
	glBlendFunc(GL_SRC_ALPHA, GL_ONE_MINUS_SRC_ALPHA);
	glDepthFunc(GL_LEQUAL);
	glutWarpPointer(width / 2, height / 2);
}

void matahari() {
	glPushMatrix();
	glColor3f(100, 0.78, 0);
	glTranslatef(0, 2200, 0);
	glutWireSphere(200.0, 360, 360);
	glPopMatrix();
	glEnd();
}

void ground() {
	glBegin(GL_QUADS);
	glColor3f(0.21, 0.08, 0);
	glVertex3f(-1500.0, 0, -1500.0);
	glVertex3f(1500.0, 0, -1500.0);
	glVertex3f(1500.0, 0, 1500.0);
	glVertex3f(-1500.0, 0, 1500.0);
	glEnd();
}

void tanah() {
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.07, 0.36, 0.07);
	glVertex3f(-1500.0, 40, -1500.0);
	glVertex3f(1500.0, 40, -1500.0);
	glVertex3f(1500.0, 40, 1500.0);
	glVertex3f(-1500.0, 40, 1500.0);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.21, 0.08, 0);
	glVertex3f(1500.0, 0, -1500.0);
	glVertex3f(1500.0, 40, -1500.0);
	glVertex3f(1500.0, 40, 1500.0);
	glVertex3f(1500.0, 0, 1500.0);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.21, 0.08, 0);
	glVertex3f(-1500.0, 0, -1500.0);
	glVertex3f(-1500.0, 40, -1500.0);
	glVertex3f(-1500.0, 40, 1500.0);
	glVertex3f(-1500.0, 0, 1500.0);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.21, 0.08, 0);
	glVertex3f(-1500.0, 0, -1500.0);
	glVertex3f(-1500.0, 40, -1500.0);
	glVertex3f(1500.0, 40, -1500.0);
	glVertex3f(1500.0, 0, -1500.0);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.21, 0.08, 0);
	glVertex3f(-1500.0, 0, 1500.0);
	glVertex3f(-1500.0, 40, 1500.0);
	glVertex3f(1500.0, 40, 1500.0);
	glVertex3f(1500.0, 0, 1500.0);
	glEnd();
}
void tanahb() {
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.09, 0.46, 0.09);
	glVertex3f(-1120.0, 45, -1120.0);
	glVertex3f(1120.0, 45, -1120.0);
	glVertex3f(1120.0, 45, 1120.0);
	glVertex3f(-1120.0, 45, 1120.0);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.09, 0.46, 0.09);
	glVertex3f(1120.0, 0, -1120.0);
	glVertex3f(1120.0, 45, -1120.0);
	glVertex3f(1120.0, 45, 1120.0);
	glVertex3f(1120.0, 0, 1120.0);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.09, 0.46, 0.09);
	glVertex3f(-1120.0, 0, -1120.0);
	glVertex3f(-1120.0, 45, -1120.0);
	glVertex3f(-1120.0, 45, 1120.0);
	glVertex3f(-1120.0, 0, 1120.0);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.09, 0.46, 0.09);
	glVertex3f(-1120.0, 0, -1120.0);
	glVertex3f(-1120.0, 45, -1120.0);
	glVertex3f(1120.0, 45, -1120.0);
	glVertex3f(1120.0, 0, -1120.0);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.09, 0.46, 0.09);
	glVertex3f(-1120.0, 0, 1120.0);
	glVertex3f(-1120.0, 45, 1120.0);
	glVertex3f(1120.0, 45, 1120.0);
	glVertex3f(1120.0, 0, 1120.0);
	glEnd();
}

void jalana() {
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.2, 0.2, 0.2);
	glVertex3f(180.0, 50, 700.0);
	glVertex3f(-180.0, 50, 700.0);
	glVertex3f(-180.0, 50, 1150.0);
	glVertex3f(180.0, 50, 1150.0);
	glEnd();

	//kiri
	glBegin(GL_QUADS);
	glColor3f(0.75, 0.75, 0.75);
	glVertex3f(-180.0, 55, 700.0);
	glVertex3f(-200.0, 55, 700.0);
	glVertex3f(-200.0, 55, 1100.0);
	glVertex3f(-180.0, 55, 1100.0);
	glEnd();

	glBegin(GL_QUADS);
	glColor3f(0.6, 0.6, 0.6);
	glVertex3f(-180.0, 40, 700.0);
	glVertex3f(-200.0, 40, 700.0);
	glVertex3f(-200.0, 40, 1100.0);
	glVertex3f(-180.0, 40, 1100.0);
	glEnd();

	glBegin(GL_QUADS);
	glColor3f(0.6, 0.6, 0.6);
	glVertex3f(-200.0, 40, 700.0);
	glVertex3f(-200.0, 55, 700.0);
	glVertex3f(-200.0, 55, 1100.0);
	glVertex3f(-200.0, 40, 1100.0);
	glEnd();

	glBegin(GL_QUADS);
	glColor3f(0.6, 0.6, 0.6);
	glVertex3f(-180.0, 40, 700.0);
	glVertex3f(-180.0, 55, 700.0);
	glVertex3f(-180.0, 55, 1100.0);
	glVertex3f(-180.0, 40, 1100.0);
	glEnd();
	glBegin(GL_QUADS);
	glColor3f(0.6, 0.6, 0.6);
	glVertex3f(-180.0, 55, 1100.0);
	glVertex3f(-200.0, 55, 1100.0);
	glVertex3f(-200.0, 40, 1100.0);
	glVertex3f(-180.0, 40, 1100.0);
	glEnd();


	//kanan
	glBegin(GL_QUADS);
	glColor3f(0.75, 0.75, 0.75);
	glVertex3f(180.0, 55, 700.0);
	glVertex3f(200.0, 55, 700.0);
	glVertex3f(200.0, 55, 1100.0);
	glVertex3f(180.0, 55, 1100.0);
	glEnd();

	glBegin(GL_QUADS);
	glColor3f(0.6, 0.6, 0.6);
	glVertex3f(180.0, 40, 700.0);
	glVertex3f(200.0, 40, 700.0);
	glVertex3f(200.0, 40, 1100.0);
	glVertex3f(180.0, 40, 1100.0);
	glEnd();

	glBegin(GL_QUADS);
	glColor3f(0.6, 0.6, 0.6);
	glVertex3f(200.0, 40, 700.0);
	glVertex3f(200.0, 55, 700.0);
	glVertex3f(200.0, 55, 1100.0);
	glVertex3f(200.0, 40, 1100.0);
	glEnd();

	glBegin(GL_QUADS);
	glColor3f(0.6, 0.6, 0.6);
	glVertex3f(180.0, 40, 700.0);
	glVertex3f(180.0, 55, 700.0);
	glVertex3f(180.0, 55, 1100.0);
	glVertex3f(180.0, 40, 1100.0);
	glEnd();
	glBegin(GL_QUADS);
	glColor3f(0.6, 0.6, 0.6);
	glVertex3f(180.0, 55, 1100.0);
	glVertex3f(200.0, 55, 1100.0);
	glVertex3f(200.0, 40, 1100.0);
	glVertex3f(180.0, 40, 1100.0);
	glEnd();
}

void jalanb() {
	//ALAS
	glBegin(GL_QUADS);
	glColor3f(0.2, 0.2, 0.2);
	glVertex3f(1500.0, 50, 1120.0);
	glVertex3f(-1500.0, 50, 1120.0);
	glVertex3f(-1500.0, 50, 1500.0);
	glVertex3f(1500.0, 50, 1500.0);
	glEnd();
	glBegin(GL_QUADS);
	glColor3f(0.2, 0.2, 0.2);
	glVertex3f(-1500.0, 50, 1120.0);
	glVertex3f(-1500.0, 50, 1500.0);
	glVertex3f(-1500.0, 40, 1500.0);
	glVertex3f(-1500.0, 40, 1120.0);
	glEnd();
	glBegin(GL_QUADS);
	glColor3f(0.2, 0.2, 0.2);
	glVertex3f(1500.0, 50, 1120.0);
	glVertex3f(1500.0, 50, 1500.0);
	glVertex3f(1500.0, 40, 1500.0);
	glVertex3f(1500.0, 40, 1120.0);
	glEnd();

	//depan
	glBegin(GL_QUADS);
	glColor3f(0.6, 0.6, 0.6);
	glVertex3f(-1500.0, 55, 1500.0);
	glVertex3f(1500.0, 55, 1500.0);
	glVertex3f(1500.0, 40, 1500.0);
	glVertex3f(-1500.0, 40, 1500.0);
	glEnd();

	glBegin(GL_QUADS);
	glColor3f(0.6, 0.6, 0.6);
	glVertex3f(-1500.0, 55, 1490.0);
	glVertex3f(1500.0, 55, 1490.0);
	glVertex3f(1500.0, 40, 1490.0);
	glVertex3f(-1500.0, 40, 1490.0);
	glEnd();

	glBegin(GL_QUADS);
	glColor3f(0.75, 0.75, 0.75);
	glVertex3f(1500.0, 55, 1500.0);
	glVertex3f(-1500.0, 55, 1500.0);
	glVertex3f(-1500.0, 55, 1490.0);
	glVertex3f(1500.0, 55, 1490.0);
	glEnd();

	glBegin(GL_QUADS);
	glColor3f(0.6, 0.6, 0.6);
	glVertex3f(-1500.0, 55, 1500.0);
	glVertex3f(-1500.0, 55, 1490.0);
	glVertex3f(-1500.0, 40, 1490.0);
	glVertex3f(-1500.0, 40, 1500.0);
	glEnd();

	glBegin(GL_QUADS);
	glColor3f(0.6, 0.6, 0.6);
	glVertex3f(1500.0, 55, 1500.0);
	glVertex3f(1500.0, 55, 1490.0);
	glVertex3f(1500.0, 40, 1490.0);
	glVertex3f(1500.0, 40, 1500.0);
	glEnd();

	//samping depan kiri
	glBegin(GL_QUADS);
	glColor3f(0.75, 0.75, 0.75);
	glVertex3f(-1500.0, 55, 1120.0);
	glVertex3f(-180.0, 55, 1120.0);
	glVertex3f(-180.0, 55, 1100.0);
	glVertex3f(-1500.0, 55, 1100.0);
	glEnd();

	glBegin(GL_QUADS);
	glColor3f(0.6, 0.6, 0.6);
	glVertex3f(-1500.0, 55, 1100.0);
	glVertex3f(-180.0, 55, 1100.0);
	glVertex3f(-180.0, 40, 1100.0);
	glVertex3f(-1500.0, 40, 1100.0);
	glEnd();
	glBegin(GL_QUADS);
	glColor3f(0.6, 0.6, 0.6);
	glVertex3f(-1500.0, 55, 1120.0);
	glVertex3f(-180.0, 55, 1120.0);
	glVertex3f(-180.0, 40, 1120.0);
	glVertex3f(-1500.0, 40, 1120.0);
	glEnd();
	glBegin(GL_QUADS);
	glColor3f(0.6, 0.6, 0.6);
	glVertex3f(-1500.0, 55, 1120.0);
	glVertex3f(-1500.0, 55, 1100.0);
	glVertex3f(-1500.0, 40, 1100.0);
	glVertex3f(-1500.0, 40, 1120.0);
	glEnd();

	//samping depan kanan
	glBegin(GL_QUADS);
	glColor3f(0.75, 0.75, 0.75);
	glVertex3f(1500.0, 55, 1120.0);
	glVertex3f(180.0, 55, 1120.0);
	glVertex3f(180.0, 55, 1100.0);
	glVertex3f(1500.0, 55, 1100.0);
	glEnd();

	glBegin(GL_QUADS);
	glColor3f(0.6, 0.6, 0.6);
	glVertex3f(1500.0, 55, 1100.0);
	glVertex3f(180.0, 55, 1100.0);
	glVertex3f(180.0, 40, 1100.0);
	glVertex3f(1500.0, 40, 1100.0);
	glEnd();
	glBegin(GL_QUADS);
	glColor3f(0.6, 0.6, 0.6);
	glVertex3f(1500.0, 55, 1120.0);
	glVertex3f(180.0, 55, 1120.0);
	glVertex3f(180.0, 40, 1120.0);
	glVertex3f(1500.0, 40, 1120.0);
	glEnd();
	glBegin(GL_QUADS);
	glColor3f(0.6, 0.6, 0.6);
	glVertex3f(1500.0, 55, 1120.0);
	glVertex3f(1500.0, 55, 1100.0);
	glVertex3f(1500.0, 40, 1100.0);
	glVertex3f(1500.0, 40, 1120.0);
	glEnd();
}


void lantai1() {
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.43, 0.43, 0.43);
	glVertex3f(-610, 110, -610);
	glVertex3f(610, 110, -610);
	glVertex3f(610, 110, 610);
	glVertex3f(-610, 110, 610);
	glEnd();

	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.36, 0.36, 0.35);
	glVertex3f(-520, 111, -520);
	glVertex3f(520, 111, -520);
	glVertex3f(520, 111, 520);
	glVertex3f(-520, 111, 520);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.69, 0.69, 0.69);
	glVertex3f(610, 40, -610);
	glVertex3f(610, 110, -610);
	glVertex3f(610, 110, 610);
	glVertex3f(610, 40, 610);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.69, 0.69, 0.69);
	glVertex3f(-610, 40, -610);
	glVertex3f(-610, 110, -610);
	glVertex3f(-610, 110, 610);
	glVertex3f(-610, 40, 610);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.69, 0.69, 0.69);
	glVertex3f(-610, 40, -610);
	glVertex3f(-610, 110, -610);
	glVertex3f(610, 110, -610);
	glVertex3f(610, 40, -610);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.69, 0.69, 0.69);
	glVertex3f(-610, 40, 610);
	glVertex3f(-610, 110, 610);
	glVertex3f(610, 110, 610);
	glVertex3f(610, 40, 610);
	glEnd();
}

void tangga() {
	//Tangga1
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.43, 0.43, 0.43);
	glVertex3f(-200, 95, 600);
	glVertex3f(200, 95, 600);
	glVertex3f(200, 95, 625);
	glVertex3f(-200, 95, 625);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.69, 0.69, 0.69);
	glVertex3f(-200, 40, 625);
	glVertex3f(-200, 95, 625);
	glVertex3f(200, 95, 625);
	glVertex3f(200, 40, 625);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.69, 0.69, 0.69);
	glVertex3f(200, 40, 600);
	glVertex3f(200, 95, 600);
	glVertex3f(200, 95, 625);
	glVertex3f(200, 40, 625);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.69, 0.69, 0.69);
	glVertex3f(-200, 40, 600);
	glVertex3f(-200, 95, 600);
	glVertex3f(-200, 95, 625);
	glVertex3f(-200, 40, 625);
	glEnd();

	//Tangga2
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.43, 0.43, 0.43);
	glVertex3f(-200, 80, 625);
	glVertex3f(200, 80, 625);
	glVertex3f(200, 80, 650);
	glVertex3f(-200, 80, 650);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.69, 0.69, 0.69);
	glVertex3f(-200, 40, 650);
	glVertex3f(-200, 80, 650);
	glVertex3f(200, 80, 650);
	glVertex3f(200, 40, 650);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.69, 0.69, 0.69);
	glVertex3f(200, 40, 625);
	glVertex3f(200, 80, 625);
	glVertex3f(200, 80, 650);
	glVertex3f(200, 40, 650);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.69, 0.69, 0.69);
	glVertex3f(-200, 40, 625);
	glVertex3f(-200, 80, 625);
	glVertex3f(-200, 80, 650);
	glVertex3f(-200, 40, 650);
	glEnd();

	//Tangga3
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.43, 0.43, 0.43);
	glVertex3f(-200, 65, 650);
	glVertex3f(200, 65, 650);
	glVertex3f(200, 65, 675);
	glVertex3f(-200, 65, 675);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.69, 0.69, 0.69);
	glVertex3f(-200, 40, 675);
	glVertex3f(-200, 65, 675);
	glVertex3f(200, 65, 675);
	glVertex3f(200, 40, 675);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.69, 0.69, 0.69);
	glVertex3f(200, 40, 650);
	glVertex3f(200, 65, 650);
	glVertex3f(200, 65, 675);
	glVertex3f(200, 40, 675);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.69, 0.69, 0.69);
	glVertex3f(-200, 40, 650);
	glVertex3f(-200, 65, 650);
	glVertex3f(-200, 65, 675);
	glVertex3f(-200, 40, 675);
	glEnd();

	//Tangga4
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.43, 0.43, 0.43);
	glVertex3f(-200, 50, 675);
	glVertex3f(200, 50, 675);
	glVertex3f(200, 50, 700);
	glVertex3f(-200, 50, 700);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.69, 0.69, 0.69);
	glVertex3f(-200, 40, 700);
	glVertex3f(-200, 50, 700);
	glVertex3f(200, 50, 700);
	glVertex3f(200, 40, 700);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.69, 0.69, 0.69);
	glVertex3f(200, 40, 675);
	glVertex3f(200, 50, 675);
	glVertex3f(200, 50, 700);
	glVertex3f(200, 40, 700);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.69, 0.69, 0.69);
	glVertex3f(-200, 40, 675);
	glVertex3f(-200, 50, 675);
	glVertex3f(-200, 50, 700);
	glVertex3f(-200, 40, 700);
	glEnd();
}

void pagar_lantai1() {

	//kiribawah1
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(610, 150, 600);
	glVertex3f(590, 150, 600);
	glVertex3f(590, 110, 600);
	glVertex3f(610, 110, 600);

	//kiribawah2
	glBegin(GL_QUADS);
	glColor3f(0.43, 0.43, 0.8);
	glVertex3f(600, 150, -600);
	glVertex3f(580, 150, -600);
	glVertex3f(580, 110, -600);
	glVertex3f(600, 110, -600);
	glEnd();
	//kiribawah3
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(600, 150, -600);
	glVertex3f(600, 150, 600);
	glVertex3f(600, 110, 600);
	glVertex3f(600, 110, -600);
	//kiribawah4
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(580, 150, -600);
	glVertex3f(580, 150, 600);
	glVertex3f(580, 110, 600);
	glVertex3f(580, 110, -600);
	//kiribawah5
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(600, 150, -600);
	glVertex3f(600, 150, 600);
	glVertex3f(580, 150, 600);
	glVertex3f(580, 150, -600);

	//kiribawah6
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(600, 210, 600);
	glVertex3f(580, 210, 600);
	glVertex3f(580, 190, 600);
	glVertex3f(600, 190, 600);

	//kiribawah7
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(600, 210, -600);
	glVertex3f(580, 210, -600);
	glVertex3f(580, 190, -600);
	glVertex3f(600, 190, -600);
	glEnd();
	//kiribawah8
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(600, 210, -600);
	glVertex3f(600, 210, 600);
	glVertex3f(600, 190, 600);
	glVertex3f(600, 190, -600);
	//kiribawah9
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(580, 210, -600);
	glVertex3f(580, 210, 600);
	glVertex3f(580, 190, 600);
	glVertex3f(580, 190, -600);
	//kiribawah10
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(600, 210, -600);
	glVertex3f(600, 210, 600);
	glVertex3f(580, 210, 600);
	glVertex3f(580, 210, -600);
	glEnd();
	//kiribawah11
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(600, 190, -600);
	glVertex3f(600, 190, 600);
	glVertex3f(580, 190, 600);
	glVertex3f(580, 190, -600);
	glEnd();



	//kananbawah1
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-600, 150, 600);
	glVertex3f(-580, 150, 600);
	glVertex3f(-580, 110, 600);
	glVertex3f(-600, 110, 600);
	//kananbawah2
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-600, 150, -600);
	glVertex3f(-580, 150, -600);
	glVertex3f(-580, 110, -600);
	glVertex3f(-600, 110, -600);
	glEnd();
	//kananbawah3
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-600, 150, -600);
	glVertex3f(-600, 150, 600);
	glVertex3f(-600, 110, 600);
	glVertex3f(-600, 110, -600);
	//kananbawah4
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-580, 150, -600);
	glVertex3f(-580, 150, 600);
	glVertex3f(-580, 110, 600);
	glVertex3f(-580, 110, -600);
	//kananbawah5
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-600, 150, -600);
	glVertex3f(-600, 150, 600);
	glVertex3f(-580, 150, 600);
	glVertex3f(-580, 150, -600);

	//kananbawah6
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-600, 210, 600);
	glVertex3f(-580, 210, 600);
	glVertex3f(-580, 190, 600);
	glVertex3f(-600, 190, 600);

	//kananbawah7
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-600, 210, -600);
	glVertex3f(-580, 210, -600);
	glVertex3f(-580, 190, -600);
	glVertex3f(-600, 190, -600);
	glEnd();

	//kananbawah8
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-600, 210, -600);
	glVertex3f(-600, 210, 600);
	glVertex3f(-600, 190, 600);
	glVertex3f(-600, 190, -600);
	//kananbawah9
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-580, 210, -600);
	glVertex3f(-580, 210, 600);
	glVertex3f(-580, 190, 600);
	glVertex3f(-580, 190, -600);
	//kananbawah10
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-600, 210, -600);
	glVertex3f(-600, 210, 600);
	glVertex3f(-580, 210, 600);
	glVertex3f(-580, 210, -600);
	glEnd();
	//kananbawah11
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-600, 190, -600);
	glVertex3f(-600, 190, 600);
	glVertex3f(-580, 190, 600);
	glVertex3f(-580, 190, -600);
	glEnd();

	//belakangbawah1
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-600, 150, -600);
	glVertex3f(-580, 150, -600);
	glVertex3f(-580, 110, -580);
	glVertex3f(-600, 110, -580);
	glEnd();

	//belakangbawah2
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(600, 150, -600);
	glVertex3f(580, 150, -600);
	glVertex3f(580, 110, -580);
	glVertex3f(600, 110, -580);
	glEnd();

	//belakangbawah3
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(600, 150, -600);
	glVertex3f(600, 150, -580);
	glVertex3f(-580, 150, -580);
	glVertex3f(-600, 150, -600);
	glEnd();

	//belakangbawah4
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(600, 150, -600);
	glVertex3f(-600, 150, -600);
	glVertex3f(-600, 110, -600);
	glVertex3f(600, 110, -600);
	glEnd();

	//belakangbawah5
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(600, 150, -580);
	glVertex3f(-600, 150, -580);
	glVertex3f(-600, 110, -580);
	glVertex3f(600, 110, -580);
	glEnd();

	//belakangbawah6
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-600, 210, -600);
	glVertex3f(-580, 210, -600);
	glVertex3f(-580, 190, -580);
	glVertex3f(-600, 190, -580);
	glEnd();

	//belakangbawah7
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(600, 210, -600);
	glVertex3f(580, 210, -600);
	glVertex3f(580, 190, -580);
	glVertex3f(600, 190, -580);
	glEnd();

	//belakangbawah8
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(600, 210, -600);
	glVertex3f(600, 210, -580);
	glVertex3f(-580, 210, -580);
	glVertex3f(-600, 210, -600);
	glEnd();

	//belakangbawah9
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(600, 210, -600);
	glVertex3f(-600, 210, -600);
	glVertex3f(-600, 190, -600);
	glVertex3f(600, 190, -600);
	glEnd();

	//belakangbawah10
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(600, 210, -580);
	glVertex3f(-600, 210, -580);
	glVertex3f(-600, 190, -580);
	glVertex3f(600, 190, -580);
	glEnd();

	//belakangbawah11
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(600, 190, -600);
	glVertex3f(600, 190, -580);
	glVertex3f(-580, 190, -580);
	glVertex3f(-600, 190, -600);
	glEnd();

	//depankananbawah1
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(600, 150, 600);
	glVertex3f(225, 150, 600);
	glVertex3f(225, 110, 600);
	glVertex3f(600, 110, 600);
	glEnd();

	//depankananbawah2
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(600, 150, 600);
	glVertex3f(580, 150, 600);
	glVertex3f(580, 110, 580);
	glVertex3f(600, 110, 580);
	glEnd();

	//depankananbawah3
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(600, 150, 600);
	glVertex3f(225, 150, 600);
	glVertex3f(225, 150, 580);
	glVertex3f(600, 150, 580);
	glEnd();

	//depankananbawah4
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(225, 150, 600);
	glVertex3f(225, 150, 580);
	glVertex3f(225, 110, 580);
	glVertex3f(225, 110, 600);
	glEnd();

	//depankananbawah5
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(600, 150, 580);
	glVertex3f(225, 150, 580);
	glVertex3f(225, 110, 580);
	glVertex3f(600, 110, 580);
	glEnd();

	//depankananbawah6
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(600, 210, 600);
	glVertex3f(225, 210, 600);
	glVertex3f(225, 190, 600);
	glVertex3f(600, 190, 600);
	glEnd();

	//depankananbawah7
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(600, 210, 600);
	glVertex3f(580, 210, 600);
	glVertex3f(580, 190, 580);
	glVertex3f(600, 190, 580);
	glEnd();

	//depankananbawah8
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(600, 210, 600);
	glVertex3f(225, 210, 600);
	glVertex3f(225, 210, 580);
	glVertex3f(600, 210, 580);
	glEnd();

	//depankananbawah9
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(225, 210, 600);
	glVertex3f(225, 210, 580);
	glVertex3f(225, 190, 580);
	glVertex3f(225, 190, 600);
	glEnd();

	//depankananbawah10
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(600, 210, 580);
	glVertex3f(225, 210, 580);
	glVertex3f(225, 190, 580);
	glVertex3f(600, 190, 580);
	glEnd();

	//depankananbawah11
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(600, 190, 600);
	glVertex3f(225, 190, 600);
	glVertex3f(225, 190, 580);
	glVertex3f(600, 190, 580);
	glEnd();

	//depankananbawah1
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-600, 150, 600);
	glVertex3f(-225, 150, 600);
	glVertex3f(-225, 110, 600);
	glVertex3f(-600, 110, 600);
	glEnd();

	//depankananbawah2
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-600, 150, 600);
	glVertex3f(-580, 150, 600);
	glVertex3f(-580, 110, 580);
	glVertex3f(-600, 110, 580);
	glEnd();

	//depankananbawah3
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-600, 150, 600);
	glVertex3f(-225, 150, 600);
	glVertex3f(-225, 150, 580);
	glVertex3f(-600, 150, 580);
	glEnd();

	//depankananbawah4
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-225, 150, 600);
	glVertex3f(-225, 150, 580);
	glVertex3f(-225, 110, 580);
	glVertex3f(-225, 110, 600);
	glEnd();

	//depankananbawah5
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-600, 150, 580);
	glVertex3f(-225, 150, 580);
	glVertex3f(-225, 110, 580);
	glVertex3f(-600, 110, 580);
	glEnd();

	//depankananbawah6
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-600, 210, 600);
	glVertex3f(-225, 210, 600);
	glVertex3f(-225, 190, 600);
	glVertex3f(-600, 190, 600);
	glEnd();

	//depankananbawah7
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-600, 210, 600);
	glVertex3f(-580, 210, 600);
	glVertex3f(-580, 190, 580);
	glVertex3f(-600, 190, 580);
	glEnd();

	//depankananbawah8
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-600, 210, 600);
	glVertex3f(-225, 210, 600);
	glVertex3f(-225, 210, 580);
	glVertex3f(-600, 210, 580);
	glEnd();

	//depankananbawah9
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-225, 210, 600);
	glVertex3f(-225, 210, 580);
	glVertex3f(-225, 190, 580);
	glVertex3f(-225, 190, 600);
	glEnd();

	//depankananbawah10
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-600, 210, 580);
	glVertex3f(-225, 210, 580);
	glVertex3f(-225, 190, 580);
	glVertex3f(-600, 190, 580);
	glEnd();

	//depankananbawah11
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-600, 190, 600);
	glVertex3f(-225, 190, 600);
	glVertex3f(-225, 190, 580);
	glVertex3f(-600, 190, 580);
	glEnd();

	//pojok1
	 //belakang
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-610, 110, 575);
	glVertex3f(-610, 220, 575);
	glVertex3f(-575, 220, 575);
	glVertex3f(-575, 110, 575);
	glEnd();

	//kanan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-575, 110, 575);
	glVertex3f(-575, 220, 575);
	glVertex3f(-575, 220, 610);
	glVertex3f(-575, 110, 610);
	glEnd();

	//kiri
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-610, 110, 575);
	glVertex3f(-610, 220, 575);
	glVertex3f(-610, 220, 610);
	glVertex3f(-610, 110, 610);
	glEnd();

	//depan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-575, 110, 610);
	glVertex3f(-575, 220, 610);
	glVertex3f(-610, 220, 610);
	glVertex3f(-610, 110, 610);
	glEnd();

	//atas
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-610, 220, 575);
	glVertex3f(-610, 220, 610);
	glVertex3f(-575, 220, 610);
	glVertex3f(-575, 220, 575);
	glEnd();

	//tengah
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-550, 110, 595);
	glVertex3f(-550, 190, 595);
	glVertex3f(-540, 190, 595);
	glVertex3f(-540, 110, 595);

	glVertex3f(-505, 110, 595);
	glVertex3f(-505, 190, 595);
	glVertex3f(-495, 190, 595);
	glVertex3f(-495, 110, 595);

	glVertex3f(-460, 110, 595);
	glVertex3f(-460, 190, 595);
	glVertex3f(-450, 190, 595);
	glVertex3f(-450, 110, 595);

	glVertex3f(-415, 110, 595);
	glVertex3f(-415, 190, 595);
	glVertex3f(-405, 190, 595);
	glVertex3f(-405, 110, 595);

	glVertex3f(-370, 110, 595);
	glVertex3f(-370, 190, 595);
	glVertex3f(-360, 190, 595);
	glVertex3f(-360, 110, 595);

	glVertex3f(-325, 110, 595);
	glVertex3f(-325, 190, 595);
	glVertex3f(-315, 190, 595);
	glVertex3f(-315, 110, 595);

	glVertex3f(-280, 110, 595);
	glVertex3f(-280, 190, 595);
	glVertex3f(-270, 190, 595);
	glVertex3f(-270, 110, 595);
	glEnd();

	//pojok2
	//belakang
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-240, 110, 575);
	glVertex3f(-240, 220, 575);
	glVertex3f(-215, 220, 575);
	glVertex3f(-215, 110, 575);
	glEnd();

	//kanan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-215, 110, 575);
	glVertex3f(-215, 220, 575);
	glVertex3f(-215, 220, 610);
	glVertex3f(-215, 110, 610);
	glEnd();

	//kiri
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-240, 110, 575);
	glVertex3f(-240, 220, 575);
	glVertex3f(-240, 220, 610);
	glVertex3f(-240, 110, 610);
	glEnd();

	//depan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-215, 110, 610);
	glVertex3f(-215, 220, 610);
	glVertex3f(-240, 220, 610);
	glVertex3f(-240, 110, 610);
	glEnd();

	//atas
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-240, 220, 575);
	glVertex3f(-240, 220, 610);
	glVertex3f(-215, 220, 610);
	glVertex3f(-215, 220, 575);
	glEnd();

	//pojok3
	//belakang
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(240, 110, 575);
	glVertex3f(240, 220, 575);
	glVertex3f(215, 220, 575);
	glVertex3f(215, 110, 575);
	glEnd();

	//kanan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(215, 110, 575);
	glVertex3f(215, 220, 575);
	glVertex3f(215, 220, 610);
	glVertex3f(215, 110, 610);
	glEnd();

	//kiri
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(240, 110, 575);
	glVertex3f(240, 220, 575);
	glVertex3f(240, 220, 610);
	glVertex3f(240, 110, 610);
	glEnd();

	//depan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(215, 110, 610);
	glVertex3f(215, 220, 610);
	glVertex3f(240, 220, 610);
	glVertex3f(240, 110, 610);
	glEnd();

	//atas
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(240, 220, 575);
	glVertex3f(240, 220, 610);
	glVertex3f(215, 220, 610);
	glVertex3f(215, 220, 575);
	glEnd();

	//tengah
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(280, 110, 595);
	glVertex3f(280, 190, 595);
	glVertex3f(270, 190, 595);
	glVertex3f(270, 110, 595);

	glVertex3f(325, 110, 595);
	glVertex3f(325, 190, 595);
	glVertex3f(315, 190, 595);
	glVertex3f(315, 110, 595);

	glVertex3f(370, 110, 595);
	glVertex3f(370, 190, 595);
	glVertex3f(360, 190, 595);
	glVertex3f(360, 110, 595);

	glVertex3f(415, 110, 595);
	glVertex3f(415, 190, 595);
	glVertex3f(405, 190, 595);
	glVertex3f(405, 110, 595);

	glVertex3f(460, 110, 595);
	glVertex3f(460, 190, 595);
	glVertex3f(450, 190, 595);
	glVertex3f(450, 110, 595);

	glVertex3f(505, 110, 595);
	glVertex3f(505, 190, 595);
	glVertex3f(495, 190, 595);
	glVertex3f(495, 110, 595);

	glVertex3f(550, 110, 595);
	glVertex3f(550, 190, 595);
	glVertex3f(540, 190, 595);
	glVertex3f(540, 110, 595);
	glEnd();

	//pojok4
	//belakang
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(610, 110, 575);
	glVertex3f(610, 220, 575);
	glVertex3f(575, 220, 575);
	glVertex3f(575, 110, 575);
	glEnd();

	//kanan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(575, 110, 575);
	glVertex3f(575, 220, 575);
	glVertex3f(575, 220, 610);
	glVertex3f(575, 110, 610);
	glEnd();

	//depan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(575, 110, 610);
	glVertex3f(575, 220, 610);
	glVertex3f(610, 220, 610);
	glVertex3f(610, 110, 610);
	glEnd();

	//atas
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(610, 220, 575);
	glVertex3f(610, 220, 610);
	glVertex3f(575, 220, 610);
	glVertex3f(575, 220, 575);
	glEnd();

	//kiri
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(610, 110, 575);
	glVertex3f(610, 220, 575);
	glVertex3f(610, 220, 610);
	glVertex3f(610, 110, 610);
	glEnd();

	//tengah
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(595, 110, 545);
	glVertex3f(595, 190, 545);
	glVertex3f(595, 190, 555);
	glVertex3f(595, 110, 555);

	glVertex3f(595, 110, 500);
	glVertex3f(595, 190, 500);
	glVertex3f(595, 190, 510);
	glVertex3f(595, 110, 510);

	glVertex3f(595, 110, 455);
	glVertex3f(595, 190, 455);
	glVertex3f(595, 190, 465);
	glVertex3f(595, 110, 465);

	glVertex3f(595, 110, 410);
	glVertex3f(595, 190, 410);
	glVertex3f(595, 190, 420);
	glVertex3f(595, 110, 420);

	glVertex3f(595, 110, 365);
	glVertex3f(595, 190, 365);
	glVertex3f(595, 190, 375);
	glVertex3f(595, 110, 375);

	glVertex3f(595, 110, 320);
	glVertex3f(595, 190, 320);
	glVertex3f(595, 190, 330);
	glVertex3f(595, 110, 330);

	glVertex3f(595, 110, 275);
	glVertex3f(595, 190, 275);
	glVertex3f(595, 190, 285);
	glVertex3f(595, 110, 285);

	glVertex3f(595, 110, 230);
	glVertex3f(595, 190, 230);
	glVertex3f(595, 190, 240);
	glVertex3f(595, 110, 240);

	glVertex3f(595, 110, 185);
	glVertex3f(595, 190, 185);
	glVertex3f(595, 190, 195);
	glVertex3f(595, 110, 195);

	glVertex3f(595, 110, 140);
	glVertex3f(595, 190, 140);
	glVertex3f(595, 190, 150);
	glVertex3f(595, 110, 150);

	glVertex3f(595, 110, 95);
	glVertex3f(595, 190, 95);
	glVertex3f(595, 190, 105);
	glVertex3f(595, 110, 105);

	glVertex3f(595, 110, 50);
	glVertex3f(595, 190, 50);
	glVertex3f(595, 190, 60);
	glVertex3f(595, 110, 60);

	glVertex3f(595, 110, 5);
	glVertex3f(595, 190, 5);
	glVertex3f(595, 190, 15);
	glVertex3f(595, 110, 15);

	glVertex3f(595, 110, -30);
	glVertex3f(595, 190, -30);
	glVertex3f(595, 190, -40);
	glVertex3f(595, 110, -40);

	glVertex3f(595, 110, -75);
	glVertex3f(595, 190, -75);
	glVertex3f(595, 190, -85);
	glVertex3f(595, 110, -85);

	glVertex3f(595, 110, -120);
	glVertex3f(595, 190, -120);
	glVertex3f(595, 190, -130);
	glVertex3f(595, 110, -130);

	glVertex3f(595, 110, -165);
	glVertex3f(595, 190, -165);
	glVertex3f(595, 190, -175);
	glVertex3f(595, 110, -175);

	glVertex3f(595, 110, -210);
	glVertex3f(595, 190, -210);
	glVertex3f(595, 190, -220);
	glVertex3f(595, 110, -220);

	glVertex3f(595, 110, -255);
	glVertex3f(595, 190, -255);
	glVertex3f(595, 190, -265);
	glVertex3f(595, 110, -265);

	glVertex3f(595, 110, -300);
	glVertex3f(595, 190, -300);
	glVertex3f(595, 190, -310);
	glVertex3f(595, 110, -310);

	glVertex3f(595, 110, -345);
	glVertex3f(595, 190, -345);
	glVertex3f(595, 190, -355);
	glVertex3f(595, 110, -355);

	glVertex3f(595, 110, -390);
	glVertex3f(595, 190, -390);
	glVertex3f(595, 190, -400);
	glVertex3f(595, 110, -400);

	glVertex3f(595, 110, -435);
	glVertex3f(595, 190, -435);
	glVertex3f(595, 190, -445);
	glVertex3f(595, 110, -445);

	glVertex3f(595, 110, -480);
	glVertex3f(595, 190, -480);
	glVertex3f(595, 190, -490);
	glVertex3f(595, 110, -490);

	glVertex3f(595, 110, -525);
	glVertex3f(595, 190, -525);
	glVertex3f(595, 190, -535);
	glVertex3f(595, 110, -535);
	glEnd();

	//tengah
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-595, 110, 545);
	glVertex3f(-595, 190, 545);
	glVertex3f(-595, 190, 555);
	glVertex3f(-595, 110, 555);

	glVertex3f(-595, 110, 500);
	glVertex3f(-595, 190, 500);
	glVertex3f(-595, 190, 510);
	glVertex3f(-595, 110, 510);

	glVertex3f(-595, 110, 455);
	glVertex3f(-595, 190, 455);
	glVertex3f(-595, 190, 465);
	glVertex3f(-595, 110, 465);

	glVertex3f(-595, 110, 410);
	glVertex3f(-595, 190, 410);
	glVertex3f(-595, 190, 420);
	glVertex3f(-595, 110, 420);

	glVertex3f(-595, 110, 365);
	glVertex3f(-595, 190, 365);
	glVertex3f(-595, 190, 375);
	glVertex3f(-595, 110, 375);

	glVertex3f(-595, 110, 320);
	glVertex3f(-595, 190, 320);
	glVertex3f(-595, 190, 330);
	glVertex3f(-595, 110, 330);

	glVertex3f(-595, 110, 275);
	glVertex3f(-595, 190, 275);
	glVertex3f(-595, 190, 285);
	glVertex3f(-595, 110, 285);

	glVertex3f(-595, 110, 230);
	glVertex3f(-595, 190, 230);
	glVertex3f(-595, 190, 240);
	glVertex3f(-595, 110, 240);

	glVertex3f(-595, 110, 185);
	glVertex3f(-595, 190, 185);
	glVertex3f(-595, 190, 195);
	glVertex3f(-595, 110, 195);

	glVertex3f(-595, 110, 140);
	glVertex3f(-595, 190, 140);
	glVertex3f(-595, 190, 150);
	glVertex3f(-595, 110, 150);

	glVertex3f(-595, 110, 95);
	glVertex3f(-595, 190, 95);
	glVertex3f(-595, 190, 105);
	glVertex3f(-595, 110, 105);

	glVertex3f(-595, 110, 50);
	glVertex3f(-595, 190, 50);
	glVertex3f(-595, 190, 60);
	glVertex3f(-595, 110, 60);

	glVertex3f(-595, 110, 5);
	glVertex3f(-595, 190, 5);
	glVertex3f(-595, 190, 15);
	glVertex3f(-595, 110, 15);

	glVertex3f(-595, 110, -30);
	glVertex3f(-595, 190, -30);
	glVertex3f(-595, 190, -40);
	glVertex3f(-595, 110, -40);

	glVertex3f(-595, 110, -75);
	glVertex3f(-595, 190, -75);
	glVertex3f(-595, 190, -85);
	glVertex3f(-595, 110, -85);

	glVertex3f(-595, 110, -120);
	glVertex3f(-595, 190, -120);
	glVertex3f(-595, 190, -130);
	glVertex3f(-595, 110, -130);

	glVertex3f(-595, 110, -165);
	glVertex3f(-595, 190, -165);
	glVertex3f(-595, 190, -175);
	glVertex3f(-595, 110, -175);

	glVertex3f(-595, 110, -210);
	glVertex3f(-595, 190, -210);
	glVertex3f(-595, 190, -220);
	glVertex3f(-595, 110, -220);

	glVertex3f(-595, 110, -255);
	glVertex3f(-595, 190, -255);
	glVertex3f(-595, 190, -265);
	glVertex3f(-595, 110, -265);

	glVertex3f(-595, 110, -300);
	glVertex3f(-595, 190, -300);
	glVertex3f(-595, 190, -310);
	glVertex3f(-595, 110, -310);

	glVertex3f(-595, 110, -345);
	glVertex3f(-595, 190, -345);
	glVertex3f(-595, 190, -355);
	glVertex3f(-595, 110, -355);

	glVertex3f(-595, 110, -390);
	glVertex3f(-595, 190, -390);
	glVertex3f(-595, 190, -400);
	glVertex3f(-595, 110, -400);

	glVertex3f(-595, 110, -435);
	glVertex3f(-595, 190, -435);
	glVertex3f(-595, 190, -445);
	glVertex3f(-595, 110, -445);

	glVertex3f(-595, 110, -480);
	glVertex3f(-595, 190, -480);
	glVertex3f(-595, 190, -490);
	glVertex3f(-595, 110, -490);

	glVertex3f(-595, 110, -525);
	glVertex3f(-595, 190, -525);
	glVertex3f(-595, 190, -535);
	glVertex3f(-595, 110, -535);
	glEnd();

	//pojok5
	//kanan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(575, 110, -575);
	glVertex3f(575, 220, -575);
	glVertex3f(575, 220, -610);
	glVertex3f(575, 110, -610);
	glEnd();

	//depan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(610, 110, -575);
	glVertex3f(610, 220, -575);
	glVertex3f(575, 220, -575);
	glVertex3f(575, 110, -575);
	glEnd();

	//atas
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(610, 220, -575);
	glVertex3f(610, 220, -610);
	glVertex3f(575, 220, -610);
	glVertex3f(575, 220, -575);
	glEnd();

	//kiri
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(610, 110, -575);
	glVertex3f(610, 220, -575);
	glVertex3f(610, 220, -610);
	glVertex3f(610, 110, -610);
	glEnd();

	//belakang
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(610, 110, -610);
	glVertex3f(610, 220, -610);
	glVertex3f(575, 220, -610);
	glVertex3f(575, 110, -610);
	glEnd();

	//tengah
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(540, 110, -595);
	glVertex3f(540, 190, -595);
	glVertex3f(550, 190, -595);
	glVertex3f(550, 110, -595);

	glVertex3f(495, 110, -595);
	glVertex3f(495, 190, -595);
	glVertex3f(505, 190, -595);
	glVertex3f(505, 110, -595);

	glVertex3f(450, 110, -595);
	glVertex3f(450, 190, -595);
	glVertex3f(460, 190, -595);
	glVertex3f(460, 110, -595);

	glVertex3f(405, 110, -595);
	glVertex3f(405, 190, -595);
	glVertex3f(415, 190, -595);
	glVertex3f(415, 110, -595);

	glVertex3f(360, 110, -595);
	glVertex3f(360, 190, -595);
	glVertex3f(370, 190, -595);
	glVertex3f(370, 110, -595);

	glVertex3f(315, 110, -595);
	glVertex3f(315, 190, -595);
	glVertex3f(325, 190, -595);
	glVertex3f(325, 110, -595);

	glVertex3f(270, 110, -595);
	glVertex3f(270, 190, -595);
	glVertex3f(280, 190, -595);
	glVertex3f(280, 110, -595);

	glVertex3f(225, 110, -595);
	glVertex3f(225, 190, -595);
	glVertex3f(235, 190, -595);
	glVertex3f(235, 110, -595);

	glVertex3f(180, 110, -595);
	glVertex3f(180, 190, -595);
	glVertex3f(190, 190, -595);
	glVertex3f(190, 110, -595);

	glVertex3f(135, 110, -595);
	glVertex3f(135, 190, -595);
	glVertex3f(145, 190, -595);
	glVertex3f(145, 110, -595);

	glVertex3f(90, 110, -595);
	glVertex3f(90, 190, -595);
	glVertex3f(100, 190, -595);
	glVertex3f(100, 110, -595);

	glVertex3f(45, 110, -595);
	glVertex3f(45, 190, -595);
	glVertex3f(55, 190, -595);
	glVertex3f(55, 110, -595);

	glVertex3f(0, 110, -595);
	glVertex3f(0, 190, -595);
	glVertex3f(10, 190, -595);
	glVertex3f(10, 110, -595);

	glVertex3f(-45, 110, -595);
	glVertex3f(-45, 190, -595);
	glVertex3f(-55, 190, -595);
	glVertex3f(-55, 110, -595);

	glVertex3f(-90, 110, -595);
	glVertex3f(-90, 190, -595);
	glVertex3f(-100, 190, -595);
	glVertex3f(-100, 110, -595);

	glVertex3f(-540, 110, -595);
	glVertex3f(-540, 190, -595);
	glVertex3f(-550, 190, -595);
	glVertex3f(-550, 110, -595);

	glVertex3f(-495, 110, -595);
	glVertex3f(-495, 190, -595);
	glVertex3f(-505, 190, -595);
	glVertex3f(-505, 110, -595);

	glVertex3f(-450, 110, -595);
	glVertex3f(-450, 190, -595);
	glVertex3f(-460, 190, -595);
	glVertex3f(-460, 110, -595);

	glVertex3f(-405, 110, -595);
	glVertex3f(-405, 190, -595);
	glVertex3f(-415, 190, -595);
	glVertex3f(-415, 110, -595);

	glVertex3f(-360, 110, -595);
	glVertex3f(-360, 190, -595);
	glVertex3f(-370, 190, -595);
	glVertex3f(-370, 110, -595);

	glVertex3f(-315, 110, -595);
	glVertex3f(-315, 190, -595);
	glVertex3f(-325, 190, -595);
	glVertex3f(-325, 110, -595);

	glVertex3f(-270, 110, -595);
	glVertex3f(-270, 190, -595);
	glVertex3f(-280, 190, -595);
	glVertex3f(-280, 110, -595);

	glVertex3f(-225, 110, -595);
	glVertex3f(-225, 190, -595);
	glVertex3f(-235, 190, -595);
	glVertex3f(-235, 110, -595);

	glVertex3f(-180, 110, -595);
	glVertex3f(-180, 190, -595);
	glVertex3f(-190, 190, -595);
	glVertex3f(-190, 110, -595);

	glVertex3f(-135, 110, -595);
	glVertex3f(-135, 190, -595);
	glVertex3f(-145, 190, -595);
	glVertex3f(-145, 110, -595);
	glEnd();
	//pojok6
	//kanan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-575, 110, -575);
	glVertex3f(-575, 220, -575);
	glVertex3f(-575, 220, -610);
	glVertex3f(-575, 110, -610);
	glEnd();

	//depan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-610, 110, -575);
	glVertex3f(-610, 220, -575);
	glVertex3f(-575, 220, -575);
	glVertex3f(-575, 110, -575);
	glEnd();

	//atas
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-610, 220, -575);
	glVertex3f(-610, 220, -610);
	glVertex3f(-575, 220, -610);
	glVertex3f(-575, 220, -575);
	glEnd();

	//kiri
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-610, 110, -575);
	glVertex3f(-610, 220, -575);
	glVertex3f(-610, 220, -610);
	glVertex3f(-610, 110, -610);
	glEnd();

	//belakang
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-610, 110, -610);
	glVertex3f(-610, 220, -610);
	glVertex3f(-575, 220, -610);
	glVertex3f(-575, 110, -610);
	glEnd();

}

void lampion_lantai1a() {
	//atas
	glBegin(GL_QUADS);
	glColor3f(0.84, 0.261, 0.25);
	glVertex3f(490, 505, 490);
	glVertex3f(490, 505, 470);
	glVertex3f(460, 505, 470);
	glVertex3f(460, 505, 490);
	glEnd();

	//bawah
	glBegin(GL_QUADS);
	glColor3f(0.84, 0.261, 0.25);
	glVertex3f(490, 410, 490);
	glVertex3f(490, 410, 470);
	glVertex3f(460, 410, 470);
	glVertex3f(460, 410, 490);
	glEnd();

	//badan
	glBegin(GL_QUADS);
	glColor3f(0.84, 0.261, 0.25);
	glVertex3f(490, 505, 490);
	glVertex3f(490, 410, 490);
	glVertex3f(460, 410, 490);
	glVertex3f(460, 505, 490);
	glEnd();
	glBegin(GL_QUADS);
	glColor3f(0.84, 0.261, 0.25);
	glVertex3f(490, 505, 470);
	glVertex3f(490, 410, 470);
	glVertex3f(460, 410, 470);
	glVertex3f(460, 505, 470);
	glEnd();
	glBegin(GL_QUADS);
	glColor3f(0.84, 0.261, 0.25);
	glVertex3f(490, 505, 490);
	glVertex3f(490, 410, 490);
	glVertex3f(490, 410, 470);
	glVertex3f(490, 505, 470);
	glEnd();
	glBegin(GL_QUADS);
	glColor3f(0.84, 0.261, 0.25);
	glVertex3f(460, 505, 490);
	glVertex3f(460, 410, 490);
	glVertex3f(460, 410, 470);
	glVertex3f(460, 505, 470);
	glEnd();
}
void lampion_lantai1b() {
	//atas
	glBegin(GL_QUADS);
	glColor3f(0.84, 0.261, 0.25);
	glVertex3f(-490, 505, 490);
	glVertex3f(-490, 505, 470);
	glVertex3f(-460, 505, 470);
	glVertex3f(-460, 505, 490);
	glEnd();

	//bawah
	glBegin(GL_QUADS);
	glColor3f(0.84, 0.261, 0.25);
	glVertex3f(-490, 410, 490);
	glVertex3f(-490, 410, 470);
	glVertex3f(-460, 410, 470);
	glVertex3f(-460, 410, 490);
	glEnd();

	//badan
	glBegin(GL_QUADS);
	glColor3f(0.84, 0.261, 0.25);
	glVertex3f(-490, 505, 490);
	glVertex3f(-490, 410, 490);
	glVertex3f(-460, 410, 490);
	glVertex3f(-460, 505, 490);
	glEnd();
	glBegin(GL_QUADS);
	glColor3f(0.84, 0.261, 0.25);
	glVertex3f(-490, 505, 470);
	glVertex3f(-490, 410, 470);
	glVertex3f(-460, 410, 470);
	glVertex3f(-460, 505, 470);
	glEnd();
	glBegin(GL_QUADS);
	glColor3f(0.84, 0.261, 0.25);
	glVertex3f(-490, 505, 490);
	glVertex3f(-490, 410, 490);
	glVertex3f(-490, 410, 470);
	glVertex3f(-490, 505, 470);
	glEnd();
	glBegin(GL_QUADS);
	glColor3f(0.84, 0.261, 0.25);
	glVertex3f(-460, 505, 490);
	glVertex3f(-460, 410, 490);
	glVertex3f(-460, 410, 470);
	glVertex3f(-460, 505, 470);
	glEnd();
}
void lampion_lantai2a() {
	//atas
	glBegin(GL_QUADS);
	glColor3f(0.84, 0.261, 0.25);
	glVertex3f(490, 810, 490);
	glVertex3f(490, 810, 470);
	glVertex3f(460, 810, 470);
	glVertex3f(460, 810, 490);
	glEnd();

	//bawah
	glBegin(GL_QUADS);
	glColor3f(0.84, 0.261, 0.25);
	glVertex3f(490, 720, 490);
	glVertex3f(490, 720, 470);
	glVertex3f(460, 720, 470);
	glVertex3f(460, 720, 490);
	glEnd();

	//badan
	glBegin(GL_QUADS);
	glColor3f(0.84, 0.261, 0.25);
	glVertex3f(490, 810, 490);
	glVertex3f(490, 720, 490);
	glVertex3f(460, 720, 490);
	glVertex3f(460, 810, 490);
	glEnd();
	glBegin(GL_QUADS);
	glColor3f(0.84, 0.261, 0.25);
	glVertex3f(490, 810, 470);
	glVertex3f(490, 720, 470);
	glVertex3f(460, 720, 470);
	glVertex3f(460, 810, 470);
	glEnd();
	glBegin(GL_QUADS);
	glColor3f(0.84, 0.261, 0.25);
	glVertex3f(490, 810, 490);
	glVertex3f(490, 720, 490);
	glVertex3f(490, 720, 470);
	glVertex3f(490, 810, 470);
	glEnd();
	glBegin(GL_QUADS);
	glColor3f(0.84, 0.261, 0.25);
	glVertex3f(460, 810, 490);
	glVertex3f(460, 720, 490);
	glVertex3f(460, 720, 470);
	glVertex3f(460, 810, 470);
	glEnd();
}
void lampion_lantai2b() {
	//atas
	glBegin(GL_QUADS);
	glColor3f(0.84, 0.261, 0.25);
	glVertex3f(-490, 810, 490);
	glVertex3f(-490, 810, 470);
	glVertex3f(-460, 810, 470);
	glVertex3f(-460, 810, 490);
	glEnd();

	//bawah
	glBegin(GL_QUADS);
	glColor3f(0.84, 0.261, 0.25);
	glVertex3f(-490, 720, 490);
	glVertex3f(-490, 720, 470);
	glVertex3f(-460, 720, 470);
	glVertex3f(-460, 720, 490);
	glEnd();

	//badan
	glBegin(GL_QUADS);
	glColor3f(0.84, 0.261, 0.25);
	glVertex3f(-490, 810, 490);
	glVertex3f(-490, 720, 490);
	glVertex3f(-460, 720, 490);
	glVertex3f(-460, 810, 490);
	glEnd();
	glBegin(GL_QUADS);
	glColor3f(0.84, 0.261, 0.25);
	glVertex3f(-490, 810, 470);
	glVertex3f(-490, 720, 470);
	glVertex3f(-460, 720, 470);
	glVertex3f(-460, 810, 470);
	glEnd();
	glBegin(GL_QUADS);
	glColor3f(0.84, 0.261, 0.25);
	glVertex3f(-490, 810, 490);
	glVertex3f(-490, 720, 490);
	glVertex3f(-490, 720, 470);
	glVertex3f(-490, 810, 470);
	glEnd();
	glBegin(GL_QUADS);
	glColor3f(0.84, 0.261, 0.25);
	glVertex3f(-460, 810, 490);
	glVertex3f(-460, 720, 490);
	glVertex3f(-460, 720, 470);
	glVertex3f(-460, 810, 470);
	glEnd();
}

void dinding_lantai1() {
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.65, 0.60, 0.581);
	glVertex3f(-400, 460, -400);
	glVertex3f(400, 460, -400);
	glVertex3f(400, 460, 400);
	glVertex3f(-400, 460, 400);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.65, 0.60, 0.581);
	glVertex3f(400, 110, -400);
	glVertex3f(400, 460, -400);
	glVertex3f(400, 460, 400);
	glVertex3f(400, 110, 400);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.65, 0.60, 0.581);
	glVertex3f(-400, 110, -400);
	glVertex3f(-400, 460, -400);
	glVertex3f(-400, 460, 400);
	glVertex3f(-400, 110, 400);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.65, 0.60, 0.581);
	glVertex3f(-400, 110, -400);
	glVertex3f(-400, 460, -400);
	glVertex3f(400, 460, -400);
	glVertex3f(400, 110, -400);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.65, 0.60, 0.581);
	glVertex3f(-400, 110, 400);
	glVertex3f(-400, 460, 400);
	glVertex3f(400, 460, 400);
	glVertex3f(400, 110, 400);
	glEnd();
}

void atap_lantai1() {
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.43, 0.43, 0.43);
	glVertex3f(-525, 510, -525);
	glVertex3f(525, 510, -525);
	glVertex3f(525, 510, 525);
	glVertex3f(-525, 510, 525);
	glEnd();
	//Atas2
	glBegin(GL_QUADS);
	glColor3f(0.3, 0.3, 0.3);
	glVertex3f(-450, 512, -450);
	glVertex3f(450, 512, -450);
	glVertex3f(450, 512, 450);
	glVertex3f(-450, 512, 450);
	glEnd();
	//Bawah
	glBegin(GL_QUADS);
	glColor3f(0.52, 0.05, 0.05);
	glVertex3f(-400, 460, -400);
	glVertex3f(400, 460, -400);
	glVertex3f(400, 460, 400);
	glVertex3f(-400, 460, 400);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.52, 0.05, 0.05);
	glVertex3f(400, 460, -400);
	glVertex3f(525, 510, -525);
	glVertex3f(525, 510, 525);
	glVertex3f(400, 460, 400);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.52, 0.05, 0.05);
	glVertex3f(-400, 460, -400);
	glVertex3f(-525, 510, -525);
	glVertex3f(-525, 510, 525);
	glVertex3f(-400, 460, 400);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.52, 0.05, 0.05);
	glVertex3f(-400, 460, -400);
	glVertex3f(-525, 510, -525);
	glVertex3f(525, 510, -525);
	glVertex3f(400, 460, -400);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.52, 0.05, 0.05);
	glVertex3f(-400, 460, 400);
	glVertex3f(-525, 510, 525);
	glVertex3f(525, 510, 525);
	glVertex3f(400, 460, 400);
	glEnd();
}

void dinding_lantai2() {
	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.65, 0.60, 0.581);
	glVertex3f(300, 510, -300);
	glVertex3f(300, 810, -300);
	glVertex3f(300, 810, 300);
	glVertex3f(300, 510, 300);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.65, 0.60, 0.581);
	glVertex3f(-300, 510, -300);
	glVertex3f(-300, 810, -300);
	glVertex3f(-300, 810, 300);
	glVertex3f(-300, 510, 300);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.65, 0.60, 0.581);
	glVertex3f(-300, 510, -300);
	glVertex3f(-300, 810, -300);
	glVertex3f(300, 810, -300);
	glVertex3f(300, 510, -300);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.65, 0.60, 0.581);
	glVertex3f(-300, 510, 300);
	glVertex3f(-300, 810, 300);
	glVertex3f(300, 810, 300);
	glVertex3f(300, 510, 300);
	glEnd();
}

void atap_lantai2() {
	//Atap1
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.43, 0.43, 0.43);
	glVertex3f(-250, 860, -250);
	glVertex3f(250, 860, -250);
	glVertex3f(250, 860, 250);
	glVertex3f(-250, 860, 250);
	glEnd();

	//Bawah
	glBegin(GL_QUADS);
	glColor3f(0.35, 0.35, 0.35);
	glVertex3f(-525, 810, -525);
	glVertex3f(525, 810, -525);
	glVertex3f(525, 810, 525);
	glVertex3f(-525, 810, 525);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.46, 0.46, 0.46);
	glVertex3f(525, 810, -525);
	glVertex3f(250, 860, -250);
	glVertex3f(250, 860, 250);
	glVertex3f(525, 810, 525);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.46, 0.46, 0.46);
	glVertex3f(-525, 810, -525);
	glVertex3f(-250, 860, -250);
	glVertex3f(-250, 860, 250);
	glVertex3f(-525, 810, 525);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.43, 0.43, 0.43);
	glVertex3f(-525, 810, -525);
	glVertex3f(-250, 860, -250);
	glVertex3f(250, 860, -250);
	glVertex3f(525, 810, -525);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.43, 0.43, 0.43);
	glVertex3f(-525, 810, 525);
	glVertex3f(-250, 860, 250);
	glVertex3f(250, 860, 250);
	glVertex3f(525, 810, 525);
	glEnd();

	//Atap2
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.43, 0.43, 0.43);
	glVertex3f(-425, 910, -425);
	glVertex3f(425, 910, -425);
	glVertex3f(425, 910, 425);
	glVertex3f(-425, 910, 425);
	glEnd();
	//Atas2
	glBegin(GL_QUADS);
	glColor3f(0.3, 0.3, 0.3);
	glVertex3f(-350, 912, -350);
	glVertex3f(350, 912, -350);
	glVertex3f(350, 912, 350);
	glVertex3f(-350, 912, 350);
	glEnd();
	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(250, 860, -250);
	glVertex3f(425, 910, -425);
	glVertex3f(425, 910, 425);
	glVertex3f(250, 860, 250);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-250, 860, -250);
	glVertex3f(-425, 910, -425);
	glVertex3f(-425, 910, 425);
	glVertex3f(-250, 860, 250);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-250, 860, -250);
	glVertex3f(-425, 910, -425);
	glVertex3f(425, 910, -425);
	glVertex3f(250, 860, -250);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-250, 860, 250);
	glVertex3f(-425, 910, 425);
	glVertex3f(425, 910, 425);
	glVertex3f(250, 860, 250);
	glEnd();
}

void pagar_lantai2() {

	//tengah depan
	for (int x = 0; x <= 21; x++)
	{
		glBegin(GL_QUADS);
		glColor3f(0.54, 0.06, 0.05);
		glVertex3f(-480 + x * 45, 510, 520);
		glVertex3f(-480 + x * 45, 590, 520);
		glVertex3f(-470 + x * 45, 590, 520);
		glVertex3f(-470 + x * 45, 510, 520);
	}

	//tengah kanan
	for (int x = 0; x <= 21; x++)
	{
		glBegin(GL_QUADS);
		glColor3f(0.54, 0.06, 0.05);
		glVertex3f(520, 510, 470 - x * 45);
		glVertex3f(520, 590, 470 - x * 45);
		glVertex3f(520, 590, 480 - x * 45);
		glVertex3f(520, 510, 480 - x * 45);
	}

	//tengah belakang
	for (int x = 0; x <= 21; x++)
	{
		glBegin(GL_QUADS);
		glColor3f(0.54, 0.06, 0.05);
		glVertex3f(470 - x * 45, 510, -520);
		glVertex3f(470 - x * 45, 590, -520);
		glVertex3f(480 - x * 45, 590, -520);
		glVertex3f(480 - x * 45, 510, -520);
	}

	//tengah kiri
	for (int x = 0; x <= 21; x++)
	{
		glBegin(GL_QUADS);
		glColor3f(0.54, 0.06, 0.05);
		glVertex3f(-520, 510, 470 - x * 45);
		glVertex3f(-520, 590, 470 - x * 45);
		glVertex3f(-520, 590, 480 - x * 45);
		glVertex3f(-520, 510, 480 - x * 45);
	}

	//kiribawah1
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 510, 525);
	glVertex3f(-495, 510, 525);
	glVertex3f(-495, 510, -525);
	glVertex3f(-525, 510, -525);
	glEnd();
	//kiribawah2
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 550, 525);
	glVertex3f(-495, 550, 525);
	glVertex3f(-495, 510, 525);
	glVertex3f(-525, 510, 525);
	glEnd();
	//kiribawah3
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-495, 550, 525);
	glVertex3f(-495, 550, -525);
	glVertex3f(-495, 510, -525);
	glVertex3f(-495, 510, 525);
	glEnd();
	//kiribawah4
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 550, 525);
	glVertex3f(-495, 550, 525);
	glVertex3f(-495, 550, -525);
	glVertex3f(-525, 550, -525);
	//kiribawah5
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 550, -525);
	glVertex3f(-495, 550, -525);
	glVertex3f(-495, 510, -525);
	glVertex3f(-525, 510, -525);
	glEnd();
	//kiribawah6
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 550, 525);
	glVertex3f(-525, 550, -525);
	glVertex3f(-525, 510, -525);
	glVertex3f(-525, 510, 525);
	glEnd();

	//kiriatas1
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 590, 525);
	glVertex3f(-495, 590, 525);
	glVertex3f(-495, 590, -525);
	glVertex3f(-525, 590, -525);
	glEnd();
	//kiriatas2
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 620, 525);
	glVertex3f(-495, 620, 525);
	glVertex3f(-495, 590, 525);
	glVertex3f(-525, 590, 525);
	glEnd();
	//kiriatas3
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-495, 620, 525);
	glVertex3f(-495, 620, -525);
	glVertex3f(-495, 590, -525);
	glVertex3f(-495, 590, 525);
	glEnd();
	//kiriatas4
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 620, 525);
	glVertex3f(-495, 620, 525);
	glVertex3f(-495, 620, -525);
	glVertex3f(-525, 620, -525);
	//kiriatas5
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 620, -525);
	glVertex3f(-495, 620, -525);
	glVertex3f(-495, 590, -525);
	glVertex3f(-525, 590, -525);
	glEnd();
	//kiriatas6
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 620, 525);
	glVertex3f(-525, 620, -525);
	glVertex3f(-525, 590, -525);
	glVertex3f(-525, 590, 525);
	glEnd();

	//pojok1
	//belakang
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-535, 510, 485);
	glVertex3f(-535, 640, 485);
	glVertex3f(-485, 640, 485);
	glVertex3f(-485, 510, 485);
	glEnd();

	//depan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-535, 510, 535);
	glVertex3f(-535, 640, 535);
	glVertex3f(-485, 640, 535);
	glVertex3f(-485, 510, 535);
	glEnd();

	//kiri
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-535, 510, 535);
	glVertex3f(-535, 640, 535);
	glVertex3f(-535, 640, 485);
	glVertex3f(-535, 510, 485);
	glEnd();

	//kanan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-485, 510, 535);
	glVertex3f(-485, 640, 535);
	glVertex3f(-485, 640, 485);
	glVertex3f(-485, 510, 485);
	glEnd();

	//atas
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-535, 640, 535);
	glVertex3f(-485, 640, 535);
	glVertex3f(-485, 640, 485);
	glVertex3f(-535, 640, 485);
	glEnd();

	//bawah
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-535, 510, 535);
	glVertex3f(-485, 510, 535);
	glVertex3f(-485, 510, 485);
	glVertex3f(-535, 510, 485);
	glEnd();

	//pojok2
	//belakang
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-535, 510, -485);
	glVertex3f(-535, 640, -485);
	glVertex3f(-485, 640, -485);
	glVertex3f(-485, 510, -485);
	glEnd();

	//depan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-535, 510, -535);
	glVertex3f(-535, 640, -535);
	glVertex3f(-485, 640, -535);
	glVertex3f(-485, 510, -535);
	glEnd();

	//kiri
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-535, 510, -535);
	glVertex3f(-535, 640, -535);
	glVertex3f(-535, 640, -485);
	glVertex3f(-535, 510, -485);
	glEnd();

	//kanan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-485, 510, -535);
	glVertex3f(-485, 640, -535);
	glVertex3f(-485, 640, -485);
	glVertex3f(-485, 510, -485);
	glEnd();

	//atas
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-535, 640, -535);
	glVertex3f(-485, 640, -535);
	glVertex3f(-485, 640, -485);
	glVertex3f(-535, 640, -485);
	glEnd();

	//bawah
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-535, 510, -535);
	glVertex3f(-485, 510, -535);
	glVertex3f(-485, 510, -485);
	glVertex3f(-535, 510, -485);
	glEnd();




	//kiribawah1
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(525, 510, 525);
	glVertex3f(495, 510, 525);
	glVertex3f(495, 510, -525);
	glVertex3f(525, 510, -525);
	glEnd();
	//kiribawah2
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(525, 550, 525);
	glVertex3f(495, 550, 525);
	glVertex3f(495, 510, 525);
	glVertex3f(525, 510, 525);
	glEnd();
	//kiribawah3
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(495, 550, 525);
	glVertex3f(495, 550, -525);
	glVertex3f(495, 510, -525);
	glVertex3f(495, 510, 525);
	glEnd();
	//kiribawah4
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(525, 550, 525);
	glVertex3f(495, 550, 525);
	glVertex3f(495, 550, -525);
	glVertex3f(525, 550, -525);
	//kiribawah5
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(525, 550, -525);
	glVertex3f(495, 550, -525);
	glVertex3f(495, 510, -525);
	glVertex3f(525, 510, -525);
	glEnd();
	//kiribawah6
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(525, 550, 525);
	glVertex3f(525, 550, -525);
	glVertex3f(525, 510, -525);
	glVertex3f(525, 510, 525);
	glEnd();

	//kiriatas1
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(525, 590, 525);
	glVertex3f(495, 590, 525);
	glVertex3f(495, 590, -525);
	glVertex3f(525, 590, -525);
	glEnd();
	//kiriatas2
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(525, 620, 525);
	glVertex3f(495, 620, 525);
	glVertex3f(495, 590, 525);
	glVertex3f(525, 590, 525);
	glEnd();
	//kiriatas3
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(495, 620, 525);
	glVertex3f(495, 620, -525);
	glVertex3f(495, 590, -525);
	glVertex3f(495, 590, 525);
	glEnd();
	//kiriatas4
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(525, 620, 525);
	glVertex3f(495, 620, 525);
	glVertex3f(495, 620, -525);
	glVertex3f(525, 620, -525);
	//kiriatas5
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(525, 620, -525);
	glVertex3f(495, 620, -525);
	glVertex3f(495, 590, -525);
	glVertex3f(525, 590, -525);
	glEnd();
	//kiriatas6
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(525, 620, 525);
	glVertex3f(525, 620, -525);
	glVertex3f(525, 590, -525);
	glVertex3f(525, 590, 525);
	glEnd();

	//kiribawah1
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 510, 525);
	glVertex3f(-495, 510, 525);
	glVertex3f(-495, 510, -525);
	glVertex3f(-525, 510, -525);
	glEnd();
	//kiribawah2
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 550, 525);
	glVertex3f(-495, 550, 525);
	glVertex3f(-495, 510, 525);
	glVertex3f(-525, 510, 525);
	glEnd();
	//kiribawah3
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-495, 550, 525);
	glVertex3f(-495, 550, -525);
	glVertex3f(-495, 510, -525);
	glVertex3f(-495, 510, 525);
	glEnd();
	//kiribawah4
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 550, 525);
	glVertex3f(-495, 550, 525);
	glVertex3f(-495, 550, -525);
	glVertex3f(-525, 550, -525);
	//kiribawah5
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 550, -525);
	glVertex3f(-495, 550, -525);
	glVertex3f(-495, 510, -525);
	glVertex3f(-525, 510, -525);
	glEnd();
	//kiribawah6
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 550, 525);
	glVertex3f(-525, 550, -525);
	glVertex3f(-525, 510, -525);
	glVertex3f(-525, 510, 525);
	glEnd();

	//kiriatas1
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 590, 525);
	glVertex3f(-495, 590, 525);
	glVertex3f(-495, 590, -525);
	glVertex3f(-525, 590, -525);
	glEnd();
	//kiriatas2
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 620, 525);
	glVertex3f(-495, 620, 525);
	glVertex3f(-495, 590, 525);
	glVertex3f(-525, 590, 525);
	glEnd();
	//kiriatas3
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-495, 620, 525);
	glVertex3f(-495, 620, -525);
	glVertex3f(-495, 590, -525);
	glVertex3f(-495, 590, 525);
	glEnd();
	//kiriatas4
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 620, 525);
	glVertex3f(-495, 620, 525);
	glVertex3f(-495, 620, -525);
	glVertex3f(-525, 620, -525);
	//kiriatas5
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 620, -525);
	glVertex3f(-495, 620, -525);
	glVertex3f(-495, 590, -525);
	glVertex3f(-525, 590, -525);
	glEnd();
	//kiriatas6
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 620, 525);
	glVertex3f(-525, 620, -525);
	glVertex3f(-525, 590, -525);
	glVertex3f(-525, 590, 525);
	glEnd();

	//pojok1
	//belakang
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(535, 510, 485);
	glVertex3f(535, 640, 485);
	glVertex3f(485, 640, 485);
	glVertex3f(485, 510, 485);
	glEnd();

	//depan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(535, 510, 535);
	glVertex3f(535, 640, 535);
	glVertex3f(485, 640, 535);
	glVertex3f(485, 510, 535);
	glEnd();

	//kiri
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(535, 510, 535);
	glVertex3f(535, 640, 535);
	glVertex3f(535, 640, 485);
	glVertex3f(535, 510, 485);
	glEnd();

	//kanan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(485, 510, 535);
	glVertex3f(485, 640, 535);
	glVertex3f(485, 640, 485);
	glVertex3f(485, 510, 485);
	glEnd();

	//atas
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(535, 640, 535);
	glVertex3f(485, 640, 535);
	glVertex3f(485, 640, 485);
	glVertex3f(535, 640, 485);
	glEnd();

	//bawah
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(535, 510, 535);
	glVertex3f(485, 510, 535);
	glVertex3f(485, 510, 485);
	glVertex3f(535, 510, 485);
	glEnd();

	//pojok2
	//belakang
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(535, 510, -485);
	glVertex3f(535, 640, -485);
	glVertex3f(485, 640, -485);
	glVertex3f(485, 510, -485);
	glEnd();

	//depan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(535, 510, -535);
	glVertex3f(535, 640, -535);
	glVertex3f(485, 640, -535);
	glVertex3f(485, 510, -535);
	glEnd();

	//kiri
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(535, 510, -535);
	glVertex3f(535, 640, -535);
	glVertex3f(535, 640, -485);
	glVertex3f(535, 510, -485);
	glEnd();

	//kanan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(485, 510, -535);
	glVertex3f(485, 640, -535);
	glVertex3f(485, 640, -485);
	glVertex3f(485, 510, -485);
	glEnd();

	//atas
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(535, 640, -535);
	glVertex3f(485, 640, -535);
	glVertex3f(485, 640, -485);
	glVertex3f(535, 640, -485);
	glEnd();

	//bawah
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(535, 510, -535);
	glVertex3f(485, 510, -535);
	glVertex3f(485, 510, -485);
	glVertex3f(535, 510, -485);
	glEnd();

	//belakang
	//kiribawah1
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 510, -525);
	glVertex3f(525, 510, -525);
	glVertex3f(525, 510, -495);
	glVertex3f(-525, 510, -495);
	glEnd();
	//kiribawah2
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 550, -525);
	glVertex3f(525, 550, -525);
	glVertex3f(525, 550, -495);
	glVertex3f(-525, 550, -495);
	glEnd();
	//kiribawah3
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 550, -525);
	glVertex3f(525, 550, -525);
	glVertex3f(525, 510, -525);
	glVertex3f(-525, 510, -525);
	glEnd();
	//kiribawah4
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 550, -495);
	glVertex3f(525, 550, -495);
	glVertex3f(525, 510, -495);
	glVertex3f(-525, 510, -495);
	glEnd();

	//kiriatas1
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 620, -525);
	glVertex3f(525, 590, -525);
	glVertex3f(525, 590, -495);
	glVertex3f(-525, 590, -495);
	glEnd();
	//kiriatas2
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 620, -525);
	glVertex3f(525, 620, -525);
	glVertex3f(525, 620, -495);
	glVertex3f(-525, 620, -495);
	glEnd();
	//kiriatas3
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 620, -525);
	glVertex3f(525, 620, -525);
	glVertex3f(525, 590, -525);
	glVertex3f(-525, 590, -525);
	glEnd();
	//kiriatas4
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 620, -495);
	glVertex3f(525, 620, -495);
	glVertex3f(525, 590, -495);
	glVertex3f(-525, 590, -495);
	glEnd();

	//depan
	//kiribawah1
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 510, 525);
	glVertex3f(525, 510, 525);
	glVertex3f(525, 510, 495);
	glVertex3f(-525, 510, 495);
	glEnd();
	//kiribawah2
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 550, 525);
	glVertex3f(525, 550, 525);
	glVertex3f(525, 550, 495);
	glVertex3f(-525, 550, 495);
	glEnd();
	//kiribawah3
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 550, 525);
	glVertex3f(525, 550, 525);
	glVertex3f(525, 510, 525);
	glVertex3f(-525, 510, 525);
	glEnd();
	//kiribawah4
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 550, 495);
	glVertex3f(525, 550, 495);
	glVertex3f(525, 510, 495);
	glVertex3f(-525, 510, 495);
	glEnd();

	//kiriatas1
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 620, 525);
	glVertex3f(525, 590, 525);
	glVertex3f(525, 590, 495);
	glVertex3f(-525, 590, 495);
	glEnd();
	//kiriatas2
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 620, 525);
	glVertex3f(525, 620, 525);
	glVertex3f(525, 620, 495);
	glVertex3f(-525, 620, 495);
	glEnd();
	//kiriatas3
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 620, 525);
	glVertex3f(525, 620, 525);
	glVertex3f(525, 590, 525);
	glVertex3f(-525, 590, 525);
	glEnd();
	//kiriatas4
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-525, 620, 495);
	glVertex3f(525, 620, 495);
	glVertex3f(525, 590, 495);
	glVertex3f(-525, 590, 495);
	glEnd();
}

void dinding_lantai3() {
	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.65, 0.60, 0.581);
	glVertex3f(250, 910, -250);
	glVertex3f(250, 1160, -250);
	glVertex3f(250, 1160, 250);
	glVertex3f(250, 910, 250);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.65, 0.60, 0.581);
	glVertex3f(-250, 910, -250);
	glVertex3f(-250, 1160, -250);
	glVertex3f(-250, 1160, 250);
	glVertex3f(-250, 910, 250);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.65, 0.60, 0.581);
	glVertex3f(-250, 910, -250);
	glVertex3f(-250, 1160, -250);
	glVertex3f(250, 1160, -250);
	glVertex3f(250, 910, -250);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.65, 0.60, 0.581);
	glVertex3f(-250, 910, 250);
	glVertex3f(-250, 1160, 250);
	glVertex3f(250, 1160, 250);
	glVertex3f(250, 910, 250);
	glEnd();
}

void atap_lantai3() {
	//Atap1
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.43, 0.43, 0.43);
	glVertex3f(-200, 1210, -200);
	glVertex3f(200, 1210, -200);
	glVertex3f(200, 1210, 200);
	glVertex3f(-200, 1210, 200);
	glEnd();

	//Bawah
	glBegin(GL_QUADS);
	glColor3f(0.35, 0.35, 0.35);
	glVertex3f(-425, 1160, -425);
	glVertex3f(425, 1160, -425);
	glVertex3f(425, 1160, 425);
	glVertex3f(-425, 1160, 425);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.46, 0.46, 0.46);
	glVertex3f(425, 1160, -425);
	glVertex3f(200, 1210, -200);
	glVertex3f(200, 1210, 200);
	glVertex3f(425, 1160, 425);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.46, 0.46, 0.46);
	glVertex3f(-425, 1160, -425);
	glVertex3f(-200, 1210, -200);
	glVertex3f(-200, 1210, 200);
	glVertex3f(-425, 1160, 425);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.43, 0.43, 0.43);
	glVertex3f(-425, 1160, -425);
	glVertex3f(-200, 1210, -200);
	glVertex3f(200, 1210, -200);
	glVertex3f(425, 1160, -425);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.43, 0.43, 0.43);
	glVertex3f(-425, 1160, 425);
	glVertex3f(-200, 1210, 200);
	glVertex3f(200, 1210, 200);
	glVertex3f(425, 1160, 425);
	glEnd();

	//Atap2
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.43, 0.43, 0.43);
	glVertex3f(-350, 1260, -350);
	glVertex3f(350, 1260, -350);
	glVertex3f(350, 1260, 350);
	glVertex3f(-350, 1260, 350);
	glEnd();
	//Atas2
	glBegin(GL_QUADS);
	glColor3f(0.35, 0.35, 0.35);
	glVertex3f(-275, 1262, -275);
	glVertex3f(275, 1262, -275);
	glVertex3f(275, 1262, 275);
	glVertex3f(-275, 1262, 275);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(200, 1210, -200);
	glVertex3f(350, 1260, -350);
	glVertex3f(350, 1260, 350);
	glVertex3f(200, 1210, 200);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-200, 1210, -200);
	glVertex3f(-350, 1260, -350);
	glVertex3f(-350, 1260, 350);
	glVertex3f(-200, 1210, 200);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-200, 1210, -200);
	glVertex3f(-350, 1260, -350);
	glVertex3f(350, 1260, -350);
	glVertex3f(200, 1210, -200);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-200, 1210, 200);
	glVertex3f(-350, 1260, 350);
	glVertex3f(350, 1260, 350);
	glVertex3f(200, 1210, 200);
	glEnd();
}

void pagar_lantai3() {

	//kiribawah1
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-425, 910, 425);
	glVertex3f(-395, 910, 425);
	glVertex3f(-395, 910, -425);
	glVertex3f(-425, 910, -425);
	glEnd();
	//kiribawah2
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-425, 950, 425);
	glVertex3f(-395, 950, 425);
	glVertex3f(-395, 910, 425);
	glVertex3f(-425, 910, 425);
	glEnd();
	//kiribawah3
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-395, 950, 425);
	glVertex3f(-395, 950, -425);
	glVertex3f(-395, 910, -425);
	glVertex3f(-395, 910, 425);
	glEnd();
	//kiribawah4
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-425, 950, 425);
	glVertex3f(-395, 950, 425);
	glVertex3f(-395, 950, -425);
	glVertex3f(-425, 950, -425);
	//kiribawah5
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-425, 950, -425);
	glVertex3f(-395, 950, -425);
	glVertex3f(-395, 910, -425);
	glVertex3f(-425, 910, -425);
	glEnd();
	//kiribawah6
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-425, 950, 425);
	glVertex3f(-425, 950, -425);
	glVertex3f(-425, 910, -425);
	glVertex3f(-425, 910, 425);
	glEnd();

	//tengah depan
	for (int x = 0; x <= 17; x++)
	{
		glBegin(GL_QUADS);
		glColor3f(0.54, 0.06, 0.05);
		glVertex3f(-400 + x * 45, 950, 425);
		glVertex3f(-400 + x * 45, 990, 425);
		glVertex3f(-390 + x * 45, 990, 425);
		glVertex3f(-390 + x * 45, 950, 425);
	}

	//kiriatas1
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-425, 990, 425);
	glVertex3f(-395, 990, 425);
	glVertex3f(-395, 990, -425);
	glVertex3f(-425, 990, -425);
	glEnd();
	//kiriatas2
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-425, 1020, 425);
	glVertex3f(-395, 1020, 425);
	glVertex3f(-395, 990, 425);
	glVertex3f(-425, 990, 425);
	glEnd();
	//kiriatas3
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-395, 1020, 425);
	glVertex3f(-395, 1020, -425);
	glVertex3f(-395, 990, -425);
	glVertex3f(-395, 990, 425);
	glEnd();
	//kiriatas4
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-425, 1020, 425);
	glVertex3f(-395, 1020, 425);
	glVertex3f(-395, 1020, -425);
	glVertex3f(-425, 1020, -425);
	//kiriatas5
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-425, 1020, -425);
	glVertex3f(-395, 1020, -425);
	glVertex3f(-395, 990, -425);
	glVertex3f(-425, 990, -425);
	glEnd();
	//kiriatas6
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-425, 1020, 425);
	glVertex3f(-425, 1020, -425);
	glVertex3f(-425, 990, -425);
	glVertex3f(-425, 990, 425);
	glEnd();

	//pojok1
	//belakang
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-435, 910, 385);
	glVertex3f(-435, 1040, 385);
	glVertex3f(-385, 1040, 385);
	glVertex3f(-385, 910, 385);
	glEnd();

	//depan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-435, 910, 435);
	glVertex3f(-435, 1040, 435);
	glVertex3f(-385, 1040, 435);
	glVertex3f(-385, 910, 435);
	glEnd();

	//kiri
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-435, 910, 435);
	glVertex3f(-435, 1040, 435);
	glVertex3f(-435, 1040, 385);
	glVertex3f(-435, 910, 385);
	glEnd();

	//kanan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-385, 910, 435);
	glVertex3f(-385, 1040, 435);
	glVertex3f(-385, 1040, 385);
	glVertex3f(-385, 910, 385);
	glEnd();

	//atas
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-435, 1040, 435);
	glVertex3f(-385, 1040, 435);
	glVertex3f(-385, 1040, 385);
	glVertex3f(-435, 1040, 385);
	glEnd();

	//bawah
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-435, 910, 435);
	glVertex3f(-385, 910, 435);
	glVertex3f(-385, 910, 385);
	glVertex3f(-435, 910, 385);
	glEnd();

	//pojok2
	//belakang
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-435, 910, -385);
	glVertex3f(-435, 1040, -385);
	glVertex3f(-385, 1040, -385);
	glVertex3f(-385, 910, -385);
	glEnd();

	//depan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-435, 910, -435);
	glVertex3f(-435, 1040, -435);
	glVertex3f(-385, 1040, -435);
	glVertex3f(-385, 910, -435);
	glEnd();

	//kiri
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-435, 910, -435);
	glVertex3f(-435, 1040, -435);
	glVertex3f(-435, 1040, -385);
	glVertex3f(-435, 910, -385);
	glEnd();

	//kanan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-385, 910, -435);
	glVertex3f(-385, 1040, -435);
	glVertex3f(-385, 1040, -385);
	glVertex3f(-385, 910, -385);
	glEnd();

	//atas
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-435, 1040, -435);
	glVertex3f(-385, 1040, -435);
	glVertex3f(-385, 1040, -385);
	glVertex3f(-435, 1040, -385);
	glEnd();

	//bawah
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-435, 910, -435);
	glVertex3f(-385, 910, -435);
	glVertex3f(-385, 910, -385);
	glVertex3f(-435, 910, -385);
	glEnd();


	//kiribawah1
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(425, 910, 425);
	glVertex3f(395, 910, 425);
	glVertex3f(395, 910, -425);
	glVertex3f(425, 910, -425);
	glEnd();
	//kiribawah2
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(425, 950, 425);
	glVertex3f(395, 950, 425);
	glVertex3f(395, 910, 425);
	glVertex3f(425, 910, 425);
	glEnd();
	//kiribawah3
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(395, 950, 425);
	glVertex3f(395, 950, -425);
	glVertex3f(395, 910, -425);
	glVertex3f(395, 910, 425);
	glEnd();
	//kiribawah4
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(425, 950, 425);
	glVertex3f(395, 950, 425);
	glVertex3f(395, 950, -425);
	glVertex3f(425, 950, -425);
	//kiribawah5
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(425, 950, -425);
	glVertex3f(395, 950, -425);
	glVertex3f(395, 910, -425);
	glVertex3f(425, 910, -425);
	glEnd();
	//kiribawah6
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(425, 950, 425);
	glVertex3f(425, 950, -425);
	glVertex3f(425, 910, -425);
	glVertex3f(425, 910, 425);
	glEnd();

	//tengah kanan
	for (int x = 0; x <= 17; x++)
	{
		glBegin(GL_QUADS);
		glColor3f(0.54, 0.06, 0.05);
		glVertex3f(425, 950, 420 - x * 45);
		glVertex3f(425, 990, 420 - x * 45);
		glVertex3f(425, 990, 430 - x * 45);
		glVertex3f(425, 950, 430 - x * 45);
	}

	//kiriatas1
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(425, 990, 425);
	glVertex3f(395, 990, 425);
	glVertex3f(395, 990, -425);
	glVertex3f(425, 990, -425);
	glEnd();
	//kiriatas2
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(425, 1020, 425);
	glVertex3f(395, 1020, 425);
	glVertex3f(395, 990, 425);
	glVertex3f(425, 990, 425);
	glEnd();
	//kiriatas3
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(395, 1020, 425);
	glVertex3f(395, 1020, -425);
	glVertex3f(395, 990, -425);
	glVertex3f(395, 990, 425);
	glEnd();
	//kiriatas4
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(425, 1020, 425);
	glVertex3f(395, 1020, 425);
	glVertex3f(395, 1020, -425);
	glVertex3f(425, 1020, -425);
	//kiriatas5
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(425, 1020, -425);
	glVertex3f(395, 1020, -425);
	glVertex3f(395, 990, -425);
	glVertex3f(425, 990, -425);
	glEnd();
	//kiriatas6
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(425, 1020, 425);
	glVertex3f(425, 1020, -425);
	glVertex3f(425, 990, -425);
	glVertex3f(425, 990, 425);
	glEnd();

	//pojok1
	//belakang
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(435, 910, 385);
	glVertex3f(435, 1040, 385);
	glVertex3f(385, 1040, 385);
	glVertex3f(385, 910, 385);
	glEnd();

	//depan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(435, 910, 435);
	glVertex3f(435, 1040, 435);
	glVertex3f(385, 1040, 435);
	glVertex3f(385, 910, 435);
	glEnd();

	//kiri
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(435, 910, 435);
	glVertex3f(435, 1040, 435);
	glVertex3f(435, 1040, 385);
	glVertex3f(435, 910, 385);
	glEnd();

	//kanan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(385, 910, 435);
	glVertex3f(385, 1040, 435);
	glVertex3f(385, 1040, 385);
	glVertex3f(385, 910, 385);
	glEnd();

	//atas
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(435, 1040, 435);
	glVertex3f(385, 1040, 435);
	glVertex3f(385, 1040, 385);
	glVertex3f(435, 1040, 385);
	glEnd();

	//bawah
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(435, 910, 435);
	glVertex3f(385, 910, 435);
	glVertex3f(385, 910, 385);
	glVertex3f(435, 910, 385);
	glEnd();

	//pojok2
	//belakang
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(435, 910, -385);
	glVertex3f(435, 1040, -385);
	glVertex3f(385, 1040, -385);
	glVertex3f(385, 910, -385);
	glEnd();

	//depan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(435, 910, -435);
	glVertex3f(435, 1040, -435);
	glVertex3f(385, 1040, -435);
	glVertex3f(385, 910, -435);
	glEnd();

	//kiri
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(435, 910, -435);
	glVertex3f(435, 1040, -435);
	glVertex3f(435, 1040, -385);
	glVertex3f(435, 910, -385);
	glEnd();

	//kanan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(385, 910, -435);
	glVertex3f(385, 1040, -435);
	glVertex3f(385, 1040, -385);
	glVertex3f(385, 910, -385);
	glEnd();

	//atas
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(435, 1040, -435);
	glVertex3f(385, 1040, -435);
	glVertex3f(385, 1040, -385);
	glVertex3f(435, 1040, -385);
	glEnd();

	//bawah
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(435, 910, -435);
	glVertex3f(385, 910, -435);
	glVertex3f(385, 910, -385);
	glVertex3f(435, 910, -385);
	glEnd();

	//belakang
	//kiribawah1
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-425, 910, -425);
	glVertex3f(425, 910, -425);
	glVertex3f(425, 910, -395);
	glVertex3f(-425, 910, -395);
	glEnd();
	//kiribawah2
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-425, 950, -425);
	glVertex3f(425, 950, -425);
	glVertex3f(425, 950, -395);
	glVertex3f(-425, 950, -395);
	glEnd();
	//kiribawah3
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-425, 950, -425);
	glVertex3f(425, 950, -425);
	glVertex3f(425, 910, -425);
	glVertex3f(-425, 910, -425);
	glEnd();
	//kiribawah4
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-425, 950, -395);
	glVertex3f(425, 950, -395);
	glVertex3f(425, 910, -395);
	glVertex3f(-425, 910, -395);
	glEnd();

	//tengah dbelakang
	for (int x = 0; x <= 17; x++)
	{
		glBegin(GL_QUADS);
		glColor3f(0.54, 0.06, 0.05);
		glVertex3f(-410 + x * 45, 950, -420);
		glVertex3f(-410 + x * 45, 990, -420);
		glVertex3f(-420 + x * 45, 990, -420);
		glVertex3f(-420 + x * 45, 950, -420);
	}

	//kiriatas1
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-425, 1020, -425);
	glVertex3f(425, 990, -425);
	glVertex3f(425, 990, -395);
	glVertex3f(-425, 990, -395);
	glEnd();
	//kiriatas2
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-425, 1020, -425);
	glVertex3f(425, 1020, -425);
	glVertex3f(425, 1020, -395);
	glVertex3f(-425, 1020, -395);
	glEnd();
	//kiriatas3
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-425, 1020, -425);
	glVertex3f(425, 1020, -425);
	glVertex3f(425, 990, -425);
	glVertex3f(-425, 990, -425);
	glEnd();
	//kiriatas4
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-425, 1020, -395);
	glVertex3f(425, 1020, -395);
	glVertex3f(425, 990, -395);
	glVertex3f(-425, 990, -395);
	glEnd();

	//depan
	//kiribawah1
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-425, 910, 425);
	glVertex3f(425, 910, 425);
	glVertex3f(425, 910, 395);
	glVertex3f(-425, 910, 395);
	glEnd();
	//kiribawah2
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-425, 950, 425);
	glVertex3f(425, 950, 425);
	glVertex3f(425, 950, 395);
	glVertex3f(-425, 950, 395);
	glEnd();
	//kiribawah3
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-425, 950, 425);
	glVertex3f(425, 950, 425);
	glVertex3f(425, 910, 425);
	glVertex3f(-425, 910, 425);
	glEnd();
	//kiribawah4
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-425, 950, 395);
	glVertex3f(425, 950, 395);
	glVertex3f(425, 910, 395);
	glVertex3f(-425, 910, 395);
	glEnd();

	//tengah kiri
	for (int x = 0; x <= 17; x++)
	{
		glBegin(GL_QUADS);
		glColor3f(0.54, 0.06, 0.05);
		glVertex3f(-425, 950, 430 - x * 45);
		glVertex3f(-425, 990, 430 - x * 45);
		glVertex3f(-425, 990, 420 - x * 45);
		glVertex3f(-425, 950, 420 - x * 45);
	}

	//kiriatas1
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-425, 1020, 425);
	glVertex3f(425, 990, 425);
	glVertex3f(425, 990, 395);
	glVertex3f(-425, 990, 395);
	glEnd();
	//kiriatas2
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-425, 1020, 425);
	glVertex3f(425, 1020, 425);
	glVertex3f(425, 1020, 395);
	glVertex3f(-425, 1020, 395);
	glEnd();
	//kiriatas3
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-425, 1020, 425);
	glVertex3f(425, 1020, 425);
	glVertex3f(425, 990, 425);
	glVertex3f(-425, 990, 425);
	glEnd();
	//kiriatas4
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-425, 1020, 395);
	glVertex3f(425, 1020, 395);
	glVertex3f(425, 990, 395);
	glVertex3f(-425, 990, 395);
	glEnd();
}

void dinding_lantai4() {
	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.65, 0.60, 0.581);
	glVertex3f(200, 1260, -200);
	glVertex3f(200, 1460, -200);
	glVertex3f(200, 1460, 200);
	glVertex3f(200, 1260, 200);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.65, 0.60, 0.581);
	glVertex3f(-200, 1260, -200);
	glVertex3f(-200, 1460, -200);
	glVertex3f(-200, 1460, 200);
	glVertex3f(-200, 1260, 200);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.65, 0.60, 0.581);
	glVertex3f(-200, 1260, -200);
	glVertex3f(-200, 1460, -200);
	glVertex3f(200, 1460, -200);
	glVertex3f(200, 1260, -200);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.65, 0.60, 0.581);
	glVertex3f(-200, 1260, 200);
	glVertex3f(-200, 1460, 200);
	glVertex3f(200, 1460, 200);
	glVertex3f(200, 1260, 200);
	glEnd();
}

void atap_lantai4() {
	//Bawah
	glBegin(GL_QUADS);
	glColor3f(0.65, 0.60, 0.581);
	glVertex3f(-225, 1460, -225);
	glVertex3f(225, 1460, -225);
	glVertex3f(225, 1460, 225);
	glVertex3f(-225, 1460, 225);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(225, 1460, -225);
	glVertex3f(225, 1475, -225);
	glVertex3f(225, 1475, 225);
	glVertex3f(225, 1460, 225);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-225, 1460, -225);
	glVertex3f(-225, 1475, -225);
	glVertex3f(-225, 1475, 225);
	glVertex3f(-225, 1460, 225);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-225, 1460, -225);
	glVertex3f(-225, 1475, -225);
	glVertex3f(225, 1475, -225);
	glVertex3f(225, 1460, -225);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-225, 1460, 225);
	glVertex3f(-225, 1475, 225);
	glVertex3f(225, 1475, 225);
	glVertex3f(225, 1460, 225);
	glEnd();

	//
	//Bawah
	glBegin(GL_QUADS);
	glColor3f(0.65, 0.60, 0.581);
	glVertex3f(-250, 1475, -250);
	glVertex3f(250, 1475, -250);
	glVertex3f(250, 1475, 250);
	glVertex3f(-250, 1475, 250);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(250, 1475, -250);
	glVertex3f(250, 1495, -250);
	glVertex3f(250, 1495, 250);
	glVertex3f(250, 1475, 250);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-250, 1475, -250);
	glVertex3f(-250, 1495, -250);
	glVertex3f(-250, 1495, 250);
	glVertex3f(-250, 1475, 250);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-250, 1475, -250);
	glVertex3f(-250, 1495, -250);
	glVertex3f(250, 1495, -250);
	glVertex3f(250, 1475, -250);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-250, 1475, 250);
	glVertex3f(-250, 1495, 250);
	glVertex3f(250, 1495, 250);
	glVertex3f(250, 1475, 250);
	glEnd();

	//
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.43, 0.43, 0.43);
	glVertex3f(-25, 1745, -25);
	glVertex3f(25, 1745, -25);
	glVertex3f(25, 1745, 25);
	glVertex3f(-25, 1745, 25);
	glEnd();

	//Bawah
	glBegin(GL_QUADS);
	glColor3f(0.35, 0.35, 0.35);
	glVertex3f(-350, 1495, -350);
	glVertex3f(350, 1495, -350);
	glVertex3f(350, 1495, 350);
	glVertex3f(-350, 1495, 350);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.46, 0.46, 0.46);
	glVertex3f(350, 1495, -350);
	glVertex3f(25, 1745, -25);
	glVertex3f(25, 1745, 25);
	glVertex3f(350, 1495, 350);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.46, 0.46, 0.46);
	glVertex3f(-350, 1495, -350);
	glVertex3f(-25, 1745, -25);
	glVertex3f(-25, 1745, 25);
	glVertex3f(-350, 1495, 350);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.43, 0.43, 0.43);
	glVertex3f(-350, 1495, -350);
	glVertex3f(-25, 1745, -25);
	glVertex3f(25, 1745, -25);
	glVertex3f(350, 1495, -350);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.43, 0.43, 0.43);
	glVertex3f(-350, 1495, 350);
	glVertex3f(-25, 1745, 25);
	glVertex3f(25, 1745, 25);
	glVertex3f(350, 1495, 350);
	glEnd();
}

void pagar_lantai4() {

	//tengah depan
	for (int x = 0; x <= 13; x++)
	{
		glBegin(GL_QUADS);
		glColor3f(0.54, 0.06, 0.05);
		glVertex3f(-300 + x * 45, 1260, 345);
		glVertex3f(-300 + x * 45, 1340, 345);
		glVertex3f(-290 + x * 45, 1340, 345);
		glVertex3f(-290 + x * 45, 1260, 345);
	}

	//tengah kanan
	for (int x = 0; x <= 13; x++)
	{
		glBegin(GL_QUADS);
		glColor3f(0.54, 0.06, 0.05);
		glVertex3f(345, 1260, 295 - x * 45);
		glVertex3f(345, 1340, 295 - x * 45);
		glVertex3f(345, 1340, 305 - x * 45);
		glVertex3f(345, 1260, 305 - x * 45);
	}

	//tengah belakang
	for (int x = 0; x <= 13; x++)
	{
		glBegin(GL_QUADS);
		glColor3f(0.54, 0.06, 0.05);
		glVertex3f(300 - x * 45, 1260, -345);
		glVertex3f(300 - x * 45, 1340, -345);
		glVertex3f(290 - x * 45, 1340, -345);
		glVertex3f(290 - x * 45, 1260, -345);
	}

	//tengah kiri
	for (int x = 0; x <= 13; x++)
	{
		glBegin(GL_QUADS);
		glColor3f(0.54, 0.06, 0.05);
		glVertex3f(-345, 1260, 295 - x * 45);
		glVertex3f(-345, 1340, 295 - x * 45);
		glVertex3f(-345, 1340, 305 - x * 45);
		glVertex3f(-345, 1260, 305 - x * 45);
	}

	//kiribawah1
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-350, 1260, 350);
	glVertex3f(-320, 1260, 350);
	glVertex3f(-320, 1260, -350);
	glVertex3f(-350, 1260, -350);
	glEnd();
	//kiribawah2
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-350, 1300, 350);
	glVertex3f(-320, 1300, 350);
	glVertex3f(-320, 1260, 350);
	glVertex3f(-350, 1260, 350);
	glEnd();
	//kiribawah3
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-320, 1300, 350);
	glVertex3f(-320, 1300, -350);
	glVertex3f(-320, 1260, -350);
	glVertex3f(-320, 1260, 350);
	glEnd();
	//kiribawah4
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-350, 1300, 350);
	glVertex3f(-320, 1300, 350);
	glVertex3f(-320, 1300, -350);
	glVertex3f(-350, 1300, -350);
	//kiribawah5
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-350, 1300, -350);
	glVertex3f(-320, 1300, -350);
	glVertex3f(-320, 1260, -350);
	glVertex3f(-350, 1260, -350);
	glEnd();
	//kiribawah6
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-350, 1300, 350);
	glVertex3f(-350, 1300, -350);
	glVertex3f(-350, 1260, -350);
	glVertex3f(-350, 1260, 350);
	glEnd();

	//kiriatas1
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-350, 1340, 350);
	glVertex3f(-320, 1340, 350);
	glVertex3f(-320, 1340, -350);
	glVertex3f(-350, 1340, -350);
	glEnd();
	//kiriatas2
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-350, 1370, 350);
	glVertex3f(-320, 1370, 350);
	glVertex3f(-320, 1340, 350);
	glVertex3f(-350, 1340, 350);
	glEnd();
	//kiriatas3
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-320, 1370, 350);
	glVertex3f(-320, 1370, -350);
	glVertex3f(-320, 1340, -350);
	glVertex3f(-320, 1340, 350);
	glEnd();
	//kiriatas4
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-350, 1370, 350);
	glVertex3f(-320, 1370, 350);
	glVertex3f(-320, 1370, -350);
	glVertex3f(-350, 1370, -350);
	//kiriatas5
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-350, 1370, -350);
	glVertex3f(-320, 1370, -350);
	glVertex3f(-320, 1340, -350);
	glVertex3f(-350, 1340, -350);
	glEnd();
	//kiriatas6
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-350, 1370, 350);
	glVertex3f(-350, 1370, -350);
	glVertex3f(-350, 1340, -350);
	glVertex3f(-350, 1340, 350);
	glEnd();

	//pojok1
	//belakang
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-360, 1260, 310);
	glVertex3f(-360, 1380, 310);
	glVertex3f(-310, 1380, 310);
	glVertex3f(-310, 1260, 310);
	glEnd();

	//depan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-360, 1260, 360);
	glVertex3f(-360, 1380, 360);
	glVertex3f(-310, 1380, 360);
	glVertex3f(-310, 1260, 360);
	glEnd();

	//kiri
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-360, 1260, 360);
	glVertex3f(-360, 1380, 360);
	glVertex3f(-360, 1380, 310);
	glVertex3f(-360, 1260, 310);
	glEnd();

	//kanan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-310, 1260, 360);
	glVertex3f(-310, 1380, 360);
	glVertex3f(-310, 1380, 310);
	glVertex3f(-310, 1260, 310);
	glEnd();

	//atas
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-360, 1380, 360);
	glVertex3f(-310, 1380, 360);
	glVertex3f(-310, 1380, 310);
	glVertex3f(-360, 1380, 310);
	glEnd();

	//bawah
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-360, 1260, 360);
	glVertex3f(-310, 1260, 360);
	glVertex3f(-310, 1260, 310);
	glVertex3f(-360, 1260, 310);
	glEnd();

	//pojok2
	//belakang
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-360, 1260, -310);
	glVertex3f(-360, 1380, -310);
	glVertex3f(-310, 1380, -310);
	glVertex3f(-310, 1260, -310);
	glEnd();

	//depan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-360, 1260, -360);
	glVertex3f(-360, 1380, -360);
	glVertex3f(-310, 1380, -360);
	glVertex3f(-310, 1260, -360);
	glEnd();

	//kiri
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-360, 1260, -360);
	glVertex3f(-360, 1380, -360);
	glVertex3f(-360, 1380, -310);
	glVertex3f(-360, 1260, -310);
	glEnd();

	//kanan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-310, 1260, -360);
	glVertex3f(-310, 1380, -360);
	glVertex3f(-310, 1380, -310);
	glVertex3f(-310, 1260, -310);
	glEnd();

	//atas
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-360, 1380, -360);
	glVertex3f(-310, 1380, -360);
	glVertex3f(-310, 1380, -310);
	glVertex3f(-360, 1380, -310);
	glEnd();

	//bawah
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-360, 1260, -360);
	glVertex3f(-310, 1260, -360);
	glVertex3f(-310, 1260, -310);
	glVertex3f(-360, 1260, -310);
	glEnd();


	//kiribawah1
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(350, 1260, 350);
	glVertex3f(320, 1260, 350);
	glVertex3f(320, 1260, -350);
	glVertex3f(350, 1260, -350);
	glEnd();
	//kiribawah2
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(350, 1300, 350);
	glVertex3f(320, 1300, 350);
	glVertex3f(320, 1260, 350);
	glVertex3f(350, 1260, 350);
	glEnd();
	//kiribawah3
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(320, 1300, 350);
	glVertex3f(320, 1300, -350);
	glVertex3f(320, 1260, -350);
	glVertex3f(320, 1260, 350);
	glEnd();
	//kiribawah4
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(350, 1300, 350);
	glVertex3f(320, 1300, 350);
	glVertex3f(320, 1300, -350);
	glVertex3f(350, 1300, -350);
	//kiribawah5
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(350, 1300, -350);
	glVertex3f(320, 1300, -350);
	glVertex3f(320, 1260, -350);
	glVertex3f(350, 1260, -350);
	glEnd();
	//kiribawah6
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(350, 1300, 350);
	glVertex3f(350, 1300, -350);
	glVertex3f(350, 1260, -350);
	glVertex3f(350, 1260, 350);
	glEnd();

	//kiriatas1
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(350, 1340, 350);
	glVertex3f(320, 1340, 350);
	glVertex3f(320, 1340, -350);
	glVertex3f(350, 1340, -350);
	glEnd();
	//kiriatas2
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(350, 1370, 350);
	glVertex3f(320, 1370, 350);
	glVertex3f(320, 1340, 350);
	glVertex3f(350, 1340, 350);
	glEnd();
	//kiriatas3
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(320, 1370, 350);
	glVertex3f(320, 1370, -350);
	glVertex3f(320, 1340, -350);
	glVertex3f(320, 1340, 350);
	glEnd();
	//kiriatas4
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(350, 1370, 350);
	glVertex3f(320, 1370, 350);
	glVertex3f(320, 1370, -350);
	glVertex3f(350, 1370, -350);
	//kiriatas5
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(350, 1370, -350);
	glVertex3f(320, 1370, -350);
	glVertex3f(320, 1340, -350);
	glVertex3f(350, 1340, -350);
	glEnd();
	//kiriatas6
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(350, 1370, 350);
	glVertex3f(350, 1370, -350);
	glVertex3f(350, 1340, -350);
	glVertex3f(350, 1340, 350);
	glEnd();

	//pojok1
	//belakang
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(360, 1260, 310);
	glVertex3f(360, 1380, 310);
	glVertex3f(310, 1380, 310);
	glVertex3f(310, 1260, 310);
	glEnd();

	//depan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(360, 1260, 360);
	glVertex3f(360, 1380, 360);
	glVertex3f(310, 1380, 360);
	glVertex3f(310, 1260, 360);
	glEnd();

	//kiri
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(360, 1260, 360);
	glVertex3f(360, 1380, 360);
	glVertex3f(360, 1380, 310);
	glVertex3f(360, 1260, 310);
	glEnd();

	//kanan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(310, 1260, 360);
	glVertex3f(310, 1380, 360);
	glVertex3f(310, 1380, 310);
	glVertex3f(310, 1260, 310);
	glEnd();

	//atas
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(360, 1380, 360);
	glVertex3f(310, 1380, 360);
	glVertex3f(310, 1380, 310);
	glVertex3f(360, 1380, 310);
	glEnd();

	//bawah
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(360, 1260, 360);
	glVertex3f(310, 1260, 360);
	glVertex3f(310, 1260, 310);
	glVertex3f(360, 1260, 310);
	glEnd();

	//pojok2
	//belakang
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(360, 1260, -310);
	glVertex3f(360, 1380, -310);
	glVertex3f(310, 1380, -310);
	glVertex3f(310, 1260, -310);
	glEnd();

	//depan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(360, 1260, -360);
	glVertex3f(360, 1380, -360);
	glVertex3f(310, 1380, -360);
	glVertex3f(310, 1260, -360);
	glEnd();

	//kiri
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(360, 1260, -360);
	glVertex3f(360, 1380, -360);
	glVertex3f(360, 1380, -310);
	glVertex3f(360, 1260, -310);
	glEnd();

	//kanan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(310, 1260, -360);
	glVertex3f(310, 1380, -360);
	glVertex3f(310, 1380, -310);
	glVertex3f(310, 1260, -310);
	glEnd();

	//atas
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(360, 1380, -360);
	glVertex3f(310, 1380, -360);
	glVertex3f(310, 1380, -310);
	glVertex3f(360, 1380, -310);
	glEnd();

	//bawah
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(360, 1260, -360);
	glVertex3f(310, 1260, -360);
	glVertex3f(310, 1260, -310);
	glVertex3f(360, 1260, -310);
	glEnd();

	//belakang
	//kiribawah1
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-350, 1260, -350);
	glVertex3f(350, 1260, -350);
	glVertex3f(350, 1260, -320);
	glVertex3f(-350, 1260, -320);
	glEnd();
	//kiribawah2
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-350, 1300, -350);
	glVertex3f(350, 1300, -350);
	glVertex3f(350, 1300, -320);
	glVertex3f(-350, 1300, -320);
	glEnd();
	//kiribawah3
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-350, 1300, -350);
	glVertex3f(350, 1300, -350);
	glVertex3f(350, 1260, -350);
	glVertex3f(-350, 1260, -350);
	glEnd();
	//kiribawah4
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-350, 1300, -320);
	glVertex3f(350, 1300, -320);
	glVertex3f(350, 1260, -320);
	glVertex3f(-350, 1260, -320);
	glEnd();

	//kiriatas1
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-350, 1370, -350);
	glVertex3f(350, 1340, -350);
	glVertex3f(350, 1340, -320);
	glVertex3f(-350, 1340, -320);
	glEnd();
	//kiriatas2
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-350, 1370, -350);
	glVertex3f(350, 1370, -350);
	glVertex3f(350, 1370, -320);
	glVertex3f(-350, 1370, -320);
	glEnd();
	//kiriatas3
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-350, 1370, -350);
	glVertex3f(350, 1370, -350);
	glVertex3f(350, 1340, -350);
	glVertex3f(-350, 1340, -350);
	glEnd();
	//kiriatas4
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-350, 1370, -320);
	glVertex3f(350, 1370, -320);
	glVertex3f(350, 1340, -320);
	glVertex3f(-350, 1340, -320);
	glEnd();

	//depan
	//kiribawah1
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-350, 1260, 350);
	glVertex3f(350, 1260, 350);
	glVertex3f(350, 1260, 320);
	glVertex3f(-350, 1260, 320);
	glEnd();
	//kiribawah2
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-350, 1300, 350);
	glVertex3f(350, 1300, 350);
	glVertex3f(350, 1300, 320);
	glVertex3f(-350, 1300, 320);
	glEnd();
	//kiribawah3
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-350, 1300, 350);
	glVertex3f(350, 1300, 350);
	glVertex3f(350, 1260, 350);
	glVertex3f(-350, 1260, 350);
	glEnd();
	//kiribawah4
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-350, 1300, 320);
	glVertex3f(350, 1300, 320);
	glVertex3f(350, 1260, 320);
	glVertex3f(-350, 1260, 320);
	glEnd();

	//kiriatas1
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-350, 1370, 350);
	glVertex3f(350, 1340, 350);
	glVertex3f(350, 1340, 320);
	glVertex3f(-350, 1340, 320);
	glEnd();
	//kiriatas2
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-350, 1370, 350);
	glVertex3f(350, 1370, 350);
	glVertex3f(350, 1370, 320);
	glVertex3f(-350, 1370, 320);
	glEnd();
	//kiriatas3
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-350, 1370, 350);
	glVertex3f(350, 1370, 350);
	glVertex3f(350, 1340, 350);
	glVertex3f(-350, 1340, 350);
	glEnd();
	//kiriatas4
	glBegin(GL_QUADS);
	glColor3f(0.60, 0.15, 0.11);
	glVertex3f(-350, 1370, 320);
	glVertex3f(350, 1370, 320);
	glVertex3f(350, 1340, 320);
	glVertex3f(-350, 1340, 320);
	glEnd();
}

void hiasdinding1() {
	//Pintu
	glBegin(GL_QUADS);
	glColor3f(0.69, 0.69, 0.69);
	glVertex3f(-100, 110, 401);
	glVertex3f(-100, 400, 401);
	glVertex3f(100, 400, 401);
	glVertex3f(100, 110, 401);
	glEnd();

	//tiang pintu
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-5, 110, 402);
	glVertex3f(-5, 400, 402);
	glVertex3f(5, 400, 402);
	glVertex3f(5, 110, 402);
	glEnd();

	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-100, 400, 401);
	glVertex3f(-100, 410, 401);
	glVertex3f(100, 410, 401);
	glVertex3f(100, 400, 401);
	glEnd();

	//Hiasan Pintu
	for (int x = 0; x <= 10; x++)
	{
		glBegin(GL_QUADS);
		glColor3f(0.54, 0.06, 0.05);
		glVertex3f(-100, 125 + x * 25, 402);
		glVertex3f(-100, 130 + x * 25, 402);
		glVertex3f(100, 130 + x * 25, 402);
		glVertex3f(100, 125 + x * 25, 402);
		glEnd();
	}

	for (int x = 0; x < 2; x++)
	{
		glBegin(GL_QUADS);
		glColor3f(0.54, 0.06, 0.05);
		glVertex3f(-70 + x * 30, 110, 402);
		glVertex3f(-70 + x * 30, 400, 402);
		glVertex3f(-65 + x * 30, 400, 402);
		glVertex3f(-65 + x * 30, 110, 402);
		glEnd();
	}

	for (int x = 0; x < 2; x++)
	{
		glBegin(GL_QUADS);
		glColor3f(0.54, 0.06, 0.05);
		glVertex3f(40 + x * 30, 110, 402);
		glVertex3f(40 + x * 30, 400, 402);
		glVertex3f(35 + x * 30, 400, 402);
		glVertex3f(35 + x * 30, 110, 402);
		glEnd();
	}

	// tiang kiri 1 dindingdepan lt1
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-130, 110, 401);
	glVertex3f(-130, 460, 401);
	glVertex3f(-100, 460, 401);
	glVertex3f(-100, 110, 401);
	glEnd();

	// tiang kiri 2 dindingdepan lt1
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-235, 110, 401);
	glVertex3f(-235, 460, 401);
	glVertex3f(-265, 460, 401);
	glVertex3f(-265, 110, 401);
	glEnd();

	// tiang kiri 3 dindingdepan lt1
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-370, 110, 401);
	glVertex3f(-370, 460, 401);
	glVertex3f(-400, 460, 401);
	glVertex3f(-400, 110, 401);
	glEnd();

	// tiang kanan 1 dindingdepan lt1
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(130, 110, 401);
	glVertex3f(130, 460, 401);
	glVertex3f(100, 460, 401);
	glVertex3f(100, 110, 401);
	glEnd();

	// tiang kanan 2 dindingdepan lt1
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(235, 110, 401);
	glVertex3f(235, 460, 401);
	glVertex3f(265, 460, 401);
	glVertex3f(265, 110, 401);
	glEnd();

	// tiang kanan 3 dindingdepan lt1
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(370, 110, 401);
	glVertex3f(370, 460, 401);
	glVertex3f(400, 460, 401);
	glVertex3f(400, 110, 401);
	glEnd();

	// Tiang kanan dinding 1 lt1
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(401, 110, 400);
	glVertex3f(401, 460, 400);
	glVertex3f(401, 460, 370);
	glVertex3f(401, 110, 370);
	glEnd();

	// Tiang kanan dinding 2 lt1
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(401, 110, 265);
	glVertex3f(401, 460, 265);
	glVertex3f(401, 460, 235);
	glVertex3f(401, 110, 235);
	glEnd();

	// Tiang kanan dinding 3 lt1
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(401, 110, 140);
	glVertex3f(401, 460, 140);
	glVertex3f(401, 460, 110);
	glVertex3f(401, 110, 110);
	glEnd();

	// Tiang kanan dinding 4 lt1
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(401, 110, 15);
	glVertex3f(401, 460, 15);
	glVertex3f(401, 460, -15);
	glVertex3f(401, 110, -15);
	glEnd();

	// Tiang kanan dinding 5 lt1
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(401, 110, -140);
	glVertex3f(401, 460, -140);
	glVertex3f(401, 460, -110);
	glVertex3f(401, 110, -110);
	glEnd();

	// Tiang kanan dinding 6 lt1
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(401, 110, -265);
	glVertex3f(401, 460, -265);
	glVertex3f(401, 460, -235);
	glVertex3f(401, 110, -235);
	glEnd();

	// Tiang kanan dinding 7 lt1
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(401, 110, -400);
	glVertex3f(401, 460, -400);
	glVertex3f(401, 460, -370);
	glVertex3f(401, 110, -370);
	glEnd();

	// tiang kiri dinding 1 lt1
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-401, 110, 370);
	glVertex3f(-401, 460, 370);
	glVertex3f(-401, 460, 400);
	glVertex3f(-401, 110, 400);
	glEnd();

	// tiang kiri dinding 2 lt1
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-401, 110, 235);
	glVertex3f(-401, 460, 235);
	glVertex3f(-401, 460, 265);
	glVertex3f(-401, 110, 265);
	glEnd();

	// tiang kiri dinding 3 lt1
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-401, 110, 110);
	glVertex3f(-401, 460, 110);
	glVertex3f(-401, 460, 140);
	glVertex3f(-401, 110, 140);
	glEnd();

	// tiang kiri dinding 4 lt1
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-401, 110, 15);
	glVertex3f(-401, 460, 15);
	glVertex3f(-401, 460, -15);
	glVertex3f(-401, 110, -15);
	glEnd();

	// tiang kiri dinding 5 lt1
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-401, 110, -110);
	glVertex3f(-401, 460, -110);
	glVertex3f(-401, 460, -140);
	glVertex3f(-401, 110, -140);
	glEnd();

	// tiang kiri dinding 6 lt1
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-401, 110, -235);
	glVertex3f(-401, 460, -235);
	glVertex3f(-401, 460, -265);
	glVertex3f(-401, 110, -265);
	glEnd();

	// tiang kiri dinding 7
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-401, 110, -370);
	glVertex3f(-401, 460, -370);
	glVertex3f(-401, 460, -400);
	glVertex3f(-401, 110, -400);
	glEnd();

	// tiang belakang 1 dinding
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-400, 110, -401);
	glVertex3f(-400, 460, -401);
	glVertex3f(-370, 460, -401);
	glVertex3f(-370, 110, -401);
	glEnd();

	// tiang belakang 2 dinding
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-265, 110, -401);
	glVertex3f(-265, 460, -401);
	glVertex3f(-235, 460, -401);
	glVertex3f(-235, 110, -401);
	glEnd();

	// tiang belakang 3 dinding
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-140, 110, -401);
	glVertex3f(-140, 460, -401);
	glVertex3f(-110, 460, -401);
	glVertex3f(-110, 110, -401);
	glEnd();

	// tiang belakang 4 dinding
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(15, 110, -401);
	glVertex3f(15, 460, -401);
	glVertex3f(-15, 460, -401);
	glVertex3f(-15, 110, -401);
	glEnd();

	// tiang belakang 5 dinding lt1
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(140, 110, -401);
	glVertex3f(140, 460, -401);
	glVertex3f(110, 460, -401);
	glVertex3f(110, 110, -401);
	glEnd();

	// tiang belakang 6 dinding lt1
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(265, 110, -401);
	glVertex3f(265, 460, -401);
	glVertex3f(235, 460, -401);
	glVertex3f(235, 110, -401);
	glEnd();

	// tiang belakang 7 dinding lt1
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(400, 110, -401);
	glVertex3f(400, 460, -401);
	glVertex3f(370, 460, -401);
	glVertex3f(370, 110, -401);
	glEnd();
}

void hiasdinding2() {
	//pintu lt 2
	glBegin(GL_QUADS);
	glColor3f(0.69, 0.69, 0.69);
	glVertex3f(-100, 700, 301);
	glVertex3f(-100, 510, 301);
	glVertex3f(100, 510, 301);
	glVertex3f(100, 700, 301);
	glEnd();

	//tiang lt2 pintu
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-5, 510, 302);
	glVertex3f(-5, 700, 302);
	glVertex3f(5, 700, 302);
	glVertex3f(5, 510, 302);
	glEnd();

	//Hiasan Pintu
	for (int x = 0; x <= 7; x++)
	{
		glBegin(GL_QUADS);
		glColor3f(0.54, 0.06, 0.05);
		glVertex3f(-100, 523 + x * 25, 302);
		glVertex3f(-100, 528 + x * 25, 302);
		glVertex3f(100, 528 + x * 25, 302);
		glVertex3f(100, 523 + x * 25, 302);
		glEnd();
	}

	for (int x = 0; x < 2; x++)
	{
		glBegin(GL_QUADS);
		glColor3f(0.54, 0.06, 0.05);
		glVertex3f(-70 + x * 30, 510, 302);
		glVertex3f(-70 + x * 30, 700, 302);
		glVertex3f(-65 + x * 30, 700, 302);
		glVertex3f(-65 + x * 30, 510, 302);
		glEnd();
	}

	for (int x = 0; x < 2; x++)
	{
		glBegin(GL_QUADS);
		glColor3f(0.54, 0.06, 0.05);
		glVertex3f(45 + x * 30, 510, 302);
		glVertex3f(45 + x * 30, 700, 302);
		glVertex3f(40 + x * 30, 700, 302);
		glVertex3f(40 + x * 30, 510, 302);
		glEnd();
	}

	//tiang pintu Kiri lt2
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-100, 510, 301);
	glVertex3f(-100, 700, 301);
	glVertex3f(-115, 700, 301);
	glVertex3f(-115, 510, 301);
	glEnd();

	//tiang pintu kanan lt2
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(115, 510, 301);
	glVertex3f(115, 700, 301);
	glVertex3f(100, 700, 301);
	glVertex3f(100, 510, 301);
	glEnd();

	//hias dinding depan Kiri lt2
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-300, 575, 301);
	glVertex3f(-300, 605, 301);
	glVertex3f(-115, 605, 301);
	glVertex3f(-115, 575, 301);
	glEnd();

	//hias dinding depan kanan lt2
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(115, 575, 301);
	glVertex3f(115, 605, 301);
	glVertex3f(300, 605, 301);
	glVertex3f(300, 575, 301);
	glEnd();

	//hias dinding depan atas1 lt2
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-300, 700, 301);
	glVertex3f(-300, 730, 301);
	glVertex3f(300, 730, 301);
	glVertex3f(300, 700, 301);
	glEnd();

	//hias dinding depan atas2 lt2
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-300, 745, 301);
	glVertex3f(-300, 775, 301);
	glVertex3f(300, 775, 301);
	glVertex3f(300, 745, 301);
	glEnd();

	//hias dinding depan atas3 lt2
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-300, 797, 301);
	glVertex3f(-300, 792, 301);
	glVertex3f(300, 792, 301);
	glVertex3f(300, 797, 301);
	glEnd();

	//hias dinding kiri lt2
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-301, 575, -300);
	glVertex3f(-301, 605, -300);
	glVertex3f(-301, 605, 300);
	glVertex3f(-301, 575, 300);
	glEnd();

	//hias dinding kiri atas1 lt2
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-301, 700, -300);
	glVertex3f(-301, 730, -300);
	glVertex3f(-301, 730, 300);
	glVertex3f(-301, 700, 300);
	glEnd();

	//hias dinding kiri atas2 lt2
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-301, 745, -300);
	glVertex3f(-301, 775, -300);
	glVertex3f(-301, 775, 300);
	glVertex3f(-301, 745, 300);
	glEnd();

	//hias dinding kiri atas3 lt2
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-301, 797, -300);
	glVertex3f(-301, 795, -300);
	glVertex3f(-301, 795, 300);
	glVertex3f(-301, 797, 300);
	glEnd();

	//hias dinding kanan lt2
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(301, 575, -300);
	glVertex3f(301, 605, -300);
	glVertex3f(301, 605, 300);
	glVertex3f(301, 575, 300);
	glEnd();

	//hias dinding kanan atas1 lt2
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(301, 700, -300);
	glVertex3f(301, 730, -300);
	glVertex3f(301, 730, 300);
	glVertex3f(301, 700, 300);
	glEnd();

	//hias dinding kanan atas2 lt2
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(301, 745, -300);
	glVertex3f(301, 775, -300);
	glVertex3f(301, 775, 300);
	glVertex3f(301, 745, 300);
	glEnd();

	//hias dinding kanan atas3 lt2
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(301, 797, -300);
	glVertex3f(301, 795, -300);
	glVertex3f(301, 795, 300);
	glVertex3f(301, 797, 300);
	glEnd();

	//hias dinding belakang lt2
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-300, 575, -301);
	glVertex3f(-300, 605, -301);
	glVertex3f(300, 605, -301);
	glVertex3f(300, 575, -301);
	glEnd();

	//hias dinding belakang atas1 lt2
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-300, 700, -301);
	glVertex3f(-300, 730, -301);
	glVertex3f(300, 730, -301);
	glVertex3f(300, 700, -301);
	glEnd();

	//hias dinding belakang atas2 lt2
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-300, 745, -301);
	glVertex3f(-300, 775, -301);
	glVertex3f(300, 775, -301);
	glVertex3f(300, 745, -301);
	glEnd();

	//hias dinding belakang atas3 lt2
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-300, 797, -301);
	glVertex3f(-300, 795, -301);
	glVertex3f(300, 795, -301);
	glVertex3f(300, 797, -301);
	glEnd();
}

void hiasdinding3() {
	// pintu lt 3
	glBegin(GL_QUADS);
	glColor3f(0.69, 0.69, 0.69);
	glVertex3f(-100, 910, 251);
	glVertex3f(-100, 1080, 251);
	glVertex3f(100, 1080, 251);
	glVertex3f(100, 910, 251);
	glEnd();

	// tiangtengah lt3 pintu
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-5, 910, 252);
	glVertex3f(-5, 1080, 252);
	glVertex3f(5, 1080, 252);
	glVertex3f(5, 910, 252);
	glEnd();

	//Hiasan Pintu
	for (int x = 0; x <= 6; x++)
	{
		glBegin(GL_QUADS);
		glColor3f(0.54, 0.06, 0.05);
		glVertex3f(-100, 918 + x * 25, 252);
		glVertex3f(-100, 923 + x * 25, 252);
		glVertex3f(100, 923 + x * 25, 252);
		glVertex3f(100, 918 + x * 25, 252);
		glEnd();
	}

	for (int x = 0; x < 2; x++)
	{
		glBegin(GL_QUADS);
		glColor3f(0.54, 0.06, 0.05);
		glVertex3f(-70 + x * 30, 910, 252);
		glVertex3f(-70 + x * 30, 1080, 252);
		glVertex3f(-65 + x * 30, 1080, 252);
		glVertex3f(-65 + x * 30, 910, 252);
		glEnd();
	}

	for (int x = 0; x < 2; x++)
	{
		glBegin(GL_QUADS);
		glColor3f(0.54, 0.06, 0.05);
		glVertex3f(45 + x * 30, 910, 252);
		glVertex3f(45 + x * 30, 1080, 252);
		glVertex3f(40 + x * 30, 1080, 252);
		glVertex3f(40 + x * 30, 910, 252);
		glEnd();
	}

	// tiang dinding pintu kanan lt3
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(115, 1080, 251);
	glVertex3f(115, 910, 251);
	glVertex3f(100, 910, 251);
	glVertex3f(100, 1080, 251);
	glEnd();

	// tiang dinding pintu kiri lt3
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-100, 1080, 251);
	glVertex3f(-100, 910, 251);
	glVertex3f(-115, 910, 251);
	glVertex3f(-115, 1080, 251);
	glEnd();

	// hias dinding depan kiri lt3
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-250, 1000, 251);
	glVertex3f(-250, 975, 251);
	glVertex3f(-115, 975, 251);
	glVertex3f(-115, 1000, 251);
	glEnd();

	// hias dinding depan kanan lt3
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(115, 1000, 251);
	glVertex3f(115, 975, 251);
	glVertex3f(250, 975, 251);
	glVertex3f(250, 1000, 251);
	glEnd();

	// hias dinding depan atas1 lt3
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-250, 1080, 251);
	glVertex3f(-250, 1100, 251);
	glVertex3f(250, 1100, 251);
	glVertex3f(250, 1080, 251);
	glEnd();

	// hias dinding depan atas2 lt3
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-250, 1130, 251);
	glVertex3f(-250, 1110, 251);
	glVertex3f(250, 1110, 251);
	glVertex3f(250, 1130, 251);
	glEnd();

	// hias dinding depan atas3 lt3
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-250, 1144, 251);
	glVertex3f(-250, 1140, 251);
	glVertex3f(250, 1140, 251);
	glVertex3f(250, 1144, 251);
	glEnd();

	// hias dinding belakang lt3
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-250, 1000, -251);
	glVertex3f(-250, 975, -251);
	glVertex3f(250, 975, -251);
	glVertex3f(250, 1000, -251);
	glEnd();

	// hias dinding belakang atas1 lt3
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-250, 1080, -251);
	glVertex3f(-250, 1100, -251);
	glVertex3f(250, 1100, -251);
	glVertex3f(250, 1080, -251);
	glEnd();

	// hias dinding belakang atas2 lt3
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-250, 1130, -251);
	glVertex3f(-250, 1110, -251);
	glVertex3f(250, 1110, -251);
	glVertex3f(250, 1130, -251);
	glEnd();

	// hias dinding belakang atas3 lt3
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-250, 1144, -251);
	glVertex3f(-250, 1140, -251);
	glVertex3f(250, 1140, -251);
	glVertex3f(250, 1144, -251);
	glEnd();

	// hias dinding kiri lt3
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-251, 1000, -250);
	glVertex3f(-251, 975, -250);
	glVertex3f(-251, 975, 250);
	glVertex3f(-251, 1000, 250);
	glEnd();

	// hias dinding kiri atas1 lt3
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-251, 1080, -250);
	glVertex3f(-251, 1100, -250);
	glVertex3f(-251, 1100, 250);
	glVertex3f(-251, 1080, 250);
	glEnd();

	// hias dinding kiri atas2 lt3
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-251, 1130, -250);
	glVertex3f(-251, 1110, -250);
	glVertex3f(-251, 1110, 250);
	glVertex3f(-251, 1130, 250);
	glEnd();

	// hias dinding kiri atas3 lt3
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-251, 1143, -250);
	glVertex3f(-251, 1140, -250);
	glVertex3f(-251, 1140, 250);
	glVertex3f(-251, 1143, 250);
	glEnd();

	// hias dinding kanan lt3
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(251, 1000, -250);
	glVertex3f(251, 975, -250);
	glVertex3f(251, 975, 250);
	glVertex3f(251, 1000, 250);
	glEnd();

	// hias dinding kanan atas1 lt3
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(251, 1080, -250);
	glVertex3f(251, 1100, -250);
	glVertex3f(251, 1100, 250);
	glVertex3f(251, 1080, 250);
	glEnd();

	// hias dinding kanan atas2 lt3
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(251, 1130, -250);
	glVertex3f(251, 1110, -250);
	glVertex3f(251, 1110, 250);
	glVertex3f(251, 1130, 250);
	glEnd();

	// hias dinding kanan atas3 lt3
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(251, 1144, -250);
	glVertex3f(251, 1140, -250);
	glVertex3f(251, 1140, 250);
	glVertex3f(251, 1144, 250);
	glEnd();
}

void hiasdinding4() {
	// pintu lt4
	glBegin(GL_QUADS);
	glColor3f(0.69, 0.69, 0.69);
	glVertex3f(-85, 1260, 201);
	glVertex3f(-85, 1390, 201);
	glVertex3f(85, 1390, 201);
	glVertex3f(85, 1260, 201);
	glEnd();

	// tiangtengah lt4 pintu
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-5, 1260, 202);
	glVertex3f(-5, 1390, 202);
	glVertex3f(5, 1390, 202);
	glVertex3f(5, 1260, 202);
	glEnd();

	//Hiasan Pintu
	for (int x = 0; x <= 5; x++)
	{
		glBegin(GL_QUADS);
		glColor3f(0.54, 0.06, 0.05);
		glVertex3f(-85, 1275 + x * 20, 202);
		glVertex3f(-85, 1270 + x * 20, 202);
		glVertex3f(85, 1270 + x * 20, 202);
		glVertex3f(85, 1275 + x * 20, 202);
		glEnd();
	}

	for (int x = 0; x < 2; x++)
	{
		glBegin(GL_QUADS);
		glColor3f(0.54, 0.06, 0.05);
		glVertex3f(-60 + x * 25, 1260, 202);
		glVertex3f(-60 + x * 25, 1390, 202);
		glVertex3f(-55 + x * 25, 1390, 202);
		glVertex3f(-55 + x * 25, 1260, 202);
		glEnd();
	}

	for (int x = 0; x < 2; x++)
	{
		glBegin(GL_QUADS);
		glColor3f(0.54, 0.06, 0.05);
		glVertex3f(35 + x * 25, 1260, 202);
		glVertex3f(35 + x * 25, 1390, 202);
		glVertex3f(30 + x * 25, 1390, 202);
		glVertex3f(30 + x * 25, 1260, 202);
		glEnd();
	}

	// tiang dinding pintu kanan lt4
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(100, 1390, 201);
	glVertex3f(100, 1260, 201);
	glVertex3f(85, 1260, 201);
	glVertex3f(85, 1390, 201);
	glEnd();

	// tiang dinding pintu kiri lt4
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-85, 1390, 201);
	glVertex3f(-85, 1260, 201);
	glVertex3f(-100, 1260, 201);
	glVertex3f(-100, 1390, 201);
	glEnd();

	// hias dinding depan kiri lt4
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-200, 1330, 201);
	glVertex3f(-200, 1310, 201);
	glVertex3f(-100, 1310, 201);
	glVertex3f(-100, 1330, 201);
	glEnd();

	// hias dinding depan kanan lt4
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(100, 1330, 201);
	glVertex3f(100, 1310, 201);
	glVertex3f(200, 1310, 201);
	glVertex3f(200, 1330, 201);
	glEnd();

	// hias dinding depan atas1 lt4
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-200, 1405, 201);
	glVertex3f(-200, 1390, 201);
	glVertex3f(200, 1390, 201);
	glVertex3f(200, 1405, 201);
	glEnd();

	// hias dinding depan atas2 lt4
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-200, 1430, 201);
	glVertex3f(-200, 1415, 201);
	glVertex3f(200, 1415, 201);
	glVertex3f(200, 1430, 201);
	glEnd();

	// hias dinding depan atas3 lt4
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-200, 1444, 201);
	glVertex3f(-200, 1440, 201);
	glVertex3f(200, 1440, 201);
	glVertex3f(200, 1444, 201);
	glEnd();

	// hias dinding belakang lt4
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-200, 1330, -201);
	glVertex3f(-200, 1310, -201);
	glVertex3f(200, 1310, -201);
	glVertex3f(200, 1330, -201);
	glEnd();

	// hias dinding belakang atas1 lt4
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-200, 1405, -201);
	glVertex3f(-200, 1390, -201);
	glVertex3f(200, 1390, -201);
	glVertex3f(200, 1405, -201);
	glEnd();

	// hias dinding belakang atas2 lt4
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-200, 1430, -201);
	glVertex3f(-200, 1415, -201);
	glVertex3f(200, 1415, -201);
	glVertex3f(200, 1430, -201);
	glEnd();

	// hias dinding belakang atas3 lt4
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-200, 1444, -201);
	glVertex3f(-200, 1440, -201);
	glVertex3f(200, 1440, -201);
	glVertex3f(200, 1444, -201);
	glEnd();

	// hias dinding kiri lt4
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-201, 1330, -200);
	glVertex3f(-201, 1310, -200);
	glVertex3f(-201, 1310, 200);
	glVertex3f(-201, 1330, 200);
	glEnd();

	// hias dinding kiri atas1 lt4
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-201, 1405, -200);
	glVertex3f(-201, 1390, -200);
	glVertex3f(-201, 1390, 200);
	glVertex3f(-201, 1405, 200);
	glEnd();

	// hias dinding kiri atas2 lt4
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-201, 1430, -200);
	glVertex3f(-201, 1415, -200);
	glVertex3f(-201, 1415, 200);
	glVertex3f(-201, 1430, 200);
	glEnd();

	// hias dinding kiri atas3 lt4
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-201, 1444, -200);
	glVertex3f(-201, 1440, -200);
	glVertex3f(-201, 1440, 200);
	glVertex3f(-201, 1444, 200);
	glEnd();

	// hias dinding kanan lt4
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(201, 1330, -200);
	glVertex3f(201, 1310, -200);
	glVertex3f(201, 1310, 200);
	glVertex3f(201, 1330, 200);
	glEnd();

	// hias dinding kanan atas1 lt4
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(201, 1405, -200);
	glVertex3f(201, 1390, -200);
	glVertex3f(201, 1390, 200);
	glVertex3f(201, 1405, 200);
	glEnd();

	// hias dinding kanan atas2 lt4
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(201, 1430, -200);
	glVertex3f(201, 1415, -200);
	glVertex3f(201, 1415, 200);
	glVertex3f(201, 1430, 200);
	glEnd();

	// hias dinding kanan atas2 lt4
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(201, 1444, -200);
	glVertex3f(201, 1440, -200);
	glVertex3f(201, 1440, 200);
	glVertex3f(201, 1444, 200);
	glEnd();
}

void TiangGapura() {
	//Kiri
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-250, 600, 800);
	glVertex3f(-225, 600, 800);
	glVertex3f(-225, 600, 825);
	glVertex3f(-250, 600, 825);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-225, 35, 825);
	glVertex3f(-225, 600, 825);
	glVertex3f(-250, 600, 825);
	glVertex3f(-250, 35, 825);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-225, 35, 800);
	glVertex3f(-225, 600, 800);
	glVertex3f(-250, 600, 800);
	glVertex3f(-250, 35, 800);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-225, 35, 800);
	glVertex3f(-225, 600, 800);
	glVertex3f(-225, 600, 825);
	glVertex3f(-225, 35, 825);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-250, 35, 800);
	glVertex3f(-250, 600, 800);
	glVertex3f(-250, 600, 825);
	glVertex3f(-250, 35, 825);
	glEnd();

	//Kanan
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(250, 600, 800);
	glVertex3f(225, 600, 800);
	glVertex3f(225, 600, 825);
	glVertex3f(250, 600, 825);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(225, 35, 825);
	glVertex3f(225, 600, 825);
	glVertex3f(250, 600, 825);
	glVertex3f(250, 35, 825);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(225, 35, 800);
	glVertex3f(225, 600, 800);
	glVertex3f(250, 600, 800);
	glVertex3f(250, 35, 800);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(225, 35, 800);
	glVertex3f(225, 600, 800);
	glVertex3f(225, 600, 825);
	glVertex3f(225, 35, 825);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(250, 35, 800);
	glVertex3f(250, 600, 800);
	glVertex3f(250, 600, 825);
	glVertex3f(250, 35, 825);
	glEnd();

	//Tengah
	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-15, 475, 825);
	glVertex3f(-15, 575, 825);
	glVertex3f(15, 575, 825);
	glVertex3f(15, 475, 825);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-15, 475, 800);
	glVertex3f(-15, 575, 800);
	glVertex3f(15, 575, 800);
	glVertex3f(15, 475, 800);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(15, 475, 800);
	glVertex3f(15, 575, 800);
	glVertex3f(15, 575, 825);
	glVertex3f(15, 475, 825);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-15, 475, 800);
	glVertex3f(-15, 575, 800);
	glVertex3f(-15, 575, 825);
	glVertex3f(-15, 475, 825);
	glEnd();

	//TiangAtas1
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-300, 475, 800);
	glVertex3f(300, 475, 800);
	glVertex3f(300, 475, 825);
	glVertex3f(-300, 475, 825);
	glEnd();

	//Bawah
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-275, 450, 825);
	glVertex3f(275, 450, 825);
	glVertex3f(275, 450, 825);
	glVertex3f(-275, 450, 825);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-275, 450, 825);
	glVertex3f(-300, 475, 825);
	glVertex3f(300, 475, 825);
	glVertex3f(275, 450, 825);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-275, 450, 800);
	glVertex3f(-300, 475, 800);
	glVertex3f(300, 475, 800);
	glVertex3f(275, 450, 800);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(275, 450, 800);
	glVertex3f(300, 475, 800);
	glVertex3f(300, 475, 825);
	glVertex3f(275, 450, 825);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-275, 450, 800);
	glVertex3f(-300, 475, 800);
	glVertex3f(-300, 475, 825);
	glVertex3f(-275, 450, 825);
	glEnd();

	//TiangAtas2
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-375, 600, 800);
	glVertex3f(375, 600, 800);
	glVertex3f(375, 600, 825);
	glVertex3f(-375, 600, 825);
	glEnd();

	//Bawah
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-350, 575, 825);
	glVertex3f(350, 575, 825);
	glVertex3f(350, 575, 825);
	glVertex3f(-350, 575, 825);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-350, 575, 825);
	glVertex3f(-375, 600, 825);
	glVertex3f(375, 600, 825);
	glVertex3f(350, 575, 825);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-350, 575, 800);
	glVertex3f(-375, 600, 800);
	glVertex3f(375, 600, 800);
	glVertex3f(350, 575, 800);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(350, 575, 800);
	glVertex3f(375, 600, 800);
	glVertex3f(375, 600, 825);
	glVertex3f(350, 575, 825);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.54, 0.06, 0.05);
	glVertex3f(-350, 575, 800);
	glVertex3f(-375, 600, 800);
	glVertex3f(-375, 600, 825);
	glVertex3f(-350, 575, 825);
	glEnd();
}

void TiangKuil() {
	//depanKiri
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-250, 500, 490);
	glVertex3f(-225, 500, 490);
	glVertex3f(-225, 505, 515);
	glVertex3f(-250, 505, 515);
	glEnd();

	//depan
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-225, 35, 515);
	glVertex3f(-225, 505, 515);
	glVertex3f(-250, 505, 515);
	glVertex3f(-250, 35, 515);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-225, 35, 500);
	glVertex3f(-225, 500, 500);
	glVertex3f(-250, 500, 500);
	glVertex3f(-250, 35, 500);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(-225, 35, 500);
	glVertex3f(-225, 505, 500);
	glVertex3f(-225, 505, 515);
	glVertex3f(-225, 35, 515);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(-250, 35, 500);
	glVertex3f(-250, 505, 500);
	glVertex3f(-250, 505, 515);
	glVertex3f(-250, 35, 515);
	glEnd();

	//depanKanan
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(250, 500, 490);
	glVertex3f(225, 500, 490);
	glVertex3f(225, 505, 515);
	glVertex3f(250, 505, 515);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(225, 35, 515);
	glVertex3f(225, 505, 515);
	glVertex3f(250, 505, 515);
	glVertex3f(250, 35, 515);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(225, 35, 500);
	glVertex3f(225, 500, 500);
	glVertex3f(250, 500, 500);
	glVertex3f(250, 35, 500);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(225, 35, 500);
	glVertex3f(225, 505, 500);
	glVertex3f(225, 505, 515);
	glVertex3f(225, 35, 515);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(250, 35, 500);
	glVertex3f(250, 505, 500);
	glVertex3f(250, 505, 515);
	glVertex3f(250, 35, 515);
	glEnd();

	//Kanankuil
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(450, 500, 10);
	glVertex3f(425, 500, 10);
	glVertex3f(425, 505, 10);
	glVertex3f(450, 505, 10);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(425, 35, 10);
	glVertex3f(425, 505, 10);
	glVertex3f(450, 505, 10);
	glVertex3f(450, 35, 10);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(425, 35, -15);
	glVertex3f(425, 505, -15);
	glVertex3f(450, 505, -15);
	glVertex3f(450, 35, -15);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(425, 35, 10);
	glVertex3f(425, 505, 10);
	glVertex3f(425, 505, -15);
	glVertex3f(425, 35, -15);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(450, 35, 10);
	glVertex3f(450, 505, 10);
	glVertex3f(450, 505, -15);
	glVertex3f(450, 35, -15);
	glEnd();

	//kirikuil
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-450, 500, 10);
	glVertex3f(-425, 500, 10);
	glVertex3f(-425, 505, 10);
	glVertex3f(-450, 505, 10);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-425, 35, 10);
	glVertex3f(-425, 505, 10);
	glVertex3f(-450, 505, 10);
	glVertex3f(-450, 35, 10);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-425, 35, -15);
	glVertex3f(-425, 505, -15);
	glVertex3f(-450, 505, -15);
	glVertex3f(-450, 35, -15);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(-425, 35, 10);
	glVertex3f(-425, 505, 10);
	glVertex3f(-425, 505, -15);
	glVertex3f(-425, 35, -15);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(-450, 35, 10);
	glVertex3f(-450, 505, 10);
	glVertex3f(-450, 505, -15);
	glVertex3f(-450, 35, -15);
	glEnd();

	//belakangKiri
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-250, 500, -490);
	glVertex3f(-225, 500, -490);
	glVertex3f(-225, 505, -515);
	glVertex3f(-250, 505, -515);
	glEnd();

	//depan
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-225, 35, -515);
	glVertex3f(-225, 505, -515);
	glVertex3f(-250, 505, -515);
	glVertex3f(-250, 35, -515);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-225, 35, -500);
	glVertex3f(-225, 500, -500);
	glVertex3f(-250, 500, -500);
	glVertex3f(-250, 35, -500);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(-225, 35, -500);
	glVertex3f(-225, 505, -500);
	glVertex3f(-225, 505, -515);
	glVertex3f(-225, 35, -515);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(-250, 35, -500);
	glVertex3f(-250, 505, -500);
	glVertex3f(-250, 505, -515);
	glVertex3f(-250, 35, -515);
	glEnd();

	//belakangKanan
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(250, 500, -490);
	glVertex3f(225, 500, -490);
	glVertex3f(225, 505, -515);
	glVertex3f(250, 505, -515);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(225, 35, -515);
	glVertex3f(225, 505, -515);
	glVertex3f(250, 505, -515);
	glVertex3f(250, 35, -515);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(225, 35, -500);
	glVertex3f(225, 500, -500);
	glVertex3f(250, 500, -500);
	glVertex3f(250, 35, -500);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(225, 35, -500);
	glVertex3f(225, 505, -500);
	glVertex3f(225, 505, -515);
	glVertex3f(225, 35, -515);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(250, 35, -500);
	glVertex3f(250, 505, -500);
	glVertex3f(250, 505, -515);
	glVertex3f(250, 35, -515);
	glEnd();


	//lantai2depanKiri
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-250, 805, 500);
	glVertex3f(-225, 805, 500);
	glVertex3f(-225, 810, 515);
	glVertex3f(-250, 810, 515);
	glEnd();

	//depan
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-225, 510, 515);
	glVertex3f(-225, 810, 515);
	glVertex3f(-250, 810, 515);
	glVertex3f(-250, 510, 515);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-225, 510, 500);
	glVertex3f(-225, 810, 500);
	glVertex3f(-250, 810, 500);
	glVertex3f(-250, 510, 500);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(-225, 510, 500);
	glVertex3f(-225, 810, 500);
	glVertex3f(-225, 810, 515);
	glVertex3f(-225, 510, 515);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(-250, 510, 500);
	glVertex3f(-250, 810, 500);
	glVertex3f(-250, 810, 515);
	glVertex3f(-250, 510, 515);
	glEnd();

	//lantai2depanKanan
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(250, 810, 500);
	glVertex3f(225, 810, 500);
	glVertex3f(225, 815, 515);
	glVertex3f(250, 815, 515);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(225, 510, 515);
	glVertex3f(225, 810, 515);
	glVertex3f(250, 810, 515);
	glVertex3f(250, 510, 515);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(225, 510, 500);
	glVertex3f(225, 810, 500);
	glVertex3f(250, 810, 500);
	glVertex3f(250, 510, 500);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(225, 510, 500);
	glVertex3f(225, 810, 500);
	glVertex3f(225, 810, 515);
	glVertex3f(225, 510, 515);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(250, 510, 500);
	glVertex3f(250, 810, 500);
	glVertex3f(250, 810, 515);
	glVertex3f(250, 510, 515);
	glEnd();

	/////////
	//Kanankuillantai2
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(450, 810, 10);
	glVertex3f(425, 810, 10);
	glVertex3f(425, 815, 10);
	glVertex3f(450, 815, 10);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(425, 510, 10);
	glVertex3f(425, 810, 10);
	glVertex3f(450, 810, 10);
	glVertex3f(450, 510, 10);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(425, 510, -15);
	glVertex3f(425, 810, -15);
	glVertex3f(450, 810, -15);
	glVertex3f(450, 510, -15);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(425, 510, 10);
	glVertex3f(425, 810, 10);
	glVertex3f(425, 810, -15);
	glVertex3f(425, 510, -15);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(450, 510, 10);
	glVertex3f(450, 810, 10);
	glVertex3f(450, 810, -15);
	glVertex3f(450, 510, -15);
	glEnd();

	//kirikuillantai2
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-450, 810, 10);
	glVertex3f(-425, 810, 10);
	glVertex3f(-425, 815, 10);
	glVertex3f(-450, 815, 10);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-425, 510, 10);
	glVertex3f(-425, 810, 10);
	glVertex3f(-450, 810, 10);
	glVertex3f(-450, 510, 10);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-425, 510, -15);
	glVertex3f(-425, 810, -15);
	glVertex3f(-450, 810, -15);
	glVertex3f(-450, 510, -15);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(-425, 510, 10);
	glVertex3f(-425, 810, 10);
	glVertex3f(-425, 810, -15);
	glVertex3f(-425, 510, -15);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(-450, 510, 10);
	glVertex3f(-450, 810, 10);
	glVertex3f(-450, 810, -15);
	glVertex3f(-450, 510, -15);
	glEnd();

	//belakangKiri
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-250, 810, -490);
	glVertex3f(-225, 810, -490);
	glVertex3f(-225, 815, -515);
	glVertex3f(-250, 815, -515);
	glEnd();

	//depan
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-225, 510, -515);
	glVertex3f(-225, 810, -515);
	glVertex3f(-250, 810, -515);
	glVertex3f(-250, 510, -515);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-225, 510, -500);
	glVertex3f(-225, 810, -500);
	glVertex3f(-250, 810, -500);
	glVertex3f(-250, 510, -500);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(-225, 510, -500);
	glVertex3f(-225, 810, -500);
	glVertex3f(-225, 810, -515);
	glVertex3f(-225, 510, -515);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(-250, 510, -500);
	glVertex3f(-250, 810, -500);
	glVertex3f(-250, 810, -515);
	glVertex3f(-250, 510, -515);
	glEnd();

	//belakangKanan
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(250, 810, -490);
	glVertex3f(225, 810, -490);
	glVertex3f(225, 815, -515);
	glVertex3f(250, 815, -515);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(225, 510, -515);
	glVertex3f(225, 810, -515);
	glVertex3f(250, 810, -515);
	glVertex3f(250, 510, -515);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(225, 510, -500);
	glVertex3f(225, 810, -500);
	glVertex3f(250, 810, -500);
	glVertex3f(250, 510, -500);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(225, 510, -500);
	glVertex3f(225, 810, -500);
	glVertex3f(225, 810, -515);
	glVertex3f(225, 510, -515);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(250, 510, -500);
	glVertex3f(250, 810, -500);
	glVertex3f(250, 810, -515);
	glVertex3f(250, 510, -515);
	glEnd();


	//lantai2depanKiri
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-250, 805, 500);
	glVertex3f(-225, 805, 500);
	glVertex3f(-225, 810, 515);
	glVertex3f(-250, 810, 515);
	glEnd();

	//depan
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-225, 510, 515);
	glVertex3f(-225, 810, 515);
	glVertex3f(-250, 810, 515);
	glVertex3f(-250, 510, 515);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-225, 510, 500);
	glVertex3f(-225, 810, 500);
	glVertex3f(-250, 810, 500);
	glVertex3f(-250, 510, 500);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(-225, 510, 500);
	glVertex3f(-225, 810, 500);
	glVertex3f(-225, 810, 515);
	glVertex3f(-225, 510, 515);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(-250, 510, 500);
	glVertex3f(-250, 810, 500);
	glVertex3f(-250, 810, 515);
	glVertex3f(-250, 510, 515);
	glEnd();


	//Lantai 3
	//lantai3depanKiri
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-250, 805, 500);
	glVertex3f(-225, 805, 500);
	glVertex3f(-225, 810, 515);
	glVertex3f(-250, 810, 515);
	glEnd();

	//depan
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-225, 905, 410);
	glVertex3f(-225, 1160, 410);
	glVertex3f(-250, 1160, 410);
	glVertex3f(-250, 905, 410);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-225, 905, 390);
	glVertex3f(-225, 1160, 390);
	glVertex3f(-250, 1160, 390);
	glVertex3f(-250, 905, 390);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(-225, 905, 390);
	glVertex3f(-225, 1160, 390);
	glVertex3f(-225, 1160, 410);
	glVertex3f(-225, 905, 410);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(-250, 905, 390);
	glVertex3f(-250, 1160, 390);
	glVertex3f(-250, 1160, 410);
	glVertex3f(-250, 905, 410);
	glEnd();

	//lantai3depanKanan
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(250, 1150, 390);
	glVertex3f(225, 1150, 390);
	glVertex3f(225, 1160, 410);
	glVertex3f(250, 1160, 410);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(225, 905, 410);
	glVertex3f(225, 1160, 410);
	glVertex3f(250, 1160, 410);
	glVertex3f(250, 905, 410);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(225, 905, 390);
	glVertex3f(225, 1160, 390);
	glVertex3f(250, 1160, 390);
	glVertex3f(250, 905, 390);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(225, 905, 390);
	glVertex3f(225, 1160, 390);
	glVertex3f(225, 1160, 410);
	glVertex3f(225, 905, 410);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(250, 905, 390);
	glVertex3f(250, 1160, 390);
	glVertex3f(250, 1160, 410);
	glVertex3f(250, 905, 410);
	glEnd();

	//Kanankuillantai3
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(325, 905, 10);
	glVertex3f(325, 1160, 10);
	glVertex3f(350, 1160, 10);
	glVertex3f(350, 905, 10);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(325, 905, 10);
	glVertex3f(325, 1160, 10);
	glVertex3f(350, 1160, 10);
	glVertex3f(350, 905, 10);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(325, 905, -15);
	glVertex3f(325, 1160, -15);
	glVertex3f(350, 1160, -15);
	glVertex3f(350, 905, -15);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(325, 905, 10);
	glVertex3f(325, 1160, 10);
	glVertex3f(325, 1160, -15);
	glVertex3f(325, 905, -15);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(350, 905, 10);
	glVertex3f(350, 1160, 10);
	glVertex3f(350, 1160, -15);
	glVertex3f(350, 905, -15);
	glEnd();

	//kirikuillantai3
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-350, 905, 10);
	glVertex3f(-325, 1160, 10);
	glVertex3f(-325, 1160, 10);
	glVertex3f(-350, 905, 10);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-325, 905, 10);
	glVertex3f(-325, 1160, 10);
	glVertex3f(-350, 1160, 10);
	glVertex3f(-350, 905, 10);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-325, 905, -15);
	glVertex3f(-325, 1160, -15);
	glVertex3f(-350, 1160, -15);
	glVertex3f(-350, 905, -15);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(-325, 905, 10);
	glVertex3f(-325, 1160, 10);
	glVertex3f(-325, 1160, -15);
	glVertex3f(-325, 905, -15);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(-350, 905, 10);
	glVertex3f(-350, 1160, 10);
	glVertex3f(-350, 1160, -15);
	glVertex3f(-350, 905, -15);
	glEnd();

	//belakangKiri
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-250, 805, -500);
	glVertex3f(-225, 805, -500);
	glVertex3f(-225, 810, -515);
	glVertex3f(-250, 810, -515);
	glEnd();

	//depan
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-225, 905, -410);
	glVertex3f(-225, 1160, -410);
	glVertex3f(-250, 1160, -410);
	glVertex3f(-250, 905, -410);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-225, 905, -390);
	glVertex3f(-225, 1160, -390);
	glVertex3f(-250, 1160, -390);
	glVertex3f(-250, 905, -390);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(-225, 905, -390);
	glVertex3f(-225, 1160, -390);
	glVertex3f(-225, 1160, -410);
	glVertex3f(-225, 905, -410);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(-250, 905, -390);
	glVertex3f(-250, 1160, -390);
	glVertex3f(-250, 1160, -410);
	glVertex3f(-250, 905, -410);
	glEnd();

	//belakangKanan
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(250, 1150, -390);
	glVertex3f(225, 1150, -390);
	glVertex3f(225, 1160, -410);
	glVertex3f(250, 1160, -410);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(225, 905, -410);
	glVertex3f(225, 1160, -410);
	glVertex3f(250, 1160, -410);
	glVertex3f(250, 905, -410);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(225, 905, -390);
	glVertex3f(225, 1160, -390);
	glVertex3f(250, 1160, -390);
	glVertex3f(250, 905, -390);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(225, 905, -390);
	glVertex3f(225, 1160, -390);
	glVertex3f(225, 1160, -410);
	glVertex3f(225, 905, -410);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(250, 905, -390);
	glVertex3f(250, 1160, -390);
	glVertex3f(250, 1160, -410);
	glVertex3f(250, 905, -410);
	glEnd();


	//lantai4
	//lantai4depanKiri
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-250, 1250, 300);
	glVertex3f(-225, 1500, 300);
	glVertex3f(-225, 1500, 315);
	glVertex3f(-250, 1250, 315);
	glEnd();

	//depan
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-225, 1250, 315);
	glVertex3f(-225, 1500, 315);
	glVertex3f(-250, 1500, 315);
	glVertex3f(-250, 1250, 315);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-225, 1250, 300);
	glVertex3f(-225, 1500, 300);
	glVertex3f(-250, 1500, 300);
	glVertex3f(-250, 1250, 300);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(-225, 1250, 300);
	glVertex3f(-225, 1500, 300);
	glVertex3f(-225, 1500, 315);
	glVertex3f(-225, 1250, 315);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(-250, 1250, 300);
	glVertex3f(-250, 1500, 300);
	glVertex3f(-250, 1500, 315);
	glVertex3f(-250, 1250, 315);
	glEnd();

	//lantai4depanKanan
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(250, 1250, 300);
	glVertex3f(225, 1500, 300);
	glVertex3f(225, 1500, 315);
	glVertex3f(250, 1250, 315);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(225, 1250, 315);
	glVertex3f(225, 1500, 315);
	glVertex3f(250, 1500, 315);
	glVertex3f(250, 1250, 315);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(225, 1250, 300);
	glVertex3f(225, 1500, 300);
	glVertex3f(250, 1500, 300);
	glVertex3f(250, 1250, 300);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(225, 1250, 300);
	glVertex3f(225, 1500, 300);
	glVertex3f(225, 1500, 315);
	glVertex3f(225, 1250, 315);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(250, 1250, 300);
	glVertex3f(250, 1500, 300);
	glVertex3f(250, 1500, 315);
	glVertex3f(250, 1250, 315);
	glEnd();

	//Kanankuillantai4
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(300, 1250, 10);
	glVertex3f(300, 1500, 10);
	glVertex3f(325, 1500, 10);
	glVertex3f(325, 1250, 10);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(300, 1250, 10);
	glVertex3f(300, 1500, 10);
	glVertex3f(325, 1500, 10);
	glVertex3f(325, 1250, 10);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(300, 1250, -15);
	glVertex3f(300, 1500, -15);
	glVertex3f(325, 1500, -15);
	glVertex3f(325, 1250, -15);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(300, 1250, 10);
	glVertex3f(300, 1500, 10);
	glVertex3f(300, 1500, -15);
	glVertex3f(300, 1250, -15);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(325, 1250, 10);
	glVertex3f(325, 1500, 10);
	glVertex3f(325, 1500, -15);
	glVertex3f(325, 1250, -15);
	glEnd();

	//kirikuillantai3
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-325, 1250, 10);
	glVertex3f(-300, 1500, 10);
	glVertex3f(-300, 1500, 10);
	glVertex3f(-325, 1250, 10);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-300, 1250, 10);
	glVertex3f(-300, 1500, 10);
	glVertex3f(-325, 1500, 10);
	glVertex3f(-325, 1250, 10);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-300, 1250, -15);
	glVertex3f(-300, 1500, -15);
	glVertex3f(-325, 1500, -15);
	glVertex3f(-325, 1250, -15);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(-300, 1250, 10);
	glVertex3f(-300, 1500, 10);
	glVertex3f(-300, 1500, -15);
	glVertex3f(-300, 1250, -15);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(-325, 1250, 10);
	glVertex3f(-325, 1500, 10);
	glVertex3f(-325, 1500, -15);
	glVertex3f(-325, 1250, -15);
	glEnd();

	//kiribelakang
	//depan
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-225, 1250, -315);
	glVertex3f(-225, 1500, -315);
	glVertex3f(-250, 1500, -315);
	glVertex3f(-250, 1250, -315);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(-225, 1250, -300);
	glVertex3f(-225, 1500, -300);
	glVertex3f(-250, 1500, -300);
	glVertex3f(-250, 1250, -300);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(-225, 1250, -300);
	glVertex3f(-225, 1500, -300);
	glVertex3f(-225, 1500, -315);
	glVertex3f(-225, 1250, -315);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(-250, 1250, -300);
	glVertex3f(-250, 1500, -300);
	glVertex3f(-250, 1500, -315);
	glVertex3f(-250, 1250, -315);
	glEnd();

	//lantai4belakangKanan
	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(250, 1250, -300);
	glVertex3f(225, 1500, -300);
	glVertex3f(225, 1500, -315);
	glVertex3f(250, 1250, -315);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(225, 1250, -315);
	glVertex3f(225, 1500, -315);
	glVertex3f(250, 1500, -315);
	glVertex3f(250, 1250, -315);
	glEnd();

	//Belakang
	glBegin(GL_QUADS);
	glColor3f(0.51, 0.05, 0.05);
	glVertex3f(225, 1250, -300);
	glVertex3f(225, 1500, -300);
	glVertex3f(250, 1500, -300);
	glVertex3f(250, 1250, -300);
	glEnd();

	//Kanan
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(225, 1250, -300);
	glVertex3f(225, 1500, -300);
	glVertex3f(225, 1500, -315);
	glVertex3f(225, 1250, -315);
	glEnd();

	//Kiri
	glBegin(GL_QUADS);
	glColor3f(0.57, 0.05, 0.05);
	glVertex3f(250, 1250, -300);
	glVertex3f(250, 1500, -300);
	glVertex3f(250, 1500, -315);
	glVertex3f(250, 1250, -315);
	glEnd();

}

void object() {

	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(-600, 60, 850);
	glVertex3f(-640, 60, 850);
	glVertex3f(-640, 60, 750);
	glVertex3f(-600, 60, 750);
	glVertex3f(-640, 60, 850);
	glVertex3f(-670, 60, 800);
	glVertex3f(-600, 60, 750);
	glVertex3f(-570, 60, 800);
	glVertex3f(-670, 60, 800);
	glVertex3f(-640, 60, 750);
	glVertex3f(-570, 60, 800);
	glVertex3f(-600, 60, 850);
	glEnd();

	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(-610, 75, 840);
	glVertex3f(-635, 75, 840);
	glVertex3f(-635, 75, 760);
	glVertex3f(-610, 75, 760);
	glVertex3f(-660, 75, 800);
	glVertex3f(-635, 75, 760);
	glVertex3f(-585, 75, 805);
	glVertex3f(-610, 75, 840);
	glVertex3f(-635, 75, 840);
	glVertex3f(-660, 75, 800);
	glVertex3f(-610, 75, 760);
	glVertex3f(-585, 75, 805);
	glEnd();

	//atas
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(-610, 135, 825);
	glVertex3f(-635, 135, 825);
	glVertex3f(-635, 135, 790);
	glVertex3f(-610, 135, 790);
	glVertex3f(-635, 135, 825);
	glVertex3f(-645, 135, 805);
	glVertex3f(-610, 135, 790);
	glVertex3f(-600, 135, 805);
	glVertex3f(-645, 135, 805);
	glVertex3f(-635, 135, 790);
	glVertex3f(-600, 135, 805);
	glVertex3f(-610, 135, 825);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(-600, 35, 850);
	glVertex3f(-600, 60, 850);
	glVertex3f(-640, 60, 850);
	glVertex3f(-640, 35, 850);
	glEnd();

	//Depan1
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(-610, 60, 840);
	glVertex3f(-610, 75, 840);
	glVertex3f(-635, 75, 840);
	glVertex3f(-635, 60, 840);
	glEnd();

	//Depan2
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(-610, 120, 825);
	glVertex3f(-610, 135, 825);
	glVertex3f(-635, 135, 825);
	glVertex3f(-635, 120, 825);
	glEnd();


	//belakang
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(-600, 35, 750);
	glVertex3f(-600, 60, 750);
	glVertex3f(-640, 60, 750);
	glVertex3f(-640, 35, 750);
	glEnd();

	//belakang2
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(-610, 60, 760);
	glVertex3f(-610, 75, 760);
	glVertex3f(-635, 75, 760);
	glVertex3f(-635, 60, 760);
	glEnd();
	//belakang3
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(-610, 120, 790);
	glVertex3f(-610, 135, 790);
	glVertex3f(-635, 135, 790);
	glVertex3f(-635, 120, 790);
	glEnd();


	//kiri1
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(-640, 35, 850);
	glVertex3f(-640, 60, 850);
	glVertex3f(-670, 60, 800);
	glVertex3f(-670, 35, 800);
	glEnd();
	//kiri1
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(-635, 60, 840);
	glVertex3f(-635, 75, 840);
	glVertex3f(-660, 75, 800);
	glVertex3f(-660, 60, 800);
	glEnd();
	//kiri1
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(-635, 120, 825);
	glVertex3f(-635, 135, 825);
	glVertex3f(-645, 135, 805);
	glVertex3f(-645, 120, 805);
	glEnd();


	//kiri2
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(-670, 35, 800);
	glVertex3f(-670, 60, 800);
	glVertex3f(-640, 60, 750);
	glVertex3f(-640, 35, 750);
	glEnd();

	//kiri2
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(-645, 60, 800);
	glVertex3f(-645, 75, 800);
	glVertex3f(-635, 75, 760);
	glVertex3f(-635, 60, 760);
	glEnd();

	//kiri2
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(-645, 120, 805);
	glVertex3f(-645, 135, 805);
	glVertex3f(-635, 135, 790);
	glVertex3f(-635, 120, 790);
	glEnd();

	//kanan1
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(-600, 35, 750);
	glVertex3f(-600, 60, 750);
	glVertex3f(-570, 60, 800);
	glVertex3f(-570, 35, 800);
	glEnd();

	//kanan1
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(-610, 60, 760);
	glVertex3f(-610, 75, 760);
	glVertex3f(-585, 75, 805);
	glVertex3f(-585, 60, 805);
	glEnd();

	//kanan1
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(-610, 120, 790);
	glVertex3f(-610, 135, 790);
	glVertex3f(-600, 135, 805);
	glVertex3f(-600, 120, 805);
	glEnd();


	//kanan2
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(-570, 35, 800);
	glVertex3f(-570, 60, 800);
	glVertex3f(-600, 60, 850);
	glVertex3f(-600, 35, 850);
	glEnd();

	//kanan2
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(-585, 60, 805);
	glVertex3f(-585, 75, 805);
	glVertex3f(-610, 75, 840);
	glVertex3f(-610, 60, 840);
	glEnd();

	//kanan2
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(-600, 120, 805);
	glVertex3f(-600, 135, 805);
	glVertex3f(-610, 135, 825);
	glVertex3f(-610, 120, 825);
	glEnd();

	//tiang
	//atas
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(-615, 120, 800);
	glVertex3f(-630, 120, 800);
	glVertex3f(-630, 120, 815);
	glVertex3f(-615, 120, 815);
	glEnd();
	//depan
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(-615, 75, 815);
	glVertex3f(-615, 120, 815);
	glVertex3f(-630, 120, 815);
	glVertex3f(-630, 75, 815);
	glEnd();
	//belakang
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(-630, 75, 800);
	glVertex3f(-630, 120, 800);
	glVertex3f(-615, 120, 800);
	glVertex3f(-615, 75, 800);
	glEnd();
	//kiri
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(-630, 75, 815);
	glVertex3f(-630, 120, 815);
	glVertex3f(-630, 120, 800);
	glVertex3f(-630, 75, 800);
	glEnd();
	//kanan
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(-615, 75, 815);
	glVertex3f(-615, 120, 815);
	glVertex3f(-615, 120, 800);
	glVertex3f(-615, 75, 800);
	glEnd();

	//tiang2
	//atas
	glBegin(GL_QUADS);
	glColor3f(1.0, 1.0, 1.0);
	glVertex3f(-630, 155, 820);
	glVertex3f(-615, 155, 820);
	glVertex3f(-630, 155, 800);
	glVertex3f(-615, 155, 800);
	glVertex3f(-630, 155, 820);
	glVertex3f(-635, 155, 810);
	glVertex3f(-615, 155, 800);
	glVertex3f(-610, 155, 810);//
	glVertex3f(-635, 155, 810);
	glVertex3f(-630, 155, 800);
	glVertex3f(-610, 155, 810);
	glVertex3f(-615, 155, 820);
	glEnd();
	//Depan2
	glBegin(GL_QUADS);
	glColor3f(1.0, 1.0, 1.0);
	glVertex3f(-630, 135, 820);
	glVertex3f(-630, 155, 820);
	glVertex3f(-615, 155, 820);
	glVertex3f(-615, 135, 820);
	glEnd();

	//bawah
	glBegin(GL_QUADS);
	glColor3f(1.0, 1.0, 1.0);
	glVertex3f(-630, 135, 800);
	glVertex3f(-630, 155, 800);
	glVertex3f(-615, 155, 800);
	glVertex3f(-615, 135, 800);
	glEnd();

	//kiri1
	glBegin(GL_QUADS);
	glColor3f(1.0, 1.0, 1.0);
	glVertex3f(-630, 135, 820);
	glVertex3f(-630, 155, 820);
	glVertex3f(-635, 155, 810);
	glVertex3f(-635, 135, 810);
	glEnd();

	//kiri2
	glBegin(GL_QUADS);
	glColor3f(1.0, 1.0, 1.0);
	glVertex3f(-635, 135, 810);
	glVertex3f(-635, 155, 810);
	glVertex3f(-630, 155, 800);
	glVertex3f(-630, 135, 800);
	glEnd();

	//kanan1
	glBegin(GL_QUADS);
	glColor3f(1.0, 1.0, 1.0);
	glVertex3f(-615, 135, 800);
	glVertex3f(-615, 155, 800);
	glVertex3f(-610, 155, 810);
	glVertex3f(-610, 135, 810);
	glEnd();

	//kanan2
	glBegin(GL_QUADS);
	glColor3f(1.0, 1.0, 1.0);
	glVertex3f(-610, 135, 810);
	glVertex3f(-610, 155, 810);
	glVertex3f(-615, 155, 820);
	glVertex3f(-615, 135, 820);
	glEnd();

	// kotak atas
	//atas
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(-610, 165, 790);
	glVertex3f(-635, 165, 790);
	glVertex3f(-635, 165, 825);
	glVertex3f(-610, 165, 825);
	glEnd();
	//depan
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(-635, 155, 825);
	glVertex3f(-635, 165, 825);
	glVertex3f(-610, 165, 825);
	glVertex3f(-610, 155, 825);
	glEnd();

	//belakang
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(-635, 155, 790);
	glVertex3f(-635, 165, 790);
	glVertex3f(-610, 165, 790);
	glVertex3f(-610, 155, 790);
	glEnd();

	//kiri
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(-635, 155, 825);
	glVertex3f(-635, 165, 825);
	glVertex3f(-635, 165, 790);
	glVertex3f(-635, 155, 790);
	glEnd();

	//kiri
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(-610, 155, 825);
	glVertex3f(-610, 165, 825);
	glVertex3f(-610, 165, 790);
	glVertex3f(-610, 155, 790);
	glEnd();

	//object kanan
		//Atas
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(600, 60, 850);
	glVertex3f(640, 60, 850);
	glVertex3f(640, 60, 750);
	glVertex3f(600, 60, 750);
	glVertex3f(640, 60, 850);
	glVertex3f(670, 60, 800);
	glVertex3f(600, 60, 750);
	glVertex3f(570, 60, 800);
	glVertex3f(670, 60, 800);
	glVertex3f(640, 60, 750);
	glVertex3f(570, 60, 800);
	glVertex3f(600, 60, 850);
	glEnd();

	//Atas
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(610, 75, 840);
	glVertex3f(635, 75, 840);
	glVertex3f(635, 75, 760);
	glVertex3f(610, 75, 760);
	glVertex3f(660, 75, 800);
	glVertex3f(635, 75, 760);
	glVertex3f(585, 75, 805);
	glVertex3f(610, 75, 840);
	glVertex3f(635, 75, 840);
	glVertex3f(660, 75, 800);
	glVertex3f(610, 75, 760);
	glVertex3f(585, 75, 805);
	glEnd();

	//atas
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(610, 135, 825);
	glVertex3f(635, 135, 825);
	glVertex3f(635, 135, 790);
	glVertex3f(610, 135, 790);
	glVertex3f(635, 135, 825);
	glVertex3f(645, 135, 805);
	glVertex3f(610, 135, 790);
	glVertex3f(600, 135, 805);
	glVertex3f(645, 135, 805);
	glVertex3f(635, 135, 790);
	glVertex3f(600, 135, 805);
	glVertex3f(610, 135, 825);
	glEnd();

	//Depan
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(600, 35, 850);
	glVertex3f(600, 60, 850);
	glVertex3f(640, 60, 850);
	glVertex3f(640, 35, 850);
	glEnd();

	//Depan1
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(610, 60, 840);
	glVertex3f(610, 75, 840);
	glVertex3f(635, 75, 840);
	glVertex3f(635, 60, 840);
	glEnd();

	//Depan2
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(610, 120, 825);
	glVertex3f(610, 135, 825);
	glVertex3f(635, 135, 825);
	glVertex3f(635, 120, 825);
	glEnd();


	//belakang
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(600, 35, 750);
	glVertex3f(600, 60, 750);
	glVertex3f(640, 60, 750);
	glVertex3f(640, 35, 750);
	glEnd();

	//belakang2
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(610, 60, 760);
	glVertex3f(610, 75, 760);
	glVertex3f(635, 75, 760);
	glVertex3f(635, 60, 760);
	glEnd();
	//belakang3
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(610, 120, 790);
	glVertex3f(610, 135, 790);
	glVertex3f(635, 135, 790);
	glVertex3f(635, 120, 790);
	glEnd();


	//kiri1
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(640, 35, 850);
	glVertex3f(640, 60, 850);
	glVertex3f(670, 60, 800);
	glVertex3f(670, 35, 800);
	glEnd();
	//kiri1
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(635, 60, 840);
	glVertex3f(635, 75, 840);
	glVertex3f(660, 75, 800);
	glVertex3f(660, 60, 800);
	glEnd();
	//kiri1
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(635, 120, 825);
	glVertex3f(635, 135, 825);
	glVertex3f(645, 135, 805);
	glVertex3f(645, 120, 805);
	glEnd();


	//kiri2
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(670, 35, 800);
	glVertex3f(670, 60, 800);
	glVertex3f(640, 60, 750);
	glVertex3f(640, 35, 750);
	glEnd();

	//kiri2
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(645, 60, 800);
	glVertex3f(645, 75, 800);
	glVertex3f(635, 75, 760);
	glVertex3f(635, 60, 760);
	glEnd();

	//kiri2
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(645, 120, 805);
	glVertex3f(645, 135, 805);
	glVertex3f(635, 135, 790);
	glVertex3f(635, 120, 790);
	glEnd();

	//kanan1
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(600, 35, 750);
	glVertex3f(600, 60, 750);
	glVertex3f(570, 60, 800);
	glVertex3f(570, 35, 800);
	glEnd();

	//kanan1
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(610, 60, 760);
	glVertex3f(610, 75, 760);
	glVertex3f(585, 75, 805);
	glVertex3f(585, 60, 805);
	glEnd();

	//kanan1
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(610, 120, 790);
	glVertex3f(610, 135, 790);
	glVertex3f(600, 135, 805);
	glVertex3f(600, 120, 805);
	glEnd();


	//kanan2
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(570, 35, 800);
	glVertex3f(570, 60, 800);
	glVertex3f(600, 60, 850);
	glVertex3f(600, 35, 850);
	glEnd();

	//kanan2
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(585, 60, 805);
	glVertex3f(585, 75, 805);
	glVertex3f(610, 75, 840);
	glVertex3f(610, 60, 840);
	glEnd();

	//kanan2
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(600, 120, 805);
	glVertex3f(600, 135, 805);
	glVertex3f(610, 135, 825);
	glVertex3f(610, 120, 825);
	glEnd();

	//tiang
	//atas
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(615, 120, 800);
	glVertex3f(630, 120, 800);
	glVertex3f(630, 120, 815);
	glVertex3f(615, 120, 815);
	glEnd();
	//depan
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(615, 75, 815);
	glVertex3f(615, 120, 815);
	glVertex3f(630, 120, 815);
	glVertex3f(630, 75, 815);
	glEnd();
	//belakang
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(630, 75, 800);
	glVertex3f(630, 120, 800);
	glVertex3f(615, 120, 800);
	glVertex3f(615, 75, 800);
	glEnd();
	//kiri
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(630, 75, 815);
	glVertex3f(630, 120, 815);
	glVertex3f(630, 120, 800);
	glVertex3f(630, 75, 800);
	glEnd();
	//kanan
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(615, 75, 815);
	glVertex3f(615, 120, 815);
	glVertex3f(615, 120, 800);
	glVertex3f(615, 75, 800);
	glEnd();

	//tiang2
	//atas
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(630, 155, 820);
	glVertex3f(615, 155, 820);
	glVertex3f(630, 155, 800);
	glVertex3f(615, 155, 800);
	glVertex3f(630, 155, 820);
	glVertex3f(635, 155, 810);
	glVertex3f(615, 155, 800);
	glVertex3f(610, 155, 810);//
	glVertex3f(635, 155, 810);
	glVertex3f(630, 155, 800);
	glVertex3f(610, 155, 810);
	glVertex3f(615, 155, 820);
	glEnd();
	//Depan2
	glBegin(GL_QUADS);
	glColor3f(1.0, 1.0, 1.0);
	glVertex3f(630, 135, 820);
	glVertex3f(630, 155, 820);
	glVertex3f(615, 155, 820);
	glVertex3f(615, 135, 820);
	glEnd();

	//bawah
	glBegin(GL_QUADS);
	glColor3f(1.0, 1.0, 1.0);
	glVertex3f(630, 135, 800);
	glVertex3f(630, 155, 800);
	glVertex3f(615, 155, 800);
	glVertex3f(615, 135, 800);
	glEnd();

	//kiri1
	glBegin(GL_QUADS);
	glColor3f(1.0, 1.0, 1.0);
	glVertex3f(630, 135, 820);
	glVertex3f(630, 155, 820);
	glVertex3f(635, 155, 810);
	glVertex3f(635, 135, 810);
	glEnd();

	//kiri2
	glBegin(GL_QUADS);
	glColor3f(1.0, 1.0, 1.0);
	glVertex3f(635, 135, 810);
	glVertex3f(635, 155, 810);
	glVertex3f(630, 155, 800);
	glVertex3f(630, 135, 800);
	glEnd();

	//kanan1
	glBegin(GL_QUADS);
	glColor3f(1.0, 1.0, 1.0);
	glVertex3f(615, 135, 800);
	glVertex3f(615, 155, 800);
	glVertex3f(610, 155, 810);
	glVertex3f(610, 135, 810);
	glEnd();

	//kanan2
	glBegin(GL_QUADS);
	glColor3f(1.0, 1.0, 1.0);
	glVertex3f(610, 135, 810);
	glVertex3f(610, 155, 810);
	glVertex3f(615, 155, 820);
	glVertex3f(615, 135, 820);
	glEnd();

	// kotak atas
	//atas
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(610, 165, 790);
	glVertex3f(635, 165, 790);
	glVertex3f(635, 165, 825);
	glVertex3f(610, 165, 825);
	glEnd();
	//depan
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(635, 155, 825);
	glVertex3f(635, 165, 825);
	glVertex3f(610, 165, 825);
	glVertex3f(610, 155, 825);
	glEnd();

	//belakang
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(635, 155, 790);
	glVertex3f(635, 165, 790);
	glVertex3f(610, 165, 790);
	glVertex3f(610, 155, 790);
	glEnd();

	//kiri
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(635, 155, 825);
	glVertex3f(635, 165, 825);
	glVertex3f(635, 165, 790);
	glVertex3f(635, 155, 790);
	glEnd();

	//kiri
	glBegin(GL_QUADS);
	glColor3f(0.67, 0.64, 0.57);
	glVertex3f(610, 155, 825);
	glVertex3f(610, 165, 825);
	glVertex3f(610, 165, 790);
	glVertex3f(610, 155, 790);
	glEnd();
}

void draw() {
	// Mulai tuliskan isi pikiranmu disini
	tanah();
	tanahb();
	jalana();
	jalanb();
	lantai1();
	pagar_lantai1();
	tangga();
	dinding_lantai1();
	atap_lantai1();
	pagar_lantai2();
	dinding_lantai2();
	atap_lantai2();
	pagar_lantai3();
	dinding_lantai3();
	atap_lantai3();
	dinding_lantai4();
	atap_lantai4();
	pagar_lantai4();
	hiasdinding2();
	hiasdinding1();
	hiasdinding3();
	hiasdinding4();
	TiangGapura();
	lampion_lantai1a();
	lampion_lantai1b();
	lampion_lantai2a();
	lampion_lantai2b();
	TiangKuil();
	object();
	ground();
	cout << "X_GESER = " << x_geser << "    Y_GESER = " << y_geser << "    Z_GESER = " << z_geser << endl;
	glFlush();
}

void display() {
	glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
	glLoadIdentity();
	camera();
	draw();
	matahari();
	glutSwapBuffers();
}

void reshape(int w, int h) {
	glViewport(0, 0, w, h);
	glMatrixMode(GL_PROJECTION);
	glLoadIdentity();
	gluPerspective(50, 16.0 / 9.0, 2, 10000);
	glMatrixMode(GL_MODELVIEW);
}

void timer(int) {
	glutPostRedisplay();
	glutWarpPointer(width / 2, height / 2);
	glutTimerFunc(1000 / FPS, timer, 0);
}

void passive_motion(int x, int y) {
	int dev_x, dev_y;
	dev_x = (width / 2) - x;
	dev_y = (height / 2) - y;
	yaw += (float)dev_x / 20.0;
	pitch += (float)dev_y / 20.0;
}

void camera() {
	if (motion.Forward) {
		camX += cos((yaw + 90) * TO_RADIANS) * 2;
		camZ -= sin((yaw + 90) * TO_RADIANS) * 2;
	}
	if (motion.Backward) {
		camX += cos((yaw + 90 + 180) * TO_RADIANS) * 2;
		camZ -= sin((yaw + 90 + 180) * TO_RADIANS) * 2;
	}
	if (motion.Left) {
		camX += cos((yaw + 90 + 90) * TO_RADIANS) * 2;
		camZ -= sin((yaw + 90 + 90) * TO_RADIANS) * 2;
	}
	if (motion.Right) {
		camX += cos((yaw + 90 - 90) * TO_RADIANS) * 2;
		camZ -= sin((yaw + 90 - 90) * TO_RADIANS) * 2;
	}
	if (motion.Naik) {
		terbang -= 2.0;
	}
	if (motion.Turun) {
		terbang += 2.0;
	}

	if (pitch >= 70)
		pitch = 70;
	if (pitch <= -90)
		pitch = -90;


	glRotatef(-pitch, 1.0, 0.0, 0.0);
	glRotatef(-yaw, 0.0, 1.0, 0.0);

	glTranslatef(-camX, -terbang, -350 - camZ);
	if (terbang < 25)
		terbang = 24;
}

void keyboard(unsigned char key, int x, int y) {
	switch (key) {
	case 'W':
	case 'w':
		motion.Forward = true;
		break;
	case 'A':
	case 'a':
		motion.Left = true;
		break;
	case 'S':
	case 's':
		motion.Backward = true;
		break;
	case 'D':
	case 'd':
		motion.Right = true;
		break;
	case 'E':
	case 'e':
		motion.Naik = true;
		break;
	case 'Q':
	case 'q':
		motion.Turun = true;
		break;
	case '6':
		x_geser += 10.0;
		break;
	case '4':
		x_geser -= 10.0;
		break;
	case '8':
		y_geser += 10.0;
		break;
	case '2':
		y_geser -= 10.0;
		break;
	case '9':
		z_geser -= 10.0;
		break;
	case '1':
		z_geser += 10.0;
		break;
	case '`': // KELUAR DARI PROGRAM
		exit(1);
	}
}

void keyboard_up(unsigned char key, int x, int y) {
	switch (key) {
	case 'W':
	case 'w':
		motion.Forward = false;
		break;
	case 'A':
	case 'a':
		motion.Left = false;
		break;
	case 'S':
	case 's':
		motion.Backward = false;
		break;
	case 'D':
	case 'd':
		motion.Right = false;
		break;
	case 'E':
	case 'e':
		motion.Naik = false;
		break;
	case 'Q':
	case 'q':
		motion.Turun = false;
		break;
	}
}

int main(int argc, char** argv) {
	glutInit(&argc, argv);
	glutInitDisplayMode(GLUT_DOUBLE | GLUT_RGB | GLUT_DEPTH);
	glutInitWindowSize(width, height);
	glutCreateWindow("TR GRAFKOM KELOMPOK 6");

	init();
	glutDisplayFunc(display);
	glutReshapeFunc(reshape);
	glutPassiveMotionFunc(passive_motion);
	glutTimerFunc(0, timer, 0);
	glutKeyboardFunc(keyboard);
	glutKeyboardUpFunc(keyboard_up);

	glutMainLoop();
	return 0;
}
