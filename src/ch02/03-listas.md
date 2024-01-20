# Uma introdução às listas

![](./assets/list.png)

Assim como listas de compras no mundo real, listas em Haskell são muito úteis. É a estrutura de dados mais usada e pode ser utilizada de várias maneiras diferentes para modelar e resolver diversos problemas. Listas são INCRÍVEIS. Nesta seção, vamos dar uma olhada nos conceitos básicos de listas, strings (que são listas) e compreensões de lista.

Em Haskell, listas são uma estrutura de dados **homogênea**. Elas armazenam vários elementos do mesmo tipo. Isso significa que podemos ter uma lista de inteiros ou uma lista de caracteres, mas não podemos ter uma lista que tenha alguns inteiros e depois alguns caracteres. E agora, uma lista!

```haskell
ghci> lostNumbers = [4,8,15,16,23,42]  
ghci> lostNumbers  
[4,8,15,16,23,42]  
```

Como você pode ver, listas são indicadas por colchetes e os valores nas listas são separados por vírgulas. Se tentássemos criar uma lista como [1,2,'a',3,'b','c',4], Haskell reclamaria que caracteres (que, aliás, são indicados como um caractere entre aspas simples) não são números. Falando em caracteres, strings são apenas listas de caracteres. `"hello"` é apenas um açúcar sintático para ['h','e','l','l','o']. Como strings são listas, podemos usar funções de lista nelas, o que é realmente útil.

Uma tarefa comum é juntar duas listas. Isso é feito usando o operador `++`.

```haskell
ghci> [1,2,3,4] ++ [9,10,11,12]  
[1,2,3,4,9,10,11,12]  
ghci> "hello" ++ " " ++ "world"  
"hello world"  
ghci> ['w','o'] ++ ['o','t']  
"woot"  
```

Cuidado ao usar repetidamente o operador `++` em strings longas. Quando você une duas listas (mesmo que você anexe uma lista singleton a outra lista, por exemplo: `[1,2,3] ++ [4]`), internamente, Haskell precisa percorrer toda a lista no lado esquerdo do `++.` Isso não é um problema ao lidar com listas que não são muito grandes. Mas adicionar algo ao final de uma lista com cinquenta milhões de entradas vai levar um tempo. No entanto, adicionar algo ao início de uma lista usando o operador `:` (também chamado de operador cons) é instantâneo.

```haskell
ghci> 'A':" SMALL CAT"  
"A SMALL CAT"  
ghci> 5:[1,2,3,4,5]  
[5,1,2,3,4,5]  
```

Observe como `:` aceita um número e uma lista de números ou um caractere e uma lista de caracteres, enquanto `++` aceita duas listas. Mesmo se você estiver adicionando um elemento ao final de uma lista com `++`, você deve envolvê-lo com colchetes para que se torne uma lista.

`[1,2,3]` é na verdade apenas açúcar sintático para `1:2:3:[]`. `[]` é uma lista vazia. Se adicionarmos `3` a ela, ela se torna `[3]`. Se adicionarmos `2` a isso, ela se torna `[2,3]`, e assim por diante.

> **Nota:** `[]`, `[[]]` e `[[],[],[]]` são coisas diferentes. O primeiro é uma lista vazia, o segundo é uma lista que contém uma lista vazia, o terceiro é uma lista que contém três listas vazias.

Se você quiser obter um elemento de uma lista pelo índice, use `!!`. Os índices começam em 0.

```haskell
ghci> "Steve Buscemi" !! 6  
'B'  
ghci> [9.4,33.2,96.2,11.2,23.25] !! 1  
33.2  
```

Mas se você tentar obter o sexto elemento de uma lista que só tem quatro elementos, você receberá um erro, então tenha cuidado!

Listas também podem conter listas. Elas também podem conter listas que contêm listas que contêm listas...

```haskell
ghci> b = [[1,2,3,4],[5,3,3,3],[1,2,2,3,4],[1,2,3]]  
ghci> b  
[[1,2,3,4],[5,3,3,3],[1,2,2,3,4],[1,2,3]]  
ghci> b ++ [[1,1,1,1]]  
[[1,2,3,4],[5,3,3,3],[1,2,2,3,4],[1,2,3],[1,1,1,1]]  
ghci> [6,6,6]:b  
[[6,6,6],[1,2,3,4],[5,3,3,3],[1,2,2,3,4],[1,2,3]]  
ghci> b !! 2  
[1,2,2,3,4]   
```

As listas dentro de uma lista podem ter comprimentos diferentes, mas não podem ter tipos diferentes. Assim como você não pode ter uma lista que tem alguns caracteres e alguns números, você não pode ter uma lista que tem algumas listas de caracteres e algumas listas de números.

