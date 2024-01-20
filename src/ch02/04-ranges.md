# Texas ranges

![](./assets/cowboy.png)

E se quisermos uma lista de todos os números entre 1 e 20? Claro, poderíamos digitá-los todos, mas obviamente essa não é uma solução para cavalheiros que exigem excelência de suas linguagens de programação. Em vez disso, usaremos intervalos. Os intervalos são uma maneira de criar listas que são sequências aritméticas de elementos que podem ser enumerados. Números podem ser enumerados. Um, dois, três, quatro, etc. Caracteres também podem ser enumerados. O alfabeto é uma enumeração de caracteres de A a Z. Nomes não podem ser enumerados. O que vem depois de "John"? Eu não sei.

Para fazer uma lista contendo todos os números naturais de 1 a 20, você escreve `[1..20]`. Isso é equivalente a escrever `[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20]`, e não há diferença entre escrever um ou outro, exceto que escrever sequências de enumeração longas manualmente é estúpido.

```haskell
ghci> [1..20]  
[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20]  
ghci> ['a'..'z']  
"abcdefghijklmnopqrstuvwxyz"  
ghci> ['K'..'Z']  
"KLMNOPQRSTUVWXYZ"   
```

Os intervalos são legais porque você também pode especificar um passo. E se quisermos todos os números pares entre 1 e 20? Ou a cada terceiro número entre 1 e 20?

```haskell
ghci> [2,4..20]  
[2,4,6,8,10,12,14,16,18,20]  
ghci> [3,6..20]  
[3,6,9,12,15,18]   
```

É apenas uma questão de separar os dois primeiros elementos com uma vírgula e depois especificar qual é o limite superior. Embora seja bastante inteligente, intervalos com passos não são tão inteligentes quanto algumas pessoas esperam que sejam. Você não pode fazer `[1,2,4,8,16..100]` e esperar obter todas as potências de 2. Primeiro, porque você só pode especificar um passo. E segundo, porque algumas sequências que não são aritméticas são ambíguas se fornecidas apenas por alguns de seus primeiros termos.

Para fazer uma lista com todos os números de 20 a 1, você não pode simplesmente fazer `[20..1]`, você tem que fazer `[20,19..1]`.

Cuidado ao usar números de ponto flutuante em intervalos! Porque eles não são completamente precisos (por definição), o uso deles em intervalos pode resultar em alguns resultados bastante estranhos.

```haskell
ghci> [0.1, 0.3 .. 1]  
[0.1,0.3,0.5,0.7,0.8999999999999999,1.0999999999999999]  
```

Minha sugestão é não usá-los em intervalos de listas.

Você também pode usar intervalos para criar listas infinitas apenas não especificando um limite superior. Mais tarde, entraremos em mais detalhes sobre listas infinitas. Por enquanto, examinemos como você obteria os primeiros 24 múltiplos de 13. Claro, você poderia fazer `[13,26..24*13]`. Mas há uma maneira melhor: `take 24 [13,26..]`. Como Haskell é preguiçoso, ele não tentará avaliar a lista infinita imediatamente, porque nunca terminaria. Ele esperará para ver o que você deseja obter dessa lista infinita e aqui ele vê que você deseja apenas os primeiros 24 elementos e atende com prazer.

Aqui estão algumas funções que produzem listas infinitas:

## `cycle` 
Pega uma lista e a transforma em uma lista infinita. Se você apenas tentar exibir o resultado, ele continuará para sempre, então você precisa cortá-lo em algum lugar.

```haskell
ghci> take 10 (cycle [1,2,3])  
[1,2,3,1,2,3,1,2,3,1]  
ghci> take 12 (cycle "LOL ")  
"LOL LOL LOL "   
```

## `repeat` 
Pega um elemento e produz uma lista infinita com apenas esse elemento. É como ciclar uma lista com apenas um elemento.

```haskell
ghci> take 10 (repeat 5)  
[5,5,5,5,5,5,5,5,5,5]  
```

Embora seja mais simples usar a função `replicate` se você deseja repetir algum número de vezes o mesmo elemento. `replicate 3 10` retorna `[10,10,10]`.