# Análisis de los harness más avanzados

Esta sección contrasta, uno por uno, la teoría sobre harness explicada en las lecciones con productos reales de vanguardia. En cada producto solo nos interesa una cosa: **cómo está diseñado su harness**; es decir, la capa de infraestructura de ingeniería que rodea al modelo: los cinco subsistemas de instrucciones, herramientas, entorno, estado y retroalimentación, junto con mecanismos esenciales como la continuidad del contexto, la inicialización, la verificación, la observabilidad, el handoff y los ciclos.

Deliberadamente no hablamos de si el modelo razona mejor o peor, de si obtiene una puntuación alta en un benchmark concreto ni presentamos de forma genérica «qué puede hacer este agent». Esas son cuestiones de la capa del modelo y de la capa del producto. Aquí solo analizamos el harness: todo lo que queda fuera de los pesos del modelo.

## Por qué merece la pena analizarlos

La primera lección ya lo explicó: un modelo capaz no garantiza una ejecución fiable. El mismo modelo, dentro de harness distintos, puede mostrar diferencias de rendimiento de un orden de magnitud. Pero las lecciones explican «cómo debería hacerse»; estos productos responden «cómo lo hacen realmente los equipos líderes».

Cada producto constituye un conjunto independiente de decisiones de diseño. Al compararlos, verás cómo distintos equipos implementan de maneras completamente diferentes los mismos mecanismos esenciales:

- **Pi** convierte el harness en un núcleo minimalista con extensiones programables y aplica ingeniería de contexto mediante «un system prompt mínimo + carga bajo demanda».
- **Claude Code** convierte el harness en un entorno de ejecución completo: memoria por capas, compaction de cinco niveles, permissions, hooks y subagents.
- **Codex** lleva al extremo la filosofía del harness: el repositorio es la fuente de verdad, AGENTS.md es solo una página de índice y el entorno se aísla mediante worktree.
- **DeepSeek Harness** define directamente el propio harness como un runtime independiente del modelo: Everything is a Plugin.

## Lista de artículos

- [Análisis del diseño del harness de Pi](./pi/): núcleo minimalista y extensiones programables que llevan la ingeniería de contexto más allá del system prompt.
- [Análisis del diseño del harness de Claude Code](./claude-code/): memoria por capas, compaction de cinco niveles, permissions y hooks en un entorno de ejecución completo para agents.
- [Análisis del diseño del harness de Codex](./codex/): el repositorio como fuente de verdad, AGENTS.md como página de índice, aislamiento del entorno y ciclos de retroalimentación.
- [Análisis del diseño de DeepSeek Harness](./deepseek/): Everything is a Plugin; hasta el propio ciclo del agent se convierte en un plugin reemplazable.

## Cómo leer esta sección

Recomendamos leer primero las lecciones iniciales, especialmente [Lección 02. Qué es realmente un harness](../lectures/lecture-02-what-a-harness-actually-is/), para familiarizarte con el marco de cinco subsistemas, y después volver aquí para ver cómo los productos reales implementan estos mecanismos.

Al final de cada artículo encontrarás las secciones «Correspondencia con el marco del curso» y «Diseños que merece la pena adoptar», que te ayudarán a traducir rápidamente el diseño del producto a los conceptos del curso y a incorporarlo directamente a tus proyectos.
