/* mbed Microcontroller Library
 * Copyright (c) 2019 ARM Limited
 * SPDX-License-Identifier: Apache-2.0
 */

#include "mbed.h"
#include "platform/mbed_thread.h"

// defines

#define BLINKING_RATE_MS                                      500

// prototipos de la funcion

void contar(void);
void visualizar(void);
void displayNumber(int num);

//tareas

Thread T_contar(osPriorityNormal,512);
Thread T_visualizar(osPriorityNormal,512);
Thread T_displayNumber(osPriorityNormal,512);

// pines y puertos 

DigitalIn btn(BUTTON1);
DigitalIn btn(BUTTON3);
DigitalOut led(LED1);
BusOut leds (D3,D4,D5,D6,D7,D8,D9,D10); //dp-a-g
BusOut trans (D11,D12);

// variables globales 

char u;
char d;
cahr i;
const char tabla[10] = {0x81, 0xcf, 0x94, 0x86, 0xcc, 0xa4, 0xa0, 0x8f, 0x80, 0x90 }; 

int main()
{

    // Arrancar las tareas e inicializar variables

    T_contar.start(contar);
    T_visualizar.start(visualizar);
    T_displayNumber.satart(displayNumber);
    u=0;
    d=0;
    while (true) {
        led = !led;
        thread_sleep_for(BLINKING_RATE_MS);
    }
}

void contar (void)
{
    while(true)
    {
        u++;
        if (u==9) {u=0; d++;
        if (d==9) d=0; }
        ThisThread::sleep_for(1000ms);
    }
    if (BUTTON1==1)
    {
    while(true)
    {
        u--;
        if (u==0) {u=0; d--;
        if (d==0) d=9; }
        ThisThread::sleep_for(1000ms);
    }
    }
}

void visualizar (void)
{
    //maquina que estados
    
    static char conteo=0;
    conteo++;
    if (conteo==4) conteo=0;
    while(true)
    {
        switch (conteo)
        {
            case 0: trans=0b10; leds = tabla[u]; break;
            case 1: trans=0; break; //apagar los transistores
            case 2: trans=0b01; leds = tabla[d]; break;
            case 3: trans=0; break; //apagar los transistores
        }
    ThisThread::sleep_for(5s);
    }
}
void displayNumber(int num)
if (BUTTON1==3)
{
    // Función para verificar si un número es primo
bool isPrime(int num) {
    if (num <= 1) return false;
    for (int i = 2; i <= num / 2; ++i) {
        if (num % i == 0) return false;
    }
    return true;
}

int main() {
    HAL_Init();
    while (1) {
        for (int num = 0; num <= 99; ++num) {
            if (isPrime(num)) {
                displayNumber(num);
                HAL_Delay(1000);  // Espera 1 segundo, ajusta según sea necesario
            }
        }
    }
}
}
