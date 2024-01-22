# Expressões de caso

![](assets/case.png)

Muitas linguagens imperativas (C, C++, Java, etc.) possuem uma sintaxe de case, e se você já programou nelas, provavelmente sabe do que se trata. Trata-se de pegar uma variável e então executar blocos de código para valores específicos dessa variável e, talvez, incluir um bloco de código curinga para o caso de a variável ter algum valor para o qual não configuramos um caso.

Haskell pega esse conceito e o eleva a um nível superior. Como o nome sugere, as expressões `case` são, bem, expressões, muito parecidas com as expressões `if else` e as vinculações `let`. Não apenas podemos avaliar expressões com base nos casos possíveis do valor de uma variável, mas também podemos realizar correspondência de padrões. Hmm, pegar uma variável, realizar correspondência de padrões nela, avaliar trechos de código com base em seu valor, onde já ouvimos isso antes? Ah sim, correspondência de padrões nos parâmetros das definições de função! Bem, na verdade, isso é apenas açúcar sintático para expressões `case`. Esses dois trechos de código fazem a mesma coisa e são intercambiáveis:

```haskell
head' :: [a] -> a  
head' [] = error "Sem head para listas vazias!"  
head' (x:_) = x  

head' :: [a] -> a  
head' xs = case xs of [] -> error "Sem head para listas vazias!"  
                      (x:_) -> x  
```

Como você pode ver, a sintaxe para expressões `case` é bastante simples:

```haskell
case expressão of padrão -> resultado  
                  padrão -> resultado  
                  padrão -> resultado  
                  ...  
```

A expressão é casada com os padrões. A ação de correspondência de padrões é a mesma esperada: o primeiro padrão que corresponde à expressão é usado. Se ela percorrer toda a expressão `case` e nenhum padrão adequado for encontrado, ocorre um erro em tempo de execução.

Enquanto a correspondência de padrões nos parâmetros das funções só pode ser feita ao definir funções, as expressões `case` podem ser usadas praticamente em qualquer lugar. Por exemplo:

```haskell
describeList :: [a] -> String  
describeList xs = "A lista é " ++ case xs of [] -> "vazia."  
                                             [x] -> "uma lista única."  
                                             xs -> "uma lista mais longa."  
```

Elas são úteis para realizar correspondência de padrões em algum lugar do meio de uma expressão. Porque a correspondência de padrões nas definições de função é açúcar sintático para expressões `case`, poderíamos ter definido isso também da seguinte forma:

```haskell
describeList :: [a] -> String  
describeList xs = "A lista é " ++ what xs  
    where what [] = "vazia."  
          what [x] = "uma lista única."  
          what xs = "uma lista mais longa."  
```