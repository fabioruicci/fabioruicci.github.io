---
layout: post
title:  "{Resumo, cheat sheet, resumão, folha de dicas} Git - Em português"
date:   2020-05-13 00:00:00 -0300
categories: Git
permalink: /resumo-git/
---

E aí, beleza?

Esse é um resumo de Git baseado no curso [Tutorial Git](https://www.youtube.com/playlist?list=PLvS2JoIlSA4DCmp7pbXXuZEUb5E-IDb-K).

Dúvidas, sugestões e correções: é só deixar um comentário.

---

## Comandos gerais
### Checar se o Git está instalado
```console
$ git --version
git version 2.17.1
```
Se não estiver, visite esse site para instalar: [https://git-scm.com/downloads](https://git-scm.com/downloads).

### Dizer ao Git quem é você
Esses comandos fazem o seu nome e e-mail aparecerem nos commits feitos por você.
```console
$ git config --global user.name "SeuNome"
$ git config --global user.email "SeuEmail"
```

### Criar um repositório
Execute esse comando na pasta onde você deseja criar o seu repositório, ou passe como argumento o local onde o repositório deve ser criado.
```console
$ git init
# ou:
$ git init fabio/projetos/meu-projeto
```

### Checar o status do repositório
Exibe a branch atual, arquivos monitorados e não monitorados, e arquivos na área de staging e fora dela.
```console
$ git status
```
Exemplo de resposta:
```
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        seu_arquivo

nothing added to commit but untracked files present (use "git add" to track)
```

### Começar a monitorar arquivos e/ou adicionar arquivos a área de staging ("preparar para o commit", "colocar as compras no carrinho")
```console
$ git add arquivo1 arquivo2 arquivo3
```
É possível passar vários arquivos de uma vez para o comando git add. Por exemplo, para adicionar todos os arquivos e pastas do diretório onde o comando é executado, basta digitar:
```console
$ git add .
```
Para adicionar uma pasta:
```console
$ git add minha-pasta
```

### Salvar alterações ("tirar uma foto do repositório", "finalizar a compra")
```console
$ git commit -m "Mensagem explicando o que foi feito"
```

### Visualizar modificações feitas em arquivos
```console
$ git diff
# outra opção:
$ git diff seu_arquivo
```

### Visualizar histórico do projeto (logs)
```console
$ git log
```
É interessante saber algumas combinações do git log, por exemplo:
```console
# Exibir os últimos n commits
$ git log -n

# Exibir commits em uma linha
$ git log --oneline
```
Recomendação: [https://www.atlassian.com/git/tutorials/git-log](https://www.atlassian.com/git/tutorials/git-log)

### Visualizar informações específicas de um commit
```console
$ git show HashDoCommit
```

### Ignorar arquivos (por exemplo pastas de ambientes virtuais, arquivos com informações confidenciais, etc.)
Crie um arquivo chamado .gitignore na raíz do projeto (é o mesmo lugar onde você digitou git init) e dentro desse arquivo escreva o nome dos arquivos ou pastas que devem ser ignorados.

Recomendação: Utilize templates!

Se estiver usando o VSCode, baixe essa extensão: [https://marketplace.visualstudio.com/items?itemName=codezombiech.gitignore](https://marketplace.visualstudio.com/items?itemName=codezombiech.gitignore)

Caso contrário, utilize esse site: [https://www.gitignore.io/](https://www.gitignore.io/)

### Enviar um repositório git local para o GitHub
Dado que o repositório no GitHub ainda não existe, o primeiro passo é criá-lo.

Acesse sua conta do GitHub e escolha a opção "New Repository".

Nomeie seu repositório, adicione uma descrição e selecione se ele será público (qualquer pessoa do mundo pode visualizar seu conteúdo) ou privado (apenas quem você autorizar poderá visualizar o conteúdo).

Não marque a opção "Initialize this repository with a README".

Clique em Create repository.

Siga as instruções da próxima tela. Por exemplo:
```console
$ git remote add origin https://github.com/SEU_NOME_DE_USUARIO/NOME_DO_REPOSITORIO.git
$ git push -u origin master
```

### Enviar alterações locais para o repositório remoto (já existente e já registrado como origin)
```console
$ git push
```

### Trazer alterações do repositório remoto para o local
```console
$ git pull
```

### Clonar um repositório do GitHub
```console
$ git clone https://github.com/USUARIO/NomeRepositorio.git
```

### Listar remotes do repositório
```console
$ git remote -v
```

### Armazenar credenciais GitHub em memória (isso é seguro!)
```console
$ git config --global credential.helper cache
# ou
$ git config --global credential.helper 'cache --timeout=TEMPO_EM_SEGUNDOS'
```

## Branches
### Listar branches
```console
$ git branch
```

### Criar uma branch
```console
$ git branch nome-da-branch
```

### Trocar de branch
```console
$ git checkout nome-da-branch
```

### Criar uma branch e já trocar pra ela
```console
$ git checkout -b nome-da-branch
```

### Criar uma branch a partir de um commit (e já trocar pra ela)
```console
$ git checkout -b nome-da-branch HashDoCommit
```

### Renomear branch
```console
$ git branch -m novo-nome
```

### Deletar branch
-D: deleta sem dó nem piedade. Cuidado para não perder o seu trabalho.

-d: só deleta se o trabalho realizado na branch tiver sido enviado para um repositório remoto ou unido com outra branch através de um merge.
```console
$ git branch -D nome-da-branch
# ou
$ git branch -d nome-da-branch
```

### Listar todas as branches (locais e remotas)
```console
$ git branch -a
```

### Deletar branch remota
```console
$ git push -d RemoteName nome-da-branch
# exemplo:
$ git push -d origin minha-branch-123
```

## Merge
### Trazer alterações de outra branch para a branch atual
```console
$ git merge nome-da-branch
```
Essa operação pode gerar conflitos. Você deve resolver esses conflitos (sozinho ou com a ajuda dos colegas de projeto) para a operação ser concluída.

Preste muita atenção nos conflitos, eles precisam ser resolvidos!!!

Se você não pode resolver os conflitos, considere abortar a tentativa de merge:
```console
$ git merge --abort
```

- Visualização do merge:

![Visualização do Merge](/assets/git_merge.png)


## Trabalho em equipe
### GitHub Flow
Desejo criar uma nova feature, o que fazer?
1. Criar uma branch baseada na master
2. Trabalhar normalmente nessa branch (com adds e commits)
3. Abrir uma pull request (com a intenção de colocar na branch master as alterações feitas na branch de desenvolvimento)
4. Discutir e revisar as alterações com os outros membros da equipe
5. Fazer um deploy das alterações
6. Finalizar o merge

Recomendação: [https://guides.github.com/introduction/flow/](https://guides.github.com/introduction/flow/)

### Forking Flow/Open Source Flow
Fluxo usado quando desejamos trabalhar em repositórios que não temos permissão de escrita.
1. Fazer fork do repositório de interesse
2. Clonar o fork para o ambiente de desenvolvimento (git clone)
3. Configurar remote origin para que ele aponte para o nosso fork
4. Configurar remote upstream para que ele aponte para o repositório central/oficial
5. Seguir com o desenvolvimento como desejar (por exemplo, seguindo o GitHub Flow)

Desse jeito, mandamos nossas alterações para o nosso repositório remoto (o nosso fork, criado na nossa conta do GitHub) e baixamos as alterações do repositório central/oficial.

- Visualização Forking Flow:

![Visualização do Forking Flow](/assets/forking_flow.png)

### Adicionar um remote
```console
$ git remote add NomeRemote https://github.com/USUARIO_GITHUB/NOME_REPOSITORIO.git
# exemplo
# git remote add upstream https://github.com/USUARIO_GITHUB/NOME_REPOSITORIO.git
```

## Revertendo comandos
## git reset
### Reverter git add
Atenção!

Se você acabou de dar um git add para começar a monitorar um arquivo e quer reverter essa operação, esse é o comando:
```console
$ git rm --cached seu_arquivo
rm 'seu_arquivo'
```
Se você quiser reverter e também deletar o arquivo:
```console
$ git rm -f seu_arquivo
rm 'seu_arquivo'
```
Esses mesmos comandos também devem ser usados para parar de monitorar um arquivo do seu repositório (independente de quando ele foi adicionado)

### Remover arquivo da área de staging
Se você usou o git add para colocar as alterações feitas em um arquivo na área de staging, o comando para reverter o git add é esse aqui:
```console
$ git reset HEAD arquivo
```
Se além de reverter o git add você também quiser descartar as alterações feitas nos arquivos, utilize esse comando:
```console
$ git checkout arquivo
```

### Reverter um git commit
Soft: Reverte o commit, voltamos exatamente um passo. As alterações continuam feitas nos arquivos e continuam na área de staging.

Mixed: Reverte o commit, voltamos exatamente dois passos. As alterações continuam feitas nos arquivos mas não continuam na área de staging.

Hard (cuidado!): Reverte o commit, voltamos exatamente três passos. As alterações feitas nos arquivos são descartadas, não há nada na área de staging.

HEAD~1 significa que estamos voltando um commit, você pode mudar esse número se quiser reverter mais commits.

```console
$ git reset --soft HEAD~1
$ git reset --mixed HEAD~1
$ git reset --hard HEAD~1
```

- Visualização git reset:

![Visualização do git reset](/assets/git_reset.png)

## git revert
### Reverter um commit
No exemplo abaixo, revertemos exatamente o último commit realizado.

HEAD~1 reverteria o penúltimo.

E assim por diante.
```console
$ git revert HEAD
# outra opção, passar o commit:
$ git revert HashDoCommit
```

- Visualização git revert:

![Visualização do git revert](/assets/git_revert.png)

## Reverter comandos no repositório remoto
Podemos usar os dois comandos vistos acima, git reset e git revert. Após executar um desses dois comandos devemos executar:
```console
$ git push -f
```

## Outros comandos
### Combinar duas branches eliminando a bifurcação (rebase)
Estando na branch que receberá as alterações, digite:
```console
$ git rebase nome-da-branch
```
### Squash: Espremer commits
Nesse caso vamos pegar os 3 commits mais recentes e fazer eles virarem apenas 1
```console
$ git rebase -i HEAD~3
```
Escreva pick (ou p) na frente do commit final
e squash (ou s) na frente dos commits que serão espremidos

### Stash: Salvar o seu trabalho sem fazer commit
Lista opções
```console
$ git stash list
```
Adiciona ao stash
```console
$ git stash
```
Recupera suas alterações
```console
$ git stash apply
# ou
$ git stash apply stash@{0}
```

### Trazer para a minha branch apenas um commit de outra branch: cherry-pick
```console
$ git cherry-pick HashDoCommit
```

# Lembre-se
As vezes o que a gente precisa não é de um resumão, e sim mergulhar na documentação. Divirta-se: [https://git-scm.com/doc](https://git-scm.com/doc)

# Participe
Dúvidas, sugestões e correções? Deixe um comentário.
