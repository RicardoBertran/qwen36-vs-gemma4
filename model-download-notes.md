# Notas para descargar los modelos

Este repositorio **no incluye ni redistribuye archivos GGUF, pesos, blobs ni modelos**.

## Archivos usados en la comparativa

Los presets esperan archivos equivalentes a:

```text
Qwen_Qwen3.6-35B-A3B-IQ2_XXS.gguf
gemma-4-26B-A4B-it-Q4_K_M.gguf
```

Los nombres exactos pueden variar según la persona u organización que haya generado la cuantización.

## Descarga responsable

1. Localiza la página oficial del modelo base y lee su licencia.
2. Localiza una conversión GGUF de una fuente que identifique con claridad el modelo base, la cuantización y las herramientas usadas.
3. Para Qwen, elige **IQ2_XXS**. Para Gemma, elige **Q4_K_M** si quieres reproducir esta comparativa.
4. Revisa si el repositorio del GGUF incluye varios shards y descarga todos los necesarios.
5. Verifica tamaño, hash si está disponible y compatibilidad con tu versión de `llama.cpp`.
6. Guarda los modelos fuera del repositorio y edita las rutas `m = ...` en `comparison.ini`.

## Qué mantener fuera de la carpeta de la comparativa

- `*.gguf`
- directorios de blobs o caché de modelos;
- tokens de acceso;
- archivos sujetos a condiciones de redistribución;
- rutas personales innecesarias.

Antes de usar los modelos, revisa la licencia y las condiciones del modelo base y de la conversión GGUF elegida. Las licencias pueden ser diferentes entre Qwen, Gemma y cada distribuidor de cuantizaciones.
