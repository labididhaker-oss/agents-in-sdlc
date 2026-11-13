---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente de nuvem do Copilot que executasse três tarefas distintas para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente de nuvem tenha sido criado para trabalhar de forma assíncrona e autônoma, ainda é importante poder monitorar essas tarefas.

Há várias ferramentas disponíveis para gerenciar tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nessa central de controle, você vê todas as tarefas dos agentes com pull requests (PRs) abertos. Também pode explorar as operações realizadas e orientar uma sessão em andamento.

Nesta lição, você vai:

- explorar a página de agentes para monitorar as tarefas do agente de nuvem.
- orientar uma sessão em andamento para solicitar funcionalidades adicionais.

## Cenário

Depois de atribuir ao agente a tarefa de criar um modo de alto contraste, a equipe percebeu que seria um bom momento para adicionar também um modo claro. Como o trabalho já envolvia atualizar o estilo do site e adicionar uma funcionalidade de alternância, pareceu lógico incluir esse recurso. Você vai orientar o agente para garantir que ele adicione um modo claro além do modo de alto contraste.

## Revise as tarefas do agente de nuvem do Copilot

Vamos conferir o status atual de todas as tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel esquerdo. Você deverá ver as tarefas atribuídas ao Copilot, incluindo:
    - atualização da documentação da base de código.
    - geração de APIs para modificar jogos.
    - adição de um modo de alto contraste ao site.
3. Selecione uma das tarefas em execução. Revise as operações realizadas pelo Copilot. Elas podem incluir:
    - obter o código do repositório.
    - criar o ambiente de trabalho do Copilot.
    - configurar servidores MCP.
    - executar várias etapas para concluir a tarefa atribuída.

> [!NOTE]
> As etapas exatas exibidas variam de acordo com o estado do trabalho do Copilot e a abordagem utilizada.

4. Observe também o painel de pull request (PR) à direita. Ele permite ver o PR e os arquivos alterados para acompanhar o trabalho.

## Oriente o agente de nuvem

Agora que você viu as tarefas em andamento, vamos pedir ao Copilot que inclua uma opção de modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão que trata da adição do modo de alto contraste. O título exato varia conforme o nome escolhido pelo Copilot e o estado atual do trabalho.

    ![Sessão de acessibilidade no Mission Control](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que ela indique que a configuração foi concluída e o trabalho começou. Você saberá que isso aconteceu quando surgirem mensagens semelhantes aos exemplos.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione o seguinte prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de tela da tarefa do agente de nuvem na página de agentes, com a caixa de diálogo para orientar a sessão ativa do Copilot destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot confirma o recebimento do prompt e o inclui no fluxo de trabalho.

## Deixe o Copilot trabalhar

Como antes, o Copilot começará a trabalhar na tarefa atualizada! Ele incorporará a nova solicitação ao fluxo depois de concluir a etapa específica que estava executando quando você enviou a mensagem.

Como isso levará alguns minutos, aproveite para fazer uma pausa e refletir sobre o que aprendeu e explorou até agora.

## Resumo e próximos passos

Esta lição explorou a página de agentes do Copilot, seu hub central para monitorar e orientar tarefas do agente de nuvem do GitHub Copilot. Com esse centro de controle, você pode acompanhar todas as tarefas ativas e concluídas, revisar o trabalho e redirecionar tarefas em andamento.

Você explorou estes conceitos:

- explorou o Mission Control e a página de agentes para monitorar as tarefas do agente de nuvem.
- redirecionou uma sessão em andamento para solicitar funcionalidades adicionais.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, vamos [iterar nos pull requests criados pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
