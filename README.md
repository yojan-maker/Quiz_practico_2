# 🦾 PyBullet Industrial Robotics Gym

Este proyecto proporciona un entorno de simulación de robots industriales usando **PyBullet**, con entrenamiento mediante algoritmos de **Reinforcement Learning (RL)** como SAC, DDPG y TD3.  
Está diseñado para experimentar con distintos modelos de robots industriales (por ejemplo, **Dobot Magician**, **UR3**, etc.) dentro de un entorno físico simulado.

---

## ⚙️ Requisitos previos

### 🔹 Sistema operativo recomendado
- Ubuntu 20.04 o superior
- Python 3.8+
- Docker y Docker Compose instalados

### 🔹 Dependencias principales

Instalar las dependencias necesarias en tu entorno virtual:

```bash
sudo apt update && sudo apt install -y python3-venv python3-pip
python3 -m venv venv
source venv/bin/activate

pip install --upgrade pip
pip install pybullet numpy gym stable-baselines3 matplotlib opencv-python
```
---
## 🐳 Proceso de Dockerización
El contenedor Docker facilita la ejecución del entorno con PyBullet y soporte gráfico (X11).
Dockerfile usado:
```bash
FROM python:3.10-slim

WORKDIR /app

# Copiar todo el proyecto
COPY . /app

# Instalar dependencias
RUN pip install --no-cache-dir pybullet numpy gym stable-baselines3 matplotlib opencv-python

# Configuración para acceso gráfico (X11)
ENV DISPLAY=:0
ENV QT_X11_NO_MITSHM=1

# Puerto para visualización
EXPOSE 6006

# Comando por defecto
CMD ["python3", "Training/train_sac.py"]
```
---
## 🚀 Ejecución en Docker
1- Construir la imagen:
```bash
docker build -t robotics-env .
```
2- Permitir acceso gráfico (solo la primera vez):
```bash
xhost +local:docker
```
3- Ejecutar el contenedor:
```bash
docker run -it \
    --env="DISPLAY=$DISPLAY" \
    --env="QT_X11_NO_MITSHM=1" \
    --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" \
    robotics-env
```
Esto abrirá la ventana de PyBullet mostrando el entorno del robot Dobot Magician (E1).

---
## 💻 Ejecución local (sin Docker)
```bash
cd ~/PyBullet_Industrial_Robotics_Gym
export PYTHONPATH=$PYTHONPATH:$(pwd)
python3 Training/train_sac.py
```
---

## 🧠 Entrenamientos disponibles

| Archivo | Algoritmo | Descripción |
|----------|------------|-------------|
| `train_sac.py` | Soft Actor-Critic (SAC) | Entrenamiento con aprendizaje por entropía |
| `train_ddpg.py` | Deep Deterministic Policy Gradient (DDPG) | Algoritmo clásico basado en política continua |
| `train_td3.py` | Twin Delayed DDPG (TD3) | Variante de DDPG más estable y robusta |


--- 
## 🔍 Visualización de resultados
Los resultados de entrenamiento se pueden visualizar mediante los scripts en:
```bash
Evaluation/Gym/Model/Training/
```
Por ejemplo:
```bash
python3 Evaluation/Gym/Model/Training/show_train_results.py
```
## 📈 Resultados esperados
Al ejecutar correctamente el script train_sac.py, se abrirá una ventana de PyBullet con el entorno E1, mostrando el robot Dobot Magician.
El algoritmo SAC comenzará a entrenar el agente para interactuar con el entorno.

---

## ⚠️ Problemas comunes y soluciones

| Error | Causa | Solución |
|-------|--------|-----------|
| `ModuleNotFoundError: No module named 'RoLE'` | No se ha agregado el proyecto al `PYTHONPATH` | Ejecutar `export PYTHONPATH=$PYTHONPATH:$(pwd)` |
| `cannot connect to X server` | Falta acceso gráfico para Docker | Ejecutar `xhost +local:docker` |
| `AttributeError: module 'Robot' has no attribute 'Universal_Robots_UR10_Str'` | El robot UR10 no está definido | Cambiar a `Parameters.Dobot_Magician_Str` o `Parameters.Universal_Robots_UR3_Str` |

---

## 📸 Registro fotográfico

![Image](https://github.com/user-attachments/assets/7ae82b1d-52b3-430c-844a-6cfd125dd91b)

![Image](https://github.com/user-attachments/assets/a2855dea-0193-47f8-a074-5ad41bf078ec)

---

---

## 🏗️ Créditos

Este proyecto hace uso de la tecnología y librerías desarrolladas por **Roman Parák** y colaboradores en el marco del repositorio original **[PyBullet_Industrial_Robotics_Gym](https://github.com/rparak/PyBullet_Industrial_Robotics_Gym)**.


El proyecto integra componentes de las siguientes tecnologías:
- **PyBullet**: simulación física de robots y entornos tridimensionales.  
- **Stable-Baselines3**: implementación en PyTorch de algoritmos de aprendizaje por refuerzo.  
- **Gymnasium (OpenAI Gym)**: marco para la definición y evaluación de entornos RL.  
- **RoLE (Robotics Library for Everyone)**: biblioteca modular para el control de estructuras robóticas.  

Esta adaptación incorpora un entorno **dockerizado** para facilitar la ejecución en sistemas Linux con soporte gráfico (X11), permitiendo el entrenamiento y visualización interactiva de los entornos industriales.

Se agradece a la comunidad de desarrolladores de **Python, PyBullet y Stable-Baselines3** por ofrecer herramientas abiertas que impulsan la investigación y el aprendizaje en el campo de la **robótica inteligente y el control autónomo**.

---

