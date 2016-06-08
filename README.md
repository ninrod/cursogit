# GIT: comandos aprendidos

* onde eu apresento de forma escrita os comandos demonstrados no curso de git para fins de estudo pós-curso.
* ao mostrar os comandos, eu vou apresentar a forma padrão e logo embaixo eu vou escrever a forma utilizando os alias do shell unix customizado que virá por padrão na instalação do arestools.

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


## tags

* Uma `tag` é uma referência. Logo, é apenas um ponteiro para um commit.
* Uma tag, uma vez criada, nunca poderá ser alterada. 
* Em outras palavras, aponta sempre para o mesmo commit e nunca muda, desde sua criação.

## branches e tags

* Um `branch` nada mais é do que uma referência. Logo é apenas um ponteiro para um commit. Por isso que se diz que no git os branches são __'leves'__.
* Um `branch` pode apontar para diferentes commits, de acordo com o vontade do usuário.
* O git commit faz com que o commit atual, apontado pelo branch atual, ganhe um filho, e faz o branch atual apontar para esse filho.

## o HEAD

* Assim como `branches` e `tags`, o `HEAD` é apenas uma referência para um commit, mas com caraterísticas especiais.
* O HEAD é uma referência que pode apontar para outra referência: os `branches`
* O `HEAD` pode apontar para um commit diretamente, ou no caso normal, aponta para um `branch`
* Como o `git commit` altera para onde um branch aponta, se o HEAD estiver apontando para um branch e o branch mudar, o HEAD também mudará para refletir essa alteração.

## No no seu repo git, o arquivo pode estar em 3 locais diferentes

* O arquivo pode estar:
  * Na `WORKING COPY`: que é o seu sistema de arquivos do seu SO. 
    * Você pode ver como o arquivo nesse local por meio de, por ex: 

```sh
$ cat arquivo.txt
```

  * No `INDEX` (tbm chamado de `staging area`, `stage`, ou `cache`): que é o seu `commit proposto`. é um commit, mas o git não aplicou um `SHA` a ele ainda. 
    * Você pode ver como o arquivo se encontra nessa área por meio do comando: 

```sh
# onde :arquivo.txt é o full path para o arquivo no seu repo.
# poderia ser config/arquivo.txt ou outra coisa
$ git show :arquivo.txt, 
```


  * No `HEAD`: que é como o arquivo está no commit `checkoutizado` agora no seu repo. 
    * Você enxerga o arquivo tal como ele está no HEAD por meio do comando:

```sh
# mesma sintaxe do INDEX, acima
$ git show HEAD:arquivo.txt
```

# Comandos git

## Trabalhando com o INDEX

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

## verificando o status do seu repositório

* verificando suas modificações locais

```sh
$ git status

# apresenta uma forma mais condensada do git status
# git status -sb
$ g
```

* verificando em qual branch você está no momento, e quantos commits está para trás ou para frente em relação ao seu tracking branch

```sh
# normal: mostra apenas em qual branch você está
$ git branch

# alias: mostra todos os branches locais, os tracking branches, e para quais commits cada respectivo branch está apontando
$ gg
```
