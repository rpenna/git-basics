# Como funciona
No git, o código pode ocupar 3 locais:
* __Workspace__: onde seu código de fato está.
* __Local Repository__: ao fazer commit, é o local na sua máquina para onde o git envia seu código.
* __Remote Repository__: servidor para onde o código é enviado quando fazemos um push. Ideal quando trabalhamos em equipe. Podemos verificar qual o servidor remoto do nosso projeto através do comando:
  ```
  git remote -v
  ```
# Init
## Em projetos novos
Ao iniciar um novo projeto que inexiste em um servidor remoto, basta rodar na pasta raiz do projeto:
```
git init
```
Após criar o repositório remoto em um cliente como github, gitlab ou bitbucket, será gerada a URL com extensão .git. Com posse desta URL, definimos o repositório remoto através do comando
```
git remote add origin <url.git>
```

## Em projetos já existentes
Caso projeto já exista em algum repositório remoto, basta ter a URL para clonar no local desejado em sua máquina:
```
git clone <url.git>
```

# Config
Existem 3 níveis de configuração do git em sua máquina:
* __Configuração do sistema__: configurações que terão efeito em todos os projetos com git na máquina. Pode ser alterado a partir do comando
  ```
  git config --system --edit
  ```

* __Configuração do usuário__: terão efeito apenas nos projetos atribuídos ao usuário dono da configuração. Pata alterar, temos que inserir o comando
  ```
  git config --global --edit
  ```
  Ao usar esse comando pela primeira vez, o git usará o editor de texto definido como padrão do sistema para abrir as configurações, possivelmente o VIM. Podemos alterar isso da seguinte maneira:
  ```
  git config --global core.editor <nome do editor>
  ```
  É uma boa prática definir os dados do usuário ao configurar este arquivo, de modo que o git possa identificar o responsável por cada commit. Basta inserir na configuração:
  ```
  [user]
    name = <nome do usuário>
    email = <email do usuário>
  ```

* __Configuração do projeto__: terá efeito apenas no projeto em questão. Pode ser alterado a partir da flag local, ou sem passar flag alguma.
    ```
    git config --local --edit
    ```
    Ou
    ```
    git config --edit
    ```
  Quando adicionamos o repositório remoto do projeto, é neste arquivo de configuração que a URL fica salva.
    

Podemos consultar todas as configurações do git em todos os níveis através do comando:
```
git config --list
```
É necessário destacar que, em caso de conflito de configuração entre os diferentes níveis, a de projeto sempre terá prioridade, enquanto a de usuário se sobrepõe apenas à de sistema.

# Status de arquivos
Os arquivos presentes na pasta raiz do respositório ou em uma de suas pastas podem assumir 4 status:
* __Untracked__: o git não reconhece o arquivo.
* __Modified__: o arquivo é reconhecido pelo git, mas sofreu modificações desde o último commit e não está na __staged area__, que identifica os arquivos que farão parte do próximo commit.
* __Tracked__: arquivo reconhecido pelo git e presente na staged area.
* __Commited__: arquivos que estiveram no último commit e não sofreram nenhum tipo de alteração.
  
Podemos verificar o status de cada arquivo do repositório __que não seja commited__ através do comando 
```
git status
```
Ou, para menos detalhes:
```
git status -s
```
# Fazendo commit
Para que todos os arquivos untracked e modified do respositório sejam adicionados à staged area, executamos
```
git add .
```
Este comando somente será executado corretamente caso ocorra na pasta raiz do projeto. Para obter o mesmo efeito independentemente da pasta onde estamos, executamos
```
git add --all
```
Ou
```
git add -A
```
Para fazer o commit dos arquivos que estão na staged area, rodamos:
```
git commit -m "<mensagem_do_commit>"
```
# Amend
Caso um arquivo seja esquecido do último commit, ele ainda pode ser acrescentado. Basta que ele seja adicionado à staged area e, em seguida, seja feito um amend:
```
git commit --amend --no-edit
```
# Stash
Se, em algum momento do projeto, precisamos retomar o cenário do último commit sem perder as alterações feitas desde então, podemos fazer um __stash__. Trata-se de uma lista de arquivos modificados/criados desde o último commit. Para criá-la, basta executar
```
git stash
```
Neste momento, note que os arquivos ou suas alterações sumiram de seu projeto. Podemos listar todos os _stashes_ com o comando
```
git stash list
```
Para retomar um stash, temos duas opções:
1. Recolocar os arquivos no projeto enquanto os remove da lista de stash:
   ```
   git stash pop
   ```
