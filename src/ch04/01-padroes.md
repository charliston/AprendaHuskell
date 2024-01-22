# Correspondência de padrões

Este capítulo abordará algumas construções sintáticas interessantes de Haskell, começando pelo correspondência de padrões. O correspondência de padrões consiste em especificar padrões aos quais alguns dados devem se conformar e, em seguida, verificar se isso acontece e desconstruir os dados de acordo com esses padrões.

Ao definir funções, é possível criar corpos de função separados para padrões diferentes. Isso leva a um código muito elegante, simples e legível. O correspondência de padrões pode ser feito em qualquer tipo de dado - números, caracteres, listas, tuplas, etc. Vamos criar uma função muito trivial que verifica se o número fornecido é sete ou não.

```haskell
lucky :: (Integral a) => a -> String  
lucky 7 = "NÚMERO DA SORTE SETE!"  
lucky x = "Desculpe, você não tem sorte, amigo!"
```

Quando você chama `lucky`, os padrões serão verificados de cima para baixo e, quando eles se conformam a um padrão, o corpo da função correspondente será usado. A única maneira de um número se conformar ao primeiro padrão aqui é se ele for 7. Se não for, ele passa para o segundo padrão, que corresponde a qualquer coisa e a vincula a `x`. Esta função também poderia ter sido implementada usando uma instrução `if`. Mas e se quiséssemos uma função que dissesse os números de 1 a 5 e dissesse "Não está entre 1 e 5" para qualquer outro número? Sem correspondência de padrões, teríamos que criar uma árvore `if then else` bastante complicada. No entanto, com ele:

```haskell
sayMe :: (Integral a) => a -> String  
sayMe 1 = "Um!"  
sayMe 2 = "Dois!"  
sayMe 3 = "Três!"  
sayMe 4 = "Quatro!"  
sayMe 5 = "Cinco!"  
sayMe x = "Não está entre 1 e 5"
```

Observe que se movêssemos o último padrão (o de captura geral) para o topo, ela sempre diria `"Não está entre 1 e 5"`, porque ela pegaria todos os números e eles não teriam a chance de passar e ser verificados para quaisquer outros padrões.

Lembra da função fatorial que implementamos anteriormente? Definimos o fatorial de um número `n` como `product [1..n]`. Também podemos definir uma função fatorial de forma _recursiva_, da maneira que geralmente é definida na matemática. Começamos dizendo que o fatorial de 0 é 1. Então afirmamos que o fatorial de qualquer número inteiro positivo é esse número multiplicado pelo fatorial do seu antecessor. Veja como isso se traduz em termos de Haskell.

```haskell
fatorial :: (Integral a) => a -> a  
fatorial 0 = 1  
fatorial n = n * fatorial (n - 1)
```

Esta é a primeira vez que definimos uma função de forma recursiva. A recursão é importante em Haskell e vamos dar uma olhada mais de perto nisso mais tarde. Mas, em poucas palavras, isso é o que acontece se tentarmos obter o fatorial de, digamos, 3. Ele tenta calcular `3 * fatorial 2`. O fatorial de 2 é `2 * fatorial 1`, então, por enquanto, temos `3 * (2 * fatorial 1)`. `fatorial 1` é `1 * fatorial 0`, então temos `3 * (2 * (1 * fatorial 0))`. Agora vem o truque - definimos o fatorial de 0 como sendo apenas 1 e, como ele encontra esse padrão antes do padrão de captura geral, ele retorna apenas 1. Portanto, o resultado final é equivalente a `3 * (2 * (1 * 1))`. Se tivéssemos escrito o segundo padrão acima do primeiro, ele pegaria todos os números, incluindo 0, e nosso cálculo nunca terminaria. É por isso que a ordem é importante ao especificar padrões, e é sempre melhor especificar os mais específicos primeiro e os mais gerais depois.

O correspondência de padrões também pode falhar. Se definirmos uma função assim:

```haskell
charName :: Char -> String  
charName 'a' = "Albert"  
charName 'b' = "Broseph"  
charName 'c' = "Cecil"  
```

E tentarmos chamá-la com uma entrada que não esperávamos, isso é o que acontece:

