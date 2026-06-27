# ar_proyecto_grupal_02

## Instalación

Clona el repositorio:

```bash
git clone <url-del-repo>
cd ar_proyecto_grupal_02
```

---

Crea un entorno virtual con `conda`:

```bash
conda create -n ar_env python=3.10 -y
conda activate ar_env
```

---

Instala las dependencias necesarias:

```bash
# macOS Apple Silicon (M1/M2/M3/M4/M5) — tensorflow-metal + ale-py para Atari
pip install -e ".[mac]"
AutoROM --accept-license   # instala las ROMs de Atari

# Linux / Windows / macOS Intel — versión del profesor
pip install -e ".[default]"
```

---

## Variables de entorno

Copia `.env.template` a `.env` y rellena las rutas:

```bash
cp .env.template .env
```

---

## Estructura del proyecto

```
ar_proyecto_grupal_02/
├── notebooks/
│   └── ar_proyecto_programacion_02.ipynb   # notebook de trabajo
├── weights/                                # generado al entrenar
│   ├── checkpoints/    # pesos cada N pasos → dqn_<env>_step<N>.h5f
│   ├── logs/           # log JSON de métricas      → dqn_<env>_log.json
│   └── final/          # pesos al terminar          → dqn_<env>_final_step<N>.h5f
│                       # mejor episodio del train   → dqn_<env>_best_step<N>.h5f
└── pyproject.toml
```

### Pesos guardados durante el entrenamiento

| Carpeta | Cuándo | Nombre |
|---|---|---|
| `checkpoints/` | Cada N pasos | `dqn_<env>_step<N>.h5f`, `_step<N>.h5f`, … |
| `final/` | Al terminar — pesos finales | `dqn_<env>_final_step2000000.h5f` |
| `final/` | Al terminar — mejor episodio | `dqn_<env>_best_step<N>.h5f` |

El archivo `_best` se determina al final del entrenamiento: durante el training se rastrea el mejor reward y se guarda el step en que se consiguió; al terminar se escribe en `final/` con ese step en el nombre.

Para reanudar un entrenamiento desde un checkpoint, carga los pesos antes de llamar a `dqn.fit()`:

```python
dqn.load_weights("weights/checkpoints/dqn_SpaceInvaders-v0_step500000.h5f")
```

---

## Recrear el entorno desde cero

Si necesitas eliminar el entorno y volver a crearlo:

```bash
conda deactivate
conda env remove -n ar_env -y

conda create -n ar_env python=3.10 -y
conda activate ar_env

# macOS Apple Silicon
pip install -e ".[mac]"
AutoROM --accept-license   # instala las ROMs de Atari

# Linux / Windows / macOS Intel
pip install -e ".[default]"
```

---
