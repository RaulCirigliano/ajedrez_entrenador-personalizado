# Contexto del Proyecto: Ajedrez - Entrenador Personalizado

## Estado del Proyecto
Este proyecto es una evolución de la aplicación base "aperturas_ajedrez". La etapa inicial (visor y práctica de aperturas) se considera finalizada y estable. 
Esta nueva etapa se desarrolla en la carpeta `ajedrez_entrenador-personalizado` y tiene el objetivo de transformar la herramienta en un **Entrenador Virtual Inteligente**.

**Reglas de Oro de esta etapa:**
1. **100% Offline y Privado:** Todo el procesamiento, almacenamiento de datos y análisis se realizará localmente.
2. **Mantener Git siempre actualizado y sincronizado:** Mantener Git actualizado para que la PC y la netbook estén centralizadas y trabajen siempre en el mismo proyecto sincronizado. Hacer commit y push de los cambios al finalizar cada sesión o mejora (`git status`, `git add`, `git commit`, `git push`), y verificar el estado de Git antes de iniciar tareas para evitar conflictos entre dispositivos.
3. **Cero Dependencias Externas:** Se mantiene la arquitectura Vanilla JS (HTML/CSS/JS puros). No se usarán frameworks como React o Node.js. Servidor web básico en Python (`python3 server.py`).

## Nuevas Funcionalidades a Desarrollar (Fase Actual)

### 1. Sistema de Perfil y ELO Local
* El usuario tendrá un perfil (nombre, ELO inicial).
* La aplicación registrará las partidas jugadas en "Modo Libre" contra la máquina.
* El sistema ajustará el ELO del usuario basado en si gana, pierde o empata contra los distintos niveles de la inteligencia artificial.
* Almacenamiento persistente en el navegador usando `localStorage`.

### 2. Panel de Estadísticas y Dominio (Dashboard)
* Informe detallado de rendimiento:
  * **Aperturas dominadas:** Basado en la tasa de éxito de la práctica activa.
  * **Historial de Partidas:** Registro de victorias, empates y derrotas.
  * **Prácticas finalizadas:** Cantidad de veces que se ha completado cada línea teórica.

### 3. Integración de "Coach Virtual" (Ollama Local)
* Se integrará una conexión con un LLM local a través de **Ollama** (escuchando en `http://localhost:11434`).
* La aplicación de JavaScript recopilará las estadísticas del usuario (ELO, aperturas débiles, últimas partidas) y las enviará a Ollama de manera invisible.
* El LLM procesará estos datos y devolverá **sugerencias personalizadas** y planes de estudio diarios ("Qué estudiar hoy", "En qué mejorar").
* Todo mantendrá el principio de privacidad total sin usar APIs en la nube como OpenAI o Gemini.

## Arquitectura y Archivos (Actual)
* `index.html`: UI principal, se le añadirán los modales o pestañas del Dashboard y Perfil.
* `app.js`: Lógica principal. Deberá incorporar las funciones de actualización de ELO, tracking de partidas y la conexión `fetch` a Ollama.
* `openings-data.js`: Base de datos de aperturas (se mantiene).
* `chess-engine.js` / `ai-engine.js`: Motor de ajedrez e IA básica (se integrará el cálculo de resultados al final del mate o tablas para el ELO).

## Próximos Pasos de Implementación
1. ~~Crear la UI del Perfil/Dashboard en `index.html` e implementar el CRUD en `localStorage`.~~ (¡COMPLETADO y corregido error de importación de ELO antiguo!)
2. ~~Actualizar la lógica de `ai-engine.js` / `app.js` para registrar el fin de la partida y calcular la variación de ELO.~~ (¡COMPLETADO!)
3. ~~Crear el módulo `coach.js` (o funciones en `app.js`) para estructurar el *prompt* con las estadísticas y hacer el *fetch* a Ollama.~~ (¡COMPLETADO! La app ya envía el perfil al coach local).

## Sincronización de Proyectos
**Nota Importante:** Este proyecto ("Ajedrez - Entrenador Personalizado") y la aplicación web base ("aperturas_ajedrez") comparten el mismo núcleo. A partir de ahora, cualquier mejora en la interfaz de usuario, corrección de errores generales o refactorización del código base debe **aplicarse en ambos repositorios** para mantenerlos sincronizados. 
Sin embargo, las **funciones exclusivas de Inteligencia Artificial** (como el Coach Virtual o la conexión a LLMs locales) deben implementarse **únicamente** en esta versión del "Entrenador Personalizado" y no deben incluirse en la versión básica de la aplicación web.

### 4. Mejoras Recientes de la Interfaz
* Se implementó un modal de **Coronación de Peones** (Pawn Promotion) manual, permitiendo al jugador elegir si desea coronar Dama, Torre, Alfil o Caballo. Esta mejora de UI se sincronizó en ambos repositorios (`aperturas_ajedrez` y `ajedrez_entrenador-personalizado`) conforme al protocolo.

### 5. Categoría "Especiales": líneas "Destruye xxxx"
* Se creó en `openings-data.js` la categoría **"Especiales"**, pensada para agregar planes de ataque del tipo **"Destruye xxxx"**: secuencias para castigar esquemas o defensas concretas del rival.
* Primera entrada: **"Destruye el Doble Fianchetto (blancas)"** (`id: "destruye-doble-fianchetto"`, ECO B00). Plan: batería `Qd2` + `Bg5`, enroque largo, `Bh6` para cambiar el alfil de g7, tormenta de peones `h4-h5` y ruptura `e5` si las negras juegan `Nf6` antes de tiempo.
* **Preparado para nuevas entradas:** para sumar otra línea "Destruye xxxx" basta con agregar un objeto al final del array `OPENINGS_DATA` con `category: "Especiales"` y el mismo formato que las demás aperturas (`moves` con `san/from/to/name/comment`, `plansWhite`, `plansBlack`, `traps`). Aparece sola en el selector y funciona en Modo Guía, Práctica Activa y Tablero Libre.
* **Selector de aperturas:** se reemplazó el `<select>` nativo por un desplegable accesible (teclado: Espacio/Enter, flechas, Esc), con scroll interno. Su altura nunca pasa la base del tablero ni el borde de la ventana. `styles.css` y `app.js` se cargan con número de versión (`?v=N`) para evitar el problema de caché del navegador.
