---
title: "Lição 2 - Adicionar classificação por estrelas: uma vitória rápida"
description: "Use o Copilot CLI para fazer uma pequena alteração nos cartões de jogos, revise-a em um navegador encaminhado e faça merge do primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou o Copilot CLI e experimentou uma conversa, é hora de fazer sua primeira alteração no projeto. Ela será pequena: os jogos já têm uma classificação por estrelas nos dados, mas os cartões da página inicial ainda não a exibem. Você pedirá ao agente que mostre a classificação, revisará a alteração e fará o merge no seu primeiro pull request.

Nesta lição, você vai:

- iniciar uma conversa focada com o Copilot em uma branch de recurso;
- pedir ao agente que faça uma pequena alteração no projeto;
- revisar a alteração com `/diff`;
- executar o aplicativo e confirmar a alteração em um navegador encaminhado;
- abrir e fazer merge do primeiro pull request.

## Cenário

Cada jogo no Tailspin Toys pode ter uma classificação por estrelas, que já aparece na página de detalhes do jogo. No entanto, os cartões de jogos na página inicial exibem somente o título, a categoria, a editora e a descrição. Como aquecimento, você pedirá ao agente que mostre a classificação existente em cada cartão: uma alteração pequena e independente, ideal para sua primeira sessão.

## Anatomia de uma conversa

Uma **conversa** é onde você trabalha com o Copilot CLI em uma tarefa. Diferentemente do aplicativo Copilot, uma conversa normal da CLI usa o repositório e a branch atualmente selecionados no terminal, em vez de criar uma worktree dedicada. As conversas salvas permitem retomar a mesma discussão depois, enquanto os arquivos e a branch continuam sendo o estado comum do Git no disco.

Em uma conversa, você verá três elementos: seus prompts e as respostas do agente, a atividade das ferramentas que o agente usa para explorar e editar arquivos e as alterações que você pode inspecionar com `/diff`.

## Iniciar uma conversa e solicitar a alteração

Vamos iniciar uma nova conversa para começar a implementar o recurso.

1. Volte ao Codespace.
2. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Se o Copilot ainda não estiver em execução, inicie-o com o seguinte comando:

   ```bash
   copilot --yolo
   ```

4. Inicie uma nova sessão usando o comando de barra `/new` e pressionando <kbd>Enter</kbd>.
5. Use o seguinte prompt para solicitar a alteração:

   ```plaintext
   Mostre a classificação starRating de cada jogo, de 5 estrelas, nos cartões da lista de jogos. Se a classificação for null, mostre "Ainda sem classificação". Mantenha o layout do cartão, adicione testes e execute as verificações relevantes.
   ```

O Copilot explorará o projeto, localizará os arquivos usados para exibir os detalhes dos jogos e criará o código necessário. Você acabou de adicionar um recurso com o Copilot CLI!

## Revisar o diff

Todas as alterações geradas por IA precisam ser revisadas antes do merge, mesmo as pequenas. Vamos explorar as alterações diretamente no Copilot CLI.

1. Digite `/diff` e inspecione cada arquivo alterado.
2. Confirme se o cartão do jogo mostra a classificação numérica quando ela existe e `Ainda sem classificação` quando `starRating` é `null`.
3. Confirme se os testes cobrem os dois estados.
4. Revise os resultados das verificações executadas pelo Copilot e peça que ele corrija as falhas.
5. Quando terminar a revisão, pressione <kbd>Esc</kbd> para sair da tela de diff.

> [!NOTE]
> Como o Copilot, assim como todas as ferramentas de IA generativa, é probabilístico e não determinístico, o código exato pode variar. Revise o comportamento em vez de esperar uma implementação específica.

## Verificar as alterações

Não devemos apenas ler o código e presumir que funciona. Vamos pedir ao Copilot que inicie o site para que possamos examinar a interface atualizada no navegador encaminhado pelo Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Inicie o aplicativo para que eu possa inspecionar no navegador a alteração da classificação por estrelas. Informe a URL e mantenha o servidor em execução.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme se os cartões dos jogos mostram as classificações de até cinco estrelas.
4. Volte ao Copilot e peça que ele pare o servidor iniciado:

   ```plaintext
   Pare o servidor de desenvolvimento que você iniciou.
   ```

## Abrir e fazer merge do primeiro pull request

Você criou o recurso. Agora é hora de abrir um pull request (PR) para mesclar o novo código ao projeto.

1. Peça ao agente padrão que faça commit da alteração:

   ```plaintext
   Faça commit das alterações revisadas na classificação por estrelas, usando uma mensagem de commit adequada.
   ```

2. Digite `/pr create`. Ao criar o PR, o Copilot CLI enviará o commit existente e exibirá a URL do PR.
3. Abra o PR mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando a URL exibida pelo Copilot CLI.
4. Revise os arquivos alterados e as verificações.
5. Quando estiver tudo pronto, selecione **Merge pull request** e confirme o merge.
6. Volte ao Codespace e saia do Copilot CLI com `/exit`.
7. Atualize sua branch local `main`:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximos passos

Parabéns! Você entregou sua primeira alteração com o GitHub Copilot CLI. Especificamente, você:

- iniciou uma conversa focada com o Copilot em uma branch de recurso;
- orientou o agente a fazer uma pequena alteração nos cartões dos jogos;
- revisou a alteração com `/diff`;
- executou o aplicativo e confirmou a classificação por estrelas em um navegador encaminhado;
- abriu e fez merge do primeiro pull request.

Em seguida, você [começará pela issue de filtragem e usará os modos Plan e Autopilot][next-lesson] para criar um recurso maior.

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
