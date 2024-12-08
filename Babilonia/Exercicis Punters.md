### Ejercicio 6: Suma de Matrices usando Punteros

**Objetivo:** Crear una función que sume dos matrices de tamaño `N x M` usando punteros.

**Instrucciones:**

1. Declara dos matrices `N x M` de enteros (puedes usar un tamaño fijo como `3x3` para simplificar).
2. Crea una función `sumarMatrices` que tome punteros a ambas matrices y almacene el resultado en una tercera matriz.
3. Muestra el resultado en el `main`.

**Pistas:**

- Usa aritmética de punteros para acceder a los elementos de las matrices.

---

### Ejercicio 7: Manejo de Memoria Dinámica para un Array de Estructuras

**Objetivo:** Crear y manejar un array dinámico de estructuras para almacenar información de estudiantes.

**Instrucciones:**

1. Define una estructura `Estudiante` con los campos `nombre`, `edad`, y `nota`.
2. En el `main`, pide al usuario cuántos estudiantes desea almacenar.
3. Usa `new` para reservar memoria dinámica para un array de `Estudiante`.
4. Pide al usuario que ingrese los datos de cada estudiante.
5. Muestra la información ingresada y luego libera la memoria con `delete[]`.

**Pistas:**

- Usa `cin` para ingresar los datos del estudiante.
- Asegúrate de liberar la memoria para evitar fugas.

---

### Ejercicio 8: Ordenar un Array con Punteros

**Objetivo:** Escribir una función `ordenarArray(int* arr, int n)` que ordene un array de enteros usando aritmética de punteros y el algoritmo de ordenamiento de burbuja (o cualquier otro que prefieras).

**Instrucciones:**

1. Define un array de enteros en `main` y pasa su dirección a la función `ordenarArray`.
2. En `ordenarArray`, usa aritmética de punteros para comparar e intercambiar los valores de los elementos del array.
3. Muestra el array ordenado al final.

**Pistas:**

- Usa `*(arr + i)` para acceder a los elementos en lugar de índices.

---

### Ejercicio 9: Copiar y Concatenar Cadenas con Punteros

**Objetivo:** Implementar funciones para copiar y concatenar dos cadenas de caracteres usando solo punteros (sin `strcpy` ni `strcat`).

**Instrucciones:**

1. Crea una función `copiarCadena(char* destino, const char* origen)` que copie la cadena `origen` a `destino`.
2. Crea otra función `concatenarCadena(char* destino, const char* origen)` que añada la cadena `origen` al final de `destino`.
3. En `main`, declara dos cadenas, usa `copiarCadena` para copiar una en la otra y luego `concatenarCadena` para concatenarlas.

**Pistas:**

- Usa un puntero para recorrer `origen` y copiar cada carácter en `destino`.
- Asegúrate de añadir el carácter nulo `\0` al final de la cadena.

---

### Ejercicio 10: Crear y Manejar una Matriz Dinámica

**Objetivo:** Reservar memoria dinámica para una matriz `N x M`, permitir la entrada de datos, realizar cálculos sobre la matriz y luego liberar la memoria.

**Instrucciones:**

1. Pide al usuario que ingrese el tamaño `N x M` de la matriz.
2. Usa memoria dinámica (con `new`) para crear una matriz de punteros de tamaño `N x M`.
3. Permite al usuario ingresar los valores de la matriz.
4. Implementa una función `sumaElementos` que recorra la matriz con punteros y devuelva la suma de todos sus elementos.
5. Muestra la suma en `main` y luego libera la memoria con `delete`.

**Pistas:**

- Usa un array de punteros para simular una matriz dinámica.
- Asegúrate de liberar toda la memoria asignada.