# 🚀 Simulaciones con PyBullet y Docker

Este documento describe paso a paso cómo desplegar tres simulaciones
diferentes utilizando **PyBullet** y **Docker**:

1.  Simulación de **drones** con el entorno `gym-pybullet-drones`.
2.  Simulación del robot **Baxter**.
3.  Simulación del robot **Atlas**.

------------------------------------------------------------------------

## 🧩 PRIMER PUNTO: Simulación de Drones con PyBullet

### 🔗 Repositorio

<https://github.com/utiasDSL/gym-pybullet-drones>

### 🧰 Requisitos previos

Asegúrate de tener instalados los siguientes programas:

``` bash
sudo apt update
sudo apt install -y docker docker.io git x11-apps
```

Verifica que Docker funcione correctamente:

``` bash
sudo systemctl status docker
```

------------------------------------------------------------------------

### ⚙️ Paso 1: Clonar el repositorio

``` bash
git clone https://github.com/utiasDSL/gym-pybullet-drones.git
cd gym-pybullet-drones
```

------------------------------------------------------------------------

### ⚙️ Paso 2: Crear la imagen Docker

``` bash
docker build -t gym-drones .
```

------------------------------------------------------------------------

### ⚙️ Paso 3: Ejecutar la simulación con interfaz gráfica

Para habilitar la interfaz gráfica de PyBullet:

``` bash
xhost +local:docker
```

Luego ejecuta:

``` bash
docker run -it --rm     -e DISPLAY=$DISPLAY     -v /tmp/.X11-unix:/tmp/.X11-unix     gym-drones     python3 gym_pybullet_drones/examples/learn.py --gui True
```

> 💡 Si todo funciona correctamente, verás una ventana con la simulación
> de los drones en vuelo.

------------------------------------------------------------------------

## 🤖 SEGUNDO PUNTO: Simulación de Baxter con PyBullet

### 🔗 Repositorio

<https://github.com/erwincoumans/pybullet_robots>

------------------------------------------------------------------------

### ⚙️ Paso 1: Clonar el repositorio

``` bash
git clone https://github.com/erwincoumans/pybullet_robots.git
cd pybullet_robots
```

------------------------------------------------------------------------

### ⚙️ Paso 2: Crear el contenedor Docker

Crea un archivo `Dockerfile` dentro del directorio `pybullet_robots` con
el siguiente contenido:

``` dockerfile
FROM python:3.10-slim

RUN apt-get update && apt-get install -y     python3-pip     xvfb     x11-apps     && rm -rf /var/lib/apt/lists/*

WORKDIR /app
COPY . /app

RUN pip install --upgrade pip && pip install pybullet numpy

CMD ["python3", "baxter_ik_demo.py"]
```

------------------------------------------------------------------------

### ⚙️ Paso 3: Construir la imagen

``` bash
docker build -t baxter-pybullet .
```

------------------------------------------------------------------------

### ⚙️ Paso 4: Ejecutar la simulación

``` bash
xhost +local:docker
docker run -it --rm     -e DISPLAY=$DISPLAY     -v /tmp/.X11-unix:/tmp/.X11-unix     baxter-pybullet
```

> Verás la simulación del robot **Baxter** moviendo sus brazos dentro
> del entorno PyBullet.

------------------------------------------------------------------------

## 🦿 TERCER PUNTO: Simulación del robot Atlas con PyBullet

### 🔗 Repositorio

<https://github.com/erwincoumans/pybullet_robots>

------------------------------------------------------------------------

### ⚙️ Paso 1: Clonar el repositorio (si no lo tienes)

``` bash
git clone https://github.com/erwincoumans/pybullet_robots.git
cd pybullet_robots
```

------------------------------------------------------------------------

### ⚙️ Paso 2: Crear un Dockerfile

Puedes usar el mismo Dockerfile anterior, solo cambia el comando final
para ejecutar el script de Atlas:

``` dockerfile
FROM python:3.10-slim

RUN apt-get update && apt-get install -y     python3-pip     xvfb     x11-apps     && rm -rf /var/lib/apt/lists/*

WORKDIR /app
COPY . /app

RUN pip install --upgrade pip && pip install pybullet numpy

CMD ["python3", "atlas_forward_kinematics.py"]
```

------------------------------------------------------------------------

### ⚙️ Paso 3: Construir y ejecutar

``` bash
docker build -t atlas-pybullet .
xhost +local:docker
docker run -it --rm     -e DISPLAY=$DISPLAY     -v /tmp/.X11-unix:/tmp/.X11-unix     atlas-pybullet
```

> Se abrirá la ventana de simulación con el robot Atlas.

------------------------------------------------------------------------


## 👨‍💻 Autor

**Jonathan David Díaz Vargas**\
Universidad Santo Tomás\
Curso: *Simulación de Robots con PyBullet y Docker*
