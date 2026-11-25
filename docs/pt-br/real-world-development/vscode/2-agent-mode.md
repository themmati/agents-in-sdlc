---
title: "Exercício 2 - Adicionar funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até as atualizações mais simples de um aplicativo normalmente exigem alterações em vários arquivos e a execução de operações como testes. O fluxo de trabalho de uma pessoa desenvolvedora costuma incluir localizar todos os arquivos necessários, fazer as alterações, executar os testes, depurar, descobrir qual arquivo foi esquecido, fazer outra atualização... e por aí vai.

É aí que entra o modo de agente do Copilot.

O modo de agente do Copilot foi criado para trabalhar com mais autonomia no IDE. Ele se comporta de maneira semelhante a uma pessoa desenvolvedora: começa explorando a estrutura existente do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os erros encontrados. Vamos explorar como usar o modo de agente para adicionar novas funcionalidades ao site.

> [!NOTE]
> Embora os nomes sejam parecidos, o modo de agente e o agente de nuvem foram criados para experiências diferentes. O modo de agente executa tarefas no IDE, permitindo ciclos rápidos de feedback e interação. O agente de nuvem funciona como uma pessoa programadora par, trabalha de forma assíncrona como integrante da equipe e interage com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo de agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações;
- o modo de agente do GitHub Copilot pode implementar novos recursos na interface e na camada de dados;
- revisar alterações e testes gerados pelo modo de agente do Copilot antes de fazer merge na base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem os jogos por categoria e editora. No exercício anterior, você adicionou um auxiliar de editoras. Agora, concluirá o restante do trabalho na camada de dados, na interface e nos testes com o modo de agente do Copilot.

## Executar o site Tailspin Toys

Antes de fazer alterações, vamos explorar o site Tailspin Toys para entender as funcionalidades atuais.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com temática de desenvolvimento. Ele permite listar jogos e exibir detalhes sobre eles. O site é um único aplicativo Astro que renderiza as páginas como HTML estático durante o build. As páginas consultam diretamente um banco de dados SQLite local no frontmatter por meio do Drizzle ORM. Não há uma API de back-end separada nem uma estrutura de interface no lado do cliente. Os auxiliares reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é adicionada com um pequeno `<script>` do Astro com escopo definido, usando APIs padrão do DOM.

### Iniciar o site

Para facilitar a execução do site, há uma tarefa `npm run dev`, que inicia o único servidor de desenvolvimento Astro. Siga estas etapas para executá-la no GitHub Codespace:

1. Volte ao codespace. Depois do exercício anterior, você deverá estar na branch `main`.
2. Abra uma nova janela do terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie uma branch e mude para ela para trabalhar na filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute o seguinte comando para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, você verá uma mensagem com a URL, semelhante a esta:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo solicitar que você abra uma janela do navegador para `http://localhost:4321`, feche-a selecionando **x**.

5. Abra o site mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Ao usar um codespace, selecionar o link da URL localhost no terminal do Codespace redirecionará você automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o codespace, que agora está hospedando o servidor Web.

### Explorar o site

Depois que o site estiver em execução, explore suas funcionalidades. Os principais recursos incluem:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você acessa uma página de detalhes com mais informações, como título, descrição, editora e categoria.

## Explorar o backlog com o Copilot

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. A seleção de modo e modelo varia para cada exercício; cada etapa indicará o que escolher quando for relevante.

A implementação inicial do site funciona, mas queremos aprimorá-la com novos recursos. Vamos começar revisando o backlog. Ao criar o projeto a partir do modelo, um backlog de issues foi criado automaticamente. Peça ao GitHub Copilot que mostre essas issues.

