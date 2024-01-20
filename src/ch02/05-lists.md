# Eu sou uma compreensão de lista

Se já fez um curso de matemática, provavelmente já se deparou com compreensões de conjuntos. Normalmente, elas são usadas para construir conjuntos mais específicos a partir de conjuntos gerais. Uma compreensão básica para um conjunto que contém os primeiros dez números naturais pares é a notação de conjunto `S = { 2·x | x∈ℕ , x≤10 }`. A parte antes da barra vertical é chamada de função de saída, `x` é a variável, `N` é o conjunto de entrada e `x <= 10` é a condição. Isso significa que o conjunto contém os duplos de todos os números naturais que satisfazem a condição.

Se quisermos escrever isso em Haskell, poderíamos fazer algo como `take 10 [2,4..]`. Mas e se não quisermos duplos dos primeiros 10 números naturais, mas algum tipo de função mais complexa aplicada a eles? Poderíamos usar uma compreensão de lista para isso. Compreensões de lista são muito semelhantes às compreensões de conjunto. Vamos nos ater a obter os primeiros 10 números pares por enquanto. A compreensão de lista que poderíamos usar é `[x*2 | x <- [1..10]]`. `x` é retirado de `[1..10]` e, para cada elemento em `[1..10]` (que vinculamos a `x`), obtemos esse elemento, apenas duplicado. Aqui está essa compreensão em ação.

```haskell
ghci> [x*2 | x <- [1..10]]  
[2,4,6,8,10,12,14,16,18,20]  
```

Como você pode ver, obtemos os resultados desejados. Agora, vamos adicionar uma condição (ou um predicado) a essa compreensão. Predicados vão após as partes de vinculação e são separados delas por uma vírgula. Digamos que queremos apenas os elementos que, duplicados, são maiores ou iguais a 12.

```haskell
ghci> [x*2 | x <- [1..10], x*2 >= 12]  
[12,14,16,18,20]  
```

Legal, funciona. E se quisermos todos os números de 50 a 100 cujo resto, quando divididos pelo número 7, é 3? Fácil.

```haskell
ghci> [ x | x <- [50..100], x `mod` 7 == 3]  
[52,59,66,73,80,87,94]  
```

Sucesso! Observe que filtrar listas por predicados também é chamado de **filtragem**. Pegamos uma lista de números e a filtramos pelo predicado. Agora, outro exemplo. Digamos que queremos uma compreensão que substitua cada número ímpar maior que 10 por `"BANG!"` e cada número ímpar menor que 10 por `"BOOM!"`. Se um número não for ímpar, o excluímos de nossa lista. Para conveniência, colocaremos essa compreensão dentro de uma função para que possamos reutilizá-la facilmente.

```haskell
boomBangs xs = [ if x < 10 then "BOOM!" else "BANG!" | x <- xs, odd x]
```

A última parte da compreensão é o predicado. A função `odd` retorna `True` em um número ímpar e `False` em um número par. O elemento é incluído na lista apenas se todos os predicados avaliarem para `True`.

```haskell
ghci> boomBangs [7..13]  
["BOOM!","BOOM!","BANG!","BANG!"]
```

Podemos incluir vários predicados. Se quisermos todos os números de 10 a 20 que não sejam 13, 15 ou 19, faríamos:

```haskell
ghci> [ x | x <- [10..20], x /= 13, x /= 15, x /= 19]  
[10,11,12,14,16,17,18,20]
```

Não apenas podemos ter vários predicados em compreensões de listas (um elemento deve satisfazer todos os predicados para ser incluído na lista resultante), podemos também retirar de várias listas. Ao retirar de várias listas, as compreensões produzem todas as combinações possíveis das listas fornecidas e, em seguida, as unem pela função de saída que fornecemos. Uma lista produzida por uma compreensão que retira de duas listas de comprimento 4 terá um comprimento de 16, desde que não as filtramos. Se tivermos duas listas, `[2,5,10]` e `[8,10,11]` e quisermos obter os produtos de todas as combinações possíveis entre números nessas listas, aqui está o que faríamos.

```haskell
ghci> [ x*y | x <- [2,5,10], y <- [8,10,11]]  
[16,20,22,40,50,55,80,100,110]
```

Como esperado, o comprimento da nova lista é 9. E se quisermos todos os produtos possíveis que sejam mais que 50?

```haskell
ghci> [ x*y | x <- [2,5,10], y <- [

8,10,11], x*y > 50]  
[55,80,100,110]
```

E que tal uma compreensão de lista que combina uma lista de adjetivos e uma lista de substantivos... para uma hilaridade épica.

```haskell
ghci> nouns = ["vagabundo","sapo","papa"]  
ghci> adjectives = ["preguiçoso","rabugento","maquiavélico"]  
ghci> [adjective ++ " " ++ noun | adjective <- adjectives, noun <- nouns]  
["preguiçoso vagabundo","preguiçoso sapo","preguiçoso papa","rabugento vagabundo","rabugento sapo",  
"rabugento papa","maquiavélico vagabundo","maquiavélico sapo","maquiavélico papa"]  
```

Eu sei! Vamos escrever nossa própria versão de `length`! Vamos chamá-la de `length'`.

```haskell
length' xs = sum [1 | _ <- xs]
```

`_` significa que não nos importamos com o que retiraremos da lista de qualquer maneira, então, em vez de escrevermos um nome de variável que nunca usaremos, simplesmente escrevemos `_`. Essa função substitui cada elemento de uma lista por `1` e, em seguida, soma isso. Isso significa que a soma resultante será o comprimento de nossa lista.

Apenas um lembrete amigável: porque strings são listas, podemos usar compreensões de lista para processar e produzir strings. Aqui está uma função que pega uma string e remove tudo, exceto as letras maiúsculas.

```haskell
removeNonUppercase st = [c | c <- st, c `elem` ['A'..'Z']]
```

Testando:

```haskell
ghci> removeNonUppercase "Hahaha! Ahahaha!"
"HA"
ghci> removeNonUppercase "EUnaoGOSTODESAPOS"
"EUGOSTODESAPOS"
```

O predicado aqui faz todo o trabalho. Ele diz que o caractere será incluído na nova lista apenas se for um elemento da lista `['A'..'Z']`. Compreensões de lista aninhadas também são possíveis se você estiver operando em listas que contêm listas. Uma lista contém várias listas de números. Vamos remover todos os números ímpares sem nivelar a lista.

```haskell
ghci> let xxs = [[1,3,5,2,3,1,2,4,5],[1,2,3,4,5,6,7,8,9],[1,2,4,2,1,6,3,1,3,2,3,6]]
ghci> [ [ x | x <- xs, even x ] | xs <- xxs]
[[2,2,4],[2,4,6,8],[2,4,2,6,2,6]]
```

Você pode escrever compreensões de lista em várias linhas. Portanto, se você não estiver no GHCI, é melhor dividir compreensões de lista mais longas em várias linhas, especialmente se estiverem aninhadas.