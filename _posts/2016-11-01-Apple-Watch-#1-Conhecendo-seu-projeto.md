---
layout: post
title:  "Apple Watch #1 - Conhecendo seu projeto"
date:   2016-09-25 12:00:00 +0000
categories: [watchOS]
---

Olá pessoal, nesse post estarei começando com uma sequência de exemplos com o desenvolvimento de aplicativos para **Apple Watch**.

## 1. Criando projeto

Abra o `Xcode` e selecione **create new project**. Escolha o template `watchOS` e a aplicação `iOS App with WatchKit App`, assim como na imagem a seguir:

![](/static/img/posts/watchOS/watchOS_template.png)

O próximo passo é escolher as opções do seu projeto, como nome, linguagem, devices, etc..
Eu escolhi o nome **AppleWatchDev** mas fiquem a vontade para alterar. Só tenham *atenção no seguinte ponto*: Deixe apenas o primeiro checkbox marcado, no decorrer do projeto iremos explicar mais afundo cada item desse grupo.

![](/static/img/posts/watchOS/watchOS_project_name.png)

Buuum! Nosso primeiro projeto para Apple Watch foi criado.

## 2. Conhecendo o ambiente

Observe o menu do lado esquerdo do seu Xcode, para quem está acostumado a desenvolver para iOS, vai notar que existem mais duas pastas além do nome do seu projeto:

* `NomeDoProjeto WatchKit App`
* `NomeDoProjeto WatchKit Extension`

![](/static/img/posts/watchOS/left_menu.png)

Na primeira pasta, temos o `Nome do seu projeto`, onde ficará todo o código responsável pelo seu aplicativo **iOS**.

Na segunta pasta, temos `NomeDoProjeto WatchKit App`, onde iremos trabalhar com toda a parte **visual** do nosso aplicativo para o Apple Watch.

Na terceira pasta, temos `NomeDoProjeto WatchKit Extension`, onde ficará todo o código responsável pelo seu aplicativo **Apple Watch**.

Agora que você conheceu um pouco do ambiente do seu projeto, vamos para o próximo passo :)
