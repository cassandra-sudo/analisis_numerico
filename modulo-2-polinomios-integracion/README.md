# Flujo de trabajo: MATLAB + Git + GitHub

Este documento define cómo organizar, versionar y entregar los ejercicios y apuntes del módulo 2 escritos en MATLAB.

---

## Formatos de archivo en MATLAB

MATLAB maneja dos tipos de archivo relevantes para este curso:

| Formato | Extensión | Uso |
|---|---|---|
| Script estándar | `.m` | Código puro, funciones, algoritmos |
| Live Script | `.mlx` | Código + texto + fórmulas + gráficas (equivalente al `.ipynb`) |
| Exportado legible | `.pdf` / `.html` | Entregable visible en GitHub sin MATLAB instalado |

**Regla general:** el `.mlx` es tu fuente de trabajo; el `.pdf` es lo que cualquiera puede leer en GitHub.

---

## Estructura de carpetas del módulo

```
modulo-2-polinomios-integracion/
│
├── README.md                    ← este archivo
│
├── polinomios/
│   ├── README.md                ← descripción del tema
│   ├── lagrange.mlx             ← Live Script (fuente)
│   ├── lagrange.pdf             ← exportado (entregable)
│   ├── newton.mlx
│   ├── newton.pdf
│   └── funciones/
│       └── eval_lagrange.m      ← funciones auxiliares reutilizables
│
└── integracion/
    ├── README.md
    ├── trapecio.mlx
    ├── trapecio.pdf
    ├── simpson.mlx
    ├── simpson.pdf
    └── funciones/
        └── regla_trapecio.m
```

---

## Flujo de trabajo diario

### 1. Antes de empezar — actualizar el repo local

Siempre parte desde la raíz del repo:

```bash
cd ~/dev/analisis_numerico
git pull origin main
```

### 2. Trabajar en MATLAB

Abre MATLAB y apunta el **Current Folder** a la carpeta del tema en el que vayas a trabajar, por ejemplo:

```
~/dev/analisis_numerico/modulo-2-polinomios-integracion/polinomios/
```

Crea o abre tu Live Script desde ahí:

```
Home → New → Live Script
```

Trabaja con normalidad: escribe texto, fórmulas en LaTeX, código y ejecuta secciones.

### 3. Exportar a PDF antes de hacer commit

Cuando termines una sesión de trabajo o un tema completo, exporta el Live Script:

```
Live Editor → Export → Export to PDF
```

Guarda el `.pdf` en la misma carpeta que el `.mlx`.

### 4. Hacer commit desde la raíz

```bash
cd ~/dev/analisis_numerico
git add .
git commit -m "Módulo 2: interpolación de Lagrange - implementación y ejemplos"
git push origin main
```

#### Convención de mensajes de commit

```
Módulo 2: <tema> - <qué hiciste>

Ejemplos:
  Módulo 2: trapecio - implementación y comparación de errores
  Módulo 2: Newton - corrección en diferencias divididas
  Módulo 2: Simpson - ejercicios 3 y 4
```

---

## Qué sí y qué no va al repo

### ✅ Sí se sube

- `*.mlx` — Live Scripts (fuente de trabajo)
- `*.pdf` — exportados de cada Live Script
- `*.m` — scripts y funciones auxiliares
- `README.md` de cada carpeta

### ❌ No se sube (ya está en `.gitignore`)

- `*.asv` — archivos de autoguardado automático de MATLAB
- `*.m~` — respaldos temporales

El `.gitignore` en la raíz del repo ya cubre estos casos.

---

## Verificar que el `.gitignore` de MATLAB está activo

Desde la raíz del repo:

```bash
cat .gitignore
```

Debe contener al menos:

```
# MATLAB
*.asv
*.m~
```

Si no está, agrégalo:

```bash
echo -e "\n# MATLAB\n*.asv\n*.m~" >> .gitignore
git add .gitignore
git commit -m "gitignore: agrega exclusiones de MATLAB"
```

---

## Checklist antes de cada push

- [ ] Estoy en la raíz del repo (`analisis_numerico/`)
- [ ] Exporté el `.pdf` del Live Script que modifiqué
- [ ] Hice `git pull origin main` antes de empezar
- [ ] El mensaje del commit describe claramente qué cambió
- [ ] No hay archivos `.asv` en el `git status`

---

## Referencia rápida de comandos

```bash
# Ver estado del repo
git status

# Ver qué archivos cambiaron
git diff --name-only

# Agregar todo y commitear
git add .
git commit -m "mensaje"

# Subir al repo remoto
git push origin main

# Bajar cambios del remoto
git pull origin main

# Guardar cambios temporalmente sin commitear
git stash

# Recuperar cambios guardados con stash
git stash pop
```
