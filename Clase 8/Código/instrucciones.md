# Text-to-SQL con RFT: cómo usarlo en la computadora

Se necesita Python 3.10 o superior y alrededor de 3 GB libres. No hace falta GPU.

## 1. Descargar y descomprimir

Descargar `text-to-sql-llmops.zip` de https://drive.google.com/file/d/1NVw3jrJMCGFygarJjEDZBQoz5UxSI2G8/view?usp=sharing, descomprimirlo y abrir una terminal en la carpeta `text-to-sql-llmops`:

```
cd ruta/a/text-to-sql-llmops
```

## 2. Crear el entorno (una sola vez)

**Mac / Linux**
```
python3 -m venv venv
source venv/bin/activate
pip install torch
pip install -r requirements.txt
```

**Windows**
```
python -m venv venv
venv\Scripts\activate
pip install torch --index-url https://download.pytorch.org/whl/cpu
pip install -r requirements.txt
```

En Linux, cambiar `pip install torch` por `pip install torch --index-url https://download.pytorch.org/whl/cpu`.

## 3. Preparar los datos

```
python 1_preparar_datos.py
```
Debe mostrar `versión de datos: c4d9e2b6`.

## 4. Registrar, evaluar y promover las versiones

Copiar los modelos ya entrenados a la carpeta del proyecto:

```
cp modelos_entrenados/*.zip .                  # mac / linux
copy modelos_entrenados\*.zip .                # windows
```

Modelo original (v0), como línea base:
```
python 3_registrar.py
python 4_evaluar.py v0
python 5_promover.py v0
```

Las tres versiones entrenadas (en Mac o Linux, todas de una vez):
```
for v in 1 2 3; do python 3_registrar.py modelo_v$v.zip && python 4_evaluar.py v$v && python 5_promover.py v$v; done
```
En Windows, ejecutar las tres líneas para cada versión, cambiando el número (`modelo_v1.zip` y `v1`, después `v2` y `v3`).

La primera evaluación descarga el modelo base (alrededor de 1 GB).

## 5. Servir, simular tráfico y monitorear

Terminal 1 (dejarla abierta):
```
python 6_servir.py
```

Terminal 2 (activar antes el entorno, como en el paso 2):
```
python 7_simular_trafico.py
python 8_monitorear.py
```

## 6. Ver los resultados

```
jupyter notebook resultados.ipynb
```

## Resultados esperados

| versión | exactitud | compuerta |
|---|---|---|
| v0 | 28 % | línea base |
| v1 | 94 % | aprobada |
| v2 | 89 % | rechazada (empeora en `agregar`) |
| v3 | 94 % | aprobada, queda en producción |

Monitoreo con v3: 17 consultas ok, 1 bloqueada y 0 errores.

## Si algo falla

- **`ModuleNotFoundError`:** el entorno no está activado o la terminal no está en la carpeta del proyecto.
- **"el modelo original ya está registrado":** para empezar de cero, `rm -rf registro logs` (en Windows, borrar esas dos carpetas).
- **`address already in use`:** quedó una API abierta. Cerrar esa terminal o, en Mac o Linux, ejecutar `lsof -ti :8000 | xargs kill`.
- **Error de conexión en el paso 7:** la API del paso 5 no está corriendo.

Para entrenar los modelos uno mismo en Colab, ver `README.md`.
