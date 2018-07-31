# Git e GitHub - Controle de versão

## Versionamento

O versionamento permite:

* Gerenciar o histórico dos arquivos;
* Facilita a colaboração durante o desenvolvimento;
* Marcar versões estáveis do código;
* Criar novas funcionalidades sem influenciar o código em funcionamento.

Possiveis problemas que podem aparecer sem a utilização do versionamento:

* Dificuldade em determinar qual a versão mais recento do seu código;
* Um colega trabalhar com uma versão do código diferente da sua;
* Muito tempo gasto para unificar o trabalho;
* Não conseguir regredir o projeto para um estado anterior ao o que estava antes de detectar um bug.

## Ferramentas de versionamento

* Mercurial
* Subversion
* **Git**

## Porque usar o Git

* Muito popular graças ao Github;
* Criado pelo mesmo criador do Linux;
* Grande maioria dos projetos open source estão no GitHub;
* Fácil de aprender e a utilizar;
* Trabalha com versionamento descentralizado.

## Centralizado vs Descentralizado

### Centralizado

O controle de versão centralizado segue a topologia em estrela, havendo apenas um único repositório central mas várias cópias de trabalho, uma para cada desenvolvedor. A comunicação entre uma área de trabalho e outra passa obrigatoriamente pelo repositório central.

No controle de versão centralizado há um único repositório e várias cópias de trabalho que se comunicam apenas através do repositório central.

Vantagens:

* Cópia inicial do repositório é rápida;
* Arquivos sempre na versão mais recente;
* Gerenciamento do acesso.

Desvantagens:

* Disponibilidade do repositório;
* Mais dificil para trabalhar com branches;
* Menos utilização em projetos mais atuais.

### Descentralizado (Git)

São vários repositórios autônomos e independentes, um para cada desenvolvedor. Cada repositório possui uma área de trabalho acoplada e as operações commit e update acontecem localmente entre os dois.

No controle de versão distribuído cada desenvolvedor possui um repositório próprio acoplado a uma área de trabalho. A comunicação entre eles continua sendo através de commit e update.

Um repositório pode se comunicar com qualquer outro através das operações básicas pull e push

Vantagens:

* Não requer repositório central;
* Velocidade em manipular o repositório;
* Flexibilidade no fluxo de trabalho;

Desvantagens:

* Curva de aprendizagem um pouco maior;
* Necessita um push para o repositório central;
* Clone inicial pode demorar.

## Como o git versiona os arquivos

Ao inicializar um repositório com o git (veremos a seguir com mais detalhes), é criado um diretório chamado .git nesse local. É nesse diretório que o git armazena todo o histórico desse diretório e seus outros objetos.

O diferencial do git está na forma que ele gerencia esses objetos com as versões dos arquivos. Enquanto os sistemas de versionamento tradicionais (SVN e Subversion por exemplo) armazenam o delta entre cada versão dos arquivos, o git vai um pouco além e cria um snapshot de toda a estrutura do repositório. Ao comparar os arquivos, caso seu conteúdo não tenha alterações, ele cria somente uma referência ao estado anterior.

**Versionamento Tradicional:**

![Tradicional Version](imagens/versionamento-tradicional.png)

**Versionamento do Git:**

![Git Version](imagens/versionamento-do-git.png)

## Versionamento com Git

Sistemas tradicionais precisam de conexão com o repositório central, contendo assim uma dependência com a disponibilidade de conexão para trabalhar com o repositório.

Já com o git, como os repositórios são descentralizados, o repositório local consegue funcionar sem a dependência do repositório central, eliminando a necessidade de utilizar uma conexão de rede para a maioria das operações, resultando num ganho interessante de performance.

Tudo que é armazenado no git, por fim, recebe uma assinatura única que identifica o estado daquela alteração. Esse pacote de alterações é o que chamamos de git object, uma estrutura simples de chave e valor que recebe um hash único do tipo SHA-1.

```md
84b1da6351252587aa492b52f8696cd6d3b00373
```

Temos que ter em mente que tudo no git é associado a um hash único e não repetido

Ao trabalhas com o git nós só estamos acessando atalhos para esse hash.

## Configurando o Git

### Listando as configurações existentes

```md
git config --list
git config [--global] -l
```

### Adicionando dados de usuário

```md
git config --global user.name treinaweb-git
git config --global user.email treinaweb@outlook.com
```

