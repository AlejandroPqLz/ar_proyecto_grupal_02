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
