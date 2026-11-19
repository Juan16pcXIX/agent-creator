# agent-creator

**Le das un proyecto y te devuelve un equipo de agentes de Claude Code, cada uno con el modelo más barato que hace bien su trabajo.**

[Read in English](README.md)

agent-creator es una skill para [Claude Code](https://code.claude.com). Le muestras una carpeta de proyecto, una conversación o una descripción corta ("una marca de ropa", "hago música", "el papeleo de logística de la empresa"), y hace esto:

1. **Lee la raíz**: qué es el proyecto, qué trabajo se repite y qué tienes ya (tus agentes, skills y conectores como Canva o Notion).
2. **Investiga rápido**: cómo se suele manejar este tipo de proyecto y qué le falta.
3. **Te pregunta**: qué tan expertos quieres a los agentes, tu presupuesto, dónde instalarlos y en qué idioma trabajan.
4. **Dibuja un árbol**: departamentos (ramas) y subagentes con un solo objetivo (hojas), con un verificador donde un error sale caro.
5. **Le asigna un modelo a cada agente** con una rúbrica de 10 puntos: Haiku para lo mecánico, Sonnet para casi toda la producción, Opus para estrategia y decisiones importantes.
6. **Te deja armar el equipo**, departamento por departamento: marcas los agentes que quieres, decides si los que ya tienes se quedan igual o se mejoran, y agregas los que se le hayan pasado ("uno que arme los carruseles en Canva").
7. **Investiga a fondo cada departamento** según el nivel que elegiste y lo guarda en archivos de conocimiento que los agentes leen.
8. **Escribe los agentes y una skill jefe** (`/tu-proyecto`) que reparte cada pedido al agente correcto, y **revisa el resultado** con un script incluido.

```
norte  (/norte)
├── marketing  ── norte-marketing-lead  [sonnet · medium]
│   ├── strategist        [opus · high]     plan de contenido de 2 a 4 semanas
│   ├── trend-scout       [haiku · medium]  tendencias de la semana, con enlaces
│   ├── carousel-writer   [sonnet · medium] un carrusel a partir de un brief
│   └── fact-checker      [sonnet · medium] revisa datos, precios y fechas
└── finance  (sin jefe: solo dos agentes)
    ├── bookkeeper           [haiku · low]     clasifica movimientos del banco
    └── cashflow-forecaster  [opus · medium]   flujo de caja a 13 semanas
```

*Salida real para una marca de ropa inventada. Todos los archivos están en [examples/clothing-brand](examples/clothing-brand/.claude). Hay otro ejemplo en español para una cantante: [examples/musician](examples/musician/TEAM.md).*

## Por qué

Armar un equipo de agentes a mano toma horas, y casi siempre sale uno de dos equipos malos: pocos agentes gigantes que hacen todo regular, o todos los agentes en el modelo más caro "por si acaso". Una llamada a Haiku cuesta más o menos la vigésima parte de una a Sonnet. Si la mitad mecánica del trabajo corre en Haiku y solo la estrategia corre en Opus, el equipo hace lo mismo gastando una fracción de los tokens.

## Instalar

**Como plugin** (recomendado, recibe actualizaciones):

```
/plugin marketplace add Juan16pcXIX/agent-creator
/plugin install agent-creator@agent-creator
```

**A mano**: copia `plugins/agent-creator/skills/agent-creator` en `~/.claude/skills/` (todos los proyectos) o en `.claude/skills/` (un proyecto).

## Usar

En Claude Code, dentro de tu proyecto:

```
/agent-creator
```

o pídelo normal: "créame un equipo de agentes para este proyecto", "divide mi marca en departamentos con agentes". También sirve sin carpeta: "hazme agentes para mi carrera musical: compongo, grabo voces en casa y saco un sencillo cada 6 semanas".

Al terminar tienes `/tu-proyecto`, una skill jefe que conoce a todo el equipo, y los agentes en `.claude/agents/`. Si después corres `/agent-creator` otra vez, lee el `team.json` guardado y solo cambia lo necesario: agregar, juntar o quitar agentes.

## Cómo elige los modelos

Cada agente recibe de 0 a 2 puntos en cinco cosas: profundidad de razonamiento, ambigüedad, costo de un error, horizonte de tiempo y criterio. El total (0 a 10) da el modelo:

| Puntaje | Modelo | Para |
|---|---|---|
| 0-4 | Haiku | Archivos, formatos, búsquedas simples, llenar datos, listas de chequeo |
| 5-7 | Sonnet | Escribir a partir de un brief, programar, investigar, verificar datos |
| 8-10 | Opus | Planes de varias semanas, estrategia, plata y decisiones de alto riesgo |

Después ajusta: tu presupuesto sube o baja todo un nivel, los agentes que corren muchas veces por pedido bajan un nivel, un verificador nunca es más débil que lo que revisa, y lo que va a leer el público es mínimo Sonnet. Fable solo se usa si eliges "máxima calidad". La rúbrica completa: [model-routing.md](plugins/agent-creator/skills/agent-creator/references/model-routing.md).

## Cómo corta el árbol

- Una **hoja** hace un trabajo que se repite, con una sola salida y un "terminado cuando" que se puede comprobar. Lo que se hace una sola vez no lleva agente.
- Cada departamento donde un error cuesta plata, reputación o la verdad lleva un **verificador** que no hizo el trabajo.
- Un **jefe de departamento** existe solo si el departamento tiene 3 o más agentes que hay que planear u ordenar. Si no, la skill jefe los llama directo, que sale más barato.
- De 3 a 7 departamentos, de 2 a 6 agentes cada uno.

## Licencia

[MIT](LICENSE)
