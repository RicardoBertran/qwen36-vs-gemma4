# Puesta en marcha en Windows y PowerShell

## Requisitos

- Windows con controladores NVIDIA instalados.
- Una compilación reciente de `llama.cpp` con CUDA.
- `llama-server.exe` disponible localmente.
- Llama UI configurada para conectarse al servidor.
- Los dos GGUF descargados por el usuario. Este repositorio no incluye pesos.

## 1. Preparar el INI

Copia `comparison.ini` a una ubicación cómoda y cambia las dos líneas `m = ...` para que apunten a tus archivos reales. No copies rutas ajenas literalmente.

Comprueba que los nombres conservan los guiones bajos, por ejemplo `IQ2_XXS`, `Q4_K_M` y `q8_0`.

## 2. Lanzar llama-server

Ejemplo directo:

```powershell
& "C:\ruta\a\llama-server.exe" --config "C:\ruta\a\comparison.ini"
```

Si tu compilación utiliza otro nombre para cargar archivos de configuración, consulta `llama-server.exe --help` y adapta únicamente ese argumento. La configuración del benchmark está en `comparison.ini`.

## 3. Conectar Llama UI

1. Abre Llama UI.
2. Conecta la interfaz al endpoint de `llama-server` que indique la consola.
3. Selecciona uno de estos presets:
   - `qwen3.6-35b-a3b-iq2-xxs-5070`
   - `gemma4-26b-a4b-5070`
4. Activa **Reasoning** para las cuatro pruebas.
5. Usa una conversación nueva antes de cada prompt.

## 4. Monitorizar la GPU

Vista compacta, actualizada cada segundo:

```powershell
nvidia-smi --query-gpu=memory.used,memory.total,utilization.gpu,power.draw --format=csv,noheader,nounits -l 1
```

Vista completa:

```powershell
nvidia-smi -l 1
```

Detén la actualización con `Ctrl+C`.

## 5. Ejecutar las pruebas

Usa los prompts en orden y sin editarlos:

```text
prompts/test-01-reasoning.txt
prompts/test-02-code.txt
prompts/test-03-spanish-instructions.txt
prompts/test-04-context.txt
```

Para cada modelo registra:

- tokens por segundo de generación;
- tiempo de generación;
- tokens mostrados por la interfaz;
- VRAM al finalizar o durante la generación;
- cumplimiento de la respuesta esperada.

## 6. Comprobaciones si no cabe

- Cierra aplicaciones que consuman VRAM.
- Confirma que `flash-attn = on` y el KV está en `q8_0`.
- Revisa en la consola cuánto se ha descargado a GPU y cuánto queda mapeado en CPU.
- No cambies cuantización, contexto o sampling y presentes el resultado como si fuera la misma prueba: documenta cualquier cambio.

Estas configuraciones rozan el límite de una GPU de 12 GB. El consumo exacto depende de la compilación, el controlador, la interfaz y otros procesos activos.
