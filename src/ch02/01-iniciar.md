# Pronto, preparar, começar!

![](./assets/startingout.png)

Bem, vamos começar! Se você é daquele tipo horrível de pessoa que não lê introduções e pulou, talvez queira ler a última seção da introdução de qualquer maneira, pois ela explica o que você precisa para seguir este tutorial e como vamos carregar funções. A primeira coisa que faremos é executar o modo interativo do GHC (ghci) e chamar algumas funções para ter uma ideia básica de Haskell. Abra o seu terminal e digite `ghci`. Você será saudado com algo parecido com isso.

```haskell
GHCi, versão 9.2.4: https://www.haskell.org/ghc/  :? para ajuda  
ghci>  
```

Parabéns, você está no GHCI!

Aqui estão algumas operações aritméticas simples:

```haskell
ghci> 2 + 15  
17  
ghci> 49 * 100  
4900  
ghci> 1892 - 1472  
420  
ghci> 5 / 2  
2.5  
ghci>  
```

Isso é bastante autoexplicativo. Também podemos usar vários operadores em uma linha, e todas as regras usuais de precedência são obedecidas. Podemos usar parênteses para tornar a precedência explícita ou para alterá-la.

```haskell
ghci> (50 * 100) - 4999  
1  
ghci> 50 * 100 - 4999  
1  
ghci> 50 * (100 - 4999)  
-244950  
```

Bem legal, né? Sim, eu sei que não é, mas aguente firme. Um pequeno _pitfall_ para ficar atento aqui é a negação de números. Se quisermos ter um número negativo, é sempre melhor colocá-lo entre parênteses. Fazer `5 * -3` fará com que o GHCI reclame, mas fazer `5 * (-3)` funcionará perfeitamente.

A álgebra booleana também é bastante direta. Como você provavelmente sabe, `&&` significa um "e" booleano, `||` significa um "ou" booleano. `not` nega um `True` ou um `False`.

```haskell
ghci> True && False  
False  
ghci> True && True  
True  
ghci> False || True  
True  
ghci> not False  
True  
ghci> not (True && True)  
False  
```

Testar igualdade é feito assim.

```haskell
ghci> 5 == 5  
True  
ghci> 1 == 0  
False  
ghci> 5 /= 5  
False  
ghci> 5 /= 4  
True  
ghci> "hello" == "hello"  
True   
```

E se quisermos fazer `5 + "llama"` ou `5 == True`? Bem, se tentarmos o primeiro trecho, recebemos uma grande e assustadora mensagem de erro!

```haskell
• Sem instância para (Num String) surgindo a partir de um uso de ‘+’  
• Na expressão: 5 + "llama"  
  Em uma equação para ‘it’: it = 5 + "llama"  
```

Eita! O que o GHCI está nos dizendo aqui é que `"llama"` não é um número e, portanto, não sabe como somá-lo a 5. Mesmo que não fosse `"llama"`, mas `"four"` ou `"4"`, Haskell ainda não o consideraria um número. `+` espera que seu lado esquerdo e direito sejam números. Se tentássemos fazer `True == 5`, o GHCI nos diria que os tipos não correspondem. Enquanto `+` funciona apenas em coisas consideradas números, `==` funciona em qualquer duas coisas que podem ser comparadas. Mas a pegadinha é que ambas precisam ser do mesmo tipo. Você não pode comparar maçãs e laranjas. Daremos uma olhada mais de perto nos tipos um pouco mais tarde. Observação: você pode fazer `5 + 4.0` porque `5` é esperto e pode se comportar como um número inteiro ou um número de ponto flutuante. `4.0` não pode se comportar como um inteiro, então `5` é quem tem que se adaptar.

Você pode não ter percebido, mas estamos usando funções desde o início. Por exemplo, `*` é uma função que recebe dois números e os multiplica. Como você viu, a chamamos colocando-a entre eles. Isso é o que chamamos de uma função _infix_. A maioria das funções que não são usadas com números são funções _prefix_. Vamos dar uma olhada nelas.

![](./assets/ringring.png)

As funções geralmente são prefix, então a partir de agora não afirmaremos explicitamente que uma função está na forma prefix, apenas assumiremos isso. Na maioria das linguagens imperativas, as funções são chamadas escrevendo o nome da função e, em seguida, escrevendo seus parâmetros entre parênteses, geralmente separados por vírgulas. Em Haskell, as funções são chamadas escrevendo o nome da função, um espaço e, em seguida, os parâmetros, separados por espaços. Para começar, tentaremos chamar uma das funções mais chatas em Haskell.

```haskell
ghci> succ 8  
9   
```

A função `succ` aceita qualquer coisa que tenha um sucessor definido e retorna esse sucessor. Como você pode ver, apenas separamos o nome da função do parâmetro com um espaço. Chamar uma função com vários parâmetros também é simples. As funções `min` e `max` recebem duas coisas que podem ser ordenadas (como inteiros!). `min` retorna o menor e `max` retorna o maior. Veja por si mesmo:

```haskell
ghci> min 9 10  
9  
ghci> max 100 101  
101   
```

A aplicação de função (chamar uma função colocando um espaço após ela e, em seguida, digitando os parâmetros) tem a maior precedência de todas. O que isso significa para nós é que estas duas declarações são equivalentes.

```haskell
ghci> succ 9 + max 5 4 + 1  
16  
ghci> (succ 9) + (max 5 4) + 1  
16  
```

No entanto, se quisermos obter o sucessor do produto dos números 9 e 10, não podemos escrever `succ 9 * 10`, porque isso obteria o sucessor de 9, que então seria multiplicado por 10. Então, 100. Teríamos que escrever `succ (9 * 10)` para obter 91.

Se uma função aceita dois parâmetros, também podemos chamá-la como uma função infix, envolvendo-a com crases. Por exemplo, a função `div` aceita dois inteiros e realiza a divisão inteira entre eles. Fazer `div 92 10` resulta em 9. Mas quando chamamos assim, pode haver alguma confusão quanto a qual número está fazendo a divisão e qual está sendo dividido. Então podemos chamá-la como uma função infix fazendo `92 ``div`` 10` e de repente fica muito mais claro.

Muitas pessoas que vêm de linguagens imperativas tendem a manter a ideia de que parênteses devem denotar a aplicação de funções. Por exemplo, em C, você usa parênteses para chamar funções como `foo()`, `bar(1)` ou `baz(3, "haha")`. Como dissemos, em Haskell, espaços são usados para a aplicação de funções. Portanto, essas funções em Haskell seriam `foo`, `bar 1` e `baz 3 "haha"`. Então, se você vir algo como `bar (bar 3)`, isso não significa que `bar` é chamado com `bar` e `3` como parâmetros. Significa que primeiro chamamos a função `bar` com `3` como parâmetro para obter algum número e depois chamamos `bar` novamente com esse número. Em C, isso seria algo como `bar(bar(3))`.