1. Na lista de agentes da exibição Chat, selecione **Agent**. O agente planeja e implementa alterações em vários arquivos de forma autônoma, executa comandos no terminal e chama ferramentas.

   ![Captura de tela da lista de agentes na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> As pessoas que criaram este workshop não estão recomendando um modelo em particular. Durante a criação do workshop, usamos o Claude Sonnet 4.5 e o incluímos nas instruções para que as sugestões de código sejam relativamente consistentes e proporcionem uma boa experiência. No entanto, como os LLMs são probabilísticos, as sugestões podem ser diferentes das mostradas no workshop. Isso é normal e esperado.

> [!NOTE]
> Como os LLMs são probabilísticos, o Copilot pode usar um comando MCP diferente, mas ainda deverá concluir a tarefa.

3. Pergunte ao Copilot sobre o backlog enviando o seguinte prompt:

   ```plaintext
   Mostre o backlog de itens do meu repositório do GitHub. Ajude-me a priorizá-los de acordo com o que será mais útil para as pessoas usuárias.
   ```

4. Selecione **Continue** para executar o comando que lista todas as issues.
5. Revise a lista de issues gerada.

Observe que o Copilot também priorizou os itens de acordo com o que considera mais útil para as pessoas usuárias.

## Revisar os arquivos de instruções

Antes de pedir ao agente que gere o código, revise o arquivo de instruções que fornecerá contexto ao Copilot. Você usará o arquivo de instruções da [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como adicionar funcionalidades ao site.

1. No codespace, acesse `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura;
   - princípios para projetar componentes, facilitar os testes e garantir a acessibilidade;
   - links para arquivos de instruções específicos de vários tipos de arquivo, incluindo:
     - Astro;
     - Tailwind CSS;
     - Drizzle.

> [!TIP]
> Os arquivos de instruções podem fazer referência a outros arquivos de instruções e a arquivos do projeto. Os caminhos são relativos ao local do arquivo de instruções. Isso permite reutilizar instruções, dividir conjuntos complexos em partes menores e mais gerenciáveis e fornecer exemplos e modelos.

## Implementar a funcionalidade de filtragem

Para concluir a filtragem, serão necessárias pelo menos três alterações separadas no aplicativo:

- adicionar ou aprimorar a lógica restante de filtragem na camada de dados (`src/lib/`);
- adicionar ou atualizar testes do comportamento de filtragem;
- atualizar a página de listagem de jogos para incluir a interface de filtragem.

Além disso, os testes precisam ser executados e aprovados antes do merge de tudo na base de código. O modo de agente do Copilot pode executar essas tarefas. Vamos adicionar a funcionalidade.

1. Continue a conversa atual com o Copilot ou inicie uma nova selecionando **New Chat**.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Captura de tela que mostra a seleção do arquivo de instruções da interface do usuário](../../../_images/ex3-select-instructions-file.png)

3. Na lista de agentes da exibição Chat, confirme se **Agent** continua selecionado.

   ![Captura de tela da lista de agentes na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirme se **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade de acordo com a issue relacionada no backlog, usando o seguinte prompt:

   ```plaintext
   Atualize o site para incluir a filtragem por editora e categoria de acordo com os requisitos da issue correspondente no backlog. Já existe um auxiliar de editoras do exercício anterior; preserve e aprimore esse trabalho conforme necessário para concluir as alterações restantes na camada de dados, na interface e nos testes. Confirme se todos os testes passam antes de concluir. O servidor já está em execução, portanto você não precisa iniciá-lo.
   ```

6. Observe enquanto o Copilot explora o projeto e localiza os arquivos relacionados à funcionalidade desejada. Ele deverá encontrar os auxiliares da camada de dados, a interface e os testes. Em seguida, começará a modificar arquivos e executar testes.

   ![Captura de tela que mostra o Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, modificar arquivos e executar testes. Dependendo da complexidade da tarefa e da base de código, isso poderá levar alguns minutos. Durante o processo, talvez apareçam os botões **Keep** e **Undo** no editor. Quando o Copilot terminar, você verá um botão **Keep** ou **Undo** para cada alteração; não é necessário selecioná-los enquanto o trabalho estiver em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Captura de tela de uma caixa de diálogo do painel Copilot Chat que pede autorização para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Talvez ocorram algumas pausas e até falhas em alguns testes durante o processo. Não tem problema. O Copilot alterna entre a geração de código e os testes até concluir a tarefa sem detectar erros.

   ![Captura de tela de uma sessão completa do modo de agente do Copilot Chat](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado para identificar possíveis problemas.

> [!CAUTION]
> Lembre-se: é sempre importante revisar o código gerado pelo Copilot ou por qualquer outra ferramenta de IA.

10. Volte ao navegador em que o site está em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Parabéns! Neste exercício, exploramos como usar o modo de agente do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Você aprendeu como:

- usar o modo de agente do GitHub Copilot para implementar recursos na interface e na camada de dados;
- explorar o projeto, identificar arquivos relevantes e coordenar alterações com o modo de agente do Copilot;
- revisar as alterações e os testes gerados pelo modo de agente do Copilot antes de fazer merge na base de código.

Agora, vamos [testar o recurso com o servidor MCP do Playwright][next-lesson] e abrir um pull request.

### Exercício extra de exploração — Implementar paginação

À medida que a lista de jogos cresce, será necessário implementar paginação. Use as habilidades aprendidas neste exercício e peça ao Copilot que atualize o site. Considere:

- seguir as práticas recomendadas existentes, inclusive usar os arquivos de instruções atuais;
- decidir como implementar a paginação: permitir que a pessoa usuária selecione o tamanho da página ou definir esse valor no código;
- fornecer orientações suficientes no prompt para que o Copilot implemente o comportamento desejado;
- iterar com o GitHub Copilot, solicitando alterações e fornecendo contexto. Esse é o fluxo normal ao trabalhar com o Copilot.

## Recursos

- [Copilot: modos Ask, Edit e Agent — para que servem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
