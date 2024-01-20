# As primeiras funções do bebê

Na seção anterior, tivemos uma ideia básica de como chamar funções. Agora, vamos tentar criar as nossas próprias! Abra o seu editor de texto favorito e insira esta função que recebe um número e o multiplica por dois.

```haskell
doubleMe x = x + x
```

As funções são definidas de maneira semelhante à forma como são chamadas. O nome da função é seguido pelos parâmetros separados por espaços. Mas ao definir funções, há um sinal de igual `=` e, após isso, definimos o que a função faz. Salve isso como `baby.hs` ou algo assim. Agora, vá até onde está salvo e execute o `ghci` de lá. Uma vez dentro do GHCI, digite: `:l baby`. Agora que nosso script está carregado, podemos brincar com a função que definimos.

```haskell
ghci> :l baby
[1 of 1] Compiling Main             ( baby.hs, interpreted )
Ok, one module loaded.
ghci> doubleMe 9
18
ghci> doubleMe 8.3
16.6
```

Como o operador `+` funciona tanto em inteiros quanto em números de ponto flutuante (qualquer coisa que possa ser considerada um número, na verdade), nossa função também funciona com qualquer número. Vamos fazer uma função que recebe dois números, multiplica cada um por dois e depois os adiciona.

```haskell
doubleUs x y = x*2 + y*2
```

Simples. Também poderíamos tê-la definido como `doubleUs x y = x + x + y + y`. Testá-la produz resultados bastante previsíveis (lembre-se de acrescentar esta função ao arquivo `baby.hs`, salve e, em seguida, faça `:l baby` dentro do GHCI).

```haskell
ghci> doubleUs 4 9
26
ghci> doubleUs 2.3 34.2
73.0
ghci> doubleUs 28 88 + doubleMe 123
478
```

Como esperado, você pode chamar suas próprias funções a partir de outras funções que você criou. Com isso em mente, poderíamos redefinir `doubleUs` assim:

```haskell
doubleUs x y = doubleMe x + doubleMe y
```

Este é um exemplo muito simples de um padrão comum que você verá em todo o Haskell. Criar funções básicas que são obviamente corretas e, em seguida, combiná-las em funções mais complexas. Dessa forma, você também evita repetição. E se alguns matemáticos descobrissem que 2 na verdade é 3 e você tivesse que mudar seu programa? Você poderia simplesmente redefinir `doubleMe` para ser `x + x + x` e, como `doubleUs` chama `doubleMe`, automaticamente funcionaria neste novo mundo estranho onde 2 é 3.

Funções em Haskell não precisam estar em uma ordem específica, então não importa se você define `doubleMe` primeiro e depois `doubleUs` ou vice-versa.

Agora vamos criar uma função que multiplica um número por 2, mas apenas se esse número for menor ou igual a 100, porque números maiores que 100 já são grandes o suficiente!

```haskell
doubleSmallNumber x = if x > 100  
                        then x  
                        else x*2   
```

![](./assets/baby.png)

Aqui introduzimos a declaração `if` em Haskell. Você provavelmente está familiarizado com declarações `if` de outras linguagens. A diferença entre a declaração `if` em Haskell e em linguagens imperativas é que a parte `else` é obrigatória em Haskell. Em linguagens imperativas, você pode simplesmente pular algumas etapas se a condição não for atendida, mas em Haskell toda expressão e função deve retornar algo. Poderíamos ter escrito essa declaração `if` em uma linha, mas eu acho este método mais legível. Outra coisa sobre a declaração `if` em Haskell é que ela é uma expressão. Uma expressão é basicamente um trecho de código que retorna um valor. 5 é uma expressão porque retorna 5, `4 + 8` é uma expressão, `x + y` é uma expressão porque retorna a soma de `x` e `y`. Como o `else` é obrigatório, uma declaração `if` sempre retornará algo, e é por isso que é uma expressão. Se quiséssemos adicionar um a cada número produzido por nossa função anterior, poderíamos ter escrito seu corpo assim.

```haskell
doubleSmallNumber' x = (if x > 100 then x else x*2) + 1  
```

Se tivéssemos omitido os parênteses, teria adicionado um apenas se `x` não fosse maior que 100. Observe o `'` no final do nome da função. Essa apóstrofe não tem nenhum significado especial na sintaxe do Haskell. É um caractere válido para usar em um nome de função. Normalmente usamos `'` para denotar uma versão rigorosa de uma função (uma que não é preguiçosa) ou uma versão ligeiramente modificada de uma função ou variável. Como `'` é um caractere válido em funções, podemos fazer uma função assim.

```haskell
conanO'Brien = "It's a-me, Conan O'Brien!"   
```

Aqui, há duas coisas a serem observadas. A primeira é que no nome da função não capitalizamos o nome de Conan. Isso ocorre porque as funções não podem começar com letras maiúsculas. Veremos o motivo um pouco mais tarde. A segunda coisa é que esta função não aceita parâmetros. Quando uma função não aceita parâmetros, geralmente dizemos que é uma _definição_ (ou um nome). Como não podemos alterar o significado de nomes (e funções) depois de definidos, `conanO'Brien` e a string `"It's a-me, Conan O'Brien!"` podem ser usados de forma intercambiável.