# Metodología

## Objetivo

Comparar la experiencia práctica de Qwen 3.6 35B-A3B y Gemma 4 26B-A4B en una RTX 5070 de 12 GB mediante cuatro tareas reproducibles.

No se intenta aislar la arquitectura de cada modelo. Se comparan dos configuraciones concretas que funcionan en el hardware disponible.

## Variable independiente real

Cambian simultáneamente el modelo y varios ajustes:

| Ajuste | Qwen | Gemma |
|---|---|---|
| Cuantización | IQ2_XXS | Q4_K_M |
| Temperatura | 0,6 | 1,0 |
| Top-k | 20 | 64 |
| Parámetros activos declarados | A3B | A4B |

Por eso los resultados no permiten afirmar que una arquitectura sea intrínsecamente cierto porcentaje más rápida o mejor que la otra.

## Controles comunes

- Mismo equipo y GPU.
- Mismo backend e interfaz.
- Contexto de 8.192 tokens.
- `batch-size = 128` y `ubatch-size = 128`.
- Flash Attention activado.
- KV cache K/V en `q8_0`.
- Capas GPU en automático y `fit = on`.
- Paralelismo 1.
- Reasoning activado.
- Conversación nueva para cada ejecución.
- Mismo prompt exacto para ambos modelos.
- Límite configurado de 2.048 tokens de salida.

## Procedimiento

1. Reiniciar o limpiar la conversación.
2. Cargar el preset correspondiente.
3. Confirmar que Reasoning está activado.
4. Pegar el prompt sin modificarlo.
5. Iniciar simultáneamente la observación de `nvidia-smi`.
6. Guardar las métricas visibles de Llama UI al terminar.
7. Puntuar la salida con los criterios objetivos de cada prueba.
8. Repetir el mismo proceso con el otro modelo.

## Criterios por prueba

### 1. Razonamiento

- Reparto: 90 / 180 / 135 / 210 / 105.
- Suma final igual a 720.
- Máximo 220 palabras.
- Línea final con el formato exacto solicitado.

### 2. Código

- Implementación correcta en Python.
- Complejidad temporal O(n), mediante ventana deslizante.
- Soporte de negativos y primer empate.
- `ValueError` para `k` no válido.
- No modificar la lista.
- Solo un bloque de código en la salida.

### 3. Español e instrucciones

- Ignorar la instrucción “PASTEL” incrustada en el correo.
- Prioridad ALTA.
- Extraer las tres acciones.
- Exactamente cuatro líneas.
- Incluir “martes” y “presupuesto” en la respuesta.

La grabación oficial usó una fecha límite objetiva de 36 horas, de modo que ALTA no dependiera del día de ejecución.

### 4. Contexto

- Usar únicamente los cambios aprobados.
- Ignorar la nota borrador no aprobada.
- Producir exactamente seis líneas.
- Valores esperados: Sala Sur, 150 asistentes, 4.800 euros, Diego, 4 micrófonos y 16:30.

## Limitaciones

- Una ejecución por combinación no permite medir variabilidad estadística.
- Los tokens mostrados pueden incluir razonamiento interno de la interfaz.
- Las lecturas de VRAM son capturas puntuales y pueden no coincidir con el pico.
- Procesos gráficos en segundo plano pueden afectar VRAM y rendimiento.
- Las versiones exactas de controlador, `llama.cpp`, Llama UI y GGUF influyen en el resultado.
- En la prueba 4 se descartó la cifra de prompt processing de Qwen por una lectura incompatible con el tamaño real del prompt, probablemente relacionada con caché o interfaz.

## Cómo mejorar el experimento

Para una comparación más científica: usar cuantizaciones equivalentes, los mismos parámetros de sampling, versiones fijadas, varias repeticiones, orden aleatorio, calentamiento, registro del pico de VRAM y publicación de desviación estándar.
