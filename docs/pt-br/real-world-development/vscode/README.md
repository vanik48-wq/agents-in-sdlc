---
slug: pt-br/real-world-development/vscode
title: "Visual Studio Code"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

O **[GitHub Copilot Chat](https://code.visualstudio.com/docs/copilot/chat/copilot-chat)** no VS Code integra o GitHub Copilot ao editor que você já usa. No Visual Studio Code e no GitHub Codespaces, você usará o modo de agente do Copilot Chat, conectará ferramentas externas pelo MCP e contará com agentes personalizados — tudo sem sair do IDE, onde o Copilot tem acesso aos seus arquivos, ao terminal e aos problemas.

Você começará adicionando instruções personalizadas e observando o Copilot segui-las; em seguida, usará o modo de agente para criar um recurso de filtragem na interface, na camada de dados e nos testes. Depois, conectará o servidor MCP do Playwright e permitirá que o Copilot controle um navegador para testar o recurso antes de abrir um pull request. Por fim, você revisará e usará um agente personalizado para trabalhar com acessibilidade, além de monitorar, orientar e iterar nas alterações do Copilot — tudo sem sair do editor.

## Exercícios

| Exercício | Tópico | Descrição |
|----------|--------|-----------|
| [0. Pré-requisitos][ex0] | Configuração | Crie seu repositório e codespace |
| [1. Instruções personalizadas][ex1] | Contexto | Adicione e verifique instruções personalizadas no VS Code |
| [2. Modo de agente][ex2] | Geração de código | Crie um recurso de filtragem com o modo de agente |
| [3. MCP com Playwright][ex3] | Ferramentas externas | Teste o recurso em um navegador com o servidor MCP do Playwright |
| [4. Agentes personalizados][ex4] | Agentes especializados | Revise e use agentes personalizados |
| [5. Gerenciar agentes][ex5] | Monitoramento | Monitore e oriente sessões de agentes |
| [6. Iterar][ex6] | Revisão | Revise o trabalho do Copilot localmente e escolha os próximos passos |
| [Opcional: Incorporar o Foundry][foundry-toolkit] | Agentes de IA | Prepare um modelo, implante um agente e conecte-o ao site em três módulos com o VS Code e o Foundry Toolkit |

## Pré-requisitos

Antes de participar deste workshop, verifique se você tem:

- [ ] Uma conta do GitHub com um plano ativo **Copilot Student, Pro, Pro+, Business ou Enterprise**
- [ ] Acesso ao GitHub Codespaces

> [!TIP]
> Não tem um plano pago? Estudantes com status verificado podem obter o GitHub Copilot gratuitamente pelo [GitHub Education][callout-student-plan-education]. O plano **Copilot Student** inclui o agente, MCP, revisão de código e os recursos do Copilot CLI usados neste workshop — assim, você pode concluir todo o percurso.

## Comece

**[Comece pelo Exercício 0: Pré-requisitos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-agent-mode/
[ex3]: 3-mcp/
[ex4]: 4-custom-agents/
[ex5]: 5-managing-agents/
[ex6]: 6-iterating/
[foundry-toolkit]: 7-foundry-toolkit/
[callout-student-plan-education]: https://github.com/education/students
