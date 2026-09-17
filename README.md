# Qwen 3.6 35B-A3B vs Gemma 4 26B-A4B

Comparativa práctica de dos configuraciones de IA local que caben y funcionan en una **NVIDIA GeForce RTX 5070 de 12 GB**, usando `llama.cpp` y Llama UI.

> **Aviso importante:** Qwen usa `IQ2_XXS` y Gemma usa `Q4_K_M`. Además, los presets emplean temperaturas distintas (`0.6` y `1`). Esto **no es una comparación científica de arquitectura contra arquitectura** ni un benchmark con cuantizaciones equivalentes. Es una prueba reproducible de dos configuraciones reales que puedo ejecutar en 12 GB de VRAM.

## Resultado rápido

Los dos modelos superaron las cuatro pruebas: razonamiento, código, instrucciones en español y contexto. La diferencia clara apareció en la velocidad de generación.

| Prueba | Qwen 3.6 | Gemma 4 | Calidad observada |
|---|---:|---:|---|
| Razonamiento | 128,54 tok/s | 35,19 tok/s | Ambos correctos |
| Código | 100,60 tok/s | 37,75 tok/s | Ambos correctos |
| Instrucciones | 132,24 tok/s | 36,93 tok/s | Ambos correctos |
| Contexto | 133,87 tok/s | 33,46 tok/s | Ambos correctos |
| **Media simple** | **123,81 tok/s** | **35,83 tok/s** | **4/4 superadas por ambos** |

Qwen 3.6 35B-A3B — IQ2_XXS
Qwen_Qwen3.6-35B-A3B-IQ2_XXS.gguf — bartowski, 10,7 GB. Hugging Face

Gemma 4 26B-A4B-it — Q4_K_M
Gemma-4-26B-A4B-it-Q4_K_M.gguf — ggml-org, 16,8 GB. Hugging Face


En esta muestra, la media de Qwen fue aproximadamente **3,46 veces** la de Gemma, o un **245,6 % superior**. Esa diferencia pertenece a estas configuraciones concretas; no debe extrapolarse como una ventaja pura de arquitectura.

## Hardware y configuración

- GPU: NVIDIA GeForce RTX 5070, 12 GB VRAM (12.227 MiB mostrados por `nvidia-smi`)
- Sistema: Windows
- Backend: `llama.cpp` / `llama-server`
- Interfaz: Llama UI
- Contexto: 8.192 tokens
- Flash Attention: activado
- KV cache: `q8_0` para K y V
- GPU layers: automático
- Paralelismo: 1
- Reasoning: activado en las cuatro pruebas desde la interfaz

| Modelo | Cuantización | Temperatura | VRAM observada |
|---|---|---:|---:|
| Qwen 3.6 35B-A3B | IQ2_XXS | 0,6 | ~10,6–10,9 GB |
| Gemma 4 26B-A4B | Q4_K_M | 1,0 | ~11,2–11,7 GB |

## Reproducir la comparativa

1. Descarga tus propios archivos GGUF siguiendo [model-download-notes.md](model-download-notes.md).
2. Copia `comparison.ini` y sustituye las rutas de ejemplo por las rutas reales de tus modelos.
3. Inicia `llama-server`:

```powershell
& "C:\ruta\a\llama-server.exe" --config "C:\ruta\a\comparison.ini"
```

4. Abre Llama UI, selecciona cada preset, activa **Reasoning** y usa un chat nuevo para cada ejecución.
5. Copia sin cambios los prompts de `prompts/`.
6. Registra tokens/s, tiempo, tokens, VRAM y corrección de la salida.

Monitorización compacta de GPU:

```powershell
nvidia-smi --query-gpu=memory.used,memory.total,utilization.gpu,power.draw --format=csv,noheader,nounits -l 1
```

Consulta [setup/windows-powershell.md](setup/windows-powershell.md) para el proceso completo y [methodology.md](methodology.md) para las reglas de la prueba.

## Estructura

```text
.
├── README.md
├── comparison.ini
├── methodology.md
├── model-download-notes.md
├── setup/
│   └── windows-powershell.md
├── prompts/
│   ├── test-01-reasoning.txt
│   ├── test-02-code.txt
│   ├── test-03-spanish-instructions.txt
│   └── test-04-context.txt
└── results/
    ├── results.md
    └── results.csv
```

## Lectura correcta de los resultados

- Los resultados describen una RTX 5070 concreta y las versiones usadas durante la grabación.
- Las capturas de VRAM son lecturas puntuales, no necesariamente el pico absoluto.
- La cifra de tokens puede incluir razonamiento interno mostrado por Llama UI.
- En la prueba 4, la métrica de procesamiento del prompt de Qwen parecía afectada por caché o por la interfaz, así que no se publica.
- No se redistribuyen modelos, pesos, blobs ni archivos GGUF en este repositorio.

## Archivos clave

- [Configuración](comparison.ini)
- [Metodología](methodology.md)
- [Resultados detallados](results/results.md)
- [Datos en CSV](results/results.csv)
- [Descarga de modelos](model-download-notes.md)

**Menos humo. Más IA real.**
