# Lía: mapa del equipo

Ejemplo de salida de agent-creator (solo el mapa) · Nivel: profesional, experto en mezcla y master · Presupuesto: equilibrado · Idioma: español

**La raíz.** Lía es cantante y productora independiente: compone, graba voces en su home studio y saca un sencillo cada 6 semanas. Lo que se repite en cada sencillo es lo que se vuelve agente.

## Árbol

```
lia  (/lia)
├── composicion  (sin jefe)
│   ├── letrista              [sonnet · medium]  primera versión de la letra desde una idea
│   ├── critico-de-letra      [sonnet · medium]  revisa métrica, rima y clichés (verificador)
│   └── buscador-referencias  [haiku · low]      canciones de referencia con enlaces
├── voz  (sin jefe)
│   ├── coach-de-voz          [sonnet · medium]  calentamiento y técnica para la sesión del día
│   └── asesor-modulacion     [sonnet · medium]  cadena de efectos de voz para el estilo pedido
├── mezcla-y-master  ── lia-mezcla-y-master-lead  [opus · medium]
│   ├── organizador-pistas    [haiku · low]      nombra, ordena y revisa pistas y stems
│   ├── revisor-de-mezcla     [sonnet · high]    revisa la mezcla con una lista de chequeo (nivel experto)
│   ├── objetivos-master      [sonnet · high]    loudness y formato por plataforma (nivel experto)
│   └── verificador-archivos  [haiku · low]      formato, sample rate y metadatos antes de enviar
└── lanzamiento  ── lia-lanzamiento-lead  [sonnet · medium]
    ├── plan-de-lanzamiento   [opus · high]      plan de 6 semanas por sencillo
    ├── press-kit             [sonnet · medium]  bio, nota de prensa y textos
    ├── pitch-playlists       [sonnet · medium]  pitch para curadores, uno por playlist
    └── checklist-distribucion [haiku · low]     todo lo que pide la distribuidora
```

## Por qué así

- **Mezcla y master tiene jefe en Opus**: Lía pidió nivel experto ahí, y alguien tiene que decidir entre lo que dicen el revisor de mezcla y los objetivos de master.
- **Composición y voz no tienen jefe**: dos o tres agentes que el jefe del equipo llama directo. Un jefe ahí solo sumaría costo.
- **Haiku para lo mecánico**: ordenar pistas, revisar formatos y llenar la checklist de la distribuidora se repiten en cada sencillo y tienen reglas claras.
- **El plan de lanzamiento en Opus**: horizonte de 6 semanas, muchas decisiones que dependen unas de otras.
- **No hay agente de "portada"**: se hace una vez por sencillo con una herramienta de diseño y la decide Lía. Lo coordina el jefe del equipo.

## Pedidos de ejemplo

- `/lia tengo esta idea para la letra: ...`
- `/lia revisa la mezcla de "Marea", los stems están en sesiones/marea/`
- `/lia arma el lanzamiento de "Marea" para el 15 de enero`
