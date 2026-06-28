# ar_proyecto_grupal_02

## Entorno: Enduro-v0

El agente aprende a jugar a **Enduro** (Atari 2600) usando el algoritmo **DQN** (Mnih et al. 2015).

| Propiedad | Valor |
|---|---|
| Observación | `(210, 160, 3)` uint8 RGB → preprocesada a `(84, 84)` gris |
| Espacio de acciones | `Discrete(9)` — NOOP, FIRE, RIGHT, LEFT, DOWN, DOWNRIGHT, DOWNLEFT, RIGHTFIRE, LEFTFIRE |
| Reward | +1 por cada coche adelantado |
| Terminación | No alcanzar la cuota del día (200 coches el día 1, 300 los siguientes) |
| **Objetivo** | Superar el primer día de carrera (≥ 200 coches) en **100 episodios consecutivos** en test |

### Preprocesado del frame

```
(210, 160, 3) RGB
  → crop 16 px inferiores (HUD de puntuación)
  → resize (84, 84) BILINEAR
  → escala de grises
  → stack de 4 frames → (4, 84, 84)
  → normalización [0, 1] en batch
```

---

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
| `checkpoints/` | Cada N pasos | `dqn_Enduro-v0_step<N>.h5f` |
| `final/` | Al terminar — pesos finales | `dqn_Enduro-v0_final_step2000000.h5f` |
| `final/` | Al terminar — mejor episodio | `dqn_Enduro-v0_best_step<N>.h5f` |

El archivo `_best` se determina al final del entrenamiento: durante el training se rastrea el mejor reward acumulado por episodio y se guardan los pesos en memoria; al terminar se escriben en `final/` con el step en el nombre.

TF guarda en formato checkpoint (`.h5f.index` + `.h5f.data-*`). Para reanudar desde un checkpoint:

```python
dqn.load_weights("weights/checkpoints/dqn_Enduro-v0_step500000.h5f")
```

Para cargar en test, buscar el `.index` y pasar el prefijo sin él:

```python
best_files = sorted(Path("weights/final").glob("dqn_Enduro-v0_best_step*.h5f.index"))
dqn.load_weights(str(best_files[-1])[:-len(".index")])
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
