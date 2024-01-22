# Variáveis de Tipo

Qual é o tipo da função `head`? Como `head` aceita uma lista de qualquer tipo e retorna o primeiro elemento, qual poderia ser o tipo? Vamos verificar!

```haskell
ghci> :t head
head :: [a] -> a
```

Hmm! O que é esse `a`? É um tipo? Lembre-se de que afirmamos anteriormente que tipos são escritos em maiúsculas, então não pode ser exatamente um tipo. Por não estar em maiúsculas, na verdade é uma **variável de tipo**. Isso significa que `a` pode ser de qualquer tipo. Isso é semelhante a genericidade em outras linguagens, só que em Haskell é muito mais poderoso, pois nos permite escrever funções muito gerais se elas não usam nenhum comportamento específico dos tipos nelas. Funções que têm variáveis de tipo são chamadas de **funções polimórficas**. A declaração de tipo de `head` afirma que ela aceita uma lista de qualquer tipo e retorna um elemento desse tipo.

Embora variáveis de tipo possam ter nomes com mais de um caractere, geralmente damos a elas nomes como `a`, `b`, `c`, `d`...

Lembra do `fst`? Ela retorna o primeiro componente de um par. Vamos examinar seu tipo:

```haskell
ghci> :t fst
fst :: (a, b) -> a
```

Vemos que `fst` aceita uma tupla que contém dois tipos e retorna um elemento que é do mesmo tipo que o primeiro componente do par. É por isso que podemos usar `fst` em um par que contém dois tipos diferentes. Note que, embora `a` e `b` sejam variáveis de tipo diferentes, não significa que elas precisam ser tipos diferentes. Apenas afirma que o tipo do primeiro componente e o tipo do valor retornado são os mesmos.