```haskell
ghci> charName 'a'  
"Albert"  
ghci> charName 'b'  
"Broseph"  
ghci> charName 'h'  
"*** Exception: tut.hs:(53,0)-(55,21): Non-exhaustive patterns in function charName  
```

Isso reclama que temos padrões não-exaustivos, e com razão. Ao fazer padrões, devemos sempre incluir um padrão geral para que nosso programa não falhe se receber uma entrada inesperada.

O correspondência de padrões também pode ser usado em tuplas. E se quiséssemos fazer uma função que recebe dois vetores num espaço 2D (na forma de pares) e os adiciona? Para somar dois vetores, adicionamos suas componentes x separadamente e, em seguida, suas componentes y separadamente. Aqui está como teríamos feito se não conhecêssemos o correspondência de padrões:

```haskell
addVectors :: (Num a) => (a, a) -> (a, a) -> (a, a)  
addVectors a b = (fst a + fst b, snd a + snd b)  
```

Bem, isso funciona, mas há uma maneira melhor de fazer isso. Vamos modificar a função para que ela use correspondência de padrões.

```haskell
addVectors :: (Num a) => (a, a) -> (a, a) -> (a, a)  
addVectors (x1, y1) (x2, y2) = (x1 + x2, y1 + y2)  
```

Aí está! Muito melhor. Note que este já é um padrão geral. O tipo de `addVectors` (em ambos os casos) é `addVectors :: (Num a) => (a, a) -> (a, a) -> (a, a)`, então temos a garantia de obter dois pares como parâmetros.

`fst` e `snd` extraem os componentes de pares. Mas e quanto a trincas? Bem, não existem funções fornecidas que façam isso, mas podemos criar as nossas.

```haskell
first :: (a, b, c) -> a  
first (x, _, _) = x  
      
second :: (a, b, c) -> b  
second (_, y, _) = y  
      
third :: (a, b, c) -> c  
third (_, _, z) = z  
```

O `_` significa a mesma coisa que em _compreensão de lista_. Significa que realmente não nos importamos com o que é aquela parte, então escrevemos um `_`.

Isso me lembra, você também pode fazer correspondência de padrões em _compreensão de lista_. Confira isso:

```haskell
ghci> let xs = [(1,3), (4,3), (2,4), (5,3), (5,6), (3,1)]  
ghci> [a+b | (a,b) <- xs]  
[4,7,6,8,11,4]   
```

Caso uma correspondência de padrões falhe, ele simplesmente passará para o próximo elemento.

Listas em si também podem ser usadas em correspondência de padrões. Você pode fazer correspondência com a lista vazia `[]` ou qualquer padrão que envolva `:` e a lista vazia. Mas como `[1,2,3]` é apenas açúcar sintático para `1:2:3:[]`, você também pode usar o primeiro padrão. Um padrão como `x:xs` vinculará a cabeça da lista a `x` e o restante a `xs`, mesmo que haja apenas um elemento, então `xs` acaba sendo uma lista vazia.

> **Nota**: O padrão `x:xs` é usado com frequência, especialmente em funções recursivas. Mas padrões que contêm `:` só correspondem a listas de comprimento 1 ou mais.

Se você deseja vincular, por exemplo, os três primeiros elementos a variáveis e o restante da lista a outra variável, pode usar algo como `x:y:z:zs`. Isso só corresponderá a listas que tenham três elementos ou mais.

Agora que sabemos como fazer correspondência de padrões em listas, vamos criar nossa própria implementação da função `head`.

```haskell
head' :: [a] -> a  
head' [] = error "Não é possível chamar head numa lista vazia, bobo!"  
head' (x:_) = x  
```

Verificando se funciona:

```haskell
ghci> head' [4,5,6]  
4  
ghci> head' "Hello"  
'H'  
```

Legal! Observe que se você deseja vincular várias variáveis (mesmo que uma delas seja apenas `_` e não vincule nada), você precisa cercá-las por parênteses. Observe também a função `error` que usamos. Ela recebe uma string e gera um erro em tempo de execução, usando essa string como informação sobre que tipo de erro ocorreu. Isso faz com que o programa trave, então não é bom usá-la com muita frequência. Mas chamar `head` numa lista vazia não faz sentido.

Vamos criar uma função trivial que nos informa alguns dos primeiros elementos da lista em inglês (in)conveniente.

