# Curso de GIT do Filipe (deinf.filipe)

* onde eu apresento de forma escrita os comandos demonstrados no curso de git para fins de estudo pós-curso.
* ao mostrar os comandos, eu vou apresentar a forma padrão e logo embaixo eu vou escrever a forma utilizando os alias do shell unix customizado.

# Ideias gerais do git

## Todo repo git é um DAG (direct acyclic graph)

* Um commit é uma série de modificações introduzidas a um repositório. Você pode pensar que um commit é um `patch`.
* Um repositório git nada mais é do que um grafo direcionado acíclico em que os vértices são os commits.
* Um commit pode ter infinitos pais.
* Geralmente um commit tem um commit pai. O commit sempre aponta para o seu pai, se tiver pai.
* Um commit pai nunca aponta para um commit filho. (por isso que é um grafo direcionado).
* Um commit nunca é destruído.

## referências no git

* referências tornam commits alcançaveis.
* um commit é alcançável quando uma referência aponta para ele ou para um filho dele.
* se um commit não é alcançável, ele não aparecerá no histórico. Não irá para o remote no momento do `git push`.
* referências são apenas ponteiros para um commit. A única coisa que uma referência faz é apontar para um commit.
* uma referência é apenas um nome ligando um commit `SHA`.
* São referências no git: o `HEAD`, qualquer `branch`, e qualquer `tag`.


## A `tag`

* Uma `tag` é uma referência. Logo, é apenas um ponteiro para um commit.
* Uma tag, uma vez criada, nunca poderá ser alterada. 
* Em outras palavras, aponta sempre para o mesmo commit e nunca muda, desde sua criação.

## O `branch`

* Um `branch` nada mais é do que uma referência. Logo é apenas um ponteiro para um commit. Por isso que se diz que no git os branches são __'leves'__.
* Um `branch` pode apontar para diferentes commits, de acordo com o vontade do usuário.
* O git commit faz com que o commit atual, apontado pelo branch atual, ganhe um filho, e faz o branch atual apontar para esse filho.
* quando você inicia um repo git, o git cria um branch com o nome padrão de `master`. 
* O `master` é só isso, um nome padrão para o primeiro branch criado. Poderia ser outro nome. Você pode mudar o nome para o nome que você quiser.


## O `HEAD`

* Assim como `branches` e `tags`, o `HEAD` é apenas uma referência para um commit. Mas não qualquer commit.
  * Veja que em qualquer momento da linha do tempo, no seu repo git, você tem um commit `checkoutizado`.
  * esse commit `checkoutizado` é apontado sempre pela referência `HEAD`.
* Além disso, o HEAD é uma referência que pode apontar para outra referência: um `branch`.
* Então o `HEAD` pode apontar para um commit diretamente, ou no caso normal, aponta para um `branch`
* Quando o `HEAD` aponta para um branch, por exemplo, `master`, ele __acompanha__ o `master`.
  * para ficar claro: suponha que o `master` estava apontando para `A` e agora aponta para `B`. 
  * então o HEAD também estava apontando para `A` e agora está apontando para `B`, via `master`.

## As __três árvores__ do git

* No seu repo git, o arquivo pode estar em 3 locais diferentes:

* O arquivo pode estar na `WORKING COPY`: que é o seu sistema de arquivos.
* Você pode ver como o arquivo nesse local por meio de, por ex: 

```sh
$ cat arquivo.txt
```

* Pode estar no `INDEX` (tbm chamado de `staging area`, `stage`, ou `cache`): que é o seu `commit proposto`. 
* é efetivamente um commit, mas o git não aplicou um `SHA` a ele ainda. então não tem referência, nem pai, nem identificador.
* Você pode ver como o arquivo se encontra nessa área especial por meio do comando: 

```sh
# onde :arquivo.txt é o full path para o arquivo no seu repo.
# poderia ser config/arquivo.txt ou outra coisa
$ git show :arquivo.txt
```

* No `HEAD`: que é como o arquivo está no commit `checkoutizado` nesse momento no seu repo.
* Então quando se diz: quero ver como o arquivo está no `HEAD`, é a mesma coisa que dizer: __quero ver como o arquivo está no commit checkoutizado no momento no meu repo__
* Você enxerga o arquivo tal como ele está no `HEAD` por meio do comando:

```sh
# mesma sintaxe do INDEX, acima
# perceba que no lugar do HEAD você poderia colocar qualquer referência: um branch, uma tag, um commit SHA, etc...
$ git show HEAD:arquivo.txt
```

## Os remotes

* é remotes, no plural mesmo, porque você pode adicionar infinitos remotes no seu git repo local
* Quando você git clona um repo, o git, por padrão, adiciona um remote chamado `origin` que aponta para o repo git clonado.
* o `origin` abriga upstream branches, ou seja, os branches que estão na máquina remota que abriga o repo git que é `a verdade`. (tem upstream tags também)
* você tem na verdade uma cópia desses branches remotos no seu origin local. Cabe a você ir `syncando` ou atualizando essa cópia.
* fora essas características, para efeitos práticos, o `origin` abriga apenas referências para commits: `origin/HEAD`, `origin/master`, `origin/feature-1`, `origin/v2.1`, etc...

# Comandos git

## Adicionando modificações ao INDEX

* você pode pensar que o index é o seu __commit proposto__.
* você tem que adicionar os arquivos modificados localmente ao INDEX. antes de commitar.
* o `git commit` simplesmente pega as modificações adicionadas ao INDEX monta um commit e adiciona esse commit no histórico.
* você adiciona um arquivo ao INDEX com o comando:

```sh
$ git add arquivo.txt
$ ga arquivo.txt

# ou para adicionar toda e qualquer mudança local no index:
$ git add --all :/
$ gal
```

## Adicionando um commit ao seu projeto

```sh
# forma padrão
$ git commit -m 'sua mensagem de commit'

# atalho: abre o vim para que vc digite uma msg de commit 
# e ainda apresenta o diff entre o seu commit proposto e o commit anterior
$ gc 
```

## Verificando o status do seu repositório

* verificando suas modificações locais

```sh
$ git status

# apresenta uma forma mais condensada do git status
# git status -sb
$ g
```

* verificando em qual branch você está no momento, e quantos commits está para trás ou para frente em relação ao seu tracking branch (ou `upstream` branch)

```sh
# normal: mostra apenas em qual branch você está
$ git branch

# alias: mostra todos os branches locais, os tracking branches, e para quais commits cada respectivo branch está apontando
$ gg
```

## Inspecionando o histórico do repositório

```sh
# normal
$ git log

# atalho: formato muito mais condensado e completo, com cores que diferenciam diferentes categorias de informação, mostra as referências, etc...
$ gl
```

* perceba a diferença entre o `git log` normal e o atalho `gl`

![screenshot-vi-tmux](http://git/DEINF.FILIPE/cursogit/raw/misc/img/diff-gl-gitlog.png)

* Bonus: você pode inspecionar o repositório considerando o `reflog`, em que o git considera as referências não alcançáveis como alcançáveis:

```sh
# mesma coisa que gl --reflog
$ glr
```

## git diff: o que exatamente mudou? 

* você pode verificar o que mudou da sua working copy para o seu INDEX com o comando git diff

```sh
# ou simplesmente $ gd
$ git diff 
```

* para que você verifique o que mudou do INDEX para o HEAD, escreva:

```sh
$ dit diff --cached
```

* veja como fica o diff no terminal:

![screenshot-vi-tmux](http://git/DEINF.FILIPE/cursogit/raw/misc/img/git-diff.png)
