# Modelos de predicción de PM₂,₅ para Bogotá

## Alcance

Conservación y trazabilidad de los modelos finales del Objetivo específico 2 y los artefactos de validación, comparación y selección del Objetivo específico 3. El estudio compara cinco estrategias para 19 estaciones y horizontes de 1, 6 y 24 horas; la selección final define 57 asignaciones de expertos por estación y horizonte.

## Modelos finales

| Estrategia | Familia | Archivos |
|---|---|---|
| M1 | iTransformer | Modelo I3 `.keras`, checkpoint final, configuración y cierre |
| M2 | PatchTST | Modelo P3 `.keras`, checkpoint final, configuración y cierre |
| M3 | LSTM especializada | 19 checkpoints `l3_checkpoint_<estacion>.weights.h5` |
| M4 | LSTM global | `l3_checkpoint_global.weights.h5` |
| M5 | XGBoost especializado | 57 modelos `x3_modelo_<estacion>_<horizonte>.ubj` |

Estas estrategias comprenden 79 instancias finales: dos Transformer, 20 LSTM y 57 XGBoost. Se conservan sus soportes finales disponibles y huellas asociadas. Los modelos base, checkpoints temporales y archivos de progreso quedan en los archivos originales.

## Índice y orden de consulta

| Orden | Carpeta o etapa | Contenido |
|---|---|---|
| 1 | `Catalogo_repositorio.csv` | Listado individual de archivos, función, tamaño en bytes y SHA-256 |
| 2 | `Entrenamiento/` | Etapas finales I3, P3, L3 y X3: modelos, configuraciones, cierres y soportes de validación del desarrollo |
| 3 | `Validacion_comparacion/`, V0–V1 | Cierres de preparación de la evaluación |
| 4 | V2 y V2.5 | Predicciones de cinco estrategias, tabla común y cierres |
| 5 | V3.1–V3.4 | Métricas generales, concentraciones altas, detección y síntesis de red |
| 6 | V4.1–V4.3 | Competencias, votos y comparación LSTM especializada/global |
| 7 | V4.4–V4.5 | Selección de expertos, composición final y síntesis del híbrido |
| 8 | V4.6 | Híbrido final, métricas finales, inventario, resumen ejecutivo y cierre |

La validación conserva 47 archivos: 42 de V0–V4.5 y cinco adicionales de V4.6. El catálogo proporciona el detalle de cada archivo. Los notebooks y HTML quedan excluidos por decisión del autor.

## Interpretación y reproducibilidad

Las predicciones de validación en Entrenamiento corresponden al desarrollo de los modelos. Los resultados del Objetivo específico 3 se consultan en Validacion_comparacion, distinguiendo particiones y agregaciones según sus cierres.

Este paquete permite inspeccionar resultados persistidos y verificar su integridad. La reproducción de la inferencia requiere las entradas preparadas, el orden de variables, las máscaras, las transformaciones, el código de construcción de las arquitecturas y un entorno compatible. Los pesos Keras requieren reconstruir la arquitectura; los archivos `.keras` pueden requerir clases personalizadas. La conservación de estos artefactos sustenta la trazabilidad, mientras que la ejecución completa depende también de esos insumos y del código del proyecto.

## Formatos

| Formato | Uso |
|---|---|
| `.parquet` | Tablas y resultados |
| `.npy` | Arreglos de predicciones, objetivos y presencia |
| `.pkl` | Cierres y soportes serializados |
| `.json` | Configuraciones |
| `.weights.h5` | Pesos Keras |
| `.keras` | Modelos serializados |
| `.ubj` | Modelos XGBoost |
| `.sha256` | Huellas de integridad |

## Integridad

Las huellas del catálogo fueron calculadas por el autor en su equipo. El catálogo selecciona entradas de los dos inventarios; la comprobación de los binarios se realiza desde la raíz del repositorio:

```powershell
$errores = @(Import-Csv '.\Catalogo_repositorio.csv' | ForEach-Object { $p = Join-Path (Get-Location).Path $_.Ruta_repositorio; if (!(Test-Path -LiteralPath $p -PathType Leaf)) { "Falta: $($_.Ruta_repositorio)" } elseif ((Get-FileHash -LiteralPath $p -Algorithm SHA256).Hash.ToLowerInvariant() -ne $_.SHA256.ToLowerInvariant()) { "SHA-256 diferente: $($_.Ruta_repositorio)" } }); if ($errores.Count) { $errores; throw 'Se detectaron diferencias.' } else { 'Integridad verificada.' }
```

## Fuentes

Catálogos de Entrenamiento y Validacion_comparacion, informes técnicos y ejecutivos de iTransformer, PatchTST y LSTM, listado actualizado de actividades de XGBoost y documentación del proceso de validación y comparación. La selección distingue las etapas finales de las ejecuciones base y de optimización.

## Inventario seleccionado

184 archivos de entrenamiento y 47 de validación: **231 archivos**, equivalentes a 299.727 MiB. Estos conteos incluyen soportes y huellas, además de los modelos.
