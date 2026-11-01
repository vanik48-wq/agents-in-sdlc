---
slug: pt-br/real-world-development/app
title: "Aplicativo GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

O **[aplicativo GitHub Copilot](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** é um aplicativo para desktop baseado no Copilot CLI que reúne o desenvolvimento agêntico em um workspace dedicado. Ele oferece sessões paralelas de agentes, modos de sessão alternáveis, canvases compartilhados e gerenciamento nativo de issues e pull requests do GitHub — incluindo o **Agent Merge**, que conduz um pull request por rebase, comentários de revisão, correções de integração contínua (CI) e mesclagem.

O workshop acompanha um fluxo de trabalho contínuo na Tailspin Toys:

1. Prepare o projeto, instale o aplicativo, conecte o repositório e explore o workspace e o backlog preenchido.
2. Faça uma alteração focada nas avaliações por estrelas, revise-a no navegador e mescle manualmente seu primeiro pull request (PR).
3. Comece pela issue de filtragem, defina a abordagem no modo **Plan**, implemente no modo **Autopilot** e, em seguida, revise no modo **Interactive**.
4. Atualize as instruções do repositório e aplique-as ao trabalho de filtragem.
5. Personalize a skill existente `quality-checks` e use-a para executar as verificações do projeto.
6. Adicione o servidor Model Context Protocol (MCP) do Playwright e use-o para explorar a filtragem em um navegador.
7. Crie um agente personalizado de garantia de qualidade (QA) e use-o para revisar requisitos, cobertura e evidências de verificação.
8. Revise a alteração completa da filtragem e use o Agent Merge no segundo PR.
9. Verifique a disponibilidade de canvases; depois, use o Database Explorer e crie e teste um canvas de triagem vinculado ao repositório. Se os recursos necessários não estiverem disponíveis, registre o impedimento e use a alternativa somente leitura ou continue para o encerramento.

Para manter o workshop focado, você criará dois PRs: um para avaliações por estrelas e outro para filtragem, com as atualizações de instruções, skill, perfil de QA e testes. Comece cada um a partir de `main` atualizado. O fluxo de filtragem e qualidade compartilha uma sessão, um worktree e uma branch para que você aproveite o trabalho já feito enquanto explora cada ferramenta. O exercício final de canvas permanece na própria sessão para que você possa se concentrar em criar e testar a superfície compartilhada sem repetir o fluxo de PR.

## Lições

| Lição | Tópico | Descrição |
|--------|-------|-----------|
| [0. Pré-requisitos][ex0] | Configuração | Instale o Node.js e crie sua cópia do projeto Tailspin Toys |
| [1. Instalar o aplicativo Copilot][ex1] | Configuração | Instale o aplicativo, conecte o projeto e conheça o workspace |
| [2. Adicionar avaliações por estrelas: uma melhoria rápida][ex2] | Primeira alteração | Exiba as avaliações existentes e o fallback para `null`; depois, mescle o PR 1 |
| [3. Modos de agente: Plan e Autopilot][ex3] | Modos de agente | Planeje a funcionalidade a partir da issue, crie com Autopilot e revise no modo Interactive |
| [4. Orientar o Copilot com instruções personalizadas][ex4] | Contexto | Explore e atualize as instruções e aplique-as à filtragem |
| [5. Personalizar e usar uma skill de quality-checks][ex5] | Verificações reutilizáveis | Explore a skill existente, altere o formato do relatório e execute-a |
| [6. Validar a funcionalidade com o MCP do Playwright][ex6] | Observação no navegador | Configure o MCP em Customize e inspecione o comportamento da filtragem |
| [7. Criar e usar um agente de QA][ex7] | Requisitos e cobertura | Crie e selecione um perfil especializado e reúna as evidências finais de verificação |
| [8. Criar e mesclar o PR da funcionalidade][ex8] | Revisão e mesclagem | Revise a filtragem, as instruções, a skill, o perfil de QA e os testes; depois, use o Agent Merge no segundo PR |
| [9. Explorar e criar canvases][ex9] | Colaboração | Use o Database Explorer; em seguida, crie e teste um canvas de triagem vinculado ao repositório |
| [10. Encerramento e próximos passos][ex10] | Resumo | Revise o fluxo de trabalho, os artefatos e outros recursos |

## Pré-requisitos

Antes de participar deste workshop, verifique se você tem:

- [ ] Uma conta do GitHub com um plano ativo **Copilot Student, Pro, Pro+, Business ou Enterprise**
- [ ] Um computador com **macOS, Linux ou Windows**
- [ ] [Git instalado][install-git] no computador

> [!TIP]
> Não tem um plano pago? Estudantes com status verificado podem obter o GitHub Copilot gratuitamente pelo [GitHub Education][callout-student-plan-education]. O plano **Copilot Student** inclui o agente, MCP, revisão de código e os recursos do Copilot CLI usados neste workshop — assim, você pode concluir todo o percurso.

> [!NOTE]
> Como o aplicativo Copilot é executado no seu computador, e não em um codespace, o [exercício de pré-requisitos][ex0] orienta você na instalação do Node.js e na criação da sua cópia do projeto antes da instalação do aplicativo.

> [!NOTE]
> Se você usa o Copilot Business ou o Copilot Enterprise, o administrador precisa manter habilitada a política do **aplicativo GitHub Copilot**. Essa política é separada da política do Copilot CLI.

## Comece

**[Comece pelos pré-requisitos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
