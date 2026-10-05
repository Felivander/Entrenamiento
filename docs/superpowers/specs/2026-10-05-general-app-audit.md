# Auditoría General y Plan de Actualización - App Rutina de Entrenamiento

**Fecha:** 2026-10-05  
**Tipo:** Auditoría de Calidad, Rendimiento, Ergonomía y PWA  
**Estado:** Hallazgos identificados y listos para aplicación

---

## 1. Hallazgos de la Auditoría

### 1.1 Robustez de Estado y Fechas (Alta prioridad)
* **Problema:** La variable `dk` (`YYYY-M-D`) se calculaba de forma estática una sola vez al cargar el script. Si el usuario deja la PWA abierta en segundo plano en su celular y entrena al día siguiente, el progreso se guardaba en la fecha del día anterior.
* **Solución:** Reemplazar por una función dinámica `getTodayKey()` llamada en cada guardado y lectura de datos.

### 1.2 Inconsistencias en Nomenclatura de Días (Media prioridad)
* **Problema:** En el objeto `DAYS`, el Día 1 tenía prefijo (`Lunes · Tren superior`), mientras que los Días 2 a 5 carecían del prefijo del día (`Piernas y potencia`, `Cardio y agilidad`, etc.).
* **Solución:** Estandarizar todos los títulos (`Lunes · ...`, `Martes · ...`, `Miércoles · ...`, `Jueves · ...`, `Viernes · ...`).

### 1.3 Control del Temporizador (Pausa / Reanudación) (Alta prioridad)
* **Problema:** En el temporizador flotante sólo existían los botones `+15s`, `Saltar` y `Cancelar`. Si el usuario necesitaba pausar 1 minuto (para tomar agua o acomodarse), debía cancelar el descanso o dejar que sonara.
* **Solución:** Añadir botón de **Pausa / Reanudar (⏯)** con congelamiento de tiempo restante y estado visual claro (`PAUSADO`).

### 1.4 Controles Sensoriales y Preferencias de Usuario (Media prioridad)
* **Problema:** No existía forma de silenciar los sonidos ni desactivar la vibración (por ejemplo, al entrenar de noche o en lugares públicos).
* **Solución:** Añadir barra de controles rápidos en la cabecera:
  - Botón de alternar Sonido (🔊 / 🔇).
  - Botón de alternar Vibración (📳 / 📴).
  - Botón de alternar Tema Oscuro / Claro (🌙 / ☀️).
  - Persistencia de preferencias en `localStorage`.

### 1.5 Navegación Rápida en el Modal de Técnica (Media prioridad)
* **Problema:** Al consultar un ejercicio en el modal, para ver el siguiente había que cerrar el modal, scrollear y abrir otro.
* **Solución:** Añadir botones "Anterior" y "Siguiente" en el modal para hojear todos los ejercicios de la sesión sin cerrar la ventana.

### 1.6 Respaldo y Restauración de Datos (Backup) (Baja prioridad / Alta utilidad)
* **Problema:** Si el usuario cambia de dispositivo o borra la caché del navegador, pierde su historial de entrenamiento.
* **Solución:** Añadir opción discreta para "Exportar copia de seguridad (JSON)" y "Restaurar copia".

### 1.7 Ciclo de Vida del Service Worker (PWA) (Alta prioridad)
* **Problema:** Los cambios nuevos en `index.html` pueden tardar en verse en dispositivos móviles si el Service Worker retiene la versión anterior en caché.
* **Solución:** Incrementar versión de caché (`rutina-pwa-v2`), agregar `cache.addAll` resiliente y script de auto-actualización cuando se detecte un nuevo Service Worker.

---

## 2. Plan de Implementación de Mejoras

1. **Actualizar `sw.js`:** Nueva versión de caché `rutina-pwa-v2` con manejo de errores en recursos.
2. **Actualizar `index.html`:**
   - Dinamizar fecha actual (`getTodayKey`).
   - Estandarizar títulos de los 5 días en `DAYS`.
   - Implementar botón de Pausa / Reanudar en el temporizador (`isPaused`, `remaining`).
   - Implementar barra de accesos rápidos: Sonido (Mute/Unmute), Vibración (On/Off), Tema (Dark/Light).
   - Navegación Anterior/Siguiente en el Modal de Técnica.
   - Herramienta de copia de respaldo (Exportar/Importar historial).
   - Detección de actualización de Service Worker.
3. **Validación:**
   - Pruebas sintácticas y funcionales.
   - Verificación de no regresión en series, temporizador y audio.
4. **Despliegue:**
   - Commit con descripción exhaustiva y push a `origin/main`.
