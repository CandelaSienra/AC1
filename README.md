# Actuación en clase - GitHub Actions y Docker (DEMO)

## Objetivo

Diagnosticar y corregir los problemas en un pipeline de CI/CD existente que utiliza GitHub Actions y Docker para conseguir un flujo de integración y despliegue funcional.

## Contexto

Se te ha entregado un repositorio con una aplicación web simple y un pipeline de CI/CD implementado con GitHub Actions y Docker. Sin embargo, el pipeline tiene varios problemas que debes identificar y corregir para que funcione correctamente.

## Pistas

Hay **10 problemas** repartidos en el repositorio. Estas pistas te orientan, pero no te dicen exactamente qué cambiar: los logs de la pestaña "Actions" y probar `pytest`, `docker build` y `docker run` en tu máquina son tus mejores aliados.

### 1. Integración Continua (4 problemas)

- Haces push a la rama principal y no pasa nada. ¿A qué ramas está escuchando realmente el pipeline?
- YAML no siempre interpreta los valores como los escribiste: un número puede no ser el número que crees.
- Un archivo que no existe no se puede instalar. Lee los nombres con atención.
- pytest busca los tests donde le dices, no donde están.

### 2. Docker (3 problemas)

- No todas las versiones de Python soportan las dependencias del proyecto. ¿Qué versión mínima piden?
- Cada `COPY` parte del contexto de build. ¿Dónde está realmente cada archivo dentro del repositorio?
- Después de `WORKDIR` y `COPY`, ¿dónde quedó cada archivo dentro del contenedor?

### 3. APP / Testing (3 problemas)

> **Nota:** el archivo `app/main.py` no debe modificarse. Los demás archivos deben corregirse para que sean consistentes con su implementación.

- Hay objetos que se usan llamándolos, no solo nombrándolos.
- Un test solo tiene sentido si prueba un endpoint que la app realmente expone.
- Compara lo que el test espera con lo que la app devuelve, carácter por carácter.

## Estructura del repositorio

```
├── app/                    # Código fuente de la aplicación
│   ├── main.py             # Aplicación simple en Python
│   └── test_main.py        # Pruebas unitarias
├── .github/
│   └── workflows/          # Contiene el workflow ci-cd.yml (tiene errores)
├── .dockerignore           # Archivos excluidos del build de Docker
├── .gitignore              # Archivos excluidos del repositorio
├── Dockerfile              # Dockerfile 
├── README.md               # Este archivo
└── requirements.txt        # Dependencias de la aplicación
```

## Evaluación

Cuando el pipeline termina correctamente, el último job (`Publish evidence`) publica automáticamente una evidencia con tu usuario de GitHub, el commit, la imagen generada y el link a la ejecución. Esa evidencia es la que se usa para corregir.

Se considera que el ejercicio está completo cuando:
1. Las pruebas se ejecutan y pasan (job `Test`)
2. La imagen Docker se construye y se publica en GitHub Container Registry (job `Build`)
3. El contenedor levanta y responde correctamente (job `Validate`)
4. La evidencia se publica (job `Publish evidence`)

Recibirás puntos por cada problema que identifiques y corrijas correctamente. No está permitido eliminar ni saltear jobs, pasos o tests, ni modificar el job `Publish evidence`: el docente revisa los cambios de cada ejecución.

## Entrega

Para completar el ejercicio:
1. Clona el repositorio generado
2. Crea la variable `EVIDENCE_URL` en *Settings → Secrets and variables → Actions → Variables* con la URL que te indique el docente
3. Identifica y corrige todos los problemas en los diferentes archivos del repo
4. Haz commit y push de tus cambios a la rama principal, con tu propio usuario de GitHub (la evidencia queda a su nombre)
5. Verifica en la pestaña "Actions" que los cuatro jobs terminen en verde

> **Importante:** la imagen solo se publica, y la evidencia solo se envía, con un push a la rama principal. En un Pull Request el pipeline corre los tests y el build, pero no la validación ni la evidencia.

¡Buena suerte!
