# Bono de Programación: Problemas Generales de Conteo
**Materia:** Matemáticas Discretas I  
**Estudiante:** Frank Kenner Olmos Prada  
**Docente:** Jhoan Sebastian Tenjo García  

## 1. Descripción del Proyecto
Este repositorio contiene una herramienta computacional interactiva desarrollada para resolver problemas generales de combinatoria. El programa no se limita a casos numéricos fijos, sino que permite ingresar parámetros dinámicos ($n, k, a, b$), valida restricciones lógicas (como el Principio de las Casillas) y aplica los modelos matemáticos adecuados para retornar soluciones exactas. Todo el motor de cálculo está diseñado empleando `BigInt` para evitar el desbordamiento de memoria ante factoriales masivos.

## 2. Instrucciones de Ejecución
El proyecto fue construido utilizando HTML, CSS y Vanilla JavaScript puro para ser portable.
1. Clone este repositorio en su máquina local.
2. Asegúrese de que la imagen de fondo (`CocoBodyopenmouthLogo2.png`) esté en el mismo directorio que el archivo principal.
3. Abra el archivo `bono_programable.html` en cualquier navegador web. 
4. *Opcional pero recomendado:* Ejecute el archivo mediante la extensión **Live Server** en Visual Studio Code para un entorno de pruebas óptimo (debe instalar la extensión, en caso de ya tenerla instalada es simplemente clic derecho y abrir con Liver Server).
5. No se requiere la instalación de compiladores, librerías externas ni dependencias (como Node.js o Python). La lógica matemática nativa maneja la ejecución general de manera aislada.

## 3. Explicación de los Problemas Resueltos

### Problema 4: Sistema de conteo de contraseñas
* **Modelo Matemático:** Se busca contar la cantidad de cadenas de longitud $n$ construidas a partir de un alfabeto de tamaño $k$.
* **Análisis con repetición:** Si se permite repetir caracteres, cada una de las $n$ posiciones tiene $k$ opciones posibles. Por la regla del producto, la fórmula implementada es $k^n$.
* **Análisis sin repetición:** Si no se permite repetición, el orden sigue importando, configurando una permutación sin repetición $P(k, n) = \frac{k!}{(k-n)!}$. El programa incluye una validación rigurosa basada en el *Principio del Palomar*: si el usuario ingresa una longitud $n > k$, el sistema bloquea el cálculo y explica que es lógicamente imposible no repetir caracteres.

### Problema 7: Distribución de objetos en cajas
* **Modelo Matemático:** Distribución de $n$ objetos idénticos en $k$ cajas distintas. Se traduce en buscar soluciones enteras a la ecuación $x_1 + x_2 + ... + x_k = n$.
* **Análisis (Cajas vacías permitidas):** Es equivalente a buscar soluciones enteras *no negativas*. Se utilizó el teorema de Estrellas y Barras, ordenando $n$ objetos y $k-1$ separadores. La fórmula implementada es $C(n + k - 1, k - 1)$.
* **Análisis (Sin cajas vacías):** Se buscan soluciones enteras *positivas* ($x_i \ge 1$). Se garantiza entregando 1 objeto a cada caja primero, quedando $n-k$ objetos por distribuir. La fórmula simplificada implementada es $C(n - 1, k - 1)$. El sistema valida de antemano que $n \ge k$; de lo contrario, advierte al usuario que faltan objetos para cumplir la restricción.

### Problema 8: Caminos mínimos en una grilla
* **Modelo Matemático:** Encontrar los caminos mínimos en una cuadrícula desde $(0,0)$ hasta un destino $(a,b)$ moviéndose exclusivamente hacia la Derecha o hacia Arriba.
* **Análisis Base:** Cualquier camino mínimo tendrá exactamente $a+b$ pasos totales. El problema se reduce a elegir en cuáles de esos $a+b$ pasos se realizarán los $a$ movimientos hacia la derecha. La fórmula aplicada es una combinación: $C(a+b, a)$.
* **Análisis con Restricción (Punto Bloqueado):** Si existe un obstáculo en la coordenada $(bx, by)$, el programa emplea el **Principio de Inclusión-Exclusión** (resta lógica). Calcula los caminos desde el origen hasta el bloqueo $C(bx+by, bx)$, los multiplica (Regla del Producto) por los caminos desde el bloqueo hasta el destino $C((a-bx)+(b-by), a-bx)$, y finalmente resta este subconjunto de "caminos inválidos" del total de caminos posibles.

## 4. Evidencias de Pruebas (Casos Pequeños y Medianos)

**Pruebas Problema 4 (Contraseñas):**
* *Caso Pequeño (Verificable a mano):* $n=3$, $k=5$, Sin repetición.
  * **Salida del programa:** $P(5, 3) = 60$ contraseñas posibles. Correcto.
* *Caso Mediano (Generado dinámicamente):* $n=12$, $k=94$ (ASCII imprimible), Sin repetición.
  * **Salida del programa:** $294 840 753 408 338 026 836 198 400$ contraseñas.
* *Prueba de Excepción:* $n=10$, $k=5$, Sin repetición.
  * **Salida:** Error capturado justificando el Principio de las Casillas.

**Pruebas Problema 7 (Distribución en cajas):**
* *Caso Pequeño (Verificable a mano):* $n=5$ objetos, $k=3$ cajas, Vacías permitidas.
  * **Salida del programa:** $C(5+3-1, 3-1) = C(7, 2) = 21$ formas. Correcto.
* *Caso Mediano (Generado dinámicamente):* $n=100$ peticiones, $k=15$ servidores, Vacías permitidas.
  * **Salida del programa:** $C(114, 14) = 312 629 484 400 483 356$ formas.

**Pruebas Problema 8 (Caminos en grilla):**
* *Caso Pequeño (Verificable a mano):* Destino $(3, 3)$ con bloqueo en $(1, 1)$.
  * **Análisis matemático:** Caminos totales $C(6, 3) = 20$. Caminos al bloqueo $C(2, 1) = 2$. Caminos del bloqueo al destino $C(4, 2) = 6$. Caminos inválidos: $2 \times 6 = 12$. Caminos válidos: $20 - 12 = 8$.
  * **Salida del programa:** 8 caminos válidos. Correcto.
* *Caso Grande (Generado dinámicamente):* Destino $(40, 30)$ con bloqueo en $(20, 15)$.
  * **Salida del programa:** Totales: $55 347 740 058 143 507 128$. Válidos: $44 798 605 287 552 721 528$.

*(Nota: Se adjuntan capturas de pantalla en este repositorio como evidencia gráfica de la ejecución de estos ejemplos en la interfaz analítica).*
