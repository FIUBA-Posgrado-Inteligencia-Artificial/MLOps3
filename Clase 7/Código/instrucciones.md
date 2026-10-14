# jev-destilacion: instrucciones de inicio

Este documento acompaña al archivo `jev-destilacion.zip`. Indica cómo descomprimir el proyecto, ejecutarlo y comprobar que los resultados son correctos.

## Antes de empezar

Debe tener instalado lo siguiente:

| recurso | comprobación |
|---|---|
| Python 3.10 o una versión más reciente | `python3 --version` |
| Ollama, abierto | `ollama --version` |
| modelo de decisión | `ollama pull qwen2.5:1.5b-instruct` |
| modelo maestro | `ollama pull qwen2.5:3b-instruct` |

En Windows, use `python` en lugar de `python3`. El archivo `README.md`, incluido en el zip, explica cómo instalar cada recurso.

## 1. Descomprimir

Guarde `jev-destilacion.zip` en la carpeta Descargas y ejecute en una terminal:

```bash
cd ~/Downloads
unzip jev-destilacion.zip
cd jev-destilacion
```

En Windows, desde PowerShell:

```powershell
cd $HOME\Downloads
Expand-Archive jev-destilacion.zip -DestinationPath .
cd jev-destilacion
```

Para comprobar que está en la carpeta correcta, ejecute `ls` (o `dir` en Windows). Deben aparecer `README.md`, `probar.py` y los demás archivos del proyecto. Si en cambio aparece otra carpeta `jev-destilacion`, entre en ella con `cd jev-destilacion`.

## 2. Ejecutar

Ejecute los comandos en este orden:

```bash
python3 probar.py
python3 comparar.py
python3 calibrar.py
python3 correr.py
python3 monitorear.py semana_1
python3 destilar.py
JEV_MOTOR=destilado python3 calibrar.py
python3 correr.py
python3 monitorear.py semana_2
```

En Windows, reemplace la séptima línea por:

```powershell
$env:JEV_MOTOR="destilado"; python calibrar.py; Remove-Item Env:JEV_MOTOR
```

La ejecución completa toma entre cinco y quince minutos, según la computadora. El paso más lento es `destilar.py`, porque usa los dos modelos.

## 3. Resultados esperados

Los valores pueden variar un poco según la versión de Ollama y la computadora.

| comando | resultado esperado |
|---|---|
| `probar.py` | termina con `Todo listo` |
| `comparar.py` | chatml obtiene mayor accuracy que plano, cerca de 0.78 frente a 0.54 |
| `calibrar.py` | accuracy en prueba cercana a 0.70 y temperatura cercana a 1.1 |
| `correr.py` | la política equilibrada resuelve cerca de 37 de 50 tickets y escala el resto |
| `monitorear.py semana_1` | `No se detecta deriva` |
| `destilar.py` | el alumno ahorra cerca de 44% de tokens, pero lo más probable es `No se adopta`, porque escala más tickets y el costo total sube |
| `JEV_MOTOR=destilado python3 calibrar.py` | si el alumno no se adoptó, se detiene con un aviso; continúe con los dos comandos siguientes |
| `monitorear.py semana_2` | `Hay deriva` en las señales de entrada, por los tickets en portugués, los pagos con QR y los correos largos |

La deriva en `semana_2` es intencional: ese lote se diseñó para que el monitoreo la detecte.

En `correr.py`, los borradores los redacta un modelo pequeño y a veces salen pobres o fuera de tema. No es un error del proyecto.

## 4. Notebooks (opcional)

`entender_jev.ipynb` introduce el método de Jev con un ejemplo de reseñas de productos y conviene revisarlo primero. `explorar.ipynb` ejecuta los pasos del proyecto y muestra los resultados intermedios. Desde la carpeta del proyecto:

```bash
python3 -m pip install notebook
jupyter notebook entender_jev.ipynb
jupyter notebook explorar.ipynb
```

En el navegador, use el menú **Kernel** y la opción **Restart Kernel and Run All Cells**.

## Si algo falla

Consulte la sección de problemas frecuentes de `README.md`. Si el error no aparece ahí, copie el mensaje completo de la terminal y compártalo.
