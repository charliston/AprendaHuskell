# Acredite no tipo

![](assets/cow.png)

Anteriormente, mencionamos que Haskell possui um sistema de tipos estático. O tipo de cada expressão é conhecido em tempo de compilação, o que leva a um código mais seguro. Se você escrever um programa em que tenta dividir um tipo booleano por algum número, nem mesmo será compilado. Isso é bom porque é melhor detectar tais erros em tempo de compilação, em vez de fazer seu programa falhar. Tudo em Haskell tem um tipo, então o compilador pode raciocinar bastante sobre seu programa antes de compilá-lo.

Ao contrário de Java ou Pascal, Haskell possui inferência de tipo. Se escrevermos um número, não precisamos dizer a Haskell que é um número. Ele pode inferir isso por conta própria, então não precisamos escrever explicitamente os tipos de nossas funções e expressões para realizar as tarefas. Cobrimos alguns conceitos básicos de Haskell com apenas um olhar superficial sobre os tipos. No entanto, entender o sistema de tipos é uma parte muito importante do aprendizado de Haskell.

Um tipo é uma espécie de rótulo que toda expressão possui. Ele nos diz em qual categoria de coisas aquela expressão se encaixa. A expressão `True` é um booleano, `"hello"` é uma string, etc.

Agora usaremos o GHCI para examinar os tipos de algumas expressões. Faremos isso usando o comando `:t` que, seguido por qualquer expressão válida, nos diz seu tipo. Vamos experimentar.

```haskell
ghci> :t 'a'  
'a' :: Char  
ghci> :t True  
True :: Bool  
ghci> :t "HELLO!"  
"HELLO!" :: [Char]  
ghci> :t (True, 'a')  
(True, 'a') :: (Bool, Char)  
ghci> :t 4 == 5  
4 == 5 :: Bool
```

Aqui vemos que ao fazer `:t` em uma expressão, ela imprime a expressão seguida por `::` e seu tipo. `::` é lido como "tem o tipo de". Tipos explícitos são sempre indicados com a primeira letra em maiúscula. `'a'`, como parece, tem um tipo `Char`. Não é difícil concluir que isso representa um caractere. `True` é do tipo `Bool`. Isso faz sentido. Mas o que é isso? Examinar o tipo de `"HELLO!"` resulta em `[Char]`. Os colchetes indicam uma lista. Portanto, lemos isso como sendo uma _lista de caracteres_. Ao contrário das listas, cada comprimento de tupla tem seu próprio tipo. Assim, a expressão `(True, 'a')` tem um tipo de `(Bool, Char)`, enquanto uma expressão como `('a','b','c')` teria o tipo `(Char, Char, Char)`. `4 == 5` sempre retornará `False`, então seu tipo é `Bool`.

![](assets/bomb.png)

Funções também têm tipos. Ao escrever nossas próprias funções, podemos escolher dar a elas uma declaração de tipo explícita. Isso é geralmente considerado uma boa prática, exceto ao escrever funções muito curtas. Daqui em diante, daremos a todas as funções que fizermos declarações de tipo. Lembra-se da compreensão de lista que fizemos anteriormente, que filtra uma string para que apenas as letras maiúsculas permaneçam? Aqui está como ela fica com uma declaração de tipo.

```haskell
removeNonUppercase :: [Char] -> [Char]  
removeNonUppercase st = [ c | c <- st, c `elem` ['A'..'Z']]
```

`removeNonUppercase` tem um tipo de `[Char] -> [Char]`, significando que mapeia de uma string para outra. Isso ocorre porque ela recebe uma string como parâmetro e retorna outra como resultado. O tipo `[Char]` é sinônimo de `String`, então é mais claro se escrevermos `removeNonUppercase :: String -> String`. Não precisávamos dar a essa função uma declaração de tipo porque o compilador pode inferir sozinho que é uma função de uma string para uma string, mas fizemos de qualquer maneira. Mas como escrever o tipo de uma função que recebe vários parâmetros? Aqui está uma função simples que recebe três inteiros e os soma:

```haskell
addThree :: Int -> Int -> Int -> Int  
addThree x y z = x + y + z
```

Os parâmetros são separados por `->` e não há uma distinção especial entre os parâmetros e o tipo de retorno. O tipo de retorno é o último item na declaração e os parâmetros são os três primeiros. Mais tarde, veremos por que todos são apenas separados por `->` em vez de ter alguma distinção mais explícita entre os tipos de retorno e os parâmetros, como `Int, Int, Int -> Int` ou algo do tipo.

Se você quiser dar uma declaração de tipo para sua função, mas não tiver certeza do que deveria ser, você sempre pode escrever a função sem ela e depois verificar com `:t`. Funções são expressões também, então `:t` funciona nelas sem problemas.

Aqui está uma visão geral de alguns tipos comuns.

## `Int` 
Representa números inteiros. É usado para números inteiros. 7 pode ser um `Int`, mas 7.2 não pode. `Int` é limitado, o que significa que ele tem um valor mínimo e máximo. Geralmente, em máquinas de 32 bits, o `Int` máximo possível é 2147483647 e o mínimo é -2147483648.

## `Integer` 
Também representa números inteiros, mas sem limites. Pode ser usado para representar números realmente grandes. No entanto, `Int` é mais eficiente.

```haskell
factorial :: Integer -> Integer
factorial n = product [1..n]

ghci> factorial 50
30414093201713378043612608166064768844377641568960512000000000000
```

## `Float` 
É um ponto flutuante real com precisão simples.

```haskell
circumference :: Float -> Float
circumference r = 2 * pi * r

ghci> circumference 4.0
25.132742
```

##`Double` 
É um ponto flutuante real com o dobro de precisão.

```haskell
circumference' :: Double -> Double
circumference' r = 2 * pi * r

ghci> circumference' 4.0
25.132741228718345
```

## `Bool`
É um tipo booleano. Pode ter apenas dois valores: `True` e `False`.

# `Char` 
Representa um caractere. É indicado por aspas simples. Uma lista de caracteres é uma string.

Tuplas são tipos, mas dependem do seu comprimento, bem como dos tipos de seus componentes. Teoricamente, há um número infinito de tipos de tuplas, o que é muito para cobrir neste tutorial. Note que a tupla vazia `()` também é um tipo que pode ter apenas um valor: `()`
