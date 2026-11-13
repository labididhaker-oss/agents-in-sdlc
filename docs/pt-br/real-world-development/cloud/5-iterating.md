---
title: "Exercício 5: Iterar no trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revise o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que adicionasse documentação ao código, criasse um recurso de jogos relacionados para a equipe de design iterar e implementasse recursos de acessibilidade, incluindo opções de modo de alto contraste e modo claro. Vamos explorar as alterações de código e, se necessário, enviar feedback ao Copilot para aprimorar o trabalho.

### Cenário

Como já destacamos várias vezes, os fundamentos do design de software e do DevOps não mudam com a inclusão da IA generativa. Sempre precisamos revisar o código gerado e seguir nosso processo normal de DevOps. Com isso em mente, vamos revisar as sugestões do GitHub Copilot para criar documentação, o recurso de jogos relacionados e os recursos de acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem do Copilot executa tarefas de forma assíncrona e sem supervisão, foram implementadas algumas restrições de segurança:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** ao branch que usará para o código.
- O agente de nuvem é executado no GitHub Actions, onde cria um ambiente separado e temporário para trabalhar.
- Todos os fluxos de trabalho do GitHub Actions precisam de aprovação humana antes de serem executados.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo o acesso a servidores MCP.

## Revise a documentação gerada

Vamos começar explorando o primeiro pull request (PR) gerado pelo agente de nuvem do GitHub Copilot: a adição de documentação ao código. Você fará isso na interface padrão de PRs do GitHub.com.

> [!NOTE]
> Ao explorar o PR, você poderá ver um aviso de que o GitHub Copilot está bloqueado por um firewall. Isso é **esperado**, pois o Copilot tem acesso limitado a recursos externos por padrão, incluindo servidores MCP externos. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem do Copilot][agent-firewall].

1. Retorne ao seu repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o pull request intitulado algo como **Add missing documentation** ou outro título mais descritivo.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request terá a indicação **[WIP]**. Nesse caso, aguarde até que ele termine o trabalho. Isso pode levar alguns minutos, então faça uma pausa ou reflita sobre o que aprendeu até agora.

4. Quando o pull request estiver pronto, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, que inclui os novos comentários de documentação TSDoc. As alterações exatas variam.

    Ao examinar as alterações, procure comentários TSDoc, convenções TypeScript e cabeçalhos de comentário. Eles vêm dos arquivos de instruções personalizadas revisados no início do Exercício 2.

6. Depois de revisar as atualizações e confirmar que tudo parece correto, volte à guia **Conversation** e role para baixo.
7. Você deverá ver um indicador de que alguns fluxos de trabalho estão aguardando aprovação.
8. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Os fluxos de trabalho aparecerão na fila da seção de verificações do pull request. Se tudo der certo, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.

## Solicite alterações ao GitHub Copilot

Trabalhar com o Copilot em um pull request não é uma via de mão única. Você pode mencionar o Copilot em comentários do pull request ou em comentários inline no código, assim como faria com outros membros da equipe. O Copilot verá esses comentários e iniciará outra sessão para resolvê-los. Como os resultados não são determinísticos, não podemos fornecer um texto exato para solicitar alterações. Veja algumas ideias:

- Adicione cabeçalhos de comentário no início de cada arquivo de código, descrevendo brevemente sua finalidade.
- Adicione comentários de documentação TSDoc aos arquivos TypeScript e Astro.
- Crie um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração na documentação gerada e mencione **@copilot**, como faria com qualquer usuário. Use uma das ideias acima ou outra sugestão relacionada à documentação que você gostaria de ver na base de código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe como ele inicia uma nova sessão para fazer as atualizações.
3. Você pode selecionar **Back to pull request** para retornar ao pull request.

    ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, um novo commit aparecerá no pull request.
5. Selecione a guia **Files changed** para revisar as alterações.

Continue iterando até ficar satisfeito. Depois, você pode marcar o PR como pronto para revisão e mesclá-lo ao branch principal.

![Marcar PR como pronto](../../../_images/ex7-ready-for-review.png)

## Revise o recurso de jogos relacionados

Vamos voltar ao PR que o Copilot gerou para resolver nossa issue sobre exibir jogos relacionados na página de detalhes.

1. Retorne ao seu repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Show related games on the game details page** ou outro título mais descritivo.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois de revisar as atualizações e confirmar que tudo parece correto, volte à guia **Conversation** e role para baixo.
6. Você deverá ver um indicador de que alguns fluxos de trabalho estão aguardando aprovação.
7. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

