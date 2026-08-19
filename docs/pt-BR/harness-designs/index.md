# Análise dos harnesses de ponta

Esta seção compara as teorias sobre harness apresentadas no material do curso com produtos reais de última geração. Em cada produto, nosso foco é uma única questão: **como seu harness foi projetado** — isto é, a camada de infraestrutura de engenharia ao redor do modelo: os cinco subsistemas de instruções, ferramentas, ambiente, estado e feedback, além de mecanismos centrais como continuidade de contexto, inicialização, validação, observabilidade, passagem de contexto e loops.

De propósito, não discutimos se a capacidade de raciocínio do modelo é forte, se ele obteve uma pontuação alta em determinado benchmark nem apresentamos genericamente "o que este agent consegue fazer". Essas são questões das camadas do modelo e do produto. Aqui, analisamos apenas o harness — tudo o que existe além dos pesos do modelo.

## Por que vale a pena analisar

A primeira aula já mostrou que um modelo capaz não é sinônimo de execução confiável. O mesmo modelo, em harnesses diferentes, pode apresentar uma diferença de desempenho de uma ordem de grandeza. Mas o material explica "como deveria ser feito", enquanto estes produtos mostram "como as equipes líderes realmente fazem".

Cada produto reúne um conjunto independente de decisões de design. Ao compará-los, você verá os mesmos mecanismos centrais implementados de maneiras completamente diferentes por cada equipe:

- **Pi** transforma o harness em um núcleo mínimo com extensões programáveis e faz engenharia de contexto por meio de um "prompt de sistema mínimo + carregamento sob demanda".
- **Claude Code** transforma o harness em um ambiente de execução completo: memória em camadas, compactação em cinco níveis, permissões, hooks e subagents.
- **Codex** leva a filosofia do harness ao extremo: o repositório é a fonte da verdade, AGENTS.md é apenas uma página de índice e o ambiente é isolado com worktree.
- **DeepSeek Harness** chega a definir o próprio harness como um runtime independente do modelo: Everything is a Plugin.

## Lista de artigos

- [Análise do design de harness do Pi](./pi/): núcleo mínimo + extensões programáveis, levando a engenharia de contexto para além do prompt de sistema.
- [Análise do design de harness do Claude Code](./claude-code/): memória em camadas, compactação em cinco níveis, permissões e hooks em um ambiente completo de execução de agent.
- [Análise do design de harness do Codex](./codex/): o repositório como fonte da verdade, AGENTS.md como página de índice, isolamento de ambiente e loops de feedback.
- [Análise do design do DeepSeek Harness](./deepseek/): Everything is a Plugin, tornando substituível até o próprio loop do agent.

## Como ler

Recomendamos começar pelas primeiras aulas do material — especialmente a [Aula 2: o que exatamente é um Harness](../lectures/lecture-02-what-a-harness-actually-is/) — para compreender o framework dos cinco subsistemas. Depois, volte aqui para ver como produtos reais implementam esses mecanismos.

Ao final de cada artigo, as seções "Mapeamento para o framework do curso" e "Designs que vale a pena adotar" ajudam a traduzir rapidamente o design do produto para os conceitos do curso, facilitando sua aplicação direta nos próprios projetos.
