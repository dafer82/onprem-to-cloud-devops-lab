# Mermaid rendering issues (Markdown + GitHub)

## 1. Contexto

Durante la documentación de la arquitectura se decidió utilizar Mermaid para mantener los diagramas versionados como código.

---

## 2. Síntoma 

El diagrama Mermaid renderizaba correctamente en Mermaid Live Editor, pero fallaba en:

- VS Code Markdown Preview
- GitHub repository preview

Mostrando errores de parseo.

---

## 3. Ejemplo de error

**_Error: Parse error on line X:_**
**_Expecting 'SEMI', 'NEWLINE', 'EOF', ..._**

---

## 4. Causa raíz 

**Mermaid es sensible a:**
- Comentarios inline (texto fuera del bloque)
- Espacios y caracteres no soportados
- Diferencias entre renderers (Live Editor vs GitHub)

---

## 5. Resolución

- Reducir el diagrama a sintaxis mínima válida
- Probar siempre en `GitHub` antes de darlo por cerrado
- Evitar comentarios inline dentro del bloque Mermaid
- Separar explicación textual del diagrama

---

## 6. Lecciones aprendidas

- Mermaid debe validarse en el renderer final (`GitHub`)
- Los diagramas como código requieren el mismo rigor que el código
- Menos sintaxis = menos errores
- **Docs-as-Code** también necesita **troubleshooting**