8. Os fluxos de trabalho aparecerão na fila da seção de verificações do pull request. Se tudo der certo, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** Você também pode alternar para esse branch no Codespace para testar manualmente o recurso de jogos relacionados. Acesse o Codespace, abra o terminal e execute estes comandos (substitua `<branch-name>` pelo nome do branch criado pelo Copilot, por exemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O Copilot criou o recurso de jogos relacionados! Como antes, você pode iterar com o agente de nuvem e solicitar atualizações. Por exemplo, peça ao Copilot que ajuste a quantidade de jogos relacionados exibidos ou que adicione cabeçalhos de comentário e comentários TSDoc (lembre-se: a tarefa foi atribuída **antes** da atualização das instruções personalizadas). Você pode fazer essas solicitações adicionando um comentário novo na guia **Conversation**; o Copilot o verá e iniciará uma nova sessão.

## Revise os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados com o agente personalizado de acessibilidade. Este PR deve incluir o modo de alto contraste atribuído no Exercício 3 e o modo claro solicitado no Mission Control no Exercício 4.

1. Retorne ao seu repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Add high contrast mode to website** ou outro título mais descritivo.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request terá a indicação **[WIP]**. Nesse caso, aguarde até que ele termine o trabalho, o que pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação, prestando atenção especial a:
   - componentes de alternância entre modos.
   - uso do armazenamento local para manter as preferências do usuário.
   - alterações de CSS ou estilo para os modos de alto contraste e claro.
   - atributos de acessibilidade (rótulos ARIA, navegação pelo teclado etc.).
   - código JavaScript/TypeScript que gerencia a alternância entre modos.

6. Depois de revisar as atualizações e confirmar que tudo parece correto, volte à guia **Conversation** e role para baixo.
7. Você deverá ver um indicador de que alguns fluxos de trabalho estão aguardando aprovação.
8. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Os fluxos de trabalho aparecerão na fila da seção de verificações do pull request. Se tudo der certo, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** Você pode alternar para esse branch no Codespace para testar manualmente os recursos de acessibilidade. Acesse o Codespace, abra o terminal e execute estes comandos (substitua `<branch-name>` pelo nome do branch criado pelo Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste os controles de modo de alto contraste e modo claro no navegador para confirmar que funcionam como esperado e mantêm as configurações após recarregar a página.

Observe como o agente personalizado de acessibilidade ajudou a orientar o Copilot para implementar esses recursos seguindo práticas recomendadas de acessibilidade. Se você identificar problemas ou melhorias de acessibilidade, mencione **@copilot** em um comentário para solicitar atualizações, como nos PRs anteriores.

## Exercício opcional — continue delegando

O agente de nuvem funciona melhor quando você pode delegar itens reais do backlog e cuidar de outras tarefas. Para desenvolver esse hábito, crie mais algumas issues no repositório e atribua-as ao Copilot. Algumas ideias:

- Criar um formulário para manifestar interesse de apoiadores na página de detalhes do jogo.
- Implementar paginação na página de lista de jogos.
- Adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu o harness do agente de nuvem. Ao longo destas lições, você:

- **Inspecionou os arquivos de instruções personalizadas incluídos no repositório** para observar seus efeitos posteriores na saída do agente de nuvem.
- **Atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** para acessibilidade, adicionando controles de modo de alto contraste e modo claro.
- **Usou a página de agentes do Copilot como central de controle** para monitorar e orientar a sessão de acessibilidade enquanto ela estava em andamento.
- **Revisou e iterou nos pull requests do agente de nuvem**, mencionando `@copilot` para solicitar alterações e aprovando fluxos de trabalho.

## Revisão e próximos passos

Você concluiu o harness do agente de nuvem. Se quiser continuar explorando, os outros harnesses complementam o que praticou:

- 🖥️ **[Harness do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP diretamente no IDE.
- 💻 **[Harness da CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills de agente, um agente de QA personalizado, MCP do Playwright, Agent Merge e uma ponte opcional `/delegate` para voltar ao agente de nuvem.

No seu próprio repositório, experimente estas ideias:

- Atribua ao agente de nuvem uma issue de refatoração ou de cobertura de testes.
- Crie um agente personalizado para outro domínio.
- Configure `copilot-setup-steps.yml` para outra stack.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
