# Guía fácil: controlar el diseño de la interfaz

A partir de esta versión, los estilos visuales nuevos se editan en `css/ui/` y los detalles propios de cada capítulo en el `estilos.css` de su carpeta `js/niveles/nivel-XX/`. **No edites `css/legacy/` para cambiar el diseño cotidiano**: esos archivos conservan estilos históricos del juego.

## 1. Cambiar el aspecto general

Abre `css/ui/00-design-tokens.css`. Es el panel de control principal:

| Quiero cambiar… | Variable |
|---|---|
| Tipografía general | `--ui-font` |
| Tipografía de títulos | `--ui-font-title` |
| Color de texto | `--ui-text` |
| Color de texto secundario | `--ui-text-muted` |
| Color principal / acento | `--ui-accent` |
| Color de acento más brillante | `--ui-accent-strong` |
| Fondo de paneles | `--ui-panel` y `--ui-panel-soft` |
| Bordes | `--ui-border` |
| Redondeo de paneles | `--ui-radius` |
| Ancho máximo de panel | `--ui-panel-width` |
| Tamaño del título | `--ui-title-size` |
| Tamaño del texto | `--ui-body-size` |
| Tamaño de botones | `--ui-button-size` |
| Espacio interior de botones | `--ui-button-padding-y` y `--ui-button-padding-x` |

Ejemplo: para cambiar el color principal de dorado a violeta, modifica `--ui-accent` y `--ui-accent-strong` en ese archivo.

## 2. Editar una pantalla sin afectar las demás

- Menú principal: `css/ui/20-main-menu.css`
- Selector de niveles: `css/ui/30-level-selector.css`
- Opciones y ventanas secundarias: `css/ui/40-options-and-dialogues.css`
- HUD, subtítulos, mensajes y barras: `css/ui/50-hud-and-text.css`
- Adaptación a móviles y pantallas pequeñas: `css/ui/90-responsive.css`
- Variables generales: `css/ui/00-design-tokens.css`
- Reglas comunes de botones, paneles y textos: `css/ui/10-base-ui.css`

## 3. Editar un nivel sin afectar otros

Abre `js/niveles/nivel-01/estilos.css`, `nivel-02.css`, etc. Cada archivo define `--ui-level-accent` y `--ui-level-text` para ese capítulo. Cambia esos dos valores para cambiar el acento y los textos de la UI durante ese nivel.

El capítulo del río tiene su propio archivo: `js/niveles/nivel-rio-mohan/estilos.css`.

Puedes añadir reglas exclusivas, por ejemplo:

```css
/* Solo se aplica al nivel 03 */
body[data-nivel="03"] #msg {
  letter-spacing: .08em;
  text-shadow: 0 0 12px #000;
}
```

Para otros niveles, cambia `03` por `01`, `02`, `04`, etc. El atributo del río es `rio-mohan`.

## 4. ¿Por qué antes se duplicaban o mezclaban los estilos?

El juego reutiliza un solo panel HTML (`#ov`) para mostrar menú principal, selección de niveles, opciones, tutorial y pantallas de resultado. No son necesariamente varios menús HTML: son pantallas distintas pintadas dentro del mismo panel. A lo largo de las versiones se fueron acumulando varias hojas CSS que modificaban los mismos selectores (`#ov`, `#ov h1`, `#ov button`). Por eso una regla antigua podía cambiar el aspecto de otra pantalla.

`js/sistemas-compartidos/interfaz/controlador-de-estilos.js` identifica qué pantalla está abierta y añade `data-ui-screen` al panel. Así cada hoja CSS puede apuntar únicamente al menú principal, al selector de niveles, a opciones o a diálogos. No crea un segundo menú ni cambia la lógica de selección.

## 5. Orden de carga: no mover

Las hojas de `css/ui/` están enlazadas al final de `index.html` deliberadamente. Así pueden establecer un diseño central por encima de las reglas heredadas. Si las mueves antes de los archivos `legacy`, estos podrían volver a sobrescribir tus cambios.

## 6. Archivos que conviene no editar para cambiar solo el aspecto

- `css/legacy/`: historial visual de versiones anteriores.
- `js/motor/runtime/`: módulos centrales del motor; consulta `js/motor/MAPA-DEL-RUNTIME.md`.
- `js/sistemas-compartidos/interfaz/controlador-de-estilos.js`: solo etiqueta la pantalla activa; no es una hoja de estilos.

**Regla sencilla:** para cambiar colores, fuentes, tamaños, márgenes, bordes o botones, empieza en `css/ui/`. Para cambiar el aspecto de la UI dentro de un solo capítulo, empieza en el `estilos.css` de la carpeta del nivel correspondiente.
