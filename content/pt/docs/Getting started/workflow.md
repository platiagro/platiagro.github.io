---
title: Ciclo de Desenvolvimento de Projetos de IA
linkTitle: Ciclo de Desenvolvimento de Projetos de IA
weight: 10
description: >
  Conheça mais sobre desenvolvimento de modelos para projetos de inteligência
  artificial.
---

As tarefas nativas disponíveis na versão de teste controlado da PlatIAgro foram desenhadas para atender ao ciclo de um projeto de inteligência artificial, mais especificamente para o desenvolvimento de modelos de predição estatística.

![Ciclo de desenvolvimento de um projeto de inteligência artificial](/images/ai-flow.png)

### Preparação dos dados

O desenvolvimento de um modelo de IA começa pela busca dos dados de entrada, ou seja, as informações que serão utilizadas como matéria-prima deste processo. Com este objetivo, uma das tarefas nativas da PlatIAgro localiza e importa arquivos de dados do tipo texto. Outras tarefas podem ser desenvolvidas para esse fim, com outras particularidades.

### Pré-processamento

Esta etapa é importante pois tende a melhorar a acurácia dos modelos. Existem diversas formas de realizar a manipulação de dados para serem consumidos por modelos de IA.  Para auxiliar na realização desta tarefa de forma automática, a PlatIAgro possui algumas tarefas nativas, como a criação e seleção de novos atributos de forma inteligente e automática.

### Modelos ou algoritmos<br>Avaliação, validação e comparação

Para escolher um método específico é necessário entender o tipo do problema, quais informações extras devem ser utilizadas (hiperparâmetros) e qual será o recorte da massa de dados históricos para uma boa acurácia. A PlatIAgro fornece alguns métodos como tarefas nativas: floresta aleatória, regressões e AutoML. O AutoML utiliza um método de otimização que busca pela melhor combinação de métodos e hiperparâmetros de forma automática.

### Implantação

A implantação também é facilitada com a PlatIAgro: utilizando alguns exemplos e instruções didáticas, o desenvolvedor de modelos é direcionado a preparar o código de forma que a implantação seja quase automática, exigindo menos conhecimento técnico.

### Predições

Com o modelo implantado, conforme novos dados de entrada chegam à PlatIAgro por uma aplicação, ela os direciona para o fluxo de tarefas associado ao projeto e realiza suas ações. Ao final, os valores estimados são devolvidos à aplicação.

### Gerenciamento e monitoramento

Para avaliar em tempo real os modelos de IA já implantados (em utilização), a PlatIAgro fornece uma tarefa nativa de monitoramento.
