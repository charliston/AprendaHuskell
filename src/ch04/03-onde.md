# Onde!?

Na seção anterior, definimos uma função de cálculo de densidade e uma resposta assim:

```haskell
densityTell :: (RealFloat a) => a -> a -> String  
densityTell mass volume  
    | mass / volume < 1.2 = "Divirta-Se nadando, mas tome cuidado com os tubarões!"  
    | mass / volume <= 1000.0 = "Divirta-se nadando, mas tome cuidado com os tubarões!"  
    | otherwise   = "Se for afundar ou nadar, você vai afundar."  
```

Observe que nos repetimos aqui duas vezes. Nos repetimos duas vezes. Se repetir (duas vezes) ao programar é tão desejável quanto levar um chute na cabeça. Como repetimos a mesma expressão duas vezes, seria ideal se pudéssemos calculá-la uma vez, atribuí-la a um nome e, em seguida, usar esse nome em vez da expressão. Bem, podemos modificar nossa função assim:

```haskell
densityTell :: (RealFloat a) => a -> a -> String  
densityTell mass volume  
    | density < 1.2 = "Divirta-Se nadando, mas tome cuidado com os tubarões!"  
    | density <= 1000.0 = "Divirta-se nadando, mas tome cuidado com os tubarões!"  
    | otherwise   = "Se for afundar ou nadar, você vai afundar."  
    where density = mass / volume  
```

Colocamos a palavra-chave `where` após as guardas (geralmente é melhor indentá-la tanto quanto as barras estão indentadas) e, em seguida, definimos vários nomes ou funções. Esses nomes são visíveis em todas os guardas e nos dão a vantagem de não precisar nos repetir. Se decidirmos que queremos calcular a densidade de uma maneira um pouco diferente, só precisamos mudar uma vez. Isso também melhora a legibilidade dando nomes às coisas e pode tornar nossos programas mais rápidos, já que coisas como nossa variável `density` aqui são calculadas apenas uma vez. Poderíamos exagerar um pouco e apresentar nossa função assim:

```haskell
densityTell :: (RealFloat a) => a -> a -> String  
densityTell mass volume  
    | density < air = "Divirta-Se nadando, mas tome cuidado com os tubarões!"  
    | density <= water = "Divirta-se nadando, mas tome cuidado com os tubarões!"  
    | otherwise   = "Se for afundar ou nadar, você vai afundar."  
    where density = mass / volume  
          air = 1.2  
          water = 1000.0  
```

Os nomes que definimos na seção `where` de uma função são visíveis apenas para essa função, então não precisamos nos preocupar com eles poluindo o espaço de nomes de outras funções. Observe que todos os nomes estão alinhados numa única coluna. Se não os alinharmos direito, o Haskell fica confuso porque então ele não sabe que todos fazem parte do mesmo bloco.

As vinculações _where_ não são compartilhadas entre os corpos de função de diferentes padrões. Se você deseja que vários padrões de uma função acessem um nome compartilhado, você precisa defini-lo globalmente.

Você também pode usar vinculações _where_ para fazer **correspondência de padrões**! Poderíamos ter reescrito a seção _where_ de nossa função anterior como:

```haskell
...  
where density = mass / volume  
      (air, water) = (1.2, 1000.0)  
```

Vamos criar outra função bastante trivial em que recebemos um primeiro e um último nome e devolvemos as iniciais.

```haskell
initials :: String -> String -> String  
initials firstname lastname = [f] ++ ". " ++ [l] ++ "."  
    where (f:_) = firstname  
          (l:_) = lastname  
```

Poderíamos ter feito essa correspondência de padrões diretamente nos parâmetros da função (seria mais curto e claro, na verdade), mas isso apenas mostra que é possível fazê-lo também em vinculações where.

Assim como definimos constantes em blocos where, você também pode definir funções. Mantendo-se fiel ao nosso tema de programação de sólidos, vamos criar uma função que recebe uma lista de pares massa-volume e retorna uma lista de densidades.

```haskell
calcDensities :: (RealFloat a) => [(a, a)] -> [a]  
calcDensities xs = [density m v | (m, v) <- xs]  
    where density mass volume = mass / volume  
```

E é isso! A razão pela qual tivemos que introduzir `density` como uma função neste exemplo é porque não podemos calcular apenas uma densidade a partir dos parâmetros da função. Precisamos examinar a lista passada para a função, e há uma densidade diferente para cada par ali.

As vinculações _where_ também podem ser aninhadas. É uma prática comum criar uma função e definir algumas funções auxiliares em sua cláusula _where_ e, em seguida, fornecer essas funções auxiliares também com sua própria cláusula _where_.