2. Recolocar os arquivos no projeto mantendo-os na lista de stash:
    ```
    git stash apply
    ```
Caso tenha mais de um stash na lista, devemos especificar a qual nos referimos.

# Convenções de commit
Existe uma [convenção](https://www.conventionalcommits.org/en/v1.0.0/#specification) bastante popular para padronização de commits, criado pela comunidade do angular. Ela prevê que as mensagens do commit assumam o seguinte formato:
```
<Tipo>[escopo opcional]: <descrição>
[corpo da mensagem opcional]
[footer opcional]
```
O tipo pode assumir os seguintes valores:
* __build__: mudanças que afetam a configuração do sistema ou dependências externas.
* __ci__: alterações em arquivos e scripts de Integração Contínua (CI).
* __docs__: alterações na documentação.
* __feat__: uma nova funcionalidade foi criada.
* __fix__: um bug foi corrigido.
* __perf__: uma alteração no código que melhora a performance do software.
* __refactor__: uma alteração no código que não corrige um bug nem cria novas funcionalidades.
* __style__: mudanças que não alteram a semântica do código (espaços em branco, formatação, etc)>
* __test__: adição de novos testes ou correção de testes existentes.

# Ignorando arquivos
Podemos configurar o git para ignorar determinados arquivos de nosso projeto, a fim de que eles não sejam comitados. Para isso, colocamos na pasta raiz do projeto o arquivo _.gitignore_, que contém os nomes e padrões regex de arquivos a serem ignorados.

# Alias
Podemos criar atalhos para o git, fazendo com que os comandos sejam resumidos e nossa produtividade seja maior. Para isso, devemos abrir o editor de configurações do git. Para que o alias esteja vinculado ao nosso usuário, acessamos suas configurações de usuário:
```
git config --global --edit
```
Criamos o bloco de alias, o atalho desejado, sinal de '_=_', sinal de exclamação '_!_' e o comando ao qual ele se refere. Por exemplo, caso se deseje criar um atalho para ```git status -s``` como ```git s```, fazemos:
```
[alias]
  s = !git status -s
```
Como o comando que foi resumido pertence ao próprio git, poderíamos também ignorar tanto o ponto de exclamação quanto o comando git, como a seguir.
```
[alias]
  s = status -s
```

# Log
Podemos listar todos os commits desde o último push no projeto com o comando
```
git log
```
POrém, ele contém muitas informações. O comando abaixo traz de forma resumida os caracteres iniciais do hash e a mensagem de cada commit, tudo em uma única linha:
```
git log --oneline
```
Podemos ainda configurar a forma de apresentação do log através da flag ```--pretty``` com o parâmetro format acompanhado das customizações desejadas.
```
git log --pretty=format:'<customizações>'
```
Entre as opções de customização, temos:
* %H: a hash completa
* %h: a hash reduzida
* %cn: commiter name, o nome do autor do commit
* %d: a branch onde se deu o commit
* %s: a mensagem do commit
* %cr: data do commit
  
Também é possível inserir cores no log. Para isso, inserimos ```%C(<cor>)``` onde desejamos inserir a cor. Diante disso, podemos personalizar nosso log como no comando abaixo: 
```
git log --pretty=format:'%C(yellow)%h %C(red)%d %C(white)%s - %C(cyan)%cn, %C(green)%cr'
```

# Tag
Podemos marcar nossos commits com tags, a fim de navegar mais facilmente em diferentes pontos do nosso sistema. As tags podem ser de dois tipos:

* __Lightweight__: mais simples, sem uma mensagem dentro. Fica associada ao último commit. Não recomendável.
  Para criar uma tag lightweight, rodamos:
  ```
  git tag <nome_da_tag>
  ```
* __Annotated__: é criada junto a uma mensagem. Recomenda-se principalmente quando vamos subir para produção. Para criar, usamos:
  ```
  git tag <nome_da_tag> -m "<mensagem_da_tag>"
  ```

Para consultar dados de uma tag, basta executar:
```
git show <nome_da_tag>
```
Podemos também associar tags a commits antigos através do uso da flag ```-a```:
```
git tag -a "<nome_da_tag>" -m "<mensagem_da_tag>" <hash_do_commit>
```
Por padrão, quando fazemos um push o git não envia as tags. Para isso, devemos informá-lo que queremos subir as tags também, rodando:
```
git push origin main --tags
```
Esse comando, porém, envia todas as tags. Mas o recomendado é que se suba apenas as tags anotadas. Para isso, rode:
```
git push origin main --follow-tags
```
Para que o git faça push automaticamente das tags anotadas, podemos alterar as configurações do usuário, inserindo a seguinte instrução:
```
[push]
  followTags = true
```
Por fim, podemos remover uma tag. Localmente, utilizamos a flag ```-d```:
```
git tag -d "<nome_da_tag>"
```
Para remover também do repositório remoto, executamos:
```
git push --delete origin "<nome_da_tag>"
```

# Removendo arquivos da staged area
Podemos remover um arquivo específico da staged area:
```
git reset <nome_do_arquivo>
```
Ou todos os arquivos da staged area:
```
git reset
```

# Como desfazer commits
Existem três formas de desfazer commits:
* __Soft__: os arquivos voltam para o estados que estavam imediatamente antes do último commit desfeito, ou seja, com os arquivos na staged area.
  ```
  git reset <hash_do_commit_onde_deseja_voltar> --soft
  ```
* __Mixed__: os arquivos voltam para os estados que estavam imediatamente antes de serem adicionados à staged area do commit mais antigo desfeito.
  ```
  git reset <hash_do_commit_onde_deseja_voltar> --mixed
  ```
* __Hard__: todas as alterações de todos os commits desfeitos são apagadas. Em outras palavras, são mantidos exatamente os mesmos status que se encontravam logo após a realização do último commit remanescente, o mais recente entre os que não foram desfeitos.
  ```
  git reset <hash_do_commit_onde_deseja_voltar> --hard
  ```

Em todos os casos, ao invés de fazer referência direta ao hash do commit para o qual desejamos voltar, podemos dizer ao git quantos commits antes do último queremos voltar. O commit mais recente é conhecido como HEAD, então passamos ele com a quantidade de commits a serem desfeitos, separados por um til: ```HEAD~<quantidade>```. Por exemplo, caso o objetivo seja desfazer apenas o commit mais recente, rodamos:
```
git reset HEAD~1 --<forma_desejada>
```

# Desfazendo alterações do commit atual
Podemos desfazer todas as alterações do commit onde estamos trabalhando atualmente. Ao rodar o comando reset sem especificar uma hash, ele considera por padrão o HEAD sem a quantidade, ou seja, o commit mais recente. O que queremos fazer com este commit vai depender da forma de reset que passamos na flag. 

Se passarmos mixed, que é o padrão caso nenhuma flag seja especificada, os arquivos ficam na staged area, ou seja, nada acontece. Ao passar a flag hard, as alterações em arquivos após o commit serão todas apagadas, mas arquivos novos em status __untracked__ não serão apagados, pois o git ainda não os conhece.

# Como desfazer um commit sem perder os que ocorreram após
O git permite desfazer um commit antigo apenas revertendo o que ele fazia, isto é, excluíndo as linhas que ele adicionou e acrescentando as linhas que ele removeu. Para isso, usamos o comando revert, que nada mais é que um novo commit que faz essas exclusões e adições:
```
git revert <hash_do_commit>
```
Normalmente, principalmente em commits mais antigos, o comando gera um conflito, pois é bastante provável que as linhas impactadas tenham sido alteradas em commits posteriores. Para solucionar isso, é preciso resolver os conflitos e fazer um novo commit. 

Caso não haja conflitos, o próprio revert realiza este novo commit, mostrando uma mensagem no editor padrão do git informando que a operaçao foi realizada com sucesso. Pra evitar esta mensagem, podemos passar a flag --no-edit:
```
git revert <hash_do_commit> --no-edit
```
Também podemos fazer um revert forçando que ele __não__ faça um commit caso a reversão seja um sucesso usando a flag --no-commit:
```
git revert <hash_do_commit> --no-commit
```

# Desfazendo alterações em arquivos modified
Arquivos que nao estão na staged area mas foram modificados em relação ao último commit podem ter suas alterações desfeitas utilizando o ```checkout```:
```
git checkout <nome_do_arquivo>
```
Ou podemos fazer isso para todos os arquivos modificados ao invés d eum específico:
```
git checkout .
```
Porém, isso apenas pode ser feito com arquivos já conhecidos pelo git, ou seja, arquivos __untracked__ não são afetados, até pq não estavam no último commit.

# Desfazendo alterações em arquivos untracked (exclusão)
Também existe um comando com comportamento parecido para arquivos untracked, porém ele acaba excluíndo o arquivo:
```
git clean -f
```
Isso remote todos os arquivos untracked do projeto. Para rodá-lo sem necessitar forçar a exclusão (flag ```-f```), devemos mudar o parâmetro ```clean.requireForce``` para ```false``` nas configurações do git.

Porém, este comando não exclui arquivos untracked dentro de pastas untracked. Para isso, precisamos também da flag ```-d```:
```
git clean -fd
```

# Removendo arquivos que já haviam sido commitados anteriormente
Basta usar o comando ```rm```:
```
git rm <nome_do_arquivo>
```
Ao rodar este comando, o arquivo é excluído, mas ainda é possível retomá-lo através de um ```reset```. Ao executar ```git status```, ele aparecerá na staged area como __deleted__, aguardando um commit para sumir de vez do projeto.

Este comando também não exclui arquivos que estavam dentro de uma pasta. Para isso, temos que explicitamente informar ao git que queremos fazer a remoção recursivamente:
 ```
 git rm <nome_do_arquivo> -r
 ```

# Retornando a um commit anterior sem desfazer o que foi feito em seguida
Podemos retomar uma versão antiga do nosso projeto:
```
git checkout <hash_do_commit ou tag>
```
Isso vai fazer com que o código fique igual ao commit em questão, mas em uma nova branch temporária criada pelo git, chamada de desconectada. Caso o usuário saia desas branch, ela é apagada imediatamente. 

Para podermos alterar essa branch desconectada, precisamos criar uma nova branch a partir dela, também usando ```checkout```:
```
git checkout -b <nome_da_nova_branch>
```
Dessa vez uma nova branch de fato será criada, a partir da branch temporária criada pelo checkout de um commit anterior.

# Como solicitar ao git para ignorar arquivos que já foram commitados anteriormente
Caso deseje que um arquivo já commitado em outras ocasiões deise de ser inspecionado pelo git, use:
```
git rm <nome_do_arquivo> -cached
```
Assim, mesmo sejam realizadas modificações no arquivo, o ```git status``` vai ignorá-las. 

Este comando é particularmente útil quando adicionamos o gitignore tardiamente em nosso projeto, pois os arquivos que, segundo ele, deveriam ser ignorados seguirão sendo levados em conta pelo git, pois estavam no último commit.

# Verificando as branchs do projeto
Podemos consultar as branchs existentes:
```
git branch
```
A branch que estiver com um asterisco é a ativa no editor no momento.

# Mudando de branch
A mudança da branch também se dá pelo checkout:
```
git checkout <nome_da_branch>
```
