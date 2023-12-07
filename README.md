#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <math.h>
#include <stdlib.h>
#include "winbgi2.h"
#include <time.h>
void init(double* x, double* y, int N);
void display(double* x, double* y,int N);
void showTable(double* x, double* y, double* xV, double* yV, int N);
void initV(double* xV, double* yV, int N);
void move(double* x, double* y, double* xV, double* yV, int N);
void collideWall(double* x, double* y, double* xV, double* yV, int N);
void collideBall(double* x, double* y, double* xV, double* yV, int N);
int main()
{
    srand(time(NULL));
    graphics(400, 400);
    double x[10], y[10];        // wspolrzedne pilek
    double xV[10], yV[10];      // skladowe predkosci pilek
    init(x, y, 10);
    initV(xV, yV, 10);
    showTable(x, y, xV, yV, 10);
    for (int i = 0; i < 50000; i++) {
        collideWall(x, y, xV, yV, 10);
        collideBall(x, y, xV, yV, 10);
        move(x, y, xV, yV, 10);
        animate(160);   // jako argument funkcji wpisujemy ilość klatek na sekundę (oczekiwanie przez 10 ms)
        clear();        // czyszczenie okna dla nowej klatki
        display(x, y, 10);
    }
        wait();
        return 0;
    }
    void init(double* x, double* y, int N) {
        int i;
        int min = 20;
        int max = 380;
        int L = max - min + 1;
        for (i = 0; i < N; i++) {
            x[i] = rand() % L + min;
            y[i] = rand() % L + min;

        }
    }
    void display(double* x, double* y, int N) {
        for (int i = 0; i < N; i++) {
            circle(x[i], y[i], 20);
        }

    }
    void showTable(double* x, double* y, double* xV, double* yV, int N) {
        for (int i = 0; i < N; i++) {
            printf("x=%lf\t y=%lf\t xV=%lf\t yV=%lf\n", x[i], y[i], xV[i], yV[i]);
        }
    }
    void initV(double* xV, double* yV, int N) {
        for (int i = 0; i < N; i++) {
            xV[i] = 2 * ((double)rand() / RAND_MAX) - 1;
            yV[i] = 2 * ((double)rand() / RAND_MAX) - 1;
        }
    }
    void move(double* x, double* y, double* xV, double* yV, int N) {
        for (int i = 0; i < N; i++) {
            x[i] += 0.3*xV[i];
            y[i] += 0.3 * yV[i];
        }
    }
    void collideWall(double* x, double* y, double* xV, double* yV, int N) {
        for (int i = 0; i < N; i++) {
            if (x[i] >= 381.0 || x[i] <= 21.0)xV[i] = -1.0 * xV[i];
            if (y[i] >= 381.0 || y[i] <= 21.0)yV[i] = -1.0 * yV[i];
        }
    }
    void collideBall(double* x, double* y, double* xV, double* yV, int N) {
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < N; j++)
                if (i != j) {
                double cx,cy, o;
                o = (pow((x[i] - x[j]), 2.) + pow((y[i] - y[j]), 2.));
                    if (sqrt(o) <= 40 ) {
                    cx = (x[i] - x[j]) * (xV[i] - xV[j]) / o;
                    cy = (y[i] - y[j]) * (yV[i] - yV[j]) / o;
                    xV[i] =  xV[i] + cx * (x[j] - x[i]);
                    xV[j] =  xV[j] - cx * (x[j] - x[i]);
                    yV[i] =  yV[i] + cy * (y[j] - y[i]);
                    yV[j] =  yV[j] - cy * (y[j] - y[i]);
                    }
                }
        }
        }
    
