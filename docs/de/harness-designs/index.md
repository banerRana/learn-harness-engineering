# Analyse führender Harness-Designs

In dieser Reihe werden die in den Kurslektionen behandelten harness-Theorien einzeln mit aktuellen, führenden Produkten aus der Praxis verglichen. Bei jedem Produkt interessiert uns nur eine Frage: **Wie ist sein harness gestaltet?** Gemeint ist also jene Engineering-Infrastruktur rund um das Modell: die fünf Subsysteme für Anweisungen, Tools, Umgebung, Zustand und Feedback sowie Kernmechanismen wie Kontextkontinuität, Initialisierung, Validierung, Observability, Handoff und Loops.

Wir sprechen bewusst weder darüber, wie gut ein Modell schlussfolgern kann, noch über einzelne Benchmark-Ergebnisse, und wir geben auch keine allgemeine Übersicht darüber, „was dieser agent kann“. Das sind Fragen der Modell- und Produktebene. Hier analysieren wir ausschließlich den harness – alles außerhalb der Modellgewichte.

## Warum sich die Analyse lohnt

Die erste Lektion stellt fest: Ein leistungsfähiges Modell garantiert keine zuverlässige Ausführung. Dasselbe Modell kann sich in unterschiedlichen harnesses um eine Größenordnung anders verhalten. Die Lektionen erklären jedoch, „wie man es tun sollte“; diese Produkte zeigen, „wie führende Teams es tatsächlich tun“.

Jedes Produkt verkörpert eine eigenständige Sammlung von Designentscheidungen. Im direkten Vergleich wird sichtbar, wie dieselben Kernmechanismen von verschiedenen Teams auf völlig unterschiedliche Weise umgesetzt werden:

- **Pi** gestaltet den harness als minimalistischen Kernel mit programmierbaren Erweiterungen und betreibt Context Engineering durch „minimale System-Prompts + Laden bei Bedarf“.
- **Claude Code** gestaltet den harness als vollständige Laufzeitumgebung: geschichtetes Memory, fünfstufige compaction, permissions, hooks und subagents.
- **Codex** treibt die harness-Philosophie auf die Spitze: Das Repository ist die maßgebliche Informationsquelle, AGENTS.md lediglich eine Verzeichnisseite, und worktrees isolieren die Umgebung.
- **DeepSeek Harness** definiert den harness selbst kurzerhand als modellunabhängige Runtime: Everything is a Plugin.

## Artikelliste

- [Analyse des harness-Designs von Pi](./pi/): minimalistischer Kernel + programmierbare Erweiterungen; Context Engineering außerhalb des System-Prompts.
- [Analyse des harness-Designs von Claude Code](./claude-code/): geschichtetes Memory, fünfstufige compaction, permissions und hooks – eine vollständige agent-Laufzeitumgebung.
- [Analyse des harness-Designs von Codex](./codex/): Das Repository als maßgebliche Informationsquelle, AGENTS.md als Verzeichnisseite sowie Umgebungsisolation und Feedback-Loops.
- [Analyse des Designs von DeepSeek Harness](./deepseek/): Everything is a Plugin – selbst der agent-Loop ist ein austauschbares plugin.

## Leseempfehlung

Lies am besten zuerst die ersten Kurslektionen – insbesondere [Lektion 2: Was ein Harness tatsächlich ist](../lectures/lecture-02-what-a-harness-actually-is/) –, um das Framework der fünf Subsysteme kennenzulernen. Kehre anschließend hierher zurück und sieh dir an, wie reale Produkte diese Mechanismen umsetzen.

Jeder Artikel endet mit den beiden Abschnitten „Zuordnung zum Kurs-Framework“ und „Übernehmenswerte Designs“. Sie helfen dir, das Produktdesign schnell in die Konzepte des Kurses zurückzuübersetzen und direkt in eigene Projekte zu übernehmen.
