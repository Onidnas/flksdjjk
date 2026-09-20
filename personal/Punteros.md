---
id: Punteros
aliases:
  - Tenemos que hay variables normales y variables punteros
tags: []
---

# Tenemos que hay variables normales y variables punteros

[[20260919-1158-aritmetica-de-punteros|Aritmetica de Punteros]]

`char* puntero;` Es variable puntero en estilo c++
`char *puntero;` Es el estandar en C clasico
*******
	**Al momento de guardar la direccion de memoria en una variable puntero tenemos dos caso en especifico
	DONDE
	
	`return Puntero; devuelve la direccion de memoria
	  return *Puntero; devuelve el valor exacto guardado en esa direccion de memoria* **

_Ya si queremos ser mas especialistas con los punteros y ser concientes de lo 
poderosos que son tenemos que saber que aca entran el como apuntan, donde ocupamos `*` y `&` Donde apuntamos y senyalamos las direcciones de memoria
especificas de cada una de estas variables_
 ***
_Vamos a ver una representacion de una funcion con punteros y arreglos para ver la diferencia
primero sin punteros._ 

```
#include <iostream>

int strlen(const char cad [ ]);

void aAlgo() 
  {
    static char cad [ ] = "Universidad Pontificia";

    std::cout << "La longitud de " << cad << " es "
      << strlen(cad) << " caracteres " << std::endl;
   }
int strlen(const char cad [ ]) 
{
  int posicion = 0;
  while (cad[posicion] != '\0')
  {
    posicion++;
  }
    return posicion;
}
```
```
*ESTE ES CON PUNTEROS* 
#include <cstring>

#include <iostream>

int strlen(const register char *);

int main() {
  static char cad[] = "Universidad Pontificia";

  std::cout << "La longitud de " << cad << " es " << strlen(cad)
            << " caracteres " << std::endl;
}
int strlen(const register char *cad) {
  int cuenta = 0;
  while (*cad++)
    ++cuenta;
  return (cuenta);
};




[[C++]]
