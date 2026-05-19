# Corpus 91 — Plataforma de predicciones del Mundial 2026

Corpus interno para el Tool 2 (RAG documentación de plataforma) del proyecto final.

## Estructura

Cada documento es un tema atómico autocontenido, con frontmatter YAML para metadata (tema, tipo, tags, fuente). FAQs se incluyen al final de cada documento temático para que el chunk de respuesta quede junto al contexto conceptual.

## Organización

- **identidad/** — qué es 91, descripción general
- **registro/** — creación de cuenta, código de referido en registro
- **predicciones/** — flujo de predicción, puntaje, bloqueo, carga automática
- **rankings/** — los 4 rankings (Global, Mis Torneos, Mi Tribu, Entre Tribus) + desempates
- **tribus/** — qué son, crear, unirse, owner, premio/penitencia, titulares/banca
- **torneos/** — tipos (individuales, tribus, referidos) + fichas de torneos activos
- **monedas-tienda/** — monedas internas, paquetes, ítems
- **perfil-referidos/** — perfil, código y link de referido, recompensas
- **navegacion/** — página de inicio y secciones informativas
- **legal/** — T&C, edad, conducta, suspensión, premios legal, jurisdicción
- **privacidad/** — datos, pagos, cookies, derechos, seguridad, menores

## Convención de frontmatter

```yaml
---
titulo: ...
tema: ...
tipo: concepto | how-to | referencia | legal | fact-sheet
fuente: archivo origen en Corpus 91/
tags: [...]
---
```
