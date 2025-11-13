# Lab5  Variabilidad de la Frecuencia Cardiaca usando la Transformada Wavelet

## INTRODUCCIÓN

Mediante el desarrollo del presente informe, se presenta la realización de la práctica de laboratorio enfocada en el análisis de la variación del ritmo cardíaco (HRV), con el objetivo de comprender cómo responde el sistema nervioso autónomo ante diferentes estímulos fisiológicos. Durante la práctica, se utilizó el software Python junto con sensores de pulso para registrar la señal cardíaca en distintas condiciones como: en reposo, durante una respiración controlada, ejecutando la maniobra de Valsalva y en el periodo de recuperación después del estres. A partir de estas señales se analizaron los intervalos entre latidos consecutivos, conocidos como intervalos R-R, que permiten observar la variabilidad cardíaca. Este parámetro es fundamental para evaluar el equilibrio entre las ramas simpática y parasimpática del sistema nervioso, ya que una mayor variabilidad indica una buena adaptación del organismo, mientras que una baja variabilidad puede estar relacionada con estrés o disfunción autonómica. El propósito de este laboratorio es desarrollar habilidades en la adquisición y análisis de señales biológicas, interpretando la HRV como una herramienta útil para estudiar la actividad del sistema nervioso en tiempo real bajo diversas condiciones fisiológicas.

<img width="827" height="1280" alt="image" src="https://github.com/user-attachments/assets/8380f123-c713-46dc-8c4c-355838946d8a" />
|Fig 1 : Diagrama de Flujo - Procedimiento de elaboracion de la guia de laboratorio.|

## ANALISIS Y RESULTADOS
# a. Fundamento teórico:

Sistema Nervioso Autónomo (SNA): El SNA regula funciones automáticas del cuerpo, como la frecuencia cardíaca, la respiración y la digestión. Se divide en:

-Simpático: activa el cuerpo ante el estrés (acelera el corazón).
-Parasimpático: promueve el descanso y la recuperación (ralentiza el corazón).
-El equilibrio entre ambos se refleja en la variabilidad de la frecuencia cardíaca.

 Variabilidad de la Frecuencia Cardíaca (HRV)
La HRV es la variación en el tiempo entre latidos consecutivos (intervalos R-R en un ECG).

-Alta HRV: indica buena adaptación del sistema cardiovascular y predominio parasimpático.
-Baja HRV: puede reflejar estrés, fatiga o riesgo cardiovascular. Se puede analizar en el dominio del tiempo (estadísticas simples) o en el dominio de la frecuencia (análisis espectral).

 Transformada Wavelet
Es una herramienta que permite analizar señales no estacionarias (como el ECG) descomponiéndolas en tiempo y frecuencia simultáneamente. A diferencia de la transformada de Fourier, la wavelet puede detectar cambios transitorios, lo que la hace ideal para estudiar cómo varían las frecuencias (LF y HF) de la HRV a lo largo del tiempo.

# b. Adquisición de la señal ECG:

Para la obtención de la señal ECG se utilizó un sistema de adquisición de datos DAQ6002, conectado a tres electrodos de superficie colocados según la configuración estándar de derivación Einthoven (brazos derecho, izquierdo y pierna derecha como referencia o tierra). La señal fue registrada durante 5 minutos en condiciones de reposo, con el sujeto sentado de manera relajada y respirando de forma natural, en un ambiente controlado para minimizar interferencias eléctricas. La señal se muestreó a una frecuencia de [1000 Hz], lo cual permite una adecuada resolución temporal para el análisis de los complejos QRS. Los datos fueron almacenados y posteriormente procesados en Python para el análisis de la variabilidad de la frecuencia cardíaca (HRV).

--> En esta primera parte del código se realiza la carga e inicialización de los datos de la señal ECG. Primero se importan las librerías necesarias: pandas para manejar archivos de Excel y estructuras de datos tipo DataFrame; matplotlib.pyplot para realizar gráficos; numpy para cálculos numéricos eficientes; scipy.signal para procesamiento de señales, y pywt para aplicar transformadas wavelet. Luego, se especifica la ruta del archivo Excel que contiene la señal ECG y se carga en un DataFrame usando pd.read_excel(). A partir de este archivo, se extraen dos columnas: la primera (df.iloc[:, 0].values) representa el tiempo en milisegundos o segundos, y la segunda (df.iloc[:, 1].values) contiene la señal cruda del ECG, es decir, los valores eléctricos medidos desde el corazón. Finalmente, se define la frecuencia de muestreo (fs = 1000), lo que indica que la señal fue registrada a mil muestras por segundo. Esto será esencial más adelante para convertir índices de muestras en tiempo real, calcular frecuencias y diseñar filtros:











