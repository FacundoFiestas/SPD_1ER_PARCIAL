# SPD 1er parcial - Display 7 segmentos
![ArduinoTinkercad](https://github.com/DylanFicocelli/SPD-1er-pacial/assets/138259829/87f50d5e-5847-4b37-96c3-b3fdec4153b2)



## Integrantes 
- Facundo Fiestas
- Dylan Ficocelli 


## Proyecto: Displays 7 segmentos - 1ra parte.
![display 7 segmentos]![display](https://github.com/DylanFicocelli/SPD-1er-pacial/assets/138259829/550d69a7-22f8-44b4-9e13-329f64bb6282)




## Descripción
El proyecto consiste en un contador digital de dos dígitos con displays de 7 segmentos y botones de control. Su principal función es contar y mostrar números en el rango de 0 a 99, permitiendo al usuario incrementar, disminuir o reiniciar el contador según sus necesidades. 

## Función principal
Esta función se encarga de reflejar el número actual en los displays y actualizar la visualización en tiempo real.

La función  printCount es la principal del proyecto. Utiliza dos funciones auxiliares, prendeDigito y mostrarNumero, para controlar y mostrar el número en los displays de siete segmentos. Primero, apaga todos los dígitos, luego muestra la cifra de las decenas en el display de la izquierda, enciende ese dígito y lo apaga, después muestra la cifra de las unidades en el display de la derecha y lo enciende.

~~~ C++ (lenguaje en el que esta escrito)
void printCount(int count)
{
 prendeDigito(APAGADOS);
 mostrarNumero(count/10);
 prendeDigito(DECENA); 
 prendeDigito(APAGADOS);
 mostrarNumero(count - 10*((int)count/10));
 prendeDigito(UNIDAD);
}
~~~

## Proyecto: Displays 7 segmentos - 2da parte.

![display 7 segmentos 2da parte](https://github.com/DylanFicocelli/SPD-1er-pacial/assets/138259829/b54fa0fb-3785-43d8-bae5-ca4378b4db0a)


## Descripción
Esta segunda parte del proyecto utiliza una serie de LEDs y displays de 7 segmentos para mostrar números. El usuario puede aumentar, disminuir o restablecer el número mostrado utilizando botones físicos. También tiene la capacidad de alternar entre mostrar números primos y números no primos. Además, ajusta la velocidad de un motor en función de la lectura de un sensor de fuerza.

## Funciónes principales

Las siguientes funciones se encargan de manejar la lógica relacionada con los números primos, el control del contador y la configuración de la velocidad de un motor en función de la lectura de un sensor de fueza. 
~~~ C++ (lenguaje en el que esta escrito)
// FUNCION PARA DETECTAR SI UN NUMERO ES O NO PRIMO.

bool esPrimo(int numero) {
  if (numero <= 1)
    return false;
  if (numero <= 3)
    return true;
  if (numero % 2 == 0 or numero % 3 == 0)
    return false;
  for (int i = 5; i * i <= numero; i += 6) {
    if (numero % i == 0 or numero % (i + 2) == 0)
      return false;
  }
  return true;
}

//FUNCIONES QUE PERMITEN QUE NUESTRO CONTADOR DE NUMEROS PRIMOS SEA CICLICO.

int siguientePrimo(int numero) {
  int siguiente = numero + 1;
  while (true) {
    if (siguiente > 97) siguiente = 2;  
    if (esPrimo(siguiente)) return siguiente;
    siguiente++;
  }
}

int anteriorPrimo(int numero) {
  int anterior = numero - 1;
  while (true) {
    if (anterior < 2) anterior = 97;
    if (esPrimo(anterior)) return anterior;
    anterior--;
  }
}
//DETECTA EL ESTADO DEL INTERRUPTOR PARA SABER QUE MOSTRAR

  if (digitalRead(INTERRUPTOR) == LOW) {
    usarNumerosPrimos = true;
  } else {
    usarNumerosPrimos = false;
  }

//SEGÚN EL VALOR ASIGNADO, AUMENTA, DISMINUYE O REINICIA EL VALOR DEL CONTADOR.

  int pressed = keyPressed();
  if (usarNumerosPrimos) {
    if (pressed == SUBE) {
      contador = siguientePrimo(contador);
    } else if (pressed == BAJA) {
      contador = anteriorPrimo(contador);
    } else if (pressed == RESET) {
      contador = 2;  
    }
 
//CONFIGURACION RELACION MOTOR-FSR 
  
  int valorFSR = analogRead(A0);
  int velocidadMotor = map(valorFSR, 1023, 109, 0, 255); 
  analogWrite(6, velocidadMotor);
  printCount(contador);
~~~ 

## Link al proyecto
- [SPD - Displays 7 segmentos](https://www.tinkercad.com/things/lVhXqOEvwvs-copy-of-1er-parcial-domiciliario-parte-1/editel?sharecode=--cA0zDOCJx_jQAPqOwLAkRBCTWHKWf4TssC110CaMk)


-Parte individual Facundo Fiestas

![Fotoresistor](https://github.com/DylanFicocelli/SPD-1er-pacial/assets/138259829/4b3b9eff-45a2-4cf4-bc7e-8ecfaa330ce7)



## Descripción
Se agrega un fototransistor al circuito.

## Función principal
La función principal involucra la lectura de la luz ambiente utilizando el fototransistor y luego muestra por serial el estado HIGH/LOW

~~~ C++ (lenguaje en el que esta escrito)
    //CONFIGURACION DEL FOTORESISTOR
  int valorFototransistor = analogRead(A1);

if (valorFototransistor == 0) {
  Serial.println("Valor del Fototransistor: LOW");
} else if (valorFototransistor != 0) {
  Serial.println("Valor del Fototransistor: HIGH");
}
~~~

## Link al proyecto
- [SPD - Displays 7 segmentos - Fotoresistor](https://www.tinkercad.com/things/grhwCnDGqEx-copy-of-1er-parcial-domiciliario-spd/editel?sharecode=kEe3BXCOUsoV5C2kqc78QYM5kmVIkmvIbPkKUwLXZms)
