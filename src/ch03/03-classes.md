# Classes de tipo 101

![](assets/classes.png)

Uma _typeclass_ é uma espécie de interface que define algum comportamento. Se um tipo faz parte de uma _typeclass_, isso significa que ele suporta e implementa o comportamento descrito pela _typeclass_. Muitas pessoas que vêm da programação orientada a objetos ficam confusas com _typeclasses_ porque pensam que são como classes em linguagens orientadas a objetos. Bem, elas não são. Você pode pensar nelas de certa forma como interfaces em Java, apenas melhores.

Qual é a assinatura de tipo da função `==`?

```haskell
ghci> :t (==)
(==) :: (Eq a) => a -> a -> Bool
```

> **Nota:** o operador de igualdade, `==`, é uma função. Assim como `+`, `*`, `-`, `/` e praticamente todos os operadores. Se uma função é composta apenas por caracteres especiais, por padrão, ela é considerada uma função infix. Se quisermos examinar seu tipo, passá-la para outra função ou chamá-la como uma função prefixa, precisamos envolvê-la em parênteses.

Interessante. Vemos algo novo aqui, o símbolo `=>`. Tudo antes do símbolo `=>` é chamado de **restrição de classe** (_class constraint_). Podemos ler a declaração de tipo anterior assim: a função de igualdade aceita dois valores de qualquer tipo e retorna um `Bool`. O tipo desses dois valores deve ser membro da classe `Eq` (esta foi a restrição de classe).

A _typeclass_ `Eq` fornece uma interface para testar a igualdade. Qualquer tipo em que faça sentido testar a igualdade entre dois valores desse tipo deve ser membro da classe `Eq`. Todos os tipos padrão do Haskell, exceto IO (o tipo para lidar com entrada e saída) e funções, fazem parte da _typeclass_ `Eq`.

A função `elem` tem um tipo de `(Eq a) => a -> [a] -> Bool` porque ela usa `==` sobre uma lista para verificar se algum valor que estamos procurando está nela.

Algumas classes de tipos básicas:

## `Eq`
É usada para tipos que suportam teste de igualdade. As funções que seus membros implementam são `==` e `/=`. Então, se houver uma restrição de classe `Eq` para uma variável de tipo em uma função, ela usa `==` ou `/=` em algum lugar dentro de sua definição. Todos os tipos que mencionamos anteriormente, exceto funções, fazem parte de `Eq`, então eles podem ser testados para igualdade.

```haskell
ghci> 5 == 5  
True  
ghci> 5 /= 5  
False  
ghci> 'a' == 'a'  
True  
ghci> "Ho Ho" == "Ho Ho"  
True  
ghci> 3.432 == 3.432  
True  
```

## `Ord` 
É para tipos que têm uma ordenação.

```haskell
ghci> :t (>)  
(>) :: (Ord a) => a -> a -> Bool  
```

Todos os tipos que cobrimos até agora, exceto funções, fazem parte de `Ord`. `Ord` cobre todas as funções de comparação padrão, como `>`, `<`, `>=` e `<=`. A função `compare` recebe dois membros de `Ord` do mesmo tipo e retorna uma ordenação. **Ordering** é um tipo que pode ser `GT`, `LT` ou `EQ`, significando _maior que_, _menor que_ e _igual_, respectivamente.

Para ser membro de `Ord`, um tipo deve primeiro ser membro do clube prestigiado e exclusivo `Eq`.

```haskell
ghci> "Abrakadabra" < "Zebra"  
True  
ghci> "Abrakadabra" `compare` "Zebra"  
LT  
ghci> 5 >= 2  
True  
ghci> 5 `compare` 3  
GT  
```

Membros de **Show** podem ser apresentados como strings. Todos os tipos cobertos até agora, exceto funções, fazem parte de `Show`. A função mais usada que lida com a classe de tipo `Show` é `show`. Ela recebe um valor cujo tipo é membro de `Show` e o apresenta para nós como uma string.

```haskell
ghci> show 3  
"3"  
ghci> show 5.334  
"5.334"  
ghci> show True  
"True"  
```

## `Read`
É meio que a classe de tipo oposta a `Show`. A função `read` recebe uma string e retorna um tipo que é membro de `Read`.

```haskell
ghci> read "True" || False  
True  
ghci> read "8.2" + 3.8  
12.0  
ghci> read "5" - 2  
3  
ghci> read "[1,2,3,4]" ++ [3]  
[1,2,3,4,3]  
```

Até agora tudo bem. Novamente, todos os tipos cobertos até agora estão nesta classe de tipo. Mas o que acontece se tentarmos apenas `read "4"`?

```haskell
ghci> read "4"  
<interactive>:1:0:  
    Ambiguous type variable `a' in the constraint:  
      `Read a' arising from a use of `read' at <interactive>:1:0-7  
    Probable fix: add a type signature that fixes these type variable(s)  
