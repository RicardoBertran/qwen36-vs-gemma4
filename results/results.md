# Resultados medidos

## Resumen

| Prueba | Qwen tok/s | Gemma tok/s | Qwen frente a Gemma | Resultado funcional |
|---|---:|---:|---:|---|
| 1. Razonamiento | 128,54 | 35,19 | +265,3 % | Ambos correctos |
| 2. Código | 100,60 | 37,75 | +166,5 % | Ambos correctos |
| 3. Instrucciones | 132,24 | 36,93 | +258,1 % | Ambos correctos |
| 4. Contexto | 133,87 | 33,46 | +300,1 % | Ambos correctos |
| **Media simple** | **123,81** | **35,83** | **+245,6 %** | **4/4 ambos** |

El porcentaje se calcula como `(Qwen / Gemma - 1) × 100`. Describe estas ejecuciones, no una ventaja arquitectónica aislada.

## Detalle de mediciones

| Prueba | Modelo | Generación | Tiempo | Tokens mostrados | Prompt tok/s | VRAM observada |
|---|---|---:|---:|---:|---:|---:|
| Razonamiento | Qwen | 128,54 tok/s | 14 s | 1.885 | 514,93 | 10.634 MiB |
| Razonamiento | Gemma | 35,19 tok/s | 59 s | 2.079 | 102,77 | 11.189 MiB |
| Código | Qwen | 100,60 tok/s | 41 s | 4.128 | 556,72 | 10.854 MiB |
| Código | Gemma | 37,75 tok/s | 66 s | 2.522 | 113,14 | 11.284 MiB |
| Instrucciones | Qwen | 132,24 tok/s | 16 s | 2.231 | — | 10.895 MiB |
| Instrucciones | Gemma | 36,93 tok/s | 119 s | 4.408 | — | 11.691 MiB |
| Contexto | Qwen | 133,87 tok/s | 12 s | 1.639 | No publicado | No registrado |
| Contexto | Gemma | 33,46 tok/s | 41 s | 1.389 | — | No registrado |

## Resultado funcional

### Prueba 1

Ambos devolvieron:

```text
RESULTADO: Prioritarios=90, A=180, B=135, C=210, D=105
```

Cumplieron suma, formato y límite de palabras.

### Prueba 2

Ambos implementaron una ventana deslizante O(n), manejaron negativos, conservaron el primer empate, validaron `k` y no modificaron la entrada.

### Prueba 3

Ambos ignoraron “PASTEL”, asignaron prioridad ALTA, extrajeron las tres tareas y respetaron las cuatro líneas con las palabras obligatorias.

### Prueba 4

Ambos devolvieron:

```text
SALA: Sala Sur
ASISTENTES: 150
CATERING: 4800 euros
RESPONSABLE: Diego
MICROFONOS: 4
LLEGADA_TECNICOS: 16:30
```

## Notas de interpretación

- Qwen: IQ2_XXS. Gemma: Q4_K_M.
- Qwen: temperatura 0,6. Gemma: temperatura 1.
- Las cifras de tokens mostrados parecen incluir razonamiento interno en Llama UI.
- La VRAM corresponde a lecturas visibles al finalizar o durante la generación, no a un muestreo de pico automatizado.
- La cifra de procesamiento del prompt de Qwen en la prueba 4 no se usa porque la interfaz mostró un valor incompatible con el prompt, posiblemente por caché.