### Verificar se existe chave SSH já criada

```md
ls -al ~/.ssh
```

### Criando chave SSH

```md
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

OBS: Ao criar a chave SSH podemos adicionar uma senha.

### Adicionando chave SSH

Habilitar o uso de chave SSH:

```md
eval "$(ssh-agent -s)"
```

Adicionar a chave SSH:

```md
ssh-add ~/.ssh/id_rsa
```

### Verificar chave SSH

Realiza o ctrl + c na chave SSH

```md
clip < ~/.ssh/id_rsa.pub
```

## Fluxo de trabalho com git

Até que as alterações realizadas nos arquivos cheguem ao repositório, elas precisam passar por algumas etapas.

* Modificamos os arquivos no nosso diretório local (working directory)
* Colocamos essas modificações em uma área de staging
* Movemos toda a área de staging na forma de commit no repositório

De forma geral, utilizamos o git para fazer essas modificações entre estados dos arquivos, como vemos no gráfico abaixo:

![Fluxo de trabalho com git](imagens/fluxo-do-git.png)

O working directory são os arquivos que visualizamos ao navegar em nosso diretório usando o navegador de arquivos do nosso sistema operacional. A medida que vamos trabalhando vamos adicionando essas modificações para a área de staging e por fim podemos enviar para o repositório do git. É possível fazer também o fluxo contrário e alterar os arquivos do disco com o conteúdo do repositório, chamando esse processo de checkout.

Podemos até considerer os casos de novos arquivos que ainda não foram monitorados pelo git. Detalhando o fluxo anterior, imagine as iterações que podem ocorrer entre o working directory e nossa área de staging.

Usamos o commando git status para analisar as diferenças entre o que está no staging com nosso repository e o working directory.

O fluxo abaixo demonstra os estados que os arquivos podem estar dentro de nosso projeto utilizando o **git status**:

* **Untracked**: Novo arquivo;
* **Unmodified**: Arquivo não modificado;
* **Modified**: Arquivo modificado;
* **Staged**: Arquivo adicionado ao staged com o commando **git add**.

![Fluxo dos arquivos no git](imagens/fluxo-arquivos-do-git.png)

## Mudanças de estados entre arquivos

Para entender melhor, considere um arquivo já commitado para o repositório do git com o nome legumes.txt.

Esse arquivo tem o seguinte conteudo:

![Legumes](imagens/legumes.png)

De acordo com os comandos do git que formos executando, esse conteúdo vai se alterar em cada uma das áreas:

Modificando o arquivo no sistema (working dir):

![Legumes Working Dir](imagens/legumes-working-dir.png)

Executando **git add**

![Legumes Staging](imagens/legumes-staging.png)

Executando **git commit** rotulando assim os arquivos do staged e deixando no repositório local.

![Legumes Repository](imagens/legumes-repository.png)

## Ações dos comandos no fluxo básico

![Fluxo Padrão](imagens/fluxo-padrao.png)

## Commandos

### Verificar versão do git instalada

```md
git --version
```

### Listar commit executados

```md
git log
```

### Criar repositório localmente

Inicia um repositório vazio em sua máquina, adicionando uma pasta chamada ".git"

```md
git init
```

Iniciar um repositório central (bare), usado para armazenar outros repositórios, algo como os repositórios no github.

Os repositórios do tipo bare são usados para centralizar o trabalho de diversos outros repositórios que enviam objetos, ele propriamente não possui Working Dir e não permite trabalhar diretamente nele.

Um repositório bare não contem um working directory com checkout de código, em outras palavras, pense nele como somente um diretório ".git" (o banco de dados do Git) sem nada alem disto.

Usado para quando precisamos criar um servidor Git.

```md
git init --bare
```

Para subir um repositório já iniciado na sua máquina para o github, podemos utilizar o comando abaixo para relacionar o repositório criado na nossa máquina, com o repositório presente no github

```md
git remote add origin url-do-repositorio
```

Após isto só é preciso enviar os dados deste repositório para o github:

```md
git push -u origin master
```

### Copiando um repositório para sua máquina

```md
git clone url-do-repositorio
```

### Adicionar arquivos editados como staged

```md
git add [<nome-dos-arquivos-editados>...]
// Para adicionar tudo
git add .
```

### Juntar arquivos no staged como um grupo de alterações

```md
git commit -m "Explicação das alterações executadas nos arquivos do staged"
```