|Fig 2 : Señal original.|

# Analisis 
La señal mostrada en la imagen corresponde a la ECG que al momento se encuentra sin procesar, tal como fue adquirida directamente del sistema de adquisición (DAQ6002). En ella se puede observar un alto nivel de ruido, con fluctuaciones rápidas y amplitudes que oscilan aproximadamente entre 1 y 5 mV, lo cual no es típico de una señal ECG fisiológica normal. Esta distorsión puede deberse a interferencias eléctricas, artefactos por movimiento o actividad muscular, lo que impide que podamos identificar con claridad los componentes característicos del ciclo cardíaco en este caso, como los complejos QRS. Esta visualización inicial requiere la necesidad de aplicar técnicas de preprocesamiento, como el filtrado pasabanda (Butterworth) y la filtración por mediana, que serán implementadas en las etapas siguientes del código para limpiar la señal y permitir una detección precisa de los picos R.

Ahora bien, En esta siguiente sección del código se realiza el preprocesamiento de la señal ECG, lo cual es esencial para eliminar ruidos y preparar la señal para un análisis más preciso.

# c. Pre - procesamiento de la señal:
▪️ 1. Filtro IIR Butterworth
Primero se diseña un filtro pasa banda Butterworth de cuarto orden para dejar pasar únicamente las frecuencias relevantes del ECG, que suelen estar entre 0.5 Hz y 35 Hz. Estos valores se eligen porque:

El componente útil del ECG, como las ondas P, QRS y T, se encuentra en ese rango.
Frecuencias menores pueden contener ruido de movimientos (muy lento), y mayores a 35 Hz pueden incluir interferencias de alta frecuencia o del ambiente. --> A demas, usando la función predefinida "scipy.signal.bilinear" para obtener la transformación bilineal de una función de transferencia analógica, se obtuvo los coeficientes de entrada (b = [ 1.1951712e-06, -1.1923701e-06, -1.1923701e-06, 1.1951712e-06 ]) y los coeficientes de salida (a = [ 1.0, -2.98538391, 2.97112673, -0.98574185 ]) quedando así la ecuación en diferencia de la siguiente manera:











|Fig 4 : Señal filtrada.|
# Analisis







Posterior a lo anterior, se realiza un suavizado adicional de la señal ECG utilizando un filtro de promedio móvil, con el objetivo de reducir pequeñas oscilaciones residuales y dejar la señal más limpia para la detección de eventos cardíacos importantes.

Primero, se define el tamaño de la ventana de suavizado como 0.03 * fs, es decir, 30 milisegundos de duración. Como la señal está muestreada a 1000 Hz (fs = 1000), eso equivale a 30 muestras. Luego, se aplica el suavizado con np.convolve(), una operación que recorre la señal y calcula el promedio de los valores dentro de esa ventana. Aquí se usa np.ones(window_size)/window_size para crear un filtro promedio, lo que significa que todos los valores dentro de la ventana tienen el mismo "peso" y por ultimo, La opción mode='same' asegura que la salida tenga el mismo tamaño que la señal original. Este suavizado actúa como un “pulido” final para la señal ya filtrada, de esta manera eliminando pequeñas variaciones que podrían confundir al algoritmo de detección de picos R (latidos) más adelante.

Siguiendo con el codigo, tenemos:
▪️ Detección de picos R
Aquí se utiliza la función signal.find_peaks() de SciPy para identificar los picos prominentes de la señal suavizada, que corresponden a los picos R del ECG (los eventos más sobresalientes en cada ciclo cardíaco).

