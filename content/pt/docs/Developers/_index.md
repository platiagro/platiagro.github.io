---
title: FAQ Desenvolvedores
linkTitle: FAQ Desenvolvedores
weight: 10
description: >
  Informações para desenvolvedores da plataforma.
---

### 1. Como fazer o build do software localmente?
Frontend: O frontend da plataforma utiliza o ReactJS. As instruções de build
estão no [README.md do repositório web-ui](https://github.com/platiagro/web-ui#readme),
e envolvem a utilização do `yarn`, ou `docker`.

Backend: O backend da plataforma é composto por vários micro-serviços. Os serviços
[projects](https://github.com/platiagro/projects) e [datasets](https://github.com/platiagro/datasets)
possuem instruções de build no README.md, e envolvem a utilização `python` ou `docker`.

O [JupyterLab](https://github.com/platiagro/jupyterlab) e a
[extensão do Jupyterlab](https://github.com/platiagro/jupyterlab-extension) também
possuem instruções de build no README.md de seus repositórios. Ambos utilizam
`typescript` e `python`.

Os demais serviços foram aproveitados do Kubeflow e, embora sejam parte da plataforma,
o build é realizado pelo time do Kubeflow. Consulte as instruções nos repositórios
[kubeflow/kubeflow](https://github.com/kubeflow/kubeflow) e
[kubeflow/pipelines](https://github.com/kubeflow/pipelines).

### 2. Como testar o software localmente?
Frontend: **Não possui testes unitários implementados.** Testes funcionais podem ser
realizados seguindo as instruções no README.md do repositório
[web-ui](https://github.com/platiagro/web-ui).

Backend: Os serviços [projects](https://github.com/platiagro/projects#testing),
[datasets](https://github.com/platiagro/datasets#testing) e o
[SDK](https://github.com/platiagro/sdk#testing) podem ser testados com o `pytest`.<br>
**Atenção! Defina as variáveis de ambiente e credenciais antes de rodar os testes.**
Os testes e a API precisam desses serviços para executar com sucesso.

### 3. Como preparar meu ambiente de desenvolvimento?
De forma geral, siga as instruções do README de cada repositório:
- [Frontend](https://github.com/platiagro/web-ui#readme)
- [Backend projects](https://github.com/platiagro/projects#readme)
- [Backend datasets](https://github.com/platiagro/datasets#readme)
- [SDK](https://github.com/platiagro/sdk#readme)
- [Jupyterlab extension](https://github.com/platiagro/jupyterlab-extension#readme)

**Recomendo o uso do [Visual Studio Code](https://code.visualstudio.com/) como IDE.**

### 4. Onde fica o código-fonte?
Em vários respositórios da organização [PlatIAgro](https://github.com/platiagro/).<br>
Qualquer pessoa é livre para realizar `git clone` (é um projeto Open-Source!)<br>
Para fazer commits, solicite autorização a um dos [administradores da organização](https://github.com/orgs/platiagro/people).

### 5. Onde está o pipeline de CI/CD? Como ele funciona?
O fluxo de CI é operado pelo [GitHub Actions](https://docs.github.com/en/actions).
Cada repositório configura seu workflow através de arquivos `.github/workflows/[algum-nome].yaml`.
Estes fluxos geralmente rodam testes unitários e fazem o build de imagens docker. Alguns exemplos:
- [Fluxo de CI do Projects](https://github.com/platiagro/projects/blob/master/.github/workflows/ci.yml)
- [Fluxo de CI das Tarefas](https://github.com/platiagro/tasks/tree/main/.github/workflows)
- [Fluxo de CI do Notebook Server](https://github.com/platiagro/kubeflow/blob/v0.2.0-kubeflow-v1.3-branch/.github/workflows/ci-platiagro-notebook-servers.yaml)

**Não há fluxo de CD operado pelo GitHub Actions.**

### 6. Onde está o backlog/roadmap da PlatIAgro?
O [CPQD](https://www.cpqd.com.br/inovacao/platiagro/) é o principal contribuinte
do roadmap da PlatIAgro. [Ferramentas internas](https://jira.cpqd.com.br/secure/RapidBoard.jspa?rapidView=3414) do CPQD guardam as informações do Roadmap.

### 7. Como funcionam os ambientes de validação/integração/testes?
O CPQD mantém [ambientes da plataforma para validação](https://docs.google.com/spreadsheets/d/1o4cjXUxYVU9mR8uomjZt1Bu1PAv3CK-SwT7VEA_mIgk/edit#gid=0). <br>
**O acesso é restrito a colaboradores do CPQD.** Cada ambiente possui um responsável
que realiza a sua atualização quando necessário.

### 8. Quem dá suporte em produção da PlatIAgro?
Não há equipe dedicada de suporte. Se desejar suporte, entre em [contato com o CPQD](https://www.cpqd.com.br/contato/).

### 9. Onde está a documentação da PlatIAgro?
Boa parte da documentação é mantida neste site. 😄<br>
Para editá-la, altere os fontes no repositório [platiagro/platiagro.github.io](https://github.com/platiagro/platiagro.github.io).<br>
Algumas outras fontes de documentação:
- [GitHub](https://github.com/platiagro/platiagro#readme)
- [Tutoriais](https://platiagro.github.io/tutorials/)
- [SwaggerUI Projects](https://platiagro.github.io/projects/)
- [SwaggerUI Datasets](https://platiagro.github.io/datasets/)
- [ReadTheDocs](https://platiagro.github.io/sdk/)
- [Apresentação - 30 min.](https://www.figma.com/proto/wkovXss54OsGls71umPKfA/Apresenta%C3%A7%C3%A3o?node-id=131%3A261&viewport=-170%2C266%2C0.028179358690977097&scaling=scale-down&hide-ui=1)  (acesso restrito a colaboradores do CPQD)
- [Apresentação - Pitch 5 min.](https://docs.google.com/presentation/d/1SGu7-Zcoyg9vffpyDCNpgkgUuyNEA7T8AK6fBgJZ4gM/edit#slide=id.g81e2827eff_0_0)
- [Google Drive](https://drive.google.com/drive/folders/0AKqhPKPk7ynyUk9PVA)  (acesso restrito a colaboradores do CPQD)
- [Protótipo](https://www.figma.com/file/BjmtaOk5P68OJARn4yPVfg/Plataforma-PlatIAgro?node-id=4291%3A27011)  (acesso restrito a colaboradores do CPQD)

### 10. Qual o papel de cada um no time?
- Gestora de Projeto: [Rosane Conti](https://br.linkedin.com/in/rosane-conti-604301)
- Gestor de Solução: [Norberto Alves Ferreira](https://br.linkedin.com/in/norberto-alves-ferreira-10b85)
- Estatística/Líder Técnica: [Graziella Bonadia](https://br.linkedin.com/in/graziella-bonadia-66a3a255)
- Arquiteto de Software: [Fábio Beranizo](https://github.com/fberanizo)
- UX: [Fabiani de Souza](https://br.linkedin.com/in/fabianisouza)
- Desenvolvedores Frontend: [Cedric Matheus](https://github.com/cedric-matheus), [Giancarlo Schaffer Jr.](https://github.com/schafferjrdev), [Luan Costa](https://github.com/LuanEdCosta)
- Desenvolvedores Backend: [Miguel Ferraz](https://github.com/miguelfferraz), [Danilo Carvalho](https://github.com/dnlcesilva), [André Luis Nogueira](https://github.com/andreluiz27)
- Engenheiros de Machine Learning: [Matheus Sasso](https://github.com/math-sasso), [Gabriela Vechini](https://github.com/gvechini), [Luiz Borro](https://github.com/lborro), [Andreza Santos](https://github.com/andisantos), [Lucas Sequeira](https://github.com/lucasns97)

### 11. Com que frequência o time se reune?
Há reuniões diárias com o time de software e 2 vezes na semana com o time completo (Machine Learning + Software).<br>
**As reuniões são restritas ao time do CPQD.**

### 12. Com quem devo falar para tirar "dúvidas de iniciante"?
Todos os [membros do projeto](https://github.com/orgs/platiagro/people) dispostos a ajudar. 😄<br>
Se quiser um contato, mande um email para o Arquiteto de Software: `fabiol@cpqd.com.br`.

### 13. Quem decide as novas features da PlatIAgro?
O time do CPQD prioriza o [Roadmap da PlatIAgro](https://jira.cpqd.com.br/secure/RapidBoard.jspa?rapidView=3414).<br>
As features geralmente surgem das necessidades levantadas em projetos de P&D com parceiros da indústria.<br>
Conheça mais no [site do CPQD](https://www.cpqd.com.br/inovacao/platiagro/#1619651082570-a922210b-f7df).

Deseja sugerir uma feature? Entre em contato por email, ou abra uma [issue no GitHub](https://github.com/platiagro/platiagro/issues).

### 14. Qual o canal de comunicação do time?
A principal forma de comunicação é por e-mail.<br>
Os colaboradores do CPQD também utilizam as salas do Google Chats:
- [Todos os membros do projeto](https://mail.google.com/mail/u/0/#chat/space/AAAAkR407TA).
- [Time de software](https://mail.google.com/mail/u/0/#chat/space/AAAAkSLt984).

### 18. Quais são os principais problemas da PlatIAgro? (em software)
- Ainda não temos uma solução 100% multi-tenant (há tabelas e canais de comunicação compartilhados entre os usuários).
- O gerenciamento de Datasets não é otimizado para Big Data. Há problemas na implementação do SDK para dados que não caibam em memória.
- Não temos interface internacionalizada (multi-idiomas).

### 19. Que features as pessoas "querem ver" na PlatIAgro?
- Gestão de Usuários/Tenants
- Internacionalização da Interface
- Integração com os Produtos do CPQD (ASR, TTS, Billing, Plat. Decisão)
- Integração com plataformas IoT
- Integração com ferramentas da AWS, GCloud, Azure...
- Marketplace de Modelos

### 20. Qual o ciclo de *releases* da PlatIAgro?
Geralmente lançamos uma versão a cada 6~12 meses.<br>
A [versão 0.1.0](https://github.com/platiagro/platiagro/releases/tag/v0.1.0) foi lançada em Nov. 2020 e a [versão 0.2.0](https://github.com/platiagro/platiagro/releases/tag/v0.2.0) em Jul. 2021.
