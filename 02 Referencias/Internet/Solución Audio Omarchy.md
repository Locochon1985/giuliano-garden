---
titulo: Solución Audio Omarchy
type: literature
author:
  - Giuliano
source_title: Configuración y resolución de audio en Omarchy (Arch Linux + PipeWire)
media_type:
  - articulo
published: 2026-11-11
creada: 2026-11-11
retocada:
description: Guía técnica sobre diagnóstico, arquitectura y resolución de problemas de salidas simultáneas de audio (HDMI + Analógico) y jack de auriculares en Omarchy (PipeWire, WirePlumber, ALSA).
tags:
  - Linux
status: Procesado
temas:
valor:
---
# Solución Audio Omarchy

## Síntesis
Guía técnica detallada sobre el diagnóstico, arquitectura y resolución de los problemas de salidas de audio en Omarchy (Arch Linux + Hyprland + PipeWire + WirePlumber + Quickshell), abordando la exclusión mutua de perfiles de hardware (HDMI vs. Analógico), la personalización del plugin de audio en la barra y la detección/desmuteo del jack físico de auriculares en ALSA.

---

## 1. Contexto y Diagnóstico del Hardware

* **Controlador:** Intel HDA (PCH) en bus PCI `0000:00:0e.0` (`alsa_card.pci-0000_00_0e.0`).
* **Códecs integrados en la misma placa:**
  * **Realtek ALC897:** Salidas analógicas (Altavoces traseros, Jack frontal de Auriculares, Micrófono).
  * **Intel Display Audio (HDMI):** Salidas digitales (HDMI 0 conectado a SONY TV; HDMI 1 y HDMI 2 desconectados).

---

## 2. Problema 1: Exclusión mutua de salidas en PipeWire

### Síntoma
En el panel de audio de la barra solo figuraba la televisión (**SONY TV**) y no existía la opción de elegir los altavoces o auriculares analógicos de la PC.

### Causa Raíz
Por defecto, PipeWire y WirePlumber utilizan perfiles ALSA Card Profile (ACP). En tarjetas con múltiples salidas (como las placas madre Intel con Realtek + HDMI), los perfiles estándar son mutuamente excluyentes:
* Si el perfil activo es `output:hdmi-surround`, PipeWire solo crea el sink para HDMI.
* La salida analógica (`output:analog-stereo`) pertenece a otro perfil, por lo que su nodo sink directamente no se instanciaba en el grafo de PipeWire.

### Solución
Se creó una regla persistente de usuario en WirePlumber:
`~/.config/wireplumber/wireplumber.conf.d/pci-audio-split.conf`

```spa-json
monitor.alsa.rules = [
  {
    matches = [
      {
        device.name = "alsa_card.pci-0000_00_0e.0"
      }
    ]
    actions = {
      update-props = {
        device.profile = "pro-audio"
      }
    }
  },
  {
    matches = [
      {
        node.name = "alsa_output.pci-0000_00_0e.0.pro-output-0"
      }
    ]
    actions = {
      update-props = {
        node.description = "Altavoces / Auriculares (PC)"
        node.nick = "Altavoces / Auriculares"
        priority.driver = 100
        priority.session = 100
      }
    }
  },
  {
    matches = [
      {
        node.name = "alsa_output.pci-0000_00_0e.0.pro-output-3"
      }
    ]
    actions = {
      update-props = {
        node.description = "SONY TV (HDMI)"
        node.nick = "SONY TV"
        priority.driver = 90
        priority.session = 90
      }
    }
  },
  {
    matches = [
      {
        node.name = "alsa_output.pci-0000_00_0e.0.pro-output-7"
      }
    ]
    actions = {
      update-props = {
        node.disabled = true
      }
    }
  },
  {
    matches = [
      {
        node.name = "alsa_output.pci-0000_00_0e.0.pro-output-8"
      }
    ]
    actions = {
      update-props = {
        node.disabled = true
      }
    }
  }
]
```

**Resultado:**
1. Al usar `pro-audio`, PipeWire instancia simultáneamente los sinks para la salida analógica (PCM 0) y para la TV HDMI (PCM 3).
2. Se desactivan los puertos HDMI 1 y 2 desconectados con `node.disabled = true` para evitar dispositivos fantasma.
3. Se asignan nombres claros y prioridades de sesión.

---

## 3. Problema 2: Personalización del Plugin de la Barra

### Síntoma
El plugin oficial de audio de Omarchy vive en `/usr/share/omarchy/shell/plugins/` (de solo lectura y sobreescribible por actualizaciones) y mostraba etiquetas técnicas crudas.

