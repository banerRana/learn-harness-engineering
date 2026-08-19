# Décryptage des harnesses de pointe

Cette rubrique confronte, produit par produit, la théorie du harness présentée dans les cours aux produits réels les plus avancés du moment. Pour chacun, une seule question nous intéresse : **comment son harness est-il conçu ?** Autrement dit, comment est pensée la couche d’infrastructure d’ingénierie qui entoure le modèle : les cinq sous-systèmes d’instructions, d’outils, d’environnement, d’état et de feedback, ainsi que les mécanismes fondamentaux de continuité du contexte, d’initialisation, de vérification, d’observabilité, de handoff et de boucle.

Nous laissons volontairement de côté la puissance de raisonnement du modèle, ses scores à tel ou tel benchmark et les présentations générales de « ce que cet agent peut faire ». Ces questions relèvent de la couche modèle et de la couche produit. Ici, nous ne démontons que le harness : tout ce qui se trouve en dehors des poids du modèle.

## Pourquoi ce décryptage vaut la peine

La première leçon l’explique : un modèle puissant ne garantit pas une exécution fiable. Avec un même modèle, les performances peuvent varier d’un ordre de grandeur selon le harness. Mais les cours expliquent « ce qu’il faudrait faire », tandis que ces produits montrent « ce que font réellement les équipes de pointe ».

Chaque produit constitue un ensemble autonome de décisions de conception. En les comparant, on voit les mêmes mécanismes fondamentaux prendre des formes radicalement différentes selon les équipes :

- **Pi** conçoit son harness comme un noyau minimal assorti d’extensions programmables et pratique l’ingénierie du contexte à travers un « prompt système minimal + chargement à la demande ».
- **Claude Code** fait du harness un environnement d’exécution complet : mémoire hiérarchisée, compaction à cinq niveaux, permissions, hooks et subagents.
- **Codex** pousse la philosophie du harness à l’extrême : le dépôt est la source de vérité, AGENTS.md n’est qu’une page d’index et les environnements sont isolés par des worktrees.
- **DeepSeek Harness** définit tout simplement le harness comme un runtime indépendant du modèle : Everything is a Plugin.

## Liste des articles

- [Décryptage de la conception du harness de Pi](./pi/) : noyau minimal et extensions programmables, pour porter l’ingénierie du contexte au-delà du prompt système.
- [Décryptage de la conception du harness de Claude Code](./claude-code/) : mémoire hiérarchisée, compaction à cinq niveaux, permissions et hooks, réunis dans un environnement d’exécution complet pour agents.
- [Décryptage de la conception du harness de Codex](./codex/) : le dépôt comme source de vérité, AGENTS.md comme page d’index, isolation de l’environnement et boucles de feedback.
- [Décryptage de la conception de DeepSeek Harness](./deepseek/) : Everything is a Plugin, jusqu’à faire de la boucle de l’agent elle-même un plugin remplaçable.

## Comment lire cette rubrique

Nous vous conseillons de commencer par les premières leçons, en particulier la [Leçon 02 : Ce qu’est réellement un harness](../lectures/lecture-02-what-a-harness-actually-is/), afin d’assimiler le cadre des cinq sous-systèmes, puis de revenir ici pour voir comment des produits réels concrétisent ces mécanismes.

Chaque article se termine par deux sections, « Correspondance avec le cadre du cours » et « Conceptions à retenir », qui permettent de traduire rapidement les choix du produit dans les concepts du cours et de les réutiliser directement dans vos propres projets.