```

O que o GHCI está nos dizendo aqui é que não sabe o que queremos em retorno. Observe que nos usos anteriores de `read` fizemos algo com o resultado depois. Dessa forma, o GHCI podia inferir que tipo de resultado queríamos de nosso `read`. Se o usássemos como um booleano, ele sabia que deveria retornar um `Bool`. Mas agora, ele sabe que queremos algum tipo que faça parte da classe `Read`, só não sabe qual. Vamos dar uma olhada na assinatura de tipo do `read`.

```haskell
ghci> :t read  
read :: (Read a) => String -> a  
```

Viu?! Ele retorna um tipo que faz parte de `Read`, mas se não tentarmos usá-lo de alguma forma mais tarde, ele não tem como saber qual tipo. Por isso, podemos usar **anotações de tipo** explícitas. Anotações de tipo são uma maneira de dizer explicitamente qual deve ser o tipo de uma expressão. Fazemos isso adicionando `::` no final da expressão e, em seguida, especificando um tipo. Observe:

```haskell
ghci> read "5" :: Int  
5  
ghci> read "5" :: Float  
5.0  
ghci> (read "5" :: Float) * 4  
20.0  
ghci> read "[1,2,3,4]" :: [Int]  
[1,2,3,4]  
ghci> read "(3, 'a')" :: (Int, Char)  
(3, 'a')  
```

A maioria das expressões são tais que o compilador pode inferir qual é o tipo por si só. Mas às vezes, o compilador não sabe se deve retornar um valor do tipo `Int` ou `Float` para uma expressão como `read "5"`. Para ver qual é o tipo, Haskell teria que avaliar realmente `read "5"`. Mas como Haskell é uma linguagem estaticamente tipada, ele precisa conhecer todos os tipos antes de compilar o código (ou, no caso do GHCI, avaliá-lo). Portanto, temos que dizer ao Haskell: "Ei, essa expressão deve ter este tipo, caso você não saiba!".

## `Enum`
Membros de **Enum** são tipos ordenados sequencialmente — eles podem ser enumerados. A principal vantagem da classe de tipo `Enum` é que podemos usar seus tipos em intervalos de listas. Eles também têm sucessores e predecessores definidos, que podem ser obtidos com as funções `succ` e `pred`. Os tipos nesta classe incluem: `()`, `Bool`, `Char`, `Ordering`, `Int`, `Integer`, `Float` e `Double`.

```haskell
ghci> ['a'..'e']  
"abcde"  
ghci> [LT .. GT]  
[LT,EQ,GT]  
ghci> [3 .. 5]  
[3,4,5]  
ghci> succ 'B'  
'C'  
```

## `Bounded`
Membros de **Bounded** têm um limite superior e inferior.

```haskell
ghci> minBound :: Int  
-2147483648  
ghci> maxBound :: Char  
'\1114111'  
ghci> maxBound :: Bool  
True  
ghci> minBound :: Bool  
False  
```

`minBound` e `maxBound` são interessantes porque têm o tipo de `(Bounded a) => a`. De certa forma, eles são constantes polimórficas.

Todos os tuplos também fazem parte de `Bounded` se os componentes também o forem.

```haskell
ghci> maxBound :: (Bool, Int, Char)  
(True,2147483647,'\1114111')  
```

## `Num` 
É uma classe de tipo numérico. Seus membros têm a propriedade de poder agir como números. Vamos examinar o tipo de um número.

```haskell
ghci> :t 20  
20 :: (Num t) => t  
```

Parece que números inteiros também são constantes polimórficas. Eles podem agir como qualquer tipo que seja um membro da classe de tipo `Num`.

```haskell
ghci> 20 :: Int  
20  
ghci> 20 :: Integer  
20  
ghci> 20 :: Float  
20.0  
ghci> 20 :: Double  
20.0  
```

Esses são tipos que estão na classe de tipo `Num`. Se examinarmos o tipo de `*`, veremos que ele aceita todos os números.

```haskell
ghci> :t (*)  
(*) :: (Num a) => a -> a -> a  
```

Ele recebe dois números do mesmo tipo e retorna um número desse tipo. É por isso que `(5 :: Int) * (6 :: Integer)` resultará em um erro de tipo, enquanto `5 * (6 :: Integer)` funcionará perfeitamente e produzirá um `Integer`, porque 5 pode agir como um `Integer` ou um `Int`.

Para integrar com `Num`, um tipo já deve ser amigo de `Show` e `Eq`.

## `Integral` 
Também é uma classe de tipo numérico. `Num` inclui todos os números, incluindo números reais e números inteiros, enquanto `Integral` inclui apenas números inteiros. Nessa classe de tipo estão `Int` e `Integer`.

## `Floating` 
Inclui apenas números de ponto flutuante, como `Float` e `Double`.

Uma função muito útil para lidar com números é `fromIntegral`. Ela possui uma declaração de tipo `fromIntegral :: (Num b, Integral a) => a -> b`. A partir da sua assinatura de tipo, vemos que ela pega um número integral e o transforma em um número mais geral. Isso é útil quando você deseja que tipos integrais e de ponto flutuante funcionem bem juntos. Por exemplo, a função `length` tem uma declaração de tipo `length :: [a] -> Int` em vez de ter um tipo mais geral de `(Num b) => length :: [a] -> b`. Eu acredito que isso está lá por razões históricas ou algo assim, embora, na minha opinião, seja bobo. De qualquer forma, se tentarmos obter o comprimento de uma lista e depois adicioná-lo a `3.2`, obteremos um erro porque tentamos somar um `Int` e um número de ponto flutuante. Para contornar isso, fazemos `fromIntegral (length [1,2,3,4]) + 3.2` e tudo funciona.

Observe que `fromIntegral` possui várias restrições de classe em sua assinatura de tipo. Isso é completamente válido e, como você pode ver, as restrições de classe são separadas por vírgulas dentro dos parênteses.