# AGENTS.MD

## Propósito
Este archivo documenta las directrices para agentes y colaboradores que trabajan en el proyecto `Prueba_Dev_IA`.

## Alcance del proyecto
- Proyecto: aplicación de línea de tiempo de hitos de Inteligencia Artificial.
- Archivo principal: `ai-timeline.html`.
- Estructura: todo el HTML, CSS y JavaScript está contenido en un solo archivo autocontenible.
- No hay dependencias externas, no hay build step ni servidor requerido aparte de un servidor HTTP simple para pruebas.

## Reglas generales para agentes
1. Mantener el repositorio pequeño y simple.
2. No agregar dependencias nuevas si no son necesarias.
3. No crear carpetas nuevas ni reorganizar la estructura existente sin una razón clara.
4. Documentar cualquier cambio importante en `AGENTS.MD`.

## Cómo contribuir
- Para cambios de contenido o eventos:
  1. Editar `ai-timeline.html` directamente.
  2. Localizar el bloque de década correcto marcado con `<!-- ══ 20XXs ══ -->`.
  3. Añadir o actualizar una tarjeta `.event` dentro de esa década.
  4. Asegurarse de que `data-decade` coincida con la década del bloque.
  5. Actualizar la clase de categoría (`cat-research`, `cat-hardware`, `cat-milestone`, `cat-model`, `cat-product`).
  6. Mantener la semántica del título, año y descripción.

- Para cambios visuales:
  - Modificar estilos dentro de la sección `<style>` en `ai-timeline.html`.
  - Ser conservador: preferir ajustes de color y diseño que no rompan la presentación responsiva.

- Para cambios funcionales mínimos:
  - Editar la sección `<script>` en `ai-timeline.html`.
  - Probar localmente con `python3 -m http.server 8080` y verificar en el navegador.

## Convenciones de contenido
- Usar español cuando se escriba documentación o texto de interfaz.
- Mantener las clases de categoría consistentes con su uso previsto.
- No eliminar el comentario de décadas ni reordenar eventos sin justificación clara.

## Ejecución local
- Iniciar un servidor HTTP desde la raíz del proyecto:
  ```bash
  python3 -m http.server 8080
  ```
- Acceder a `http://localhost:8080/ai-timeline.html`.

## Notas adicionales
- `AGENTS.MD` describe la arquitectura y el flujo de edición de contenido para este repositorio.
- Debe usarse como guía principal para agentes que automatizan o asisten con cambios.
