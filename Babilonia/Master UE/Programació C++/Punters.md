### 1. **Punteros**

Un puntero en C++ es una variable que almacena la dirección de memoria de otra variable. Esto es útil porque te permite manipular directamente el valor de una variable desde otra ubicación en memoria.

Para definir un puntero, usamos el operador `*`. Por ejemplo:

```cpp
int numero = 5;
int* puntero = &numero;  // El puntero apunta a la dirección de 'numero'
```
Aquí:

- `numero` es una variable `int` con valor `5`.
- `&numero` obtiene la **dirección de memoria** de `numero`.
- `int*` define `puntero` como un puntero a un entero, y le asignamos la dirección de `numero`.

Entonces, `puntero` ahora tiene la dirección de `numero`, y no su valor.

### 2. **Referencia**

Una referencia es un "alias" o un segundo nombre para una variable existente. En lugar de trabajar con direcciones, una referencia permite acceder directamente al valor original, sin necesidad de un puntero explícito. Se declara usando el operador `&`.

Aquí:
``` cpp
int numero = 5;
int& referencia = numero;  // 'referencia' es otro nombre para 'numero'
```

- `numero` y `referencia` apuntan a la misma ubicación en memoria.
- Cambiar `referencia` es lo mismo que cambiar `numero`, ya que ambos están "enlazados" a la misma variable.

Las referencias en C++ son constantes, es decir, una vez creada una referencia a una variable, no se puede cambiar para que apunte a otra variable.

### 3. **Dereferencia**

La dereferencia es el proceso de acceder al valor al que apunta un puntero. Para hacerlo, usamos el operador `*` delante de un puntero.

```cpp
int numero = 5;
int* puntero = &numero;   // puntero apunta a 'numero'
int valor = *puntero;     // 'valor' ahora es el contenido en la dirección apuntada por 'puntero'
```

Aquí:

- `*puntero` accede al **valor almacenado** en la dirección a la que apunta `puntero`, que es `numero`.
- `valor` recibe el valor `5`.

### Resumen

- **Punteros**: Variables que almacenan direcciones de memoria de otras variables. Se crean con `*` y acceden a direcciones con `&`.
- **Referencias**: Alias a variables, creados con `&`, no cambian de destino y no necesitan dereferenciarse.
- **Dereferencia**: Obtener el valor al que apunta un puntero con `*`.

**Ejemplo completo:**

```cpp
#include <iostream>
using namespace std;

int main() {
    int numero = 10;

    // Creando un puntero y una referencia
    int* puntero = &numero;
    int& referencia = numero;

    // Mostrando valores y direcciones
    cout << "Valor de numero: " << numero << endl;
    cout << "Direccion de numero: " << &numero << endl;
    cout << "Direccion almacenada en puntero: " << puntero << endl;
    cout << "Valor apuntado por puntero (dereferencia): " << *puntero << endl;
    cout << "Valor a traves de referencia: " << referencia << endl;

    return 0;
}
```



Este ejemplo muestra el valor de `numero`, la dirección de `numero`, el contenido del puntero, y cómo se accede al valor usando tanto el puntero como la referencia.

[[Exercicis Punters]]