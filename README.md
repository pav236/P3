PAV - P3: detección de pitch
============================

Esta práctica se distribuye a través del repositorio GitHub [Práctica 3](https://github.com/albino-pav/P3).
Siga las instrucciones de la [Práctica 2](https://github.com/albino-pav/P2) para realizar un `fork` de la
misma y distribuir copias locales (*clones*) del mismo a los distintos integrantes del grupo de prácticas.

Recuerde realizar el *pull request* al repositorio original una vez completada la práctica.

Ejercicios básicos
------------------

- Complete el código de los ficheros necesarios para realizar la detección de pitch usando el programa
  `get_pitch`.

   * Complete el cálculo de la autocorrelación e inserte a continuación el código correspondiente.

     void PitchAnalyzer::autocorrelation(const vector<float> &x, vector<float> &r) const {

      for (unsigned int l = 0; l < r.size(); ++l) {
        /// \TODO Compute the autocorrelation r[l]
        for(unsigned int n = 0; l < x.size()-1-l; n++){
          r[l] = x[n]*x[n+l] + r[l];
          }
          r[l] = (1.0F/x.size())*r[l];
        }

      if (r[0] == 0.0F) //to avoid log() and divide zero
        r[0] = 1e-10;
      }

   * Inserte una gŕafica donde, en un *subplot*, se vea con claridad la señal temporal de un sonido sonoro
     y su periodo de pitch; y, en otro *subplot*, se vea con claridad la autocorrelación de la señal y la
	 posición del primer máximo secundario.

   Con la señal completa tendriamos:
    <img src="img/3.PNG" width="640" align="center">

    El codigo usado es el siguiente:
    <img src="img/4.PNG" width="640" align="center">

    Recortamos el sonido sonoro y lo representamos en la gráfica:
    <img src="img/6.PNG" width="640" align="center">

    Marcamos el periodo de pitch que es de 38ms:
    <img src="img/7.PNG" width="640" align="center">

    Podriamos verlo también en la gráfica anterior,donde hemos hecho zoom:
    <img src="img/8.PNG" width="640" align="center">

    Ahora veremos la autocorrelación de la señal y la posición del primer máximo secundario:
    <img src="img/10.PNG" width="640" align="center">
    <img src="img/9.PNG" width="640" align="center">

    Es correcto ya que vemos que tenemos 2N-1 muestras y el máximo centrado en la posición N que como vemos en la imagen corresponde al valor del eje Y 47500.


	 NOTA: es más que probable que tenga que usar Python, Octave/MATLAB u otro programa semejante para
	 hacerlo. Se valorará la utilización de la librería matplotlib de Python.

   * Determine el mejor candidato para el periodo de pitch localizando el primer máximo secundario de la
     autocorrelación. Inserte a continuación el código correspondiente.
   * Implemente la regla de decisión sonoro o sordo e inserte el código correspondiente.
    <img src="img/11.PNG" width="640" align="center">   
    <img src="img/12.PNG" width="640" align="center">
    <img src="img/13.PNG" width="640" align="center">


- Una vez completados los puntos anteriores, dispondrá de una primera versión del detector de pitch. El
  resto del trabajo consiste, básicamente, en obtener las mejores prestaciones posibles con él.

  * Utilice el programa `wavesurfer` para analizar las condiciones apropiadas para determinar si un
    segmento es sonoro o sordo.

	  - Inserte una gráfica con la detección de pitch incorporada a `wavesurfer` y, junto a ella, los
	    principales candidatos para determinar la sonoridad de la voz: el nivel de potencia de la señal
		(r[0]), la autocorrelación normalizada de uno (r1norm = r[1] / r[0]) y el valor de la
		autocorrelación en su máximo secundario (rmaxnorm = r[lag] / r[0]).

		Puede considerar, también, la conveniencia de usar la tasa de cruces por cero.

	    Recuerde configurar los paneles de datos para que el desplazamiento de ventana sea el adecuado, que
		en esta práctica es de 15 ms.

    <img src="img/16.PNG" width="640" align="center">
    <img src="img/17.PNG" width="640" align="center">

      - Use el detector de pitch implementado en el programa `wavesurfer` en una señal de prueba y compare
	    su resultado con el obtenido por la mejor versión de su propio sistema.  Inserte una gráfica
		ilustrativa del resultado de ambos detectores.

  * Optimice los parámetros de su sistema de detección de pitch e inserte una tabla con las tasas de error
    y el *score* TOTAL proporcionados por `pitch_evaluate` en la evaluación de la base de datos
	`pitch_db/train`..

  Para evaluar la base de datos primero adjuntamos el primer resultado de los parámetros sobre un archivo de audio aleatorio:
  <img src="img/26.png" width="640" align="center">

  Posteriormente, diferenciamos entre un audio de mujer y uno de hombre siendo primero el del hombre.
  <img src="img/27.png" width="640" align="center">
  <img src="img/23.png" width="640" align="center">

  Por último vemos el resumen con la base de datos entera.
  <img src="img/24.png" width="640" align="center">

  Al principio tuvimos un problema ya que no nos aparecia toda la base de datos, es decir, solo se nos ejectuban algunas. Finalmente, vimos que el error se encontraba en la librería libsndfile. Una vez hechas las comprobaciones y actualizado la libreria, vemos que se nos ejecuta toda la base de datos sin problemas:
  <img src="img/25.png" width="640" align="center">

  Para acabar, hemos juntado el pitch_evaluate en el run_get_pitch y guardado el resultado en un fichero de texto output.txt, presente también en el GitHub.

   * Inserte una gráfica en la que se vea con claridad el resultado de su detector de pitch junto al del
     detector de Wavesurfer. Aunque puede usarse Wavesurfer para obtener la representación, se valorará
	 el uso de alternativas de mayor calidad (particularmente Python).

     La decisión de éste umbral da el resultado que puede ser observado en las siguientes figuras, donde hemos querido comprobarlo tanto para un hombre como para una mujer.

     el pitch obtenido es  el  del  segundo  gráfico  desde  arriba,  mientras  que  el primero  es  el  pitch  creado  por  el  propio  wavesurfer  y  el tercer y cuarto gráficos son el pitch real del audio y la propia señal de audio respectivamente.

   HOMBRE:

   La primera gráfica nos muestra el pitch creado por el propio wavesurfer. La segunda y la quinta son gráficas con el pitch real y la propia señal de audio respectivamente y la tercera muestra el pitch obtenido con nuestra deicisión de umbral. Con el código que hemos adjuntado en una foto anterior, mostramos tambien la gráfica de potencia correspondiente al audio del hombre.

   <img src="img/18.PNG" width="640" align="center">
   <img src="img/20.PNG" width="640" align="center">

   MUJER:

   Primera gráfica es la potencia, obtenida de la misma manera que el anterior. La segunda es el pitch generado por wavesurfer. Luego tenemos el pitch del propio audio que lo comparamos con el de abajo que es el que hemos creado nosotras. Y por último, la muestra de audio de la mujer.

   <img src="img/19.PNG" width="640" align="center">
   <img src="img/21.PNG" width="640" align="center">


   Conclusiones:

   Hemos conseguido desarrollar un sistema de detección de pitch basado en la autocorrelación aun sabiendo que es un sistema
   que introduce errores en zonas de ruido cosa que afecta al obtener el grado de error.
   De todos modos, los resultados obtenidos son bastante positivos dado el alto porcentaje de éxito. Hemos tenido dificultades
   con el tema de ejecución de la práctica pero finalmente lo pudimos arreglar. Hemos aprendido mucho con esta práctica.


Ejercicios de ampliación
------------------------

- Usando la librería `docopt_cpp`, modifique el fichero `get_pitch.cpp` para incorporar los parámetros del
  detector a los argumentos de la línea de comandos.

  Esta técnica le resultará especialmente útil para optimizar los parámetros del detector. Recuerde que
  una parte importante de la evaluación recaerá en el resultado obtenido en la detección de pitch en la
  base de datos.

  * Inserte un *pantallazo* en el que se vea el mensaje de ayuda del programa y un ejemplo de utilización
    con los argumentos añadidos.

- Implemente las técnicas que considere oportunas para optimizar las prestaciones del sistema de detección
  de pitch.

  Entre las posibles mejoras, puede escoger una o más de las siguientes:

  * Técnicas de preprocesado: filtrado paso bajo, *center clipping*, etc.
  * Técnicas de postprocesado: filtro de mediana, *dynamic time warping*, etc.
  * Métodos alternativos a la autocorrelación: procesado cepstral, *average magnitude difference function*
    (AMDF), etc.
  * Optimización **demostrable** de los parámetros que gobiernan el detector, en concreto, de los que
    gobiernan la decisión sonoro/sordo.
  * Cualquier otra técnica que se le pueda ocurrir o encuentre en la literatura.

  Encontrará más información acerca de estas técnicas en las [Transparencias del Curso](https://atenea.upc.edu/pluginfile.php/2908770/mod_resource/content/3/2b_PS Techniques.pdf)
  y en [Spoken Language Processing](https://discovery.upc.edu/iii/encore/record/C__Rb1233593?lang=cat).
  También encontrará más información en los anexos del enunciado de esta práctica.

  Incluya, a continuación, una explicación de las técnicas incorporadas al detector. Se valorará la
  inclusión de gráficas, tablas, código o cualquier otra cosa que ayude a comprender el trabajo realizado.

  También se valorará la realización de un estudio de los parámetros involucrados. Por ejemplo, si se opta
  por implementar el filtro de mediana, se valorará el análisis de los resultados obtenidos en función de
  la longitud del filtro.


Evaluación *ciega* del detector
-------------------------------

Antes de realizar el *pull request* debe asegurarse de que su repositorio contiene los ficheros necesarios
para compilar los programas correctamente ejecutando `make release`.

Con los ejecutables construidos de esta manera, los profesores de la asignatura procederán a evaluar el
detector con la parte de test de la base de datos (desconocida para los alumnos). Una parte importante de
la nota de la práctica recaerá en el resultado de esta evaluación.
