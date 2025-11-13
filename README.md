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
Es una herramienta que permite analizar señales no estacionarias (como el ECG) descomponiéndolas en tiempo y frecuencia simultáneamente. A diferencia de la transformada de Fourier, la wavelet puede detectar cambios transitorios, lo que la hace ideal para estudiar cómo varían las frecuencias (LF y HF) de la HRV a lo largo del tiemp