Listas podem ser comparadas se o conteúdo delas puder ser comparado. Ao usar `<`, `<=`, `>` e `>=` para comparar listas, elas são comparadas em ordem lexicográfica. Primeiro, as cabeças são comparadas. Se forem iguais, os segundos elementos são comparados, e assim por diante.

```haskell
ghci> [3,2,1] > [2,1,0]  
True  
ghci> [3,2,1] > [2,10,100]  
True  
ghci> [3,4,2] > [3,4]  
True  
ghci> [3,4,2] > [2,4]  
True  
ghci> [3,4,2] == [3,4,2]  
True  
```

O que mais você pode fazer com listas? Aqui estão algumas funções básicas que operam em listas.

## `head`
Recebe uma lista e retorna sua cabeça. A cabeça de uma lista é basicamente o seu primeiro elemento.

```haskell
ghci> head [5,4,3,2,1]  
5   
```

## `tail` 
Recebe uma lista e retorna sua cauda. Em outras palavras, ela corta a cabeça de uma lista.

```haskell
ghci> tail [5,4,3,2,1]  
[4,3,2,1]   
```

## `last` 
Recebe uma lista e retorna seu último elemento.

```haskell
ghci> last [5,4,3,2,1]  
1   
```

## `init` 
Recebe uma lista e retorna tudo, exceto seu último elemento.

```haskell
ghci> init [5,4,3,2,1]  
[5,4,3,2]   
```

Se pensarmos em uma lista como um monstro, aqui está o que é o quê.

![](./assets/listmonster.png)

Mas o que acontece se tentarmos obter a cabeça de uma lista vazia?

```haskell
ghci> head []  
*** Exception: Prelude.head: empty list  
```

Oh meu Deus! Tudo explode em nosso rosto! Se não houver monstro, ele não terá cabeça. Ao usar `head`, `tail`, `last` e `init`, tenha cuidado para não usá-los em listas vazias. Este erro não pode ser capturado em tempo de compilação, então é sempre uma boa prática tomar precauções contra dizer acidentalmente ao Haskell para fornecer alguns elementos de uma lista vazia.

## `length` 
Recebe uma lista e retorna seu comprimento, obviamente.

```haskell
ghci> length [5,4,3,2,1]  
5  
```

## `null` 
Verifica se uma lista está vazia. Se estiver, retorna `True`; caso contrário, retorna `False`. Use esta função em vez de `xs == []` (se você tiver uma lista chamada `xs`).

```haskell
ghci> null [1,2,3]  
False  
ghci> null []  
True  
```

## `reverse` 
Inverte uma lista.

```haskell
ghci> reverse [5,4,3,2,1]  
[1,2,3,4,5]  
```

## `take` 
Recebe um número e uma lista. Ela extrai aquele número de elementos do início da lista. Veja.

```haskell
ghci> take 3 [5,4,3,2,1]  
[5,4,3]  
ghci> take 1 [3,9,3]  
[3]  
ghci> take 5 [1,2]  
[1,2]  
ghci> take 0 [6,6,6]  
[]  
```

Veja como, se tentarmos pegar mais elementos do que há na lista, ela simplesmente retorna a lista. Se tentarmos pegar 0 elementos, obtemos uma lista vazia.

`drop` funciona de maneira semelhante, apenas remove o número de elementos do início de uma lista.

```haskell
ghci> drop 3 [8,4,2,1,5,6]  
[1,5,6]  
ghci> drop 0 [1,2,3,4]  
[1,2,3,4]  
ghci> drop 100 [1,2,3,4]  
[]   
```

`maximum` recebe uma lista de coisas que podem ser colocadas em algum tipo de ordem e retorna o maior elemento.

`minimum` retorna o menor.

```haskell
ghci> minimum [8,4,2,1,5,6]  
1  
ghci> maximum [1,9,2,3,4]  
9   
```

`sum` recebe uma lista de números e retorna a soma deles.

`product` recebe uma lista de números e retorna o produto deles.

```haskell
ghci> sum [5,2,1,6,3,2,5,7]  
31  
ghci> product [6,2,1,2]  
24  
ghci> product [1,2,5,6,7,9,2,0]  
0   
```

`elem` recebe uma coisa e uma lista de coisas e nos diz se essa coisa é um elemento da lista. Geralmente, é chamada como uma função infix porque é mais fácil de ler dessa forma.

```haskell
ghci> 4 `elem` [3,4,5,6]  
True  
ghci> 10 `elem` [3,4,5,6]  
False  
```

Essas foram algumas funções básicas que operam em listas. Vamos dar uma olhada em mais funções de [lista](../ch07/02-list.md) posteriormente.