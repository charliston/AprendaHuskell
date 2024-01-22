# Deixe estar

Muito semelhante aos _bindings de where_ são os _bindings de let_. _Bindings de where_ são uma construção sintática que permite vincular variáveis no final de uma função e toda a função pode acessá-las, incluindo todas os _guardas_. _Bindings de let_ permitem vincular variáveis em qualquer lugar e são expressões em si mesmas, mas são muito locais, portanto, não abrangem _guardas_. Assim como qualquer construção em Haskell usada para vincular valores a nomes, _bindings de let_ podem ser usados para correspondência de padrões. Vamos vê-los em ação! Assim é como poderíamos definir uma função que nos dá a área da superfície de um cilindro com base em sua altura e raio:

```haskell
cylinder :: (RealFloat a) => a -> a -> a  
cylinder r h = 
    let sideArea = 2 * pi * r * h  
        topArea = pi * r ^2  
    in  sideArea + 2 * topArea
```

![](assets/letitbe.png)

A forma é `let <bindings> in <expression>`. Os nomes que você define na parte _let_ são acessíveis à expressão após a parte _in_. Como você pode ver, também poderíamos ter definido isso com um _binding de where_. Observe que os nomes também estão alinhados em uma única coluna. Então, qual é a diferença entre os dois? Por enquanto, parece que _let_ coloca os bindings primeiro e a expressão que os usa depois, enquanto _where_ é o contrário.

A diferença é que _bindings de let_ são expressões em si mesmas. _Bindings de where_ são apenas construções sintáticas. Lembre-se de quando fizemos a declaração `if` e foi explicado que uma instrução `if else` é uma expressão e pode ser encaixada em quase qualquer lugar?

```haskell
ghci> [if 5 > 3 then "Woo" else "Boo", if 'a' > 'b' then "Foo" else "Bar"]  
["Woo", "Bar"]  
ghci> 4 * (if 10 > 5 then 10 else 0) + 2  
42
```

Você também pode fazer isso com _bindings de let_.

```haskell
ghci> 4 * (let a = 9 in a + 1) + 2  
42
```

Eles também podem ser usados para introduzir funções em um escopo local:

```haskell
ghci> [let square x = x * x in (square 5, square 3, square 2)]  
[(25,9,4)]
```

Se quisermos vincular vários valores _inline_, obviamente não podemos alinhá-los em colunas. É por isso que podemos separá-los com ponto e vírgula.

```haskell
ghci> (let a = 100; b = 200; c = 300 in a*b*c, let foo="Manda "; bar = "salve!" in foo ++ bar)  
(6000000,"Manda salve!")
```

Você não precisa colocar um ponto e vírgula após o último binding, mas pode se quiser. Como mencionamos antes, você pode fazer pattern matching com _bindings de let_. Eles são muito úteis para desmontar rapidamente uma tupla em componentes e vinculá-los a nomes, e assim por diante.

```haskell
ghci> (let (a,b,c) = (1,2,3) in a+b+c) * 100  
600
```

Você também pode colocar _bindings de let_ dentro de _compreensão de lista_. Vamos reescrever nosso exemplo anterior de cálculo de listas de pares massa-volume para usar um _let_ dentro de uma _compreensão de lista_ em vez de definir uma função auxiliar com um _where_.

```haskell
calcDensities :: (RealFloat a) => [(a, a)] -> [a]  
calcDensities xs = [density | (m, v) <- xs, let density = m / v]
```

Incluímos um _let_ dentro de uma _compreensão de lista_ assim como faríamos com um predicado, apenas ele não filtra a lista, apenas vincula nomes. Os nomes definidos em um _let_ dentro de uma _compreensão de lista_ são visíveis para a função de saída (a parte antes do `|`) e todos os predicados e seções que vêm depois do _binding_. Portanto, poderíamos fazer nossa função retornar apenas as densidades que flutuariam no ar:

```haskell
calcDensities :: (RealFloat a) => [(a, a)] -> [a]  
calcDensities xs = [density | (m, v) <- xs, let density = m / v, density < 1.2]
```

Não podemos usar o nome `density` na parte `(m, v) <- xs` porque ele é definido antes do _binding de let_.

Omitimos a parte in do _binding de let_ quando os usamos em _compreensão de lista_ porque a visibilidade dos nomes já está predefinida lá. No entanto, poderíamos usar um _binding de let in_ num predicado e os nomes definidos seriam visíveis apenas para esse predicado. A parte _in_ também pode ser omitida ao definir funções e constantes diretamente no GHCi. Se fizermos isso, os nomes serão visíveis durante toda a sessão interativa.

```haskell
ghci> let zoot x y z = x * y + z  
ghci> zoot 3 9 2  
29  
ghci> let boot x y z = x * y + z in boot 3 4 2  
14  
ghci> boot  
<interactive>:1:0: Not in scope: `boot'
```

Se _bindings de let_ são tão legais, por que não usá-los o tempo todo em vez de _bindings de where_, você pergunta? Bem, como _bindings de let_ são expressões e são bastante locais no seu escopo, não podem ser usados em _guardas_. Algumas pessoas preferem _bindings de where_ porque os nomes vêm após a função em que estão sendo usados. Dessa forma, o corpo da função está mais próximo da declaração de nome e tipo, e para alguns, isso é mais legível.