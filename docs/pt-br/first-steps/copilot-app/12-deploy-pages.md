---
title: "Lição 12 - Implantar o site com o GitHub Pages"
description: "Use o aplicativo GitHub Copilot para configurar um fluxo de trabalho do Pages, revisar o pull request e verificar o Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Publique o Space Quiz no GitHub Pages para que ele fique disponível publicamente. O Copilot pode preparar o fluxo de trabalho e orientar a implantação, mas cabe a você revisar as alterações e confirmar que o site está no ar.

Nesta lição, você vai:

- pedir ao Copilot para configurar a implantação no GitHub Pages;
- revisar o fluxo de trabalho e o pull request antes do merge;
- acompanhar a implantação e verificar o site publicado.

## Cenário

O Space Quiz já está em um repositório do GitHub, mas as pessoas ainda precisam de uma maneira simples de experimentá-lo. Publicar o site, que consiste em um único arquivo, no GitHub Pages disponibiliza o site em uma URL pública, sem adicionar um servidor ou uma dependência de ambiente de execução.

## Pedir ao Copilot que prepare a implantação

> [!WARNING]
> A criação de um pull request pode fazer commit e enviar código. Revise as configurações de aprovação e o repositório de destino antes de enviar este prompt. Se a opção **Approve all** estiver selecionada, não espere uma solicitação de confirmação separada.

Na sessão do aplicativo Copilot para `space-quiz`, envie este prompt:

```plaintext
Prepare uma implantação do GitHub Pages para este repositório. Crie um fluxo de trabalho do GitHub Actions que compile o site, se necessário, e o implante no GitHub Pages depois que as alterações chegarem à branch padrão. Adicione um README que descreva o aplicativo e explique como executá-lo localmente, caso ainda não exista um. Crie um pull request com essas alterações e informe o link e quaisquer configurações ou permissões do Pages necessárias. Não faça merge, não habilite o merge automático, não altere as configurações do Pages nem implante o site. Pare e aguarde minha revisão.
```

## Revisar o fluxo de trabalho e o pull request

1. Confirme se o README foi adicionado somente se o repositório ainda não tinha um.
2. Revise as alterações do pull request, incluindo as etapas de compilação e implantação, os gatilhos e as permissões do fluxo de trabalho.
3. Confirme se o fluxo implanta a partir da branch padrão, e não de um pull request ainda não mesclado.
4. Solicite as correções necessárias e revise o diff atualizado. Aguarde a aprovação das verificações obrigatórias do pull request antes de autorizar o merge.

## Autorizar o merge e a implantação

> [!WARNING]
> O prompt a seguir autoriza o agente a fazer merge do pull request revisado, alterar as configurações do Pages do repositório e publicar o site. Envie-o somente depois de concluir a revisão e confirmar o repositório e o pull request. Dependendo das configurações de aprovação, essas ações podem ocorrer sem uma nova solicitação de confirmação.

Substitua `<PR_NUMBER>` pelo número do pull request que você revisou e envie este prompt separado:

```plaintext
Revisei o pull request <PR_NUMBER> neste repositório e autorizo o merge e a implantação. Habilite o GitHub Pages usando o GitHub Actions, faça merge desse pull request somente depois que as verificações obrigatórias forem aprovadas e monitore a implantação até que ela seja concluída com êxito. Informe a URL final do site publicado. Se você não tiver permissão para habilitar o Pages ou fazer merge, informe exatamente o que preciso aprovar ou fazer; não informe que a tarefa foi concluída até que o site esteja no ar.
```

![Diagrama em três etapas: um pull request do space-quiz mesclado; uma execução bem-sucedida de Deploy website no GitHub Actions, com as etapas Build website, Upload artifact e Deploy to Pages; e o Space Quiz publicado em yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

## Verificar a implantação

1. Acompanhe a execução de implantação do GitHub Actions até que ela seja concluída com êxito.
2. Abra a URL informada e confirme se o Space Quiz carrega.

Se o agente não puder habilitar o Pages ou fazer merge do pull request, siga as instruções específicas fornecidas por ele. Não considere a tarefa concluída até que a implantação seja bem-sucedida e o site esteja no ar.

## Resumo e próximos passos

Agora o Space Quiz está disponível como um site publicado, e você verificou a implantação em vez de confiar apenas no relato do agente. Continue para [Lição 13: Revisão e próximos passos][next-lesson].

[next-lesson]: ../13-review/
