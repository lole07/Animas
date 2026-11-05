# El Camino de las Ánimas

Proyecto web modular para editar y publicar en GitHub Pages.

## Empezar

1. Descomprime el ZIP completo.
2. Abre la carpeta en Visual Studio Code.
3. Ejecuta `index.html` con Live Server para probarlo localmente.
4. Para publicar, sube el contenido completo a GitHub y configura GitHub Pages.
5. No cambies el orden de los scripts de `index.html` sin revisar sus dependencias.

## Quiero modificar un nivel

Entra en `js/niveles/` y abre la carpeta del capítulo. Empieza por `LEEME.txt` y `MANIFIESTO.json`; `nivel.js` es el punto de entrada y `estilos.css` contiene los estilos propios. Cuando existen, las subcarpetas separan escenario, jugabilidad, personajes y cinemáticas.

| Capítulo | Carpeta |
|---|---|
| 01 · El Camino | `js/niveles/nivel-01/` |
| 02 · La Llorona | `js/niveles/nivel-02/` |
| 03 · Madre Monte | `js/niveles/nivel-03/` |
| 04 · El Cementerio | `js/niveles/nivel-04/` |
| 05 · La Escuela | `js/niveles/nivel-05/` |
| 06 · La Iglesia | `js/niveles/nivel-06/` |
| 07 · El Rostro del Monte | `js/niveles/nivel-07/` |
| Río del Mohán | `js/niveles/nivel-rio-mohan/` |

**Para entregar un capítulo a un programador o IA:** ejecuta `python herramientas/preparar-nivel.py 03` (cambia `03` por el capítulo; para el río usa `rio-mohan`). El ZIP se crea en `entregas/` e incluye el capítulo, sus dependencias de motor y sistemas compartidos y sus estilos. Es un paquete de contexto de desarrollo, no una compilación ejecutable aislada.

## Sistemas que usan varios niveles

Están en `js/sistemas-compartidos/`:

- `audio/`: música, rezos, voces y ambientes.
- `interfaz/`: menús, HUD, opciones, tutoriales y controles.
- `jugabilidad/`: combate, machete, combos y obstáculos comunes.
- `mecanicas-de-niveles/`: reglas que se aplican a varios capítulos.
- `personajes/`: jefes, subjefes, Cadejos y animaciones compartidas.
- `mundo/`: vegetación, arquitectura, iluminación y cielo.
- `cinematicas/`: reproductor, escenas e introducciones compartidas.
- `narrativa/`: textos y transiciones.
- `recursos/`: modelos, geometrías e imágenes.

## Motor común

`js/motor/` contiene el gestor de niveles, el cargador de recursos, compatibilidad y el runtime. Los datos codificados de modelos especiales se guardan en `js/sistemas-compartidos/recursos/`, separados de la lógica. El antiguo runtime monolítico se dividió en 12 archivos dentro de `js/motor/runtime/`, organizados por responsabilidad. `index.html` los carga secuencialmente en orden numérico. **Todavía comparten variables globales:** la división permite localizar y entregar mejor el código, pero no significa que cada archivo sea independiente. Consulta `js/motor/MAPA-DEL-RUNTIME.md` antes de modificarlo.

## Documentación

- `js/ARQUITECTURA-DEL-JUEGO.md`: mapa de responsabilidades.
- `js/GUIA-DE-MODULOS.md`: índice rápido.
- `js/motor/README.md`: cómo está organizado el motor.
- `js/motor/MAPA-DEL-RUNTIME.md`: detalle de los 12 módulos.
- `GUIA-ESTILOS-UI.md`: edición de menús y estilos generales.
- `js/orden-de-carga.json`: orden de scripts extraído de `index.html`.

## Reglas importantes

- No conviertas los scripts clásicos a `type="module"`: comparten variables globales.
- No agregues `async` ni `defer` sin revisar dependencias.
- No ordenes alfabéticamente los scripts.
- Si un cambio afecta a varios niveles, hazlo en `sistemas-compartidos/` y documenta su alcance.
- Haz una copia o commit antes de cada cambio importante y prueba el juego después de modificarlo.
