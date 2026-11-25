---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica — ou se há informações de contexto que o Copilot deve conhecer —, esse contexto precisa estar disponível. Há várias maneiras de compartilhar informações específicas com o Copilot. Uma das principais são os [arquivos de instruções][instruction-files], que fornecem orientações sobre a geração de código.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de codificação e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e de arquivos de instruções com escopo definido por caminho;
- gerar a primeira parte dos dados para a filtragem (um auxiliar de editoras) usando as instruções *atuais*;
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`;
- executar o mesmo prompt novamente e observar o código gerado adotar o novo padrão;
- fazer commit das instruções atualizadas e da parte de filtragem na branch `main` para que o Copilot possa usar as novas orientações no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir todos os padrões definidos, pois o Copilot é não determinístico. O objetivo deste exercício é observar a *tendência* de mudança no comportamento depois da atualização das instruções, não obter um resultado idêntico caractere por caractere.

## Arquivos de instruções

### Cenário

Como toda boa empresa de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- A camada de dados sempre precisa ter testes unitários.
- A interface deve usar o modo escuro e ter uma aparência moderna.
- O código deve incluir documentação na forma de comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever sua finalidade.

Com os arquivos de instruções, você garantirá que o Copilot tenha as informações necessárias para executar as tarefas de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de codificação e seus requisitos. Esse recurso permite orientar as sugestões e os trechos de código para que sejam mais relevantes. Você pode especificar convenções de código, bibliotecas e até os tipos de comentários que prefere. As instruções podem se aplicar ao repositório inteiro ou a tipos específicos de arquivos, fornecendo contexto para determinadas tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um único arquivo de instruções enviado ao Copilot em **todas** as solicitações feitas no repositório. Ele deve conter orientações gerais para o projeto — contexto relevante para a maioria das solicitações feitas ao Copilot Chat ou pela CLI, como a pilha de tecnologias, uma visão geral do que está sendo criado, práticas recomendadas e orientações gerais.
- Arquivos `.github/instructions/*.instructions.md`, que podem ser criados para tarefas ou tipos específicos de arquivos. Você pode usá-los para definir orientações para linguagens específicas, como TypeScript ou Astro, ou para tarefas como criar um componente de interface ou testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, não para conclusões de código ou sugestões da próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam tanto o arquivo de instruções do repositório quanto os arquivos `*.instructions.md` com frontmatter `applyTo`.
>
> O Copilot também oferece suporte a arquivos de instruções em outros padrões, como AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Uma conversa completa sobre a criação de arquivos de instruções está fora do escopo deste workshop. No entanto, os exemplos do projeto ilustram uma abordagem representativa. Em linhas gerais:

- Mantenha as instruções em `copilot-instructions.md` focadas em orientações para o projeto, como uma descrição do que está sendo criado, a estrutura do projeto e os padrões gerais de código.
- Use arquivos `*.instructions.md` para fornecer instruções específicas para tipos de arquivo, como testes unitários, componentes Astro e a camada de dados, ou para determinadas tarefas.
- Use linguagem natural e mantenha as orientações claras. Inclua exemplos do que o código deve ou não deve fazer.

Não há uma única maneira correta de criar arquivos de instruções, assim como não há uma única maneira de usar IA. Experimente para descobrir o que funciona melhor para seu projeto e seus cenários.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto sólido de arquivos de instruções. Ao explorar os arquivos deste projeto, talvez você encontre instruções para vários tipos de tarefas, incluindo [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface oferece essa funcionalidade de uma maneira diferente; por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI. A lição sobre cada interface indicará quando esse recurso for relevante.
>
> Quer encontrar modelos ou um ponto de partida? Explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizadas deste projeto

Leia os arquivos de instruções incluídos neste repositório. Há um arquivo principal, `copilot-instructions.md`, e vários arquivos `*.instructions.md` para tarefas diferentes. Abra-os no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Na seção **Code standards**, observe as orientações aninhadas sobre **GitHub Actions Workflows**. Elas se aplicam a qualquer interação com o Copilot.
3. Abra a pasta `.github/instructions` e explore seu conteúdo. Observe que há instruções para arquivos Astro, a camada de dados Drizzle, testes e muito mais.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo. Ele define um padrão glob (relativo à raiz do repositório) que determina os arquivos aos quais as instruções se aplicam. Neste caso, qualquer arquivo de teste TypeScript — por exemplo, um que corresponda a `**/*.test.ts` — será abrangido.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Por fim, abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes no projeto. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório. Assim, você pode dividir conjuntos de instruções maiores em arquivos menores e reutilizáveis e apontar o Copilot para exemplos a seguir durante a geração do código.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de código do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentários nos arquivos.

## Criar uma branch para as alterações

Vamos seguir algumas práticas recomendadas e criar uma branch para as alterações.

1. Volte ao codespace usado no exercício anterior.
2. Abra um novo terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie uma branch e mude para ela:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar o Copilot Chat *antes* de atualizar as instruções

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. A seleção de modo e modelo varia para cada exercício; cada etapa indicará o que escolher quando for relevante.

Para observar o impacto das instruções personalizadas, primeiro envie um prompt com o arquivo de instruções atual. Depois, atualize-o e envie o mesmo prompt novamente para ver a diferença.

1. Feche as guias do editor abertas nos exercícios anteriores para que o Copilot use somente o contexto desejado.
2. Abra `src/lib/publishers.ts` para mostrar ao Copilot onde o auxiliar deve ficar.
3. Na lista de agentes da exibição Chat, selecione **Agent** para que o Copilot possa aplicar alterações aos arquivos.

   ![Captura de tela da lista de agentes na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envie o seguinte prompt:

   ```plaintext
   Crie ou atualize src/lib/publishers.ts com um auxiliar de acesso a dados que retorne uma lista de todas as editoras, com o nome e a ID de cada uma. Aplique as alterações aos arquivos.
   ```

5. O Copilot explorará o projeto e aplicará atualizações ao código, geralmente tanto no arquivo do auxiliar quanto nos testes.
6. Observe que o auxiliar proposto é uma função tipada que recebe um cliente `db` como primeiro argumento e retorna uma matriz tipada de editoras. Essas convenções vêm das instruções da camada de dados em `.github/instructions/drizzle.instructions.md`, que se aplicam a `src/lib/*.ts`.
7. Observe que o código proposto **não inclui** comentários TSDoc nem um cabeçalho de comentários no nível do arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, talvez ele inclua comentários de documentação mesmo sem receber essa orientação. Tudo bem — o objetivo é observar a melhora de *consistência* depois da atualização das instruções.

## Adicionar um novo padrão para o repositório

Como mencionado, `.github/copilot-instructions.md` fornece informações gerais do projeto ao Copilot. Vamos documentar os padrões de código do repositório para melhorar as sugestões.

1. Abra `.github/copilot-instructions.md` novamente.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe como ela documenta os padrões de código, mas ainda não contém regras para documentação no código. Por isso, o auxiliar gerado não tinha comentários de documentação.
3. Adicione as linhas Markdown a seguir logo após os padrões existentes para orientar o Copilot a incluir cabeçalhos de comentários nos arquivos e comentários TSDoc:

   ```markdown
   - Toda função exportada deve ter um comentário TSDoc que descreva sua finalidade, seus parâmetros e seu valor de retorno.
   - Antes das importações ou de qualquer código, adicione ao arquivo um bloco de comentários que explique sua finalidade.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instruções podem ser criados no nível do repositório (`.github/copilot-instructions.md`) para definir orientações gerais ou como arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o local adequado para padrões gerais, como a regra sobre comentários de documentação que você acabou de adicionar.

## Executar o prompt novamente e observar a alteração

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o contexto.
2. Selecione novamente `src/lib/publishers.ts` para que o Copilot se concentre no arquivo correto.
3. Envie **o mesmo prompt** de antes:

   ```plaintext
   Crie ou atualize src/lib/publishers.ts com um auxiliar de acesso a dados que retorne uma lista de todas as editoras, com o nome e a ID de cada uma. Aplique as alterações aos arquivos.
   ```

4. Observe que o arquivo proposto agora começa com um bloco de comentários semelhante a este:

   ```typescript
   /**
    * Auxiliares de acesso aos dados de editoras da plataforma de financiamento coletivo da Tailspin Toys.
    * Fornece funções para recuperar informações de editoras no banco de dados.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a este:

   ```typescript
   /**
    * Retorna uma lista de todas as editoras com suas IDs e seus nomes.
    *
    * @param db - O cliente do banco de dados Drizzle.
    * @returns Uma promessa que é resolvida com uma matriz de objetos de editoras.
    */
   ```

Você acabou de orientar o Copilot a seguir um novo padrão do projeto e aplicá-lo ao código real que servirá de base para o próximo exercício.

## Fazer commit das instruções e enviar a branch

Os arquivos de instruções são recursos do repositório, assim como qualquer outro arquivo, e são gerenciados com o mesmo fluxo de controle do código-fonte. Vamos fazer commit das alterações e enviar a branch ao repositório.

1. Abra uma nova janela do terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. No terminal, confirme se as alterações nas instruções e no auxiliar estão presentes:

   ```bash
   git status
   ```

3. No terminal, prepare e faça commit da atualização das instruções e de todos os arquivos alterados pelo Copilot para a base do auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se git status mostrar outras atualizações de suporte (por exemplo, testes), adicione também esses arquivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Criar e fazer merge de um pull request

Depois de enviar a branch, criaremos um pull request e o associaremos à issue sobre o padrão de documentação no backlog. Poderíamos fazer isso manualmente, mas o Copilot pode cuidar dessa tarefa com as ferramentas do GitHub já conectadas ao projeto. Vamos pedir ao Copilot que encontre a issue, crie um PR vinculado a ela e faça o merge.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot executa ações no GitHub em seu nome. O modelo do projeto já conecta as ferramentas do GitHub ao workspace, então não é necessário configurar nada. Na primeira vez que o Copilot usar uma ferramenta do GitHub, o VS Code talvez peça para você entrar no GitHub. Siga as instruções para permitir a conexão. Você aprenderá como ela funciona (Model Context Protocol) em um exercício posterior.

1. Abra o Copilot Chat no codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a exibição Copilot Chat e confirme se **Agent** está selecionado na lista de agentes.
3. Peça ao Copilot que encontre a issue sobre a atualização dos arquivos de instruções e crie um PR a partir da branch atual, descrevendo tanto as atualizações das instruções quanto a nova base do auxiliar de editoras:

   ```
   Encontre a issue relacionada à atualização do arquivo de instruções. Crie um novo PR a partir da branch atual, destaque que o PR fecha essa issue e mencione que também adicionamos a base do auxiliar de editoras para o próximo trabalho de filtragem.
   ```

4. O Copilot começará a procurar a issue e criar o PR.
5. Se o Copilot pedir autorização para executar ações no GitHub, revise o comando e selecione **Allow**, se apropriado.
6. Depois da criação do PR, peça ao Copilot que faça o merge e retorne sua branch local para `main`:

   ```
   Faça merge do PR em main. Depois, volte para main localmente e obtenha o código mais recente para atualizar o repositório.
   ```

7. Se o Copilot pedir autorização para executar ações no GitHub e comandos do shell em seu nome, revise o comando e selecione **Allow**, se apropriado.

Você acabou de criar e mesclar um pull request com a ajuda do GitHub Copilot!

## Resumo e próximas etapas

Você explorou como o Copilot obtém contexto dos arquivos de instruções deste projeto e, em seguida, usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado com as instruções *existentes*;
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`;
- enviar o mesmo prompt novamente e observar o código proposto adotar o novo padrão;
- fazer commit das instruções atualizadas e da base do auxiliar em `main` para que o próximo exercício possa usá-las.

Em seguida, você aplicará essas instruções no [modo de agente][next-lesson], enquanto o Copilot adiciona um novo recurso à base de código.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]

[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
