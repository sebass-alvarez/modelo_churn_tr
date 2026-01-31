Modelo de predicción de Churn

Proyecto en Python para **entrenar** y/o **servir** un modelo de predicción de *churn* (deserción) usando un artefacto serializado (`.pkl`) y un dataset de ejemplo.

> En este repositorio encontrarás scripts de entrenamiento/inferencia, un dataset (`customer_churn_dataset.csv`) y un modelo ya entrenado (`modelo_xgboost_churn.pkl`). 

---

## Contenido

- [Descripción](#descripción)
- [Estructura del repositorio](#estructura-del-repositorio)
- [Requisitos](#requisitos)
- [Instalación](#instalación)
- [Uso](#uso)
  - [1) Entrenar (Modelo_01.py)](#1-entrenar-modelo_01py)
  - [2) Cargar modelo y predecir (Modelo_02_read_pkl.py)](#2-cargar-modelo-y-predecir-modelo_02_read_pklpy)
  - [3) Ejecutar app de inferencia (app.py)](#3-ejecutar-app-de-inferencia-apppy)
- [Dataset](#dataset)
- [Artefacto del modelo](#artefacto-del-modelo)
- [Deploy](#deploy)
- [Licencia](#licencia)

---

## Descripción

Este repositorio contiene un flujo típico de *Machine Learning* para churn:

- Entrenamiento de un modelo (script `Modelo_01.py`).
- Serialización del modelo entrenado a un archivo `.pkl`.
- Lectura del `.pkl` y predicción (script `Modelo_02_read_pkl.py`).
- Una aplicación (`app.py`) para servir predicciones (local o deploy).

---

## Estructura del repositorio

Archivos relevantes detectados en la raíz del repo:

- `Modelo_01.py` — entrenamiento / pipeline principal.
- `Modelo_02_read_pkl.py` — ejemplo de carga del `.pkl` + predicción.
- `app.py` — app para inferencia (API/servicio).
- `customer_churn_dataset.csv` — dataset de ejemplo.
- `modelo_xgboost_churn.pkl` — modelo entrenado serializado.
- `requirements.txt` — dependencias.
- `Procfile` — comando de arranque para deploy (p. ej. plataforma tipo PaaS).
- `runtime.txt` — versión de Python para deploy.
- `LICENSE` — licencia MIT.

---

## Requisitos

- Python (recomendado: la versión indicada en `runtime.txt`).
- `pip` / `venv`.

> Si estás en Windows: usa PowerShell o terminal integrada de VS Code.

---

### Pasos para correr local

```bash
# 1) Clonar repo
git clone https://github.com/sebass-alvarez/modelo_churn_tr.git
cd modelo_churn_tr

# 2) Crear y activar entorno virtual
python -m venv .venv

# Windows (PowerShell)
.venv\Scripts\Activate.ps1

# Mac/Linux
# source .venv/bin/activate

# 3) Instalar dependencias
pip install -r requirements.txt

# 4) Levantar API
uvicorn app:app --reload --host 0.0.0.0 --port 8080


### Ejemplo del request

curl -X POST "http://localhost:8080/predict" \
  -H "Content-Type: application/json" \
  -d '{
    "tenure": 12,
    "monthly_charges": 75.5,
    "total_charges": 905.3,
    "support_calls": 2,
    "contract": "Month-to-month",
    "payment_method": "Electronic check",
    "internet_service": "Fiber optic",
    "tech_support": "No",
    "online_security": "Yes"
  }'

#### Response

{
  "Churn": false,
  "ProbabilidadChurn": 0.2134,
  "Threshold": 0.5
}

