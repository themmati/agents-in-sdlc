---
title: "Exercício 3 - Testar seu recurso com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar o recurso de filtragem com o modo de agente. Antes de abrir um pull request, confirme se ele realmente funciona no navegador. Em vez de testar o aplicativo manualmente, conecte o **servidor MCP do Playwright** e deixe o Copilot controlar um navegador real para testar o recurso. Em seguida, publique sua branch e abra o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam os recursos do Copilot;
- adicionar o servidor MCP do Playwright ao workspace;
- pedir ao Copilot que use o servidor para testar manualmente o recurso de filtragem no navegador;
- publicar a branch e abrir um pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente se torna muito mais poderoso quando pode acessar recursos fora do editor. O Copilot usa o Model Context Protocol (MCP) para isso: um padrão que permite ao agente se comunicar com ferramentas e serviços externos.

![Diagrama que mostra o funcionamento interno do modo de agente e como ele interage com contexto, LLM e ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de IA se comuniquem com ferramentas e serviços externos em tempo real. Assim, eles podem acessar informações atualizadas (usando recursos) e executar ações em seu nome (usando ferramentas).

Essas ferramentas e recursos são acessados por um servidor MCP, que atua como uma ponte entre o agente de IA e as ferramentas e serviços externos. O servidor MCP gerencia a comunicação entre o agente de IA e as ferramentas externas, como APIs existentes ou ferramentas locais, incluindo pacotes npm. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente de IA pode acessar.

Alguns servidores MCP conhecidos:

- **[Servidor MCP do GitHub](https://github.com/github/github-mcp-server)**: fornece acesso a um conjunto de APIs para gerenciar repositórios do GitHub. Ele permite que o agente de IA crie repositórios, atualize repositórios existentes e gerencie issues e pull requests.
- **[Servidor MCP do Playwright](https://github.com/microsoft/playwright-mcp)**: oferece recursos de automação de navegador com o Playwright. Ele permite que o agente de IA navegue por páginas Web, preencha formulários e selecione botões.

Há muitos outros servidores MCP que fornecem acesso a ferramentas e recursos diferentes. O GitHub hospeda um [registro MCP](https://github.com/mcp) para facilitar a descoberta de servidores e as contribuições para o ecossistema.

> [!CAUTION]
> Em relação à segurança, trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor, revise cuidadosamente o código-fonte, verifique o editor e avalie as implicações de segurança. Use somente servidores MCP confiáveis e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revisar a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste workspace do VS Code.

1. Abra `.vscode/mcp.json` no codespace.
2. Você verá um servidor `github` já configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

A entrada `github` vem do modelo do projeto. Por isso, o Copilot conseguiu ler o backlog de issues no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], então não é necessário instalar nada localmente. O VS Code se conecta por HTTP e, na primeira vez que o Copilot usar uma das ferramentas, você fará a autenticação no GitHub.

## Adicionar o servidor MCP do Playwright

Agora você adicionará um segundo servidor. O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador que ele pode controlar, o que é exatamente o necessário para testar o recurso.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github`, para que o arquivo fique assim:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Salve o arquivo.

Diferentemente do servidor remoto `github`, o `playwright` é um servidor **local**: o VS Code inicia o servidor na sua máquina executando `npx @playwright/mcp@latest`. O sinalizador `--headless` faz com que o Playwright execute o navegador sem uma janela visível. Isso é necessário em um codespace, que não tem um desktop para exibi-la.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright nos testes de ponta a ponta, então o navegador necessário geralmente já está instalado. Se o Copilot informar que não há um navegador disponível, peça que execute `npx playwright install chromium` e tente novamente.

## Iniciar e confiar no servidor

O VS Code inicia servidores MCP **sob demanda**: na primeira vez que o Copilot precisar das ferramentas do servidor, o VS Code o iniciará e pedirá que você confirme se confia nele.

1. Em `.vscode/mcp.json`, aparecerá uma ação **Start** acima da entrada `playwright`. Selecione-a para iniciar o servidor agora e confirmar se a configuração está correta.
2. Se o VS Code pedir que você confirme se confia no servidor, revise a configuração e escolha a opção para confiar nele e permitir que seja iniciado.

Iniciá-lo manualmente é opcional. Se você ignorar esta etapa, o VS Code iniciará o servidor e pedirá que você confirme se confia nele na próxima etapa, quando o Copilot precisar das ferramentas.

## Testar o recurso de filtragem

O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador real para controlar. Em vez de percorrer o aplicativo manualmente, o agente pode abrir uma página, navegar, aplicar filtros, ler os resultados e resumir o que observou. É a maneira mais rápida de confirmar se um recurso funciona como esperado, sem sair da conversa.

Nos bastidores, o servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página, em vez de capturas de tela. Assim, o agente avalia elementos estruturados e rotulados (botões, links, itens de lista) como a tecnologia assistiva faz. Portanto, uma verificação funcional rápida também pode ajudar a identificar problemas básicos de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot que teste o recurso de filtragem:

```text
Usando o servidor MCP do Playwright, abra um navegador para o aplicativo em execução em http://localhost:4321 e verifique o novo recurso de filtragem de jogos:

1. Acesse a página de jogos e observe quantos jogos estão listados.
2. Aplique um filtro de categoria e confirme se a lista passa a mostrar somente jogos dessa categoria.
3. Limpe o filtro e aplique um filtro de editora. Confirme se a lista mostra somente os jogos dessa editora.
4. Combine um filtro de categoria com um de editora e confirme se os resultados correspondem a ambos.

Relate o que observou em cada etapa e destaque qualquer comportamento inesperado.
```

O Copilot abrirá um navegador pelo servidor MCP do Playwright, seguirá cada etapa e relatará o que encontrou. Compare o resumo com os critérios de aceitação da issue. Se algo parecer errado, faça perguntas complementares ou peça que o Copilot corrija o código antes de abrir o pull request.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321` para esse teste. Se você tiver parado o servidor de desenvolvimento, inicie-o novamente antes de enviar o prompt. Na primeira vez que usar o servidor MCP do Playwright, talvez o Copilot precise baixar um navegador. Se ele informar que não há um navegador disponível, peça que execute `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar a branch e criar um pull request

Agora que confirmou que o recurso funciona, você pode abrir um pull request (PR) para que a equipe o revise. O primeiro passo é publicar a branch `filtering-vscode`.

1. Acesse o painel **Source Control** no codespace e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Captura de tela do painel Source Control com as alterações feitas](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Criar o pull request

Há várias maneiras de criar um pull request, inclusive pelo github.com e pela interface de linha de comando (CLI) do GitHub. Como você já está trabalhando com o GitHub Copilot, vamos pedir a ele que crie o PR. Ele poderá localizar a issue correspondente e associá-la ao PR.

1. Acesse o painel Copilot Chat e selecione **New Chat** para iniciar uma nova sessão.
2. Confirme se **Agent** está selecionado na lista de agentes para que o Copilot possa usar as ferramentas do GitHub.
3. Peça ao Copilot que crie o PR:

   ```plaintext
   Encontre no repositório a issue relacionada à filtragem por categoria e editora. Crie um novo pull request para a branch atual e associe-o à issue correta.
   ```

4. Se necessário, selecione **Continue** para permitir que o Copilot reúna informações e execute as operações. Na primeira vez que usar uma ferramenta do GitHub, talvez seja solicitado que você se autentique. Siga as instruções para permitir a autenticação.
5. Observe enquanto o Copilot pesquisa as issues, encontra a correta e cria o PR.
6. Selecione o link gerado pelo Copilot para revisar o pull request, mas **não faça merge ainda**.

## Resumo e próximas etapas

Parabéns! Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam os recursos do Copilot;
- adicionou o servidor MCP do Playwright ao workspace;
- usou o servidor para testar manualmente o recurso de filtragem no navegador antes de entregá-lo;
- publicou a branch e abriu um pull request para o trabalho de filtragem.

Você usou um servidor MCP para *testar* um recurso, mas o MCP é apenas uma das maneiras de ampliar o Copilot. Em seguida, vamos [criar um agente personalizado][next-lesson] para facilitar tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Playwright da Microsoft][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