### Solución
1. Se clonó el plugin oficial al entorno de usuario:
   ```bash
   omarchy plugin clone omarchy.audio
   ```
   Esto generó el plugin personal en `~/.config/omarchy/plugins/giuliano.audio/` y lo activó en la barra (`giuliano.audio`).

2. Se editó `~/.config/omarchy/plugins/giuliano.audio/Model.js`:
   * **Limpieza de etiquetas (`friendlyDeviceLabel`):** Si una descripción contiene nombres entre corchetes (ej. `[SONY TV]`), extrae ese identificador limpio y remueve prefijos redundantes como `Built-in Audio` o `Audio Interno`.
   * **Asignación de glifos (`sinkGlyph` e `isHeadphones`):** Asigna el ícono de TV/Monitor (`󰍹`) a salidas HDMI/TV y el ícono de altavoz (`󰓃`) o auriculares (`󰋋`) a la salida analógica según corresponda.

---

## 4. Problema 3: Auriculares conectados sin sonido

### Síntoma
Al conectar los auriculares al jack frontal de 3.5mm de la PC, no se escuchaba sonido.

### Causa Raíz
A nivel hardware en el controlador ALSA del chip Realtek ALC897:
1. El control de mezcla `Headphone` estaba silenciado (`[off]`) y con volumen en `0 [0%] [-65.25dB]`.
2. El modo `Auto-Mute Mode` estaba en `Disabled`, impidiendo que el chip conmutara el sonido de los altavoces hacia los auriculares al detectar la inserción del jack.

### Solución
1. **Activación inmediata en ALSA:**
   ```bash
   amixer -c 0 sset 'Headphone' 100% unmute
   amixer -c 0 sset 'Speaker' 100% unmute
   amixer -c 0 sset 'Auto-Mute Mode' 'Enabled'
   ```
   Con esto, el chip de sonido detecta el jack físico (`values=on` en `Headphone Jack`), silencia automáticamente los altavoces y envía el audio a los auriculares.

2. **Persistencia tras reinicios de Omarchy:**
   Se creó el hook ejecutable en `~/.config/omarchy/hooks/post-boot.d/10-alsa-unmute.sh`:
   ```bash
   #!/bin/bash
   amixer -c 0 sset 'Headphone' 100% unmute >/dev/null 2>&1 || true
   amixer -c 0 sset 'Speaker' 100% unmute >/dev/null 2>&1 || true
   amixer -c 0 sset 'Auto-Mute Mode' 'Enabled' >/dev/null 2>&1 || true
   ```
   Permisos asignados: `chmod +x ~/.config/omarchy/hooks/post-boot.d/10-alsa-unmute.sh`.

---

## 5. Resumen de Archivos Afectados

| Archivo | Propósito |
|---------|-----------|
| `~/.config/wireplumber/wireplumber.conf.d/pci-audio-split.conf` | Habilita salidas simultáneas HDMI + Analógico y nombres en PipeWire. |
| `~/.config/omarchy/plugins/giuliano.audio/Model.js` | Lógica de presentación, limpieza de nombres e íconos en la barra. |
| `~/.config/omarchy/plugins/giuliano.audio/Panel.qml` | Interfaz gráfica y menú selector de salidas en Quickshell. |
| `~/.config/omarchy/hooks/post-boot.d/10-alsa-unmute.sh` | Hook post-arranque para desmutear auriculares y activar auto-mute en ALSA. |

---

## 6. Comandos Útiles de Verificación

* **Ver estado actual de PipeWire:**
  ```bash
  wpctl status
  ```
* **Conmutar salida de audio por terminal/atajo:**
  ```bash
  omarchy audio output switch
  ```
* **Verificar estado de jacks físicos en ALSA:**
  ```bash
  amixer -c 0 contents | grep -C 2 -i "headphone jack"
  ```
* **Reiniciar servicios de audio:**
  ```bash
  omarchy restart audio
  ```
* **Recargar la barra de Omarchy:**
  ```bash
  omarchy restart shell
  ```

---

## Reflexiones
> "Por defecto, PipeWire y WirePlumber utilizan perfiles ALSA Card Profile (ACP)... En tarjetas con múltiples salidas, los perfiles estándar son mutuamente excluyentes."

* **Reflexión técnica:** La arquitectura de audio moderna en Linux desacopla el hardware (ALSA) de las políticas de sesión (WirePlumber) y el grafo de enrutamiento (PipeWire). Cuando la interfaz gráfica oculta una salida válida, la solución no pasa por parches cosméticos en la UI, sino por forzar perfiles de bajo nivel como `pro-audio`, que exponen simultáneamente todos los canales PCM del chip.

---

## Referencias
* Documentación oficial de PipeWire y WirePlumber: ALSA Monitor Configuration & Rules.
* Omarchy Desktop Documentation: `omarchy plugin clone` y hooks post-arranque.
