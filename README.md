# TrueGDCubism (GDCubism)

**Integración no oficial de Live2D Cubism SDK para Godot Engine 4.3+**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Godot Engine](https://img.shields.io/badge/Godot-4.3+-478CBF?logo=godotengine&logoColor=white)](https://godotengine.org)
[![Live2D Cubism](https://img.shields.io/badge/Live2D-Cubism%205%20SDK-FF69B4)](https://www.live2d.com)

---

## Resumen

TrueGDCubism (fork de **[GDCubism](https://github.com/MizunagiKB/gd_cubism)**) es una [GDExtension](https://docs.godotengine.org/en/stable/tutorials/scripting/gdextension.html) no oficial que integra el [Live2D Cubism SDK](https://www.live2d.com/download/cubism-sdk/) en [Godot Engine](https://godotengine.org) 4.3+. Permite cargar, renderizar y controlar personajes Live2D directamente desde **GDScript** o **C#**.

- Renderiza modelos Live2D como nodos `Node2D` de Godot
- Controla parámetros, movimientos, expresiones y físicas
- Efectos incorporados: parpadeo, respiración, seguimiento de cabeza, detección de áreas de impacto
- Plugin de editor con carga de modelos por arrastrar y soltar
- Renderizado directo (sin SubViewport) desde v0.9

---

## Características

| Característica          | Descripción                                                                                            |
| ----------------------- | ------------------------------------------------------------------------------------------------------ |
| **Renderizado**         | Carga y renderiza archivos `.model3.json` con mallas Live2D completas                                  |
| **Movimientos**         | Reproduce animaciones `.motion3.json`; se importan como recursos `Animation` de Godot                  |
| **Expresiones**         | Aplica expresiones faciales desde archivos `.exp3.json`                                                |
| **Física**              | Simula físicas Live2D con `.physics3.json`                                                             |
| **Pose**                | Aplica poses desde `.pose3.json`                                                                       |
| **Renderizado Directo** | Desde v0.9, los modelos se renderizan como nodos `MeshInstance2D` — sin SubViewport                    |
| **Modos de Mezcla**     | Soporte para Add, Mix, Multiply y variantes de máscara invertida                                       |
| **Sistema de Efectos**  | Efectos basados en nodos: parpadeo, respiración, seguimiento, áreas de impacto, efectos personalizados |
| **Plugin de Editor**    | Selección drag-and-drop, mover/rotar/escalar en el editor                                              |
| **GDScript + C#**       | API completa disponible desde GDScript y C#                                                            |
| **Multiplataforma**     | Windows, macOS, Linux, iOS, Android                                                                    |

---

## Estructura del Proyecto

```
TrueGDCubism/
├── LICENSE               # Licencia MIT
├── README.md             # Este archivo
├── CHANGELOG.md          # Historial de versiones
├── .gitignore
├── .gitmodules           # Submódulo godot-cpp
├── SConstruct            # Script de compilación SCons
├── SConspage             # Configuración de documentación
│
├── src/                  # Código fuente C++ de la GDExtension
│   ├── gd_cubism*.cpp/hpp
│   ├── private/          # Implementación interna de renderizador y modelo
│   ├── loaders/          # Cargadores de formato personalizados (motion3.json)
│   ├── plugin.cpp/hpp    # Plugin de editor
│   └── register_types.cpp/hpp  # Punto de entrada de la extensión
│
├── godot-cpp/            # Submódulo git (bindings C++ de Godot)
│
├── demo/                 # Proyecto Godot de demostración
│   ├── project.godot
│   ├── addons/gd_cubism/ # El addon GDExtension compilado
│   │   ├── gd_cubism.gdextension
│   │   ├── bin/          # DLLs precompilados (por plataforma)
│   │   ├── cs/           # Clases envolventes C#
│   │   └── res/shader/   # Shaders de modos de mezcla Live2D
│   ├── examples/         # Escenas y scripts de demostración
│   │   ├── viewer.tscn   # Visor de modelos principal
│   │   ├── demo_*.tscn   # Varias demos de efectos (GDScript + C#)
│   │   └── res/          # Carpeta para modelos del usuario
│   └── models/           # Modelos Live2D de muestra
│
├── docs-src/             # Fuente de documentación (Antora/AsciiDoc)
│   └── modules/ROOT/pages/{es,en,ja}/
│
├── doc_classes/          # Documentación de referencia de clases XML
└── thirdparty/           # Live2D Cubism SDK (el usuario debe descargarlo)
```

---

## Requisitos Previos

### Obligatorios

- **Godot Engine 4.3+** (descargar de [godotengine.org](https://godotengine.org))
- **Live2D Cubism SDK for Native 5-r.x** (descargar de [live2d.com](https://www.live2d.com/download/cubism-sdk/))

### Para compilar desde código fuente

- **Python 3.8+**
- **SCons 3.0+** (usar 4.7, NO 4.8 — ver docs de compilación)
- **Compilador C++:**
  - Windows: Visual Studio 2019+ (con carga de trabajo C++)
  - macOS: Xcode
  - Linux: GCC 7+ o Clang 6+

### Opcionales

- **Android Studio + NDK** (para compilaciones Android)
- **Node.js** (para compilar el sitio de documentación Antora)

---

## Inicio Rápido (Usando Binarios Precompilados)

1. **Descarga** la última versión desde la [página de Releases](https://github.com/MizunagiKB/gd_cubism/releases) (o usa los DLLs precompilados en `demo/addons/gd_cubism/bin/`)
2. **Copia** la carpeta `addons/gd_cubism/` al directorio `addons/` de tu proyecto Godot
3. **Activa** el addon en Godot: Proyecto → Configuración del Proyecto → Plugins → Activar GDCubism
4. **Añade un modelo Live2D:**
   - Crea un nodo `GDCubismUserModel`
   - Establece la propiedad `Assets` a tu archivo `.model3.json`
5. **Ejecuta** tu escena — el modelo se renderizará automáticamente

---

## Compilación desde Código Fuente

### 1. Clonar el Repositorio

```bash
git clone https://github.com/MizunagiKB/gd_cubism.git
cd gd_cubism
git submodule update --init
```

### 2. Descargar Cubism SDK

Descarga **Cubism SDK for Native 5-r.x** desde el [sitio de Live2D](https://www.live2d.com/download/cubism-sdk/) y extráelo en:

```
thirdparty/CubismSdkForNative-5-r.x/
├── Core/
├── Framework/
└── Samples/
```

### 3. Compilar

#### Windows

```bash
scons platform=windows vsproj=yes arch=x86_64 target=template_debug
scons platform=windows vsproj=yes arch=x86_64 target=template_release
```

#### macOS

```bash
scons platform=macos arch=x86_64 target=template_debug   # Intel
scons platform=macos arch=arm64 target=template_debug     # Apple Silicon
```

#### Linux

```bash
scons platform=linux arch=x86_64 target=template_debug
scons platform=linux arch=x86_64 target=template_release
```

#### iOS / Android

Ver guía completa de compilación en [documentación](docs-src/modules/ROOT/pages/es/build.adoc).

### Archivos Generados

Los binarios compilados se colocan en `demo/addons/gd_cubism/bin/`:

- Windows: `libgd_cubism.windows.{debug,release}.x86_64.dll`
- macOS: `libgd_cubism.macos.{debug,release}.framework`
- Linux: `libgd_cubism.linux.{debug,release}.x86_64.so`

---

## Uso

### Desde GDScript

```gdscript
extends Node2D

@onready var model = $GDCubismUserModel

func _ready():
    # Cargar un modelo
    model.assets = "res://ruta/al/modelo.model3.json"

    # Reproducir un movimiento
    model.start_motion("motions/MiMovimiento.motion3.json", 0, 3.0)

    # Establecer un parámetro
    var param = model.get_parameter("ParamAngleX")
    param.value = 30.0

func _process(delta):
    # Acceder a parámetros del modelo cada frame
    var param = model.get_parameter("ParamMouthOpenY")
    print(param.value)
```

### Desde C\#

```csharp
using Godot;
using GDCubism;

public partial class MiModelo : Node2D
{
    private GDCubismUserModelCS _model;

    public override void _Ready()
    {
        _model = GetNode<GDCubismUserModelCS>("GDCubismUserModel");
        _model.SetAssets("res://ruta/al/modelo.model3.json");
        _model.StartMotion("motions/MiMovimiento.motion3.json", 0, 3.0);
    }
}
```

### Sistema de Efectos

Añade efectos como nodos hijos de `GDCubismUserModel`:

| Nodo de Efecto              | Descripción                                     |
| --------------------------- | ----------------------------------------------- |
| `GDCubismEffectEyeBlink`    | Parpadeo automático de ojos                     |
| `GDCubismEffectBreath`      | Animación de respiración                        |
| `GDCubismEffectTargetPoint` | Cabeza/cuerpo/ojos siguen una posición objetivo |
| `GDCubismEffectHitArea`     | Detección de clic/hover en áreas de impacto     |
| `GDCubismEffectCustom`      | Efecto personalizable mediante señales          |

---

## Proyecto de Demostración

La carpeta `demo/` contiene un proyecto Godot completo con escenas de ejemplo:

| Escena                                   | Descripción                                                  |
| ---------------------------------------- | ------------------------------------------------------------ |
| `examples/viewer.tscn`                   | Visor completo con selección de movimiento/expresión         |
| `examples/demo_simple.tscn`              | Visualización mínima del modelo                              |
| `examples/demo_fade.tscn`                | Modulación de color mediante SubViewport                     |
| `examples/demo_transparent.tscn`         | Ventana transparente con paso de ratón                       |
| `examples/demo_effect_custom_01/02/03`   | Efectos personalizados (parpadeo forzado, señales, lip-sync) |
| `examples/demo_effect_hit_area.tscn`     | Detección de áreas de impacto                                |
| `examples/demo_effect_target_point.tscn` | Seguimiento de cabeza con el ratón                           |

Cada demo está disponible tanto en **GDScript** (.gd) como en **C#** (.cs).

> **Nota:** Las escenas demo requieren un modelo Live2D. Descarga [Nijiiro Mao](https://www.live2d.com) del sitio de Live2D y colócalo en `examples/res/live2d/`.

---

## Referencia de API

Las siguientes clases están registradas en Godot:

| Clase                                                                              | Tipo               | Descripción                   |
| ---------------------------------------------------------------------------------- | ------------------ | ----------------------------- |
| [`GDCubismUserModel`](doc_classes/GDCubismUserModel.xml)                           | Node2D             | Nodo principal del modelo     |
| [`GDCubismEffect`](doc_classes/GDCubismEffect.xml)                                 | Node (virtual)     | Clase base de efectos         |
| [`GDCubismEffectBreath`](doc_classes/GDCubismEffectBreath.xml)                     | Node               | Animación de respiración      |
| [`GDCubismEffectCustom`](doc_classes/GDCubismEffectCustom.xml)                     | Node               | Efecto personalizable         |
| [`GDCubismEffectEyeBlink`](doc_classes/GDCubismEffectEyeBlink.xml)                 | Node               | Parpadeo de ojos              |
| [`GDCubismEffectHitArea`](doc_classes/GDCubismEffectHitArea.xml)                   | Node               | Detección de áreas de impacto |
| [`GDCubismEffectTargetPoint`](doc_classes/GDCubismEffectTargetPoint.xml)           | Node               | Seguimiento de cabeza/cuerpo  |
| [`GDCubismParameter`](doc_classes/GDCubismParameter.xml)                           | Resource           | Acceso a parámetros           |
| [`GDCubismPartOpacity`](doc_classes/GDCubismPartOpacity.xml)                       | Resource           | Acceso a opacidad de partes   |
| [`GDCubismMotionEntry`](doc_classes/GDCubismMotionEntry.xml)                       | Resource           | Entrada de cola de movimiento |
| [`GDCubismMotionQueueEntryHandle`](doc_classes/GDCubismMotionQueueEntryHandle.xml) | Resource           | Manejador de movimiento       |
| [`GDCubismValueAbs`](doc_classes/GDCubismValueAbs.xml)                             | Resource (virtual) | Base abstracta de valores     |

Documentación API completa (AsciiDoc) disponible en `docs-src/modules/ROOT/pages/{es,en,ja}/`.

---

## Compatibilidad

### Plataformas Soportadas

| Plataforma | Arquitecturas            | Estado       |
| ---------- | ------------------------ | ------------ |
| Windows    | x86_32, x86_64           | Estable      |
| macOS      | x86_64, arm64, universal | Estable      |
| Linux      | x86_64                   | Estable      |
| iOS        | arm64, universal         | Experimental |
| Android    | armv7, arm64v8           | Experimental |

### Versiones de Godot

| Versión GDCubism | Versión Godot |
| ---------------- | ------------- |
| v0.9.x           | 4.3+          |
| v0.8.x           | 4.1, 4.2      |
| v0.7.x           | 4.1, 4.2      |
| v0.6.x           | 4.0, 4.1      |

---

## Licencia

Este proyecto está bajo la **Licencia MIT** — ver el archivo [LICENSE](LICENSE) para más detalles.

Copyright (c) 2023 MizunagiKB <mizukb@live.jp>

### Licencias de Terceros

Este proyecto enlaza contra **Live2D Cubism SDK**, que tiene sus propios términos de licencia:

- **Live2D Cubism Core**: [Live2D Proprietary Software License](https://www.live2d.com/eula/live2d-proprietary-software-license-agreement_en.html)
- **Live2D Cubism Framework**: [Live2D Open Software License](https://www.live2d.com/eula/live2d-open-software-license-agreement_en.html)
- **godot-cpp**: Licencia MIT (ver `godot-cpp/LICENSE.md`)

Al distribuir aplicaciones construidas con GDCubism, debes cumplir con los requisitos de licencia de Live2D.

---

## Enlaces

- [Live2D Cubism](https://www.live2d.com)
- [Live2D Cubism SDK](https://www.live2d.com/download/cubism-sdk/)
- [Cubism Native Framework (GitHub)](https://github.com/Live2D/CubismNativeFramework)
- [Repositorio Original GDCubism](https://github.com/MizunagiKB/gd_cubism)
- [Godot Engine](https://godotengine.org)
- [Documentación GDExtension de Godot](https://docs.godotengine.org/en/stable/tutorials/scripting/gdextension.html)
