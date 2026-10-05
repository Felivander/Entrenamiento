# Especificación de Diseño: App de Entrenamiento PWA Pro

**Fecha:** 2026-10-05  
**Autor:** Antigravity  
**Estado:** Aprobado (/goal)

---

## 1. Resumen Ejecutivo
Transformar la aplicación de seguimiento de rutina (`index.html`) en una **Progressive Web App (PWA) de nivel profesional**, rápida, instalable en dispositivos móviles (iOS/Android), con funcionamiento offline, GIFs de ejercicios cargados directamente vía CDN de alta disponibilidad (sin búsquedas dinámicas lentas), retroalimentación táctil (vibración háptica) y sonora, modal con consejos técnicos de postura/seguridad, y métricas de racha y progreso semanal.

---

## 2. Requerimientos y Objetivos

### 2.1 Rendimiento y Multimedia
* **Eliminar consultas a APIs lentas:** Reemplazar el motor de búsqueda en vivo `oss.exercisedb.dev` por un catálogo estático estructurado con enlaces directos servidos por CDN jsDelivr ([ExerciseGymGifsDB](https://github.com/JahelCuadrado/ExerciseGymGifsDB)).
* **Cero llamadas de red bloqueantes:** El catálogo de ejercicios incluirá URLs directas, nombres, tiempos, repeticiones y tips técnicos.
* **Fallback robusto:** En caso de fallo de red en algún GIF, mostrar botón directo a YouTube con búsqueda del ejercicio.
* **Modal de Técnica:** Al tocar el GIF o la tarjeta del ejercicio, abrir una vista ampliada con el GIF y consejos anatómicos/posturales (incluyendo precauciones específicas para la cicatriz en los ejercicios de Core).

### 2.2 Temporizador y Retroalimentación Sensorial
* **Cronómetro Flotante:** Barra inferior fija con indicación clara de "Trabajo" (azul/cian) y "Descanso" (verde esmeralda), tiempo formateado con fuente monoespaciada tabular, y barra de progreso animada.
* **Vibración Háptica:** Uso de `navigator.vibrate()` en dispositivos móviles:
  * Pulsos cortos en los últimos 3 segundos (3, 2, 1).
  * Doble pulso al completar el intervalo o serie.
* **Sonido (Web Audio API):** Tonos sintetizados nítidos para cuenta regresiva y finalización, sin requerir archivos mp3 externos.
* **Wake Lock API:** Evitar que la pantalla del celular se apague durante el entrenamiento.

### 2.3 Progreso, Rachas y Persistencia
* **Progreso Diario:** Contador de series completadas / totales con barra de progreso reactiva.
* **Racha Semanal (Streak):** Indicador visual de días entrenados en la semana actual (Lunes a Viernes) y racha de días consecutivos.
* **Persistencia Local (`localStorage`):**
  * Historial de series realizadas por fecha.
  * Preferencias de sonido/vibración.
  * Caché de GIFs vistos.

### 2.4 PWA y Soporte Móvil
* `manifest.json`: Definición de la PWA (nombre "Entrenamiento", tema oscuro `#0e141a`, `display: standalone`, orientación portrait, iconos SVG/PNG vectoriales nítidos).
* `sw.js` (Service Worker): Estrategia de caché "Stale While Revalidate" o "Cache First" para recursos de la app, permitiendo apertura instantánea incluso sin conexión a internet.
* Botón de instalación / indicador de compatibilidad PWA.

---

## 3. Arquitectura de Archivos

```
C:\Users\Felipe\Documents\Feli\
├── index.html          # Interfaz principal, diseño, componentes y lógica JS
├── manifest.json       # Manifiesto PWA para instalación móvil
├── sw.js               # Service worker para funcionamiento offline
├── icon.svg            # Icono vectorial para app y accesos directos
└── docs/
    └── superpowers/
        └── specs/
            └── 2026-10-05-pwa-workout-tracker-design.md
```

---

## 4. Detalle de Componentes y Datos

### 4.1 Catálogo de Ejercicios (`EXERCISES`)
Cada ejercicio contiene:
- `id`: Identificador único.
- `name`: Nombre en español.
- `sets`: Número de series.
- `reps`: Descripción de repeticiones o tiempo.
- `rest`: Segundos de descanso.
- `work`: Segundos de trabajo cronometrado (0 si no aplica).
- `gif`: URL directa CDN jsDelivr.
- `tips`: Lista de 2-3 puntos clave de técnica y seguridad (respiración, postura, advertencias).

### 4.2 Bloques de Entrenamiento
1. **Entrada en Calor (`warmup`):** Soga suave, círculos de brazos, rotación de cadera, gato-vaca, sentadilla de aire, zancadas dinámicas, jumping jacks.
2. **Bloque Principal (`days 1-5`):**
   - Día 1: Tren superior (Dominadas, flexiones, fondos, etc.)
   - Día 2: Piernas y potencia (Búlgara, saltos, zancadas, etc.)
   - Día 3: Cardio y agilidad (Soga, trote, escalera de agilidad)
   - Día 4: Cuerpo completo (Chin-ups, flexiones con pausa, remo, etc.)
   - Día 5: Específico de fútbol (Sprints, intervalos 30/30, slalom con conos, saltos al banco)
3. **Core (`core`):** Plancha, Dead bug, Bird dog, Plancha lateral, Puente de glúteos. Con aviso explícito de precaución sobre la cicatriz.

---

## 5. Pruebas y Validación
* Verificar que la interfaz renderice correctamente en desktop y mobile.
* Comprobar que los puntos de series (dots) registren estado, avancen el contador y activen los temporizadores correspondientes.
* Validar que el Service Worker se registre sin errores de consola.
* Validar la sintaxis de `manifest.json`.
* Validar que los botones de control de temporizador (+15s, saltar, cancelar) funcionen adecuadamente.
* Comprobar la sincronización con Git y GitHub.
