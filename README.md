#include <stdio.h>
#include <stdlib.h>

//Velázquez Herrera Maria Thais Itzel


int calcularAgua(int altura[], int n) {
    if (n <= 2) return 0;

    int *maxIzquierda = (int *)malloc(n * sizeof(int));
    int *maxDerecha = (int *)malloc(n * sizeof(int));
    int agua = 0;
    int i, alturaMin;

    
    maxIzquierda[0] = altura[0];
    i = 1;
    while (i < n) {
        if (altura[i] > maxIzquierda[i - 1])
            maxIzquierda[i] = altura[i];
        else
            maxIzquierda[i] = maxIzquierda[i - 1];
        i++;
    }


    maxDerecha[n - 1] = altura[n - 1];
    i = n - 2;
    while (i >= 0) {
        if (altura[i] > maxDerecha[i + 1])
            maxDerecha[i] = altura[i];
        else
            maxDerecha[i] = maxDerecha[i + 1];
        i--;
    }

    i = 0;
    while (i < n) {
        alturaMin = (maxIzquierda[i] < maxDerecha[i]) ? maxIzquierda[i] : maxDerecha[i];
        agua += (alturaMin - altura[i]);
        i++;
    }

    free(maxIzquierda);
    free(maxDerecha);

    return agua;
}

int main() {
    int n, i, resultado;
    int *altura;

    printf("Ingrese la cantidad de barras del mapa de elevacion: ");
    scanf("%d", &n);

    if (n <= 0) {
        printf("Cantidad invalida.\n");
        return 1;
    }

    altura = (int *)malloc(n * sizeof(int));
    if (altura == NULL) {
        printf("Error al asignar memoria.\n");
        return 1;
    }

    printf("Ingrese las alturas de las barras (valores enteros no negativos):\n");
    i = 0;
    while (i < n) {
        printf("Altura[%d]: ", i);
        scanf("%d", &altura[i]);
        if (altura[i] < 0) {
            printf("La altura debe ser un numero entero no negativo.\n");
            free(altura);
            return 1;
        }
        i++;
    }

    resultado = calcularAgua(altura, n);
    printf("Cantidad de agua atrapada: %d\n", resultado);

    free(altura);
    return 0;
}
