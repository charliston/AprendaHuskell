# Guardas, guardas!

![](assets/guards.png)

Enquanto padrões são uma maneira de garantir que um valor esteja em conformidade com alguma forma e deconstruí-lo, guardas são uma maneira de testar se alguma propriedade de um valor (ou várias delas) é verdadeira ou falsa. Isso soa muito como uma instrução if e é muito semelhante. A diferença é que as guardas são muito mais legíveis quando você tem várias condições e se integram muito bem com padrões.

Em vez de explicar a sintaxe, vamos mergulhar e criar uma função usando guardas. Vamos fazer uma função simples que responde de maneira diferente dependendo da [densidade](https://en.wikipedia.org/wiki/Density) fornecida. A densidade (ou massa específica) é a massa de uma substância por unidade de volume (aqui, gramas por litro). Se uma substância tiver uma densidade inferior a 1,2, ela flutuará no ar, pois 1,2 g/L é a densidade do ar. Se tiver mais de 1000 g/L (a densidade da água), ela afundará na água. Entre esses extremos, há coisas (como pessoas, geralmente) que não flutuarão nem afundarão na água. Então, aqui está a função (não estaremos calculando a densidade agora, esta função apenas recebe uma densidade e responde):

```haskell
densityTell :: (RealFloat a) => a -> String  
densityTell density  
    | density < 1.2 = "Uau! Você está indo para um passeio no céu!"  
    | density <= 1000.0 = "Divirta-se nadando, mas tome cuidado com os tubarões!"  
    | otherwise   = "Se for afundar ou nadar, você vai afundar."  
```

Guardas são indicadas por barras verticais que seguem o nome de uma função e seus parâmetros. Normalmente, são recuadas um pouco para a direita e alinhadas. Uma guarda é basicamente uma expressão booleana. Se ela avaliar como `True`, o corpo correspondente da função é usado. Se avaliar como `False`, a verificação passa para a próxima guarda e assim por diante. Se chamarmos esta função com `24,3`, ela primeiro verificará se é menor ou igual a `1,2`. Como não é, ela passa para a próxima guarda. A verificação é realizada com a segunda guarda e, como `24,3` é menor que `1000,0`, a segunda string é retornada.

Isso é muito reminiscente de uma grande árvore if else em linguagens imperativas, apenas isso é muito melhor e mais legível. Embora grandes árvores if else sejam geralmente desaprovadas, às vezes um problema é definido de maneira tão discreta que você não pode evitá-las. Guardas são uma alternativa muito boa para isso.

Muitas vezes, a última guarda é a palavra-chave `otherwise`. `otherwise` é definida simplesmente como `otherwise = True` e captura tudo. Isso é muito semelhante aos padrões, apenas eles verificam se a entrada satisfaz um padrão, mas guardas verificam condições booleanas. Se todas as guardas de uma função avaliarem como `False` (e não fornecemos uma guarda catch-all com `otherwise`), a avaliação passa para o próximo **padrão**. É assim que padrões e guardas se complementam. Se não forem encontradas guardas ou padrões adequados, um erro é lançado.

Claro, podemos usar guardas com funções que aceitam tantos parâmetros quanto quisermos. Em vez de fazer com que o usuário calcule a densidade da substância por conta própria antes de chamar a função, vamos modificar esta função para que ela receba uma massa (em gramas) e um volume (em litros).

```haskell
densityTell :: (RealFloat a) => a -> a -> String  
densityTell mass volume  
    | mass / volume < 1.2 = "Uau! Você está indo para um passeio no céu!"  
    | mass / volume <= 1000.0 = "Divirta-se nadando, mas tome cuidado com os tubarões!"  
    | otherwise   = "Se for afundar ou nadar, você vai afundar."  
```

Vamos ver se a ração de gato vai flutuar...

```haskell
ghci> densityTell 400 1  
"Divirta-se nadando, mas tome cuidado com os tubarões!"  
```

Parece que sim! Pelo menos até ela se dissolver na piscina... Que nojo!

Observe que não há `=` imediatamente após o nome da função e seus parâmetros, antes da primeira guarda. Muitos iniciantes recebem erros de sintaxe porque, às vezes, colocam isso lá.

Outro exemplo muito simples: vamos implementar nossa própria função `max`. Se você se lembra, ela pega duas coisas que podem ser comparadas e retorna a maior delas.

```haskell
max' :: (Ord a) => a -> a -> a  
max' a b  
    | a > b     = a  
    | otherwise = b  
```

Guardas também podem ser escritas em linha, embora eu aconselhe contra isso porque é menos legível, mesmo para funções muito curtas. Mas, para demonstrar, poderíamos escrever `max'` assim:

```haskell
max' :: (Ord a) => a -> a -> a  
max' a b | a > b = a | otherwise = b  
```

Vixe, não ficou muito legível! Seguindo em frente: vamos implementar nosso próprio `compare` usando guardas.

```haskell
myCompare :: (Ord a) => a -> a -> Ordering  
a `myCompare` b  
    | a > b     = GT  
    | a == b    = EQ  
    | otherwise = LT  
```

```haskell
ghci> 3 `myCompare` 2  
GT  
```

> **Nota:** Não apenas podemos chamar funções como operadores infixos com acentos graves, mas também podemos defini-las usando acentos graves. Às vezes, é mais fácil de ler dessa maneira.