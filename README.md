# listas enlazadas

https://dbcolmenaresp.github.io/listasenlazadas.github.io/listasenlazadas.html

https://dbcolmenaresp.github.io/listasenlazadas.github.io/listasenlazadas.svg


Clase teórica: Listas enlazadas desde los fundamentos

Duración sugerida: 90 minutos
Prerrequisitos: variables, referencias/punteros, funciones, memoria dinámica básica y noción intuitiva de complejidad.
Idea central: una lista enlazada no es un arreglo: es una cadena de nodos donde cada nodo conoce al siguiente. Se accede desde una referencia llamada cabeza (head).

---

1. Objetivos de aprendizaje

Al finalizar la clase, el estudiante deberá poder:

1. Definir qué es un nodo y qué es una lista enlazada.
2. Explicar la diferencia entre el TAD Lista y su implementación mediante enlaces.
3. Recorrer, insertar, buscar y eliminar elementos en una lista simplemente enlazada.
4. Analizar la complejidad temporal de las operaciones principales.
5. Identificar errores frecuentes: perder la cabeza, no actualizar la cola, recorrer un nulo, fugas de memoria.
6. Comparar listas enlazadas con arreglos y decidir cuándo usar cada estructura.

---

2. Motivación: ¿por qué no usar siempre arreglos?

Un arreglo permite acceso directo por índice en O(1), pero tiene limitaciones:

· Tamaño fijo o redimensionamiento costoso.
· Insertar o eliminar en medio requiere desplazar elementos: O(n).
· La memoria suele ser contigua.

Una lista enlazada propone otra idea:

No guardamos los elementos uno al lado del otro. Guardamos nodos dispersos en memoria y los conectamos mediante referencias.

Analogía: un tren. Cada vagón tiene carga y un enganche al siguiente vagón. Solo conocemos la ubicación del primer vagón; para llegar al quinto, debemos pasar por los anteriores.

---

3. Concepto de nodo

El bloque fundamental es el nodo. Un nodo contiene:

· Un dato.
· Una referencia al siguiente nodo.

Pseudocódigo conceptual:

```text
Nodo:
    dato
    siguiente
```

En C/C++ sería un puntero; en Java o Python, una referencia. Usaremos nulo para representar NULL, null o None.

Diagrama de una lista simplemente enlazada:

```text
cabeza
  |
  v
+------+------+     +------+------+     +------+------+
| dato | next | --> | dato | next | --> | dato | null |
+------+------+     +------+------+     +------+------+
```

La lista está vacía si:

```text
cabeza = nulo
```

---

4. Definición formal

Una lista enlazada es una colección lineal y dinámica de nodos donde:

1. Cada nodo almacena un dato y una referencia al siguiente nodo.
2. Existe una referencia de acceso llamada cabeza (head).
3. El último nodo tiene siguiente = nulo.
4. Si cabeza = nulo, la lista está vacía.

Es importante distinguir:

· TAD Lista: concepto abstracto: secuencia ordenada con operaciones como insertar, eliminar, buscar, recorrer.
· Lista enlazada: una implementación concreta del TAD Lista usando nodos y referencias.

---

5. Operación básica: recorrido

Para recorrer una lista no usamos índices. Usamos una referencia auxiliar:

```text
recorrer(lista):
    actual = lista.cabeza
    mientras actual != nulo:
        procesar(actual.dato)
        actual = actual.siguiente
```

Complejidad: O(n).

Error clásico: escribir actual = actual.siguiente sin comprobar que actual no sea nulo. Eso produce un error de referencia nula.

---

6. Operaciones fundamentales

6.1 Insertar al inicio

```text
insertarAlInicio(lista, x):
    nuevo = Nodo(x)
    nuevo.siguiente = lista.cabeza
    lista.cabeza = nuevo
    si lista.cola == nulo:
        lista.cola = nuevo
```

Complejidad: O(1).

Diagrama:

```text
Antes:  cabeza -> A -> B -> nulo
Después: cabeza -> X -> A -> B -> nulo
```

---

6.2 Insertar después de un nodo conocido

```text
insertarDespues(nodo, x):
    si nodo == nulo: error
    nuevo = Nodo(x)
    nuevo.siguiente = nodo.siguiente
    nodo.siguiente = nuevo
    si nodo == lista.cola:
        lista.cola = nuevo
```

Complejidad: O(1), porque no hay que buscar el nodo.

Orden importante: primero se enlaza el nuevo nodo con el siguiente; luego se actualiza el enlace del nodo anterior. Si se hace al revés, se pierde el resto de la lista.

---

6.3 Insertar al final

Sin referencia a la cola:

```text
insertarAlFinal(lista, x):
    nuevo = Nodo(x)
    si lista.cabeza == nulo:
        lista.cabeza = nuevo
        lista.cola = nuevo
        return
    actual = lista.cabeza
    mientras actual.siguiente != nulo:
        actual = actual.siguiente
    actual.siguiente = nuevo
    lista.cola = nuevo
```

Complejidad:

· Sin cola: O(n).
· Con referencia cola: O(1).

---

6.4 Buscar un elemento

```text
buscar(lista, x):
    actual = lista.cabeza
    mientras actual != nulo:
        si actual.dato == x:
            return actual
        actual = actual.siguiente
    return nulo
```

Complejidad: O(n). La búsqueda es secuencial: no hay acceso aleatorio.

---

6.5 Eliminar al inicio

```text
eliminarAlInicio(lista):
    si lista.cabeza == nulo:
        return
    temp = lista.cabeza
    lista.cabeza = temp.siguiente
    si lista.cabeza == nulo:
        lista.cola = nulo
    liberar(temp)
```

Complejidad: O(1).

Casos límite:

· Lista vacía: no hacer nada o lanzar error controlado.
· Lista con un solo nodo: actualizar cabeza y cola a nulo.

---

6.6 Eliminar después de un nodo conocido

```text
eliminarDespues(prev):
    si prev == nulo o prev.siguiente == nulo:
        return
    temp = prev.siguiente
    prev.siguiente = temp.siguiente
    si temp == lista.cola:
        lista.cola = prev
    liberar(temp)
```

Complejidad: O(1).

En una lista simplemente enlazada, para eliminar un nodo por valor normalmente hay que buscar también su anterior, porque necesitamos actualizar anterior.siguiente. Por eso eliminar por valor cuesta O(n).

---

7. Complejidad de operaciones

Operación Lista simplemente enlazada
Acceder por índice O(n)
Buscar por valor O(n)
Insertar al inicio O(1)
Insertar al final sin cola O(n)
Insertar al final con cola O(1)
Insertar después de nodo conocido O(1)
Eliminar al inicio O(1)
Eliminar después de nodo conocido O(1)
Eliminar al final sin doble enlace O(n)
Recorrer O(n)
Espacio O(n)

---

8. Variantes de listas enlazadas

8.1 Lista simplemente enlazada

Cada nodo apunta solo al siguiente.

```text
A -> B -> C -> nulo
```

Ventaja: simple y poca memoria por nodo.
Desventaja: no se puede retroceder.

8.2 Lista doblemente enlazada

Cada nodo tiene siguiente y anterior.

```text
nulo <- A <-> B <-> C -> nulo
```

Ventajas: recorrido en ambos sentidos; eliminar un nodo conocido es O(1).
Desventaja: más memoria y más enlaces que mantener.

8.3 Lista circular

El último nodo apunta a la cabeza.

```text
A -> B -> C -> A
```

Cuidado: al recorrerla no se busca nulo; hay que detenerse al volver a la cabeza.

8.4 Lista con nodo centinela

Se usa un nodo ficticio al inicio para simplificar casos límite. Es una técnica habitual en implementaciones profesionales.

---

9. Listas enlazadas vs arreglos

Aspecto Arreglo Lista enlazada
Acceso por índice O(1) O(n)
Inserción/eliminación al inicio O(n) O(1)
Inserción/eliminación en medio O(n) O(1) si se conoce el nodo
Tamaño fijo o redimensionable dinámico
Memoria contigua dispersa
Localidad de caché buena peor
Sobrecarga por elemento baja referencia(s) extra
Implementación sencilla requiere cuidado con enlaces

Regla práctica: si necesitas acceso por índice frecuente, usa arreglo o lista dinámica. Si necesitas muchas inserciones/eliminaciones y recorridos secuenciales, una lista enlazada puede ser mejor.

---

10. Errores frecuentes

1. Perder la referencia cabeza.
2. No actualizar cola al insertar o eliminar el último nodo.
3. Acceder a actual.siguiente cuando actual es nulo.
4. Modificar enlaces en el orden incorrecto y perder el resto de la lista.
5. No liberar memoria en lenguajes sin recolector de basura.
6. Confundir el nodo con el dato que contiene.
7. En listas circulares, recorrer indefinidamente porque nunca se llega a nulo.

---

11. Ejemplo integrador en pseudocódigo

```text
lista = ListaVacia()
insertarAlInicio(lista, 30)
insertarAlInicio(lista, 20)
insertarAlInicio(lista, 10)
```

Estado:

```text
cabeza -> 10 -> 20 -> 30 -> nulo
```

Luego:

```text
insertarDespues(buscar(lista, 20), 25)
```

Estado:

```text
cabeza -> 10 -> 20 -> 25 -> 30 -> nulo
```

Luego:

```text
eliminarAlInicio(lista)
```

Estado:

```text
cabeza -> 20 -> 25 -> 30 -> nulo
```

---

12. Cierre docente

Ideas clave para dejar claras:

1. Una lista enlazada es una secuencia de nodos conectados por referencias.
2. La cabeza es la puerta de entrada; si se pierde, se pierde la lista.
3. El acceso es secuencial, no aleatorio.
4. Su gran ventaja es reorganizar enlaces en O(1) cuando se conoce el punto de inserción o eliminación.
5. Su gran costo es la búsqueda O(n), el uso de memoria extra y la gestión cuidadosa de casos límite.

Frase de cierre:

“En una lista enlazada no movemos datos: movemos enlaces. Ahí está su potencia y también su riesgo.”

---

13. Ejercicios propuestos

1. Dibujar la lista resultante de insertar 5, 10, 15 al inicio y luego 20 al final.
2. Escribir una función iterativa y otra recursiva para calcular la longitud de una lista.
3. Implementar buscar y eliminarPorValor en una lista simplemente enlazada.
4. Invertir una lista simplemente enlazada usando tres referencias: anterior, actual, siguiente.
5. Explicar por qué eliminar el último nodo de una lista simplemente enlazada cuesta O(n).
6. Comparar una lista enlazada con un arreglo para implementar una cola de impresión.
7. Detectar si una lista tiene un ciclo. Pista: algoritmo de Floyd, “liebre y tortuga”.
