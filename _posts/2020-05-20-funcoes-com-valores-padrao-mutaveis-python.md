---
layout: post
title:  "Funções com valores padrão mutáveis: Qual é o problema? - Dicas de Python"
date:   2020-05-20 00:00:00 -0300
categories: Python
permalink: /funcoes-com-valores-padrao-mutaveis-python/
---

E aí, beleza?

Esse conteúdo pode ser visto em vídeo no [YouTube](https://youtu.be/o6Rn7yqjEDs).

Dúvidas, sugestões e correções: é só deixar um comentário.

---

# Funções com valores mutáveis: Qual é o problema? - Dicas de Python
E aí, beleza?

Dá uma olhada na célula abaixo. O que você acha que esse bloco vai exibir como output?

```python
def acrescenta_1(lista=[]):
    lista.append(1)
    return lista

print(acrescenta_1(), acrescenta_1())
```
O resultado é:
```
[1, 1] [1, 1]
```

Era o que você esperava? Quer entender o motivo do resultado ser esse? Então é só seguir com a leitura.

Só preciso aproveitar agora que eu tenho a sua atenção e dizer que essa aula só existe por causa desse tweet: https://twitter.com/jakevdp/status/1235271748867612673. Vale a pena entrar nesse link e acompanhar a discussão.

Então vamos lá. A primeira coisa que vamos entender é por que as respostas erradas estão erradas. Como você deve ter reparado no tweet que inspira a aula de hoje, 52% das pessoas acharam que o output desse código seria \[1\] \[1\]. Uma possível linha de raciocínio que nos permite chegar nessa resposta é a seguinte:

1. Chamo a função pela primeira vez e, com isso, uma lista vazia é criada;
2. Adiciono um novo item a essa lista, no caso esse item é o número 1;
3. Nesse exato momento minha lista está assim: \[1\], então a função print vai exibir exatamente isso;
4. Chamo a função pela segunda vez e, com isso, uma nova lista vazia é criada;
5. Adiciono um novo item a essa lista, no caso esse item é o número 1;
6. Nesse exato momento minha lista está assim: \[1\], então a função print vai exibir exatamente isso;

Não se sinta mal por ter pensado desse jeito. O raciocínio faz sentido, mas chega na resposta errada, porque não é assim que o Python funciona!

30% das pessoas acharam que a resposta seria \[1\] \[1, 1\]. Uma possível linha de raciocínio que nos permite chegar nessa resposta é a seguinte:

1. Chamo a função pela primeira vez e, com isso, uma lista vazia é criada;
2. Adiciono um novo item a essa lista, no caso esse item é o número 1;
3. Nesse exato momento minha lista está assim: \[1\], então a função print vai exibir exatamente isso;
4. Chamo a função pela segunda vez e, dessa vez, ela vai usar a mesma lista criada na primeira chamada;
5. Adiciono um novo item a essa lista, no caso esse item é o número 1;
6. Nesse exato momento minha lista está assim: \[1, 1\], então a função print vai exibir exatamente isso;

Quase lá! Esse raciocínio chega muito perto da resposta correta, e mostra que entendemos como funcionam os objetos mutáveis no Python. Mas faltou um detalhe pra acertar...

Vamos discutir agora alguns fundamentos de Python necessários para chegar na resposta correta (e entendê-la). Não é bruxaria, não é o Python de sacanagem com a gente. É simplesmente uma questão de entender como o Python funciona. Vamos lá:

# Objetos mutáveis e suas armadilhas
Isso aqui é um clássico do Python e já pegou muita gente desprevenida.

Observe esse bloco de código e pense: Qual vai ser o valor das variáveis __números_do_fabio__ e __números_da_josi__?

```python
numeros_do_fabio = [1, 2, 3]
numeros_da_josi = numeros_do_fabio

numeros_da_josi.append(4)
print(f"Números escolhidos pela Josi: {numeros_da_josi}")
print(f"Números escolhidos pelo Fabio: {numeros_do_fabio}")
```

Output:
```
Números escolhidos pela Josi: [1, 2, 3, 4]
Números escolhidos pelo Fabio: [1, 2, 3, 4]
```

O resultado te surpreendeu novamente, ou por essa você já esperava?

Pra entender o que aconteceu nesse bloco de código precisamos entender como __variáveis__ funcionam no Python. Calma, já já vamos voltar a falar sobre a função que iniciou a aula de hoje, agora precisamos garantir que entendemos o seguinte: no Python, variáveis não são caixas. Variáveis, no Python, são nomes, rótulos, etiquetas, apelidos.

Muitos de nós aprendemos em algum momento da nossa vida a famosa analogia de que variáveis são caixas que armazenam objetos. Essa analogia é ótima e funciona muito bem para várias linguagens de programação. No Python, se variáveis fossem caixas (ou gavetas), que armazenam um objeto, o bloco de código acima retornaria algo diferente. A operação __números_da_josi = números_do_fabio__ poderia ser entendida como a criação de uma nova caixa, e essa caixa armazenaria o valor atual da variável __números_do_fabio__, então teríamos duas caixas diferentes, e, portanto, alterações no conteúdo de uma não deveria afetar o conteúdo de outra.

Mas não é assim que as variáveis funcionam no Python. É melhor pensar em variáveis como nomes que damos para objetos. Ou apelidos. Ou rótulos. Ou etiquetas. Acho que uma imagem exemplifica isso muito bem:

Variáveis no Python __não são__ caixas:

![Caixa A](http://www.omahapython.org/IdiomaticPython_files/a2box.png)
![Caixa B](http://www.omahapython.org/IdiomaticPython_files/b2box.png)

Variáveis no Python __são__ nomes, rótulos, etiquetas, apelidos:
![Rótulo AB](http://www.omahapython.org/IdiomaticPython_files/ab2tag.png)

Fonte dessas imagens: [http://www.omahapython.org/IdiomaticPython.html](http://www.omahapython.org/IdiomaticPython.html). Artigo excelente. Recomendo estudá-lo quando possível.

Observações:

1. Claro que variáveis são apenas variáveis. Quando digo que elas são nomes, ou caixas, ou qualquer outra coisa, é apenas uma analogia.
2. Esse caso é especial porque estamos lidando com listas, que são objetos __mutáveis__! O exemplo abaixo não deve causar nenhuma estranheza ou confusão, e também não tem nenhuma pegadinha:

```python
objeto_nao_mutavel_do_fabio = 42
objeto_nao_mutavel_da_josi = objeto_nao_mutavel_do_fabio

objeto_nao_mutavel_da_josi = 43
print(f"Objeto da Josi: {objeto_nao_mutavel_da_josi}")
print(f"Objeto do Fabio: {objeto_nao_mutavel_do_fabio}")
```
Output:
```
Objeto da Josi: 43
Objeto do Fabio: 42
```


3. Esse bloco de código também não deve causar nenhuma estranheza. A lista __números_do_fabio__ não é modificada. Isso acontece porque, apesar das duas listas serem visualmente idênticas, elas são objetos diferentes, iniciados em momentos diferentes, ocupando lugares (endereços) diferentes na memória do computador.

```python
numeros_do_fabio = [1, 2, 3]
numeros_da_josi = [1, 2, 3]

numeros_da_josi.append(4)
print(f"Números escolhidos pela Josi: {numeros_da_josi}")
print(f"Números escolhidos pelo Fabio: {numeros_do_fabio}")
```
Output:
```
Números escolhidos pela Josi: [1, 2, 3, 4]
Números escolhidos pelo Fabio: [1, 2, 3]
```

4. Nosso objetivo inicial com o código __números_da_josi = números_do_fabio__ era ter dois objetos diferentes, apontando para duas listas diferentes (sim, listas com valores iguais, mas objetos diferentes. Se você ainda estiver com dúvidas com relação a isso, me pergunte!), podemos fazer isso com uma pequena modificação no código, basta utilizar o método copy:

```python
numeros_do_fabio = [1, 2, 3]
numeros_da_josi = numeros_do_fabio.copy()

numeros_da_josi.append(4)
print(f"Números escolhidos pela Josi: {numeros_da_josi}")
print(f"Números escolhidos pelo Fabio: {numeros_do_fabio}")
```
Output:
```
Números escolhidos pela Josi: [1, 2, 3, 4]
Números escolhidos pelo Fabio: [1, 2, 3]
```

Ok. Até aqui nós entendemos como variáveis funcionam no Python ("nomes" em vez de "caixas"), e entendemos que saber esse fundamento nos permite não sermos surpreendidos pelo bloco que iniciou essa seção (onde equivocadamente achamos que estamos modificando apenas uma lista e acabamos modificando as duas, porque na verdade não tem duas listas, é só uma mesmo!)

Isso nos permite voltar para a função do início da aula:

```python
def acrescenta_1(lista=[]):
    lista.append(1)
    return lista
```

Quando chamamos uma função sem parêntesis, o Python nos retorna uma referência a essa função:
```python
>>> acrescenta_1
<function __main__.acrescenta_1(lista=[])>
```

Repare que a lista vazia é criada __exatamente uma vez, no momento em que a função é definida__.

Todas as vezes que executarmos essa função, a mesma lista será utilizada:

```python
>>> acrescenta_1()
[1]
```

```python
>>> acrescenta_1
<function __main__.acrescenta_1(lista=[1])>
```

```python
>>> acrescenta_1()
[1, 1]
```

```python
>>> acrescenta_1
<function __main__.acrescenta_1(lista=[1, 1])>
```

# Como as funções funcionam
Agora temos o último fundamento que nos permite entender o comportamento do código que iniciou a aula:

Quando escrevemos __print(acrescenta_1(), acrescenta_1())__, estamos chamando a __função__ print, certo?

O pulo do gato aqui é o seguinte: Antes de executar a função print, o Python primeiro executa as duas chamadas dentro do parêntesis! Isso não é uma particularidade da função print, e sim uma particularidade de funções no Python.

Na prática é isso aqui que está acontecendo:

```
lista_1 = acrescenta_1()
lista_2 = acrescenta_1()
print(lista_1, lista_2)
```

Como as duas variáveis __lista_1__ e __lista_2__ apontam para o mesmo objeto, é natural que o resultado do print seja esse, afinal, estamos printando duas vezes o mesmo objeto!

É exatamente isso: Estamos exibindo na tela duas vezes o mesmo objeto, acontece que esse objeto possui dois nomes/apelidos/rótulos.

Lembre-se disso: ![Rótulo AB](http://www.omahapython.org/IdiomaticPython_files/ab2tag.png)

# Curiosidade: Pylint nos alerta sobre isso!

Faça esse teste! Crie um arquivo python com o código dessa função. Você receberá o alerta __Dangerous default value [] as argument pylint(dangerous-default-value)__

# Eu preciso usar uma lista vazia na minha função, qual é o procedimento correto?

Basta fazer essas pequenas modificação na função:

```python
def acrescenta_1_sem_padrao_mutavel(lista=None):
    if lista is None:
        lista = []
    lista.append(1)
    return lista
```

Agora sim, com o código escrito dessa forma, uma lista nova será criada todas as vezes que chamarmos a função (e não passarmos uma lista como argumento do parâmetro lista)!

Então, com essa modificação, você consegue adivinhar o output do código abaixo?

```python
>>> print(acrescenta_1_sem_padrao_mutavel(), acrescenta_1_sem_padrao_mutavel())
[1] [1]
```

# Existe algum caso em que usar um valor mutável padrão como parâmetro seja útil?
Sim! Já ouviu falar de memoization? Observe esse exemplo:

```python
def enesimo_elemento_fibonacci(n, fibonacci_cached={0: 0, 1: 1}):
    if n not in fibonacci_cached:
        fibonacci_cached[n] = enesimo_elemento_fibonacci(n - 1) + enesimo_elemento_fibonacci(n - 2)
    return fibonacci_cached[n]
```

```python
>>> enesimo_elemento_fibonacci(10)
55
```

```python
>>> enesimo_elemento_fibonacci
<function __main__.enesimo_elemento_fibonacci(n, fibonacci_cached={0: 0, 1: 1, 2: 1, 3: 2, 4: 3, 5: 5, 6: 8, 7: 13, 8: 21, 9: 34, 10: 55})>
```

Observações:
- Embora escrever uma função que calcule o enésimo elemento da sequência de Fibonacci de forma recursiva seja muito elegante, obtemos uma performance melhor quando escrevemos de forma iterativa;
- O Python tem um decorator (@lru_cache) que pode ser usado para memoization;
- Esse exemplo foi feito com dicionário em vez de lista, mas isso não é um problema: Listas e dicionários são objetos mutáveis no Python;
- Os códigos dessa seção e da anterior são inspirados nesse artigo da documentação do Python: https://docs.python.org/3/faq/programming.html#why-are-default-values-shared-between-objects.

# Lição de casa
- Crie uma função parecida com a função inicial, avalie o seu comportamento conforme ela vai sendo chamada. Faça ela com listas, sets (conjuntos) e dicionários.

- Você conhece a função id()? Crie objetos mutáveis e "brinque" com essa função.
