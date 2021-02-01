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