El parámetro distance se fija en int(0.6 * fs), pues es un intervalo mínimo de 600 milisegundos (0.6 segundos) entre picos, y en parte es equivalente a una frecuencia cardíaca máxima de 100 latidos por minuto, pero ¿Que cual es su funcion? pues, este valor actúa como filtro para evitar detectar falsos positivos (picos que no son R reales).
El parámetro height=np.max(smoothed_ecg)*0.35 indica que solo se considerarán picos con una altura mínima del 35% del valor máximo de la señal suavizaday esto se hace con el finde que esto descarta pequeños picos de ruido o de otras ondas (como T o P) que no corresponden al complejo QRS; y como resultado es un array peaks que contiene los índices de los picos R detectados en la señal temporal.
▪️5. Cálculo de intervalos R-R
Una vez localizados los picos R, se calculan los intervalos R-R como la diferencia entre los tiempos de cada pico consecutivo, usando np.diff(t[peaks]). Estos intervalos representan el tiempo entre un latido y el siguiente, medidos en segundos. Tambien, se usa np.diff() para calcular la diferencia entre elementos consecutivos del array t[peaks], que contiene los tiempos asociados a cada pico R. Además, se incluye una verificación con if len(peaks) > 1 para asegurarse de que al menos haya dos picos y así poder calcular al menos un intervalo. Si no se encuentran suficientes picos, se muestra un mensaje y se asigna un array vacío a rr_intervals.

# d. Análisis de la HRV en el dominio del tiempo:

▪️ 6. Crear señal con información de picos R
Para la siguiente parte, se construye un nuevo arreglo llamado r_peak_signal, que tiene la misma longitud que la señal suavizada pero está relleno con ceros. Luego, en los índices correspondientes a los picos R detectados, se colocan los valores reales de la señal. Pues, este paso permite crear una señal que solo contiene los picos R, facilitando su análisis individual o la visualización superpuesta. No altera la señal original, sino que crea una representación enmascarada donde los únicos puntos no nulos son los picos detectados.

▪️7. Visualización principal con picos R
Aquí se crea una gráfica en la que se muestra:








|Fig 5 : Señal filtrada teniendo en cuenta cada Intervalo R-R.|


Bien, el siguiente segmento del código se dedica al análisis de los intervalos R-R en el dominio del tiempo, es decir, al estudio de la variabilidad del ritmo cardíaco (HRV) a partir de los tiempos entre latidos consecutivos, extraídos de los picos R previamente detectados.
Este análisis en el dominio del tiempo es esencial en el estudio del ritmo cardíaco porque permite:

Se grafica cada intervalo R-R en función del tiempo, lo cual permite observar cómo varían los latidos con el tiempo.
Detectar patrones de bradicardia (frecuencia muy baja) o taquicardia (frecuencia alta).
Evaluar la variabilidad de la frecuencia cardíaca (HRV), que es un indicador importante del estado del sistema nervioso autónomo y de la salud cardiovascular.
Identificar posibles irregularidades o arritmias.
Es decir, este bloque proporciona una caracterización cuantitativa y visual de la dinámica cardíaca a partir de los intervalos R-R, facilitando la interpretación clínica o científica del comportamiento del corazón.










|Fig 6 : Grafico creado a partir de los intervalos hallados entre los R-R (HRV).|
#Analisis 






# e. Aplicación de transformada Wavelet:
Se realiza un análisis espectral de la variabilidad de la frecuencia cardíaca (HRV) en el dominio tiempo-frecuencia mediante la Transformada Wavelet Continua (CWT). Evaluar cómo varía la energía (amplitud) en distintas bandas de frecuencia del ritmo cardíaco a lo largo del tiempo. Esto es útil para poder identificar la actividad del sistema nervioso simpático y parasimpático y tambien, poder analizar la HRV en condiciones de no estacionariedad, algo en lo que las wavelets sobresalen frente al análisis de Fourier que hemos trabajado anteriormente.
Y teniendo en cuenta que se asume una frecuencia de muestreo constante de 1 Hz sobre la serie de intervalos R-R, lo cual es una simplificación válida si los intervalos están más o menos espacioados significativamente.
Todo esto, mostrando un espectrograma con amplitud de cada frecuencia en cada instante de tiempo.

Es importante aclarar que este espectrograma con CWT es una herramienta para poder observar cómo varía la HRV en el tiempo con mucho más detalle que el análisis de frecuencia clásico, a parte es útil en estudios médicos, de estrés o de calidad del sueño.








|Fig 8 : Resultados --> Transformada de Wavelet.|
 # Analisis 






## CONCLUSIONES: 