```haskell
tell :: (Show a) => [a] -> String  
tell [] = "A lista está vazia"  
tell (x:[]) = "A lista tem um elemento: " ++ show x  
tell (x:y:[]) = "A lista tem dois elementos: " ++ show x ++ " e " ++ show y  
tell (x:y:_) = "Esta lista é longa. Os dois primeiros elementos são: " ++ show x ++ " e " ++ show y  
```

Essa função é segura porque cuida da lista vazia, de uma lista unitária, de uma lista com dois elementos e de uma lista com mais de dois elementos. Observe que `(x:[])` e `(x:y:[])` poderiam ser reescritos como `[x]` e `[x,y]` (porque é açúcar sintático, não precisamos dos parênteses). Não podemos reescrever `(x:y:_)` com colchetes porque ele corresponde a qualquer lista de comprimento 2 ou mais.

Já implementamos nossa própria função `length` usando _list comprehension_. Agora faremos isso usando correspondência de padrões e um pouco de recursão:

```haskell
length' :: (Num b) => [a] -> b  
length' [] = 0  
length' (_:xs) = 1 + length' xs  
```

Isso é semelhante à função fatorial que escrevemos anteriormente. Primeiro, definimos o resultado de uma entrada conhecida — a lista vazia. Isso também é conhecido como a condição de borda. Em seguida, no segundo padrão, dividimos a lista em cabeça e cauda. Dizemos que o comprimento é igual a 1 mais o comprimento da cauda. Usamos `_` para corresponder à cabeça porque na verdade não nos importamos com o que é. Além disso, observe que cuidamos de todos os padrões possíveis de uma lista. O primeiro padrão corresponde a uma lista vazia, e o segundo padrão corresponde a qualquer coisa que não seja uma lista vazia.

Vamos ver o que acontece se chamarmos `length'` em "ham". Primeiro, ele verificará se é uma lista vazia. Como não é, ele passa para o segundo padrão. Ele corresponde ao segundo padrão, e lá diz que o comprimento é 1 + `length' "am"`, porque dividimos em cabeça e cauda e descartamos a cabeça. O `length'` de "am" é, de maneira semelhante, 1 + `length' "m"`. Portanto, agora temos 1 + (1 + `length' "m"`). `length' "m"` é 1 + `length' ""` (também pode ser escrito como 1 + `length' []`). E definimos `length' []` como 0. Então, no final, temos 1 + (1 + (1 + 0)).

Vamos implementar a função `sum`. Sabemos que a soma de uma lista vazia é 0. Escrevemos isso como um padrão. E também sabemos que a soma de uma lista é a cabeça mais a soma do restante da lista. Portanto, se escrevermos isso, obtemos:

```haskell
sum' :: (Num a) => [a] -> a  
sum' [] = 0  
sum' (x:xs) = x + sum' xs  
```

Há também algo chamado de padrões com `as`. Eles são uma maneira útil de quebrar algo de acordo com um padrão e vinculá-lo a nomes, mantendo ainda uma referência ao todo. Você faz isso colocando um nome e um `@` na frente de um padrão. Por exemplo, o padrão `xs@(x:y:ys)`. Este padrão corresponderá exatamente à mesma coisa que `x:y:ys`, mas você pode obter facilmente a lista inteira via `xs` em vez de repetir `x:y:ys` no corpo da função. Aqui está um exemplo rápido e sujo:

```haskell
capital :: String -> String  
capital "" = "String vazia, oops!"  
capital all@(x:xs) = "A primeira letra de " ++ all ++ " é " ++ [x]  
```

```haskell
ghci> capital "Dracula"  
"A primeira letra de Dracula é D"  
```

Normalmente, usamos padrões com `as` para evitar repetições ao corresponder a um padrão maior quando precisamos usar o todo novamente no corpo da função.

Mais uma coisa — você não pode usar `++` em correspondência de padrões. Se você tentasse casar com `(xs ++ ys)`, o que estaria na primeira e o que estaria na segunda lista? Isso não faz muito sentido. Faria sentido corresponder a coisas como `(xs ++ [x,y,z])` ou apenas `(xs ++ [x])`, mas, devido à natureza das listas, você não pode fazer isso.