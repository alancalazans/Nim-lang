# NimLang

## Introdução

Este documento é uma coletânea de conteúdos provenientes de diversas fontes sobre a linguagem Nim (como o [guia oficial da linguagem](https://nim-lang.org/documentation.html), tutoriais de terceiros, o [ebook de Stefan Salewski](https://nimprogrammingbook.com/), IA e etc...), além de experimentos próprios. Está organizado de forma lógica, buscando cobrir a linguagem desde o nível introdutório até o avançado, tanto quanto possível.

## Instalação do Nim no sistema

### choosenim

Choosenim instala a linguagem de programação Nim a partir de downloads e fontes oficiais, permitindo alternar facilmente entre compiladores estáveis e de desenvolvimento.

O objetivo desta ferramenta é duplo:

- Fornece uma maneira fácil de instalar o compilador e as ferramentas do Nim.
- Gerencie diversas instalações do Nim e permita que elas sejam selecionadas sob demanda.

### Uso típico

Para selecionar a versão estável atual do Nim:

```bash
$ choosenim stable
  Installed component 'nim'
  Installed component 'nimble'
  Installed component 'nimgrep'
  Installed component 'nimpretty'
  Installed component 'nimsuggest'
  Installed component 'testament'
   Switched to Nim 1.0.0
$ nim -v
Nim Compiler Version 1.0.0 [Linux: amd64]
```

Para atualizar para a versão estável mais recente do Nim:

```bash
$ choosenim update stable
```

Para exibir quais versões estão instaladas atualmente:

```bash
$ choosenim show
  Selected: 1.6.6
   Channel: stable
      Path: /home/dom/.choosenim/toolchains/nim-1.6.6

  Versions:
            #devel
          * 1.6.6
            1.0.0
            #v1.0.0
```

As versões podem ser selecionadas via Choosenim 1.6.6 ou pelo nome da ramificação/tag via Choosenim #devel (observe que a seleção de ramificações provavelmente exigirá que o Nim seja inicializado, o que pode ser lento).

## Instalando o choosenim

### Windows

Baixe a versão mais recente do Windows na página de [releases](https://github.com/dom96/choosenim/releases) (o arquivo .zip, por exemplo, aqui é [`v0.8.4`](https://github.com/dom96/choosenim/releases/download/v0.8.4/choosenim-installer-0.8.4_windows_amd64-download-this-not-the-exe.zip)).

Extraia o arquivo zip e execute o script runme.bat. Siga todas as instruções na tela e aproveite sua nova instalação do Nim e do Choosenim.

> <img src="icons/sticky-notes01.png" width=48/> **Obs.:** Mais informações sobre o choosenim na [página do projeto](https://github.com/dom96/choosenim)

### Iniciando o compilador e executando o programa

**O comando**

```sh
$ nim c test.nim
```

é a invocação mais básica do compilador. A extensão .nim é opcional, o compilador pode inferir essa extensão de arquivo. Este comando compila nosso programa no modo de depuração padrão; ele usa o back-end do compilador C e gera um executável nativo. O modo de depuração significa que o executável gerado inclui muitas verificações, como verificações de índice de array, verificações de intervalo, verificações de desreferência nula, entre outras. O executável gerado pode não ser executado muito rápido e será grande, mas se o seu programa tiver bugs, ele fornecerá uma mensagem de erro significativa na maioria dos casos. Somente depois de testar cuidadosamente seu programa, você poderá considerar compilá-lo sem o modo de depuração. Você pode fazer isso com:

```sh
$ nim c -d:release test.nim

$ nim c -d:danger test.nim
```

A opção do compilador `-d:release` remove a maioria das verificações e depuração do código e permite a otimização do backend passando a opção "-O3" para o backend do compilador C, resultando em um arquivo executável muito rápido e pequeno. A opção `-d:danger` inclui `-d:release` e remove todas as verificações. Você deve estar ciente de que compilar com `-d:danger` significa que seu programa pode travar sem nenhuma informação útil ou, pior ainda, pode ser executado, mas conter erros não detectados, como overflows, que podem levar a resultados incorretos. Geralmente, você deve compilar seu programa primeiro com `nim c` simples. Depois de testá-lo bem e se precisar de desempenho adicional, você pode mudar para a opção `-d:release`. Para jogos, benchmarks ou outras tarefas não críticas, você pode tentar a opção `-d:danger`, para obter um executável sem qualquer verificação de desempenho máximo.

Para testar scripts pequenos, compilar e executar um programa sem gerar um arquivo executável permanente:

```sh
$ nim r test.nim
```

## [Comentários](https://nim-lang.org/docs/tut1.html#lexical-elements-comments)

Os comentários começam em qualquer lugar fora de uma cadeia de caracteres ou literal de caractere com o caractere `de hash #`. Os comentários da documentação começam com `##`:

```nim
# Um comentário.

var myVariable: int ## um comentário de documentação
```

Os comentários da documentação são tokens; eles só são permitidos em determinados lugares no arquivo de entrada, pois pertencem à árvore de sintaxe! Esse recurso permite geradores de documentação mais simples.

Os comentários de várias linhas são iniciados com `#[` e terminados com `]#`. Comentários de várias linhas também podem ser aninhados.

```nim
#[
Você pode comentar qualquer texto de código Nim
dentro disso sem restrições de indentação.
      yes("Posso fazer uma pergunta inútil?")
  #[
     Nota: estes podem ser aninhados!!
  ]#
]#
```

## Nosso primeiro programa Nim

```nim
# test.nim
# Imprimindo algo na tela
echo "Bem vindo ao Nim!!!"
```

Agora no terminal:

```bash
$ nim r test.nim
```

## Declaração de variável

Nim é uma linguagem de programação com tipagem estática, o que significa que o tipo de uma atribuição precisa ser declarado antes de usar o valor.

Em Nim, também distinguimos valores que podem mudar, ou sofrer mutação, daqueles que não podem, mais sobre isso mais tarde. Podemos declarar uma variável (uma atribuição mutável) usando a palavra-chave ***var***, apenas declarando seu nome e tipo (o valor pode ser adicionado posteriormente) usando esta sintaxe:

```nim
var <name>: <type>
```

Se já sabemos seu valor, podemos declarar uma variável e dar a ela um valor imediatamente:

```nim
var <name>: <type> = <value>
```

> Os colchetes angulares (***<>***) são usados para mostrar algo que você pode alterar.
> Portanto, ***<name>*** não é literalmente a palavra ***name*** entre colchetes angulares, mas qualquer nome.

Nim também possui capacidade de inferência de tipo: o compilador pode detectar automaticamente o tipo de atribuição de um nome a partir de seu valor, sem declarar explicitamente o tipo. Veremos mais sobre os vários tipos no próximo capítulo.

Portanto, podemos atribuir uma variável sem um tipo explícito como este:

```nim
var <name> = <value>
```

Um exemplo disso em Nim é assim:

```nim
# A variável `a` é do tipo int (inteiro) sem
# nenhum valor definido explicitamente.
var a: int

# A variável `b` tem valor `7`. Seu tipo é
# detectado automaticamente como inteiro.
var b = 7
```

Ao atribuir nomes, é importante escolher nomes que signifiquem algo para o seu programa. Simplesmente nomeá-los como a, b, c, e assim por diante, rapidamente se tornará confuso. Não é possível usar espaços em um nome, pois isso iria dividi-lo em dois. Portanto, se o nome que você escolher consistir em mais de uma palavra, a maneira usual é escrevê-lo no estilo camelCase (observe que a primeira letra de um nome deve ser minúscula).

Observe, entretanto, que <strong style="text-decoration: underline; font-style: italic;">Nim não faz distinção entre maiúsculas e minúsculas e sublinhados</strong>, o que significa que ***olaMundo*** e ***ola_mundo*** seriam o mesma variável. <strong style="text-decoration: underline; font-style: italic;">A exceção a isso é o primeiro caractere, que diferencia maiúsculas de minúsculas</strong>. Os nomes também podem incluir números e outros caracteres ***UTF-8***, até mesmo emojis, caso deseje, mas lembre-se de que você e possivelmente outras pessoas terão de digitá-los.

Em vez de digitar ***var*** para cada variável, várias variáveis (não necessariamente do mesmo tipo) podem ser declaradas no mesmo bloco ***var***. No Nim, os blocos são partes do código com o mesmo recuo (mesmo número de espaços antes do primeiro caractere) e o nível de recuo padrão é de dois espaços. Você verá esses blocos em todos os lugares em um programa Nim, não apenas para atribuir nomes.

```nim
var
  c = -11
  d = "Olá"
  e = '!'
```

> No Nim, as guias de indentação não são permitidas como recuo (tabulação).
> Você pode configurar seu editor de código para converter o pressionamento de Tab em qualquer número de espaços.
> No VS Code, a configuração padrão é converter Tab em quatro espaços. Isso é facilmente substituído nas configurações (Ctrl +,) definindo 'editor.tabSize': 2.

Como as variáveis do tipo ***var*** mencionadas anteriormente são mutáveis, ou seja, seu valor pode mudar (várias vezes), mas seu tipo deve permanecer o mesmo que o declarado.

```nim
# A variável `f` tem um valor inicial de `7`
# e seu tipo é inferido como `int`.
var f = 7

# O valor de `f` é alterado primeiro para `-3` e depois para `19`.
# Ambos são inteiros, iguais ao valor original.
f = -3
f = 19

# Tentar alterar o valor de `f` para `Hello` produz um erro porque `Hello`
# não é um número e isso mudaria o tipo de `f` de um inteiro para uma `string`.
f = "Hello" # error 
```

## Atribuição imutável

Ao contrário das variáveis declaradas com a palavra-chave ***var***, mais dois tipos de atribuição existem em Nim, cujo valor não pode ser alterado, um declarado com a palavra-chave ***const*** e o outro declarado com a palavra-chave ***let***.

### Const

O valor de uma atribuição imutável declarada com a palavra-chave `const` <span style="font-style: italic; font-weight: bold; text-decoration: underline;">deve ser conhecido em tempo de compilação</span> (antes que o programa seja executado).

Por exemplo, podemos declarar a aceleração da gravidade como ***const g = 9.81*** ou ***pi*** como ***const pi = 3.14***, pois sabemos seus valores de antemão e esses valores não mudarão durante a execução de nosso programa.

```nim
const
  pi = 3.14
  g  = 9.81
# O valor de uma constante não pode ser alterado.
g = -27 # error

# A variável `h` não é avaliada em tempo de compilação
# (é uma variável e seu valor pode mudar durante a
# execução de um programa), conseqüentemente o valor da
# constante `i` não pode ser conhecido em tempo de
# compilação, e isso gerará um erro.
var h = -5
const i = h + 7 # error
```

Em algumas linguagens de programação, é uma prática comum ter os nomes das constantes escritos em ***ALL_CAPS*** (caixa alta). As constantes em Nim são escritas como qualquer outra variável.

### Let

Atribuições imutáveis declaradas com ***let*** <span style="font-style: italic; text-decoration: underline; font-weight: bold;">não precisam ser conhecidas em tempo de compilação</span>, seu valor pode ser definido a qualquer momento durante a execução de um programa, mas uma vez definido, seu valor não pode ser alterado.

```nim
let j = 35
# O valor de um imutável não pode ser alterado.
j = -27 # error

var k = -5
# Em contraste com o exemplo `const` acima,
# isso funciona.
let l = k + 7
```

> **Obs.:**
>
> Na prática, você verá ou usará ***let*** com mais freqüência do que ***const***.
>
> Embora você possa usar ***var*** para tudo, sua escolha padrão deve ser ***let***. Use ***var*** apenas para as variáveis que serão modificadas.

### Resumindo `var`, `let` e `const`

- ***var***: São variáveis que podem ser criadas em <u>*runtime*</u> (tempo de execução) - quando valores são solicitados durante a execução do programa ou em <u>*compiletime*</u> (tempo de compilação) - valores são conhecidos durante a compilação e em ambos os casos estas variáveis podem ter seu valor atualizado durante a execução do programa.

- ***let*** e ***const***: São constantes, o símbolo definido como ***const*** seu valor deve ser conhecido em <u>*compiletime*</u> enquanto ***let*** seu valor é atribuído uma única vez tanto em <u>*runtime*</u> quanto em <u>*compiletime*</u>.

> **Exemplos:**
>
> ```nim
> var variavel: int = 0
> variavel = 5 # Certo!
> 
> let constanteLet: int = 2
> # constanteLet = 10 # Errado!
> let outroLet = variavel + constanteLet # Ok!
> 
> # const constante: int = variavel + 2 # Errado!
> const constante: int = 7 # Certo!
> ```

## Tipos de dados básicos

### Inteiros (integers)

Como visto no capítulo anterior, inteiros são números que são escritos sem um componente fracionário e sem um ponto decimal.

Por exemplo: ***32, -174, 0, 10_000_000*** são todos inteiros. Observe que podemos usar `_` como um separador de milhares, para tornar os números maiores mais legíveis (é mais fácil ver que estamos falando de ***10 milhões*** quando é escrito como ***10_000_000*** em vez de ***10000000***).

Os operadores matemáticos usuais - **adição** ( `+` ), **subtração** ( `-` ), **multiplicação** ( `*` ) e **divisão** ( `/` ) - funcionam como esperado. As três primeiras operações sempre produzem inteiros, enquanto a divisão de dois inteiros sempre dá um número de ponto flutuante (um número com um ponto decimal) como resultado, mesmo se dois números puderem ser divididos sem resto.

A divisão inteira (divisão em que a parte fracionária é descartada) pode ser obtida com o operador ***div***. Um operador ***mod*** é usado se alguém estiver interessado no resto (módulo) de uma divisão inteira. O resultado dessas duas operações é sempre um número inteiro.

***integers.nim***

```nim
let
  a = 10
  b = 3

# Comandos e suas respectivas saídas:
echo "a + b = ", a + b     # --> a + b = 13
echo "a - b = ", a - b     # --> a - b = 7
echo "a * b = ", a * b     # --> a * b = 30
echo "a / b = ", a / b     # --> a / b = 3.333333333333333
echo "a div b = ", a div b # --> a div b = 3
echo "a mod b = ", a mod b # --> a mod b = 1
```

### Números em ponto fluante (Floats)

Os números de vírgula flutuante, ou simplesmente flutuantes, são uma representação aproximada de números reais.

Por exemplo: ***2.73, -3.14, 5.0, 4e7*** são flutuantes. Observe que podemos usar notação científica para grandes flutuadores, onde o número após o ***"e"***  é o expoente. Neste exemplo, ***4e7*** é uma notação que representa ***4 \* 10 ^ 7***.

Também podemos usar as quatro operações matemáticas básicas entre dois números flutuantes. Operadores ***div*** e ***mod*** não são definidos para números flutuantes.

***floats.nim***

```nim
let
  c = 6.75
  d = 2.25

# Comandos e suas respectivas saídas:
echo "c + d = ", c + d # --> c + d = 9.0 
echo "c - d = ", c - d # --> c - d = 4.5
echo "c * d = ", c * d # --> c * d = 15.1875
echo "c / d = ", c / d # --> c / d = 3.0
```

A precedência das operações matemáticas é como seria de esperar: ***multiplicação*** e ***divisão*** têm prioridade mais alta do que ***adição*** e ***subtração***.

```nim
echo 2 + 3 * 4  # --> saída: 14
echo 24 - 8 / 4 # --> saída: 22.0
```

### Convertendo floats e inteiros

Operações matemáticas entre variáveis de diferentes tipos numéricos não são possíveis no Nim e irão produzir um erro:

```nim
let
  e = 5
  f = 23.456

echo e + f # error
```

Os valores das variáveis precisam ser convertidos para o mesmo tipo. A conversão é direta: para converter para um inteiro, usamos a função ***int***, e para converter para um ***float***, a função ***float*** é usada.

```nim
let
  e = 5
  f = 23.987

# Imprimir uma versão flutuante de um inteiro e
# (e permanece do tipo inteiro).
echo float(e) # --> saída: 5.0
    
# Imprimindo uma versão inteira de um float f.
echo int(f) # --> saída: 23
    
# Ambos os operandos são flutuantes e podem
# ser adicionados.
echo float(e) + f # --> saída: 28.987
    
# Ambos os operandos são inteiros e podem ser
# adicionados.
echo e + int(f) # --> saída: 28
```

> Ao usar a função ***int()*** para converter um ***float*** em um ***inteiro***, nenhum arredondamento será executado. O número simplesmente elimina quaisquer casas decimais.
> Para realizar o arredondamento devemos chamar outra função, mas para isso devemos saber um pouco mais sobre como usar o Nim.

### Caracteres (Characters)

O tipo ***char*** é usado para representar um único caractere ***ASCII***.

Os caracteres são escritos entre duas aspas simples ( ***'*** ). Os caracteres podem ser letras, símbolos ou dígitos únicos. Vários dígitos ou várias letras produzem um erro.

```nim
let
  h = 'z'
  i = '+'
  j = '2'
  k = '35' # error
  l = 'xy' # error
```

### Strings

***Strings*** podem ser descritos como uma série de caracteres. Seu conteúdo é escrito entre duas aspas duplas ( ***"*** ).

Podemos pensar em ***strings*** como palavras, mas elas podem conter mais de uma palavra, alguns símbolos ou dígitos.

```nim
let
  m = "palavra"
  n = "Uma frase com interpunção."

  # Uma `string` vazia.
  o = ""
    
  # Este não é um número `int`. Ele está
  # entre aspas duplas, o que o torna uma 
  # `string`.
  p = "32"
    
  # Embora seja apenas um caractere, não é um
  # caractere mas uma `string` pois está entre
  # aspas duplas.
  q = "!"  
```

### Caracteres especiais

Se tentarmos imprimir a seguinte string:

```nim
echo "some\nim\tips"
```

O resultado pode nos surpreender:

```
some
im    ips
```

Isso ocorre porque existem vários caracteres que têm um significado especial. Eles são usados colocando o caractere de escape **\\** antes deles.

- `\n` é um caractere de nova linha
- `\t` é um caractere de tabulação
- `\\` é uma barra invertida (uma vez que **\\** é usado como o caractere de escape)

Se quisermos imprimir o exemplo acima como foi escrito, temos duas possibilidades:

- Use **\\\\** em vez de **\\** para imprimir barras invertidas ou
- Use ***strings*** brutas com sintaxe ***r '...'*** (colocando uma letra ***r*** imediatamente antes da primeira aspa), nas quais não haja caracteres de escape e nenhum significado especial: tudo é impresso como está.

```nim
echo "some\\nim\\tips" # --> saída: some\nim\tips
echo r"some\nim\tips" # --> saída: some\nim\tips
```

Existem mais caracteres especiais do que os listados acima, e todos eles são encontrados no [manual do Nim](https://nim-lang.org/docs/manual.html#lexical-analysis-string-literals).

### Concatenação de string

***Strings*** em ***Nim*** são mutáveis, o que significa que seu conteúdo pode mudar. Com a função ***add***, podemos adicionar (anexar) outra ***string*** ou um caractere a uma ***string*** existente. Se não quisermos alterar a ***string*** original, também podemos ***concatenar*** (juntar) ***strings*** com o operador ***&***, isso retorna uma nova ***string***.

***stringConcat.nim***

```nim
# Se planejamos modificar `strings`, eles
# devem ser declarados como `var`.
var                     
  p = "abc"
  q = "xy"
  r = 'z'

# Adicionar outra `string` modifica a
# `string` existente `p` no local,
# alterando seu valor.
p.add("def")            
echo "p é agora: ", p # --> saída: p é agora: abcdef

# Também podemos adicionar um
# `char` a uma `string`.
q.add(r)                
echo "q é agora: ", q # --> saída: q é agora: xyz

# Concatenar duas `strings` produz uma nova `string`,
# sem modificar as `strings` originais.
echo "concateanado: ", p & q # --> saída: concatenado: abcdefxyz

echo "p ainda é: ", p # --> saída: p ainda é: abcdef
echo "q ainda é: ", q # --> saída: q ainda é: xyz
```

### Boleano

Um tipo de dado ***boolean*** (ou apenas ***bool***) pode ter apenas dois valores: ***true*** ou ***false***. Os booleanos são geralmente usados para controlar o fluxo (consulte o próximo capítulo) e geralmente são o resultado de operadores relacionais.

A convenção de nomenclatura usual para variáveis booleanas é escrevê-las como perguntas simples sim/não (verdadeiro/falso), por exemplo, ***IsEmpty***, ***isFinished***, ***isMoving***, etc.

### Operadores relacionais

Os operadores relacionais testam a relação entre duas entidades, que devem ser comparáveis.

Para comparar se dois valores são iguais, **==** (dois sinais de igual) é usado. Não confunda com **=**, que é usado para atribuição como vimos anteriormente.

Aqui estão todos os operadores relacionais definidos para inteiros:

***relationalOperators.nim***

```nim
let
  g = 31
  h = 99

# Instruções e suas respectivas saídas:
echo "g é maior que h: ", g > h # --> g é maior que h: false
echo "g é menor que h: ", g < h # --> g é menor que h: true
echo "g é igual a h: ", g == h # --> g é igual a h: false
echo "g é diferente de h: ", g != h # --> g é diferente de h: true
echo "g é maior ou igual a h: ", g >= h # --> g é maior ou igual a h: false
echo "g é menor ou igual a h: ", g <= h # --> g é menor ou igual a h: true
```

Também podemos comparar ***caracteres*** e ***strings***:

***relationalOperators.nim***

```nim
let
  i = 'a'
  j = 'd'
  k = 'Z'

echo i < j # --> saída: true
    
# Todas as letras maiúsculas vêm
# antes das letras minúsculas.
echo i < k # --> saída: false

let
  m = "axyb"
  n = "axyz"
  o = "ba"
  p = "ba "

# A comparação de `strings` funciona caractere
# por caractere. Os três primeiros caracteres
# são iguais e o caractere `b` é menor que o
# caractere `z`.
echo m < n #  --> saída: true

# O comprimento da `string` não importa para
# comparação se seus caracteres não forem
# idênticos.
echo n < o # --> saída: true

# A `string` mais curta é menor do que a
# mais longa.
echo o < p # --> saída: true
```

### Operadores lógicos

Operadores lógicos são usados para testar a veracidade de uma expressão que consiste em um ou mais valores booleanos.

- Lógico ***and*** retorna verdadeiro apenas se ambos os membros forem verdadeiros
- Lógico ***or*** retorna verdadeiro se houver pelo menos um membro verdadeiro
- O ***xor*** lógico retorna verdadeiro se um membro for verdadeiro, mas o outro não
- O lógico ***not*** nega a veracidade de seu membro: mudando verdadeiro para falso e vice-versa (é o único operador lógico que leva apenas um operando)

***logicalOperators.nim***

```nim
echo "true and true: ", true and true # --> true
echo "true and false: ", true and false # --> false
echo "false and false: ", false and false # --> false
echo "---"
echo "true or true: ", true or true # --> true
echo "true or false: ", true or false # --> true
echo "false or false: ", false or false # --> false
echo "---"
echo "true xor true: ", true xor true # --> false
echo "true xor false: ", true xor false # --> true
echo "false xor false: ", false xor false # --> false
echo "---"
echo "not true: ", not true # --> false
echo "not false: ", not false # --> true
```

Operadores relacionais e lógicos podem ser combinados para formar expressões mais complexas.

Por exemplo: ***(5 < 7) and (11 + 9 == 32 - 2 \* 6)*** é o mesmo que **(5 < 7) and (20 == 20)** , que se tornará ***true and true***, e no final dará o resultado final ***true***.

## Números

### A) Tipos numéricos

```nim
var
  # Com sinal - int ~ int8 ~ int16 ~ int32 ~ int64
  iNum1: int      = 0
  iNum2: int8     = -128
  iNum3: int16    = 12314
  iNum4: int32    = -124
  iNum5: int64    = 1235
  # Sem sinal - uint ~ uint8 ~ uint16 ~ uint32 ~ uint64
  uNum6:  uint    = 0
  uNum7:  uint8   = 128
  uNum8:  uint16  = 12314
  uNum9:  uint32  = 124
  uNum10: uint64  = 1235
  # Floats - float ~ float32 ~ float64 (Possui somente estes tipos)
  fNum1: float   = 2.71
  fNum2: float32 = 3.1415
  fNum3: float64 = 14.352
```

### B) Int com Int

A relação entre as variações de maior potencia é direta:

`int` ↔ `int8` ↔ `int16` ↔ `int32` ↔ `int64`

> <img src="icons/sticky-notes01.png" width=48/> **Obs.:** Se <u>não</u> estiver lidando com embarcados opte por `int64` para evitar problemas com `overflow`.

### C) Int com Float

Operações entre `int` e `float` <u>não</u> são permitidas.

### D) Cast entre tipos

```nim
var
  # Float To Int
  floatToInt   = int(3.14)
  floatToInt8  = int8(1.41)
  floatToInt16 = int16(2.71)
  floatToInt32 = int32(2.23)
  floatToInt64 = int64(0.33)
  # Int To UInt
  intToUInt    = uint(364)
  intToUInt8   = uint8(132)
  intToUInt16  = uint16(877)
  intToUInt32  = uint32(32)
  intToUInt64  = uint64(64)
  # Int To Float
  intToFloat   = float(53)
  intToFloat32 = float32(42)
  intToFloat64 = float64(75)
```

### Strings

Lida da mesma forma como em outras linguagens (exceto em `C` - onde <u>não</u> há `string`).

> <img src="icons/sticky-notes01.png" width=48/> **Obs.:**
>
> - `$` converte outros tipos para `string`;
> - `&` concatena `strings`.
>
> **Exemplos:**
>
> ```nim
> var
>   letra: char = 'a'
>   palavra: string = "Olá"
>   intToStr: string = $132
>   robos = "R2D2" & " e " & "C3PO"
> ```

## Controle de fluxo

Até agora, em nossos programas, cada linha de código foi executada em algum ponto. As declarações de fluxo de controle nos permitem ter partes de código que serão executadas apenas se alguma condição booleana for satisfeita.

Se pensarmos em nosso programa como uma estrada, podemos pensar no fluxo de controle como vários ramos, e escolhemos nosso caminho dependendo de alguma condição. Por exemplo, compraremos ovos apenas se seu preço for inferior a algum valor. Ou, se estiver chovendo, levaremos um guarda-chuva, caso contrário (***else***) levaremos óculos escuros.

Exemplo:

```nim
let
    precoOvos = 10
    precoEsperado = 15
    estaChovendo = false

if precoOvos < precoEsperado:
  echo "compre ovos" # --> saída: compre ovos

if estaChovendo:
  echo "traga quarda-chuva"
else:
  echo "traga óculos escuros" # --> saída: traga óculos escuros
```

### Declaração If

Uma instrução ***if***, conforme mostrado acima, é a maneira mais simples de ramificar nosso programa.

A sintaxe Nim para escrever a instrução ***if*** é:

```nim
# A condição deve ser do tipo booleano: uma variável
# booleana ou uma expressão relacional `and` / `or`
# lógico.
if <condicao>:
    # Todas as linhas após a linha `if` que são
    # indentadas com dois espaços formam o mesmo
    # bloco e serão executadas apenas se a condição
    # for verdadeira.
	<bloco identado>
```

As instruções ***if*** podem ser aninhadas, ou seja, dentro de um bloco ***if***, pode haver outra instrução ***if***.

***if.nim***

```nim
let
  a = 11
  b = 22
  c = 999

if a < b: # true
  echo "`a` é menor que `b`"
  if 10*a < b: # false  
    echo "não só isso, `a` é *muito* menor que `b`"

if b < c: # true
  echo "`b` é menor que `c`"
  if 10*b < c: # true  
    echo "não só isso, `b` é *muito* menor que `c`"

if a+b > c: # false     
  echo "a e b são maiores que c"
  if 1 < 100 and 321 > 123: # false  
    echo "você sabia que 1 é menor que 100?"
    echo "e 321 é maior que 123! uau!"

# --> saída:
# `a` é menor que `b`
# `b` é menor que `c`
# não só isso, `b` é *muito* menor que `c`
```

### Else

***Else*** segue após um bloco ***if*** e nos permite ter uma ramificação do código que será executada quando a condição na instrução ***if*** não for verdadeira.

***else.nim***

```nim
let
  d = 63
  e = 2.718

if d < 10:
  echo "`d` é um número pequeno"
else:
  echo "`d` é número grande"

if e < 10:
  echo "`e` é um número pequeno"
else:
  echo "`e` é um número grande"

# --> saída:
# `d` é número grande
# `e` é um número pequeno
```

> Se você deseja executar um bloco apenas se a declaração for falsa, você pode simplesmente negar a condição com o operador ***not***.

### Elif

***Elif*** é a abreviatura de ***'else if'*** e nos permite encadear várias instruções ***if***.

O programa testa cada afirmação até encontrar uma que seja verdadeira. Depois disso, todas as outras instruções são ignoradas.

***elif.nim***

```nim
let
  f = 3456
  g = 7

if f < 10:
  echo "`f` é menor que 10"
elif f < 100:
  echo "`f` está entre 10 e 100"
elif f < 1000:
  echo "`f` esta entre 100 e 1000"
else:
  echo "`f` é maior que 1000"

if g < 1000:
  echo "`g` é menor que 1000"
elif g < 100:
  echo "`g` é menor que 100"
elif g < 10:
  echo "`g` é menor que 10"

# --> saída:
# `f` é maior que 1000
# `g` é menor que 1000
```

> No caso de ***g***, embora ***g*** satisfaça todas as três condições, apenas o primeiro ramo é executado, pulando automaticamente todos os outros ramos.

## Case

Uma instrução ***case*** é outra maneira de escolher apenas um dos vários caminhos possíveis, semelhante à instrução ***if*** com vários ***elifs***. Uma declaração de caso, no entanto, não leva várias condições booleanas, mas sim qualquer valor com estados distintos e um caminho para cada valor possível.

Código escrito em bloco ***if-elif*** parecido com este:

```nim
if x == 5:
  echo "Cinco!"
elif x == 7:
  echo "Sete!"
elif x == 10:
  echo "Dez!"
else:
  echo "Número desconhecido!"
```

Pode ser escrito com uma instrução ***case*** como esta:

```nim
case x
of 5:
  echo "Cinco!"
of 7:
  echo "Sete!"
of 10:
  echo "Dez!"
else:
  echo "Número desconhecido!"
```

Ao contrário da instrução ***if***, a instrução case deve cobrir todos os casos possíveis. Se alguém não estiver interessado em algum desses casos, então: descarte pode ser usado.

***case.nim***

```nim
let h = 'y'

case h
of 'x':
  echo "Você escolheu x"
of 'y':
  echo "Você escolheu y"
of 'z':
  echo "Você escolheu z"
else: discard # Mesmo que estejamos interessados em
              # apenas três valores de `h`, devemos
              # incluir esta linha para cobrir todos
              # os outros casos possíveis (todos os
              # outros caracteres). Sem ele, o código
              # não seria compilado.

# --> saída:
# Você escolheu y
```

Também podemos usar vários valores para cada ramificação se a mesma ação ocorrer para mais de um valor.

***multipleCase.nim***

```nim
let i = 7

case i
  of 0:
    echo "i é zero"
  of 1, 3, 5, 7, 9:
    echo "i é ímpar"
  of 2, 4, 6, 8:
    echo "i é par"
  else:
    echo "i é muito grande"

# --> saída:
# i é ímpar
```

## Loops

Os ***loops*** são outra construção de fluxo de controle que nos permite executar algumas partes do código várias vezes.

Neste capítulo, conheceremos dois tipos de ***loops***:

- ***loop for***: executa um número conhecido de vezes
- ***loop while***: execute enquanto alguma condição for satisfeita

### Loop for

### A sintaxe de um loop for é:

```nim
for <variavelDeLoop> in <interavel>:
  <corpoDoLoop>
```

Tradicionalmente, ***i*** é freqüentemente usado como um nome de uma variável de loop, mas qualquer outro nome pode ser usado. Essa variável estará disponível apenas dentro do loop. Assim que o loop terminar, o valor da variável é descartado.

O iterável é qualquer objeto por meio do qual possamos iterar. Dos tipos já mencionados, as ***strings*** são objetos iteráveis. (Mais tipos iteráveis serão introduzidos no próximo capítulo.)

Todas as linhas no corpo do ***loop*** são executadas em cada ***loop***, o que nos permite escrever com eficiência partes repetitivas do código.

Se quisermos iterar por meio de um intervalo de números (inteiros) em Nim, a sintaxe do iterável é ***início .. término***, onde início e término são números. Isso irá iterar por todos os números entre o início e o fim, incluindo o início e o fim. Para o intervalo padrão iterável, o início precisa ser menor do que o final.

Se quisermos iterar até um número (sem incluí-lo), podemos usar **`..<`**:

***for1.nim***

```nim
for n in 5 .. 9: # Iterando através de um intervalo de números usando `..`
                 # ambas as extremidades estão incluídas no intervalo.  
  echo n
# --> saída:
# 5
# 6
# 7
# 8
# 9

echo ""

for n in 5 ..< 9: # Iterando pelo mesmo intervalo usando `..<`
                  # itera até a extremidade superior, sem incluí-lo. 
  echo n
# --> saída:
# 5
# 6
# 7
# 8
```

Se quisermos iterar por meio de um intervalo de números com um tamanho de passo diferente de um, a contagem é usada. Com a contagem, definimos o valor inicial, o valor de parada (incluído no intervalo) e o tamanho do passo.

***for2.nim***

```nim
for n in countup(0, 16, 4): # Contando de zero a 16, com
                            # salto de quatro-em-quatro
                            # O final (16) esta incluído
                            # resultado.
  echo n
# --> saída:
# 0
# 4
# 8
# 12
# 16
```

Para iterar por um intervalo de números onde o início é maior do que o final, uma função semelhante chamada contagem regressiva é usada. Mesmo que estejamos em contagem regressiva, o tamanho do passo deve ser positivo.

***for2.nim***

```nim
for n in countdown(4, 0): # Para iterar de um número superior
                          # para um número inferior, devemos
                          # usar a contagem regressiva (o
                          # perador `..` só pode ser usado
                          # quando o valor inicial é menor que
                          # o valor final).
  echo n
# --> saída:
# 4
# 3
# 2
# 1
# 0

echo ""

for n in countdown(-3, -9, 2): # Mesmo durante a contagem regressiva, o
                               # tamanho do passo deve ser um número
                               # possitvo.
  echo n
# --> saída:
# -3
# -5
# -7
# -9
```

Como a ***string*** é iterável, podemos usar um loop ***for*** para iterar por meio de cada caractere da ***string*** (esse tipo de iteração às vezes é chamado de loop ***for-each***).

***for3.nim***

```nim
let palavra = "alfabeto"

for letra in palavra:
  echo letra
# --> saída:
# a
# l
# f
# a
# b
# e
# t
# o
```

Se também precisarmos ter um contador de iteração (começando do zero), podemos conseguir isso usando a syntax  ***for \<counterVariable\>, \<loopVariable\> in \<iterator\>:***. Isso é muito prático se você deseja iterar por meio de um iterável e, simultaneamente, acessar outro iterável no mesmo deslocamento.

***for3.nim***

```nim
let palavra = "alfabeto"
for indice, letra in palavra:
  echo "A letra ", indice, " é: ", letra
# --> saída:
# A letra 0 é: a
# A letra 1 é: l
# A letra 2 é: f
# A letra 3 é: a
# A letra 4 é: b
# A letra 5 é: e
# A letra 6 é: t
# A letra 7 é: o
```

## Loop while

 Os loops ***while*** são semelhantes às instruções ***if***, mas eles continuam executando seu bloco de código enquanto a condição permanecer verdadeira. Eles são usados quando não sabemos com antecedência quantas vezes o loop será executado.

Devemos ter certeza de que o loop terminará em algum ponto e não se tornará um loop infinito.

***while.nim***

```nim
var a = 1

# Esta condição será verificada todas as vezes
# antes de entrar no novo `loop` e executar o
# código dentro dele.
while a*a < 10: 
  echo "`a` é: ", a
  inc a # Incrementa `a` em uma unidade.

# --> saída:
# `a` é: 1
# `a` é: 2
# `a` é: 3

echo "O valor final de `a`: ", a
# --> saída:
# O valor final de `a`: 4
```

## Break e continue

A instrução break é usada para sair prematuramente de um loop, geralmente se alguma condição for atendida.

No próximo exemplo, se não houvesse uma instrução ***if*** com ***break***, o loop continuaria a ser executado e impresso até que ***i*** se tornasse 1000. Com a instrução ***break***, quando ***i*** chegar a 3, saímos imediatamente do loop (antes de imprimir o valor de ***i***).

***break.nim***

```nim
var i = 1

while i < 1000:
  if i == 3:
    break
  echo i
  inc i
# --> saída:
# 1
# 2
```

A instrução continue inicia a próxima iteração de um loop imediatamente, sem executar as linhas restantes da iteração atual. Observe como 3 e 6 estão faltando na saída do seguinte código:

***continue.nim***

```nim
for i in 1 .. 8:
  if (i == 3) or (i == 6):
    continue
  echo i
# --> saída:
# 1
# 2
# 4
# 5
# 7
# 8
```

## Containers

Os contêineres são tipos de dados que contêm uma coleção de itens e nos permitem acessar esses elementos. Normalmente, um contêiner também é iterável, o que significa que podemos usá-los da mesma forma que usamos strings no capítulo de loops.

Por exemplo, uma lista de compras é um contêiner de itens que queremos comprar, e uma lista de números primos é um contêiner de números. Escrito em pseudocódigo:

```nim
listaCompras = [presunto, ovos, pão, maçãs]
primos = [1, 2, 3, 5, 7]
```

### Arrays

Uma matriz é o tipo de contêiner mais simples. As matrizes são homogêneas, ou seja, todos os elementos em uma matriz devem ter o mesmo tipo. Os arrays também têm um tamanho constante, o que significa que a quantidade de elementos (ou melhor: a quantidade de elementos possíveis) deve ser conhecida em tempo de compilação. Isso significa que chamamos arrays de 'contêiner homogêneo de comprimento constante'.

O tipo de array é declarado usando array [comprimento, tipo], onde comprimento é a capacidade total do array (número de elementos que pode caber) e tipo é um tipo de todos os seus elementos. A declaração pode ser omitida se o comprimento e o tipo puderem ser inferidos dos elementos passados.

Os elementos de uma matriz são colocados entre colchetes.

```nim
var
  a: array[3, int] = [5, 7, 9] # Declarando `array` com comprimento,
                               # tipo e valores.
    
  b = [5, 7, 9] # A variável `a` acima embora esteja correto a forma
                # como foi declarado não há necessidade de especificar
                # comprimento e tipo quando se tem os valores.
        
  c = []  # error - Comprimento e tipo não podem ser inferidos.
        
  d: array[7, string] # Maneira correta de declarar um `array`
                      # vazio fornecendo o comprimento e tipo.
```

Uma vez que o comprimento de uma matriz deve ser conhecido em tempo de compilação, isso não funcionará:

```nim
const m = 3
let n = 5

var a: array[m, char]
var b: array[n, char] # error - Comprimento `n` não pode ser
                      # conhecido em tempo de compilação.
```

### Sequences

Sequências são contêineres semelhantes a arrays, mas seu comprimento não precisa ser conhecido no tempo de compilação e pode mudar durante o tempo de execução: declaramos apenas o tipo dos elementos contidos com seq [type]. As sequências também são homogêneas, ou seja, cada elemento em uma sequência deve ser do mesmo tipo.

Os elementos de uma sequência são colocados entre ***@ [*** e ***]***.

```nim
var
  e1: seq[int] = @[] # O tipo de uma sequência vazia deve
                     # ser declarado.
    
  f = @["abc", "def"] # O tipo de sequência não vazia pode
                      # ser inferido. Nesse caso, é uma
                      # sequência contendo `strings`.
```

Outra maneira de inicializar uma sequência vazia é chamar o procedimento ***newSeq***. Veremos mais chamadas de procedimento no próximo capítulo, mas por enquanto apenas saiba que esta também é uma possibilidade:

```nim
var
  e = newSeq[int]() # Fornecer o parâmetro de tipo entre
                    # colchetes permite que o procedimento
                    # saiba que deve retornar uma sequência
                    # de um determinado tipo.
                    # Um erro frequente é a omissão do
                    # final `()`, que deve ser incluído.
```

***seq.nim***

```nim
var
  g = @['x', 'y']
  h = @['1', '2', '3']

g.add('z') # Adicionando um novo elemento
           # do mesmo tipo (char).  
echo g # --> saída: @['x', 'y', 'z']

h.add(g) # Adicionando outra sequência
         # contendo o mesmo tipo.    
echo h # --> saída: @['1', '2', '3', 'x', 'y', 'z']
```

Tentar passar tipos diferentes para as sequências existentes produzirá um erro:

```nim
var i = @[9, 8, 7]

i.add(9.81) # error - Tentando adicionar um `float`
            # a uma sequência de `int`.

g.add(i)    # error - Tentando adicionar uma sequência
            # de `int` a uma sequência de `char`.
```

Como as sequências podem variar em comprimento, precisamos encontrar uma maneira de obter seu comprimento, para isso podemos usar a função ***len***.

```nim
var i = @[9, 8, 7]
echo i.len # --> saída: 3

i.add(6)
echo i.len # --> saída: 4
```

## Indexar e fatiar (Indexing and slicing)

A indexação nos permite obter um elemento específico de um contêiner por meio de seu índice. Pense no índice como uma posição dentro do contêiner.

Nim, como muitas outras linguagens de programação, tem indexação baseada em zero, o que significa que o primeiro elemento em um contêiner tem o índice zero, o segundo elemento tem o índice um, etc.

Se quisermos indexar "para trás", isso é feito usando o prefixo **`^`**. O último elemento (primeiro na parte de trás) tem o índice **`^ 1`**.

A sintaxe para indexação é ***\<container\>[\<index\>]***.

***indexing.nim***

```nim
let j = ['a', 'b', 'c', 'd', 'e']

# Indexação baseada em zero: o elemento no índice 1 é b.
echo j[1] # --> saída: b

# Obtendo o último elemento.
echo j[^1] # --> saída: e
```

O fatiamento nos permite obter uma série de elementos com uma chamada. Ele usa a mesma sintaxe dos intervalos (introduzidos na seção de ***loop for***).

Se usarmos a sintaxe ***start .. stop***, ambas as extremidades serão incluídas na fatia. Usando a sintaxe ***start ..< stop***, o índice de parada não é incluído na fatia.

A sintaxe para fatiar é ***\<container\>[\<start\> .. \<stop\>]***.

***indexing.nim***

```nim
echo j[0 .. 3] # --> saída: @[a, b, c, d]
echo j[0 ..< 3] # --> saída: @[a, b, c]
```

Tanto a indexação quanto o fracionamento podem ser usados para atribuir novos valores aos contêineres e strings mutáveis existentes.

***assign.nim***

```nim
var
  k: array[5, int]
  l = @['p', 'w', 'r']
  m = "Tom and Jerry"

for i in 0 .. 4: # A matriz de comprimento 5 possui índices
                 # de zero a quatro.  
  k[i] = 7 * i # Iremos atribuir um valor a
               # cada elemento do array.

echo k # --> saída: [0, 7, 14, 21, 28]

l[1] = 'q' # Atribuir (alterar) o segundo elemento
           # (índice 1) de uma sequência.

echo l # @['p', 'q', 'r']

m[8 .. 9] = "Ba" # Alterar caracteres de uma `string`
                 # nos índices 8 e 9.

                  # 0123456789012
echo m # --> saída: Tom and Barry
```

## Tuplas

Ambos os contêineres que vimos até agora são homogêneos. Por outro lado, as <strong style="text-decoration: underline; font-style: italic;">tuplas contêm dados heterogêneos</strong>, ou seja, os elementos de uma tupla podem ser de tipos diferentes. <strong style="text-decoration: underline; font-style: italic;">Da mesma forma que as matrizes (arrays), as tuplas têm tamanho fixo</strong>.

Os elementos de uma <strong style="text-decoration: underline; font-style: italic;">tupla são colocados entre parênteses</strong>.

***tuples.nim***

```nim
let n = ("Banana", 2, 'c') # As tuplas podem conter campos de diferentes
                           # tipos. Neste caso: `string`, `int` e `char`.

echo n # --> saída: ("Banana", 2, 'c')
```

Também podemos nomear cada campo em uma tupla para distingui-los. Isso pode ser usado para acessar os elementos da tupla, em vez de indexar.

***tuples.nim***

```nim
var o = (nome: "Banana", peso: "2Kg", preço: 15)

o[1] = "1Kg" # Alterar o valor de um campo
             # usando o índice do campo.

o.nome = "Maçã" # Alterar o valor de um campo
                # usando o nome do campo.

echo o # --> saída: (nome: "Maçã", peso: "1Kg", preço: 15)
```

## Procedures

Os procedimentos, ou funções como são chamados em outras linguagens de programação, são partes do código que executam uma tarefa específica, empacotados como uma unidade. A vantagem de agrupar o código assim é que podemos chamar esses procedimentos em vez de escrever todo o código novamente quando quisermos usar o código do procedimento.

Em alguns dos capítulos anteriores, vimos a conjectura de Collatz em vários cenários diferentes. Envolvendo a lógica da conjectura de Collatz em um procedimento, poderíamos ter chamado o mesmo código para todos os exercícios.

Até agora, usamos muitos procedimentos integrados, como ***echo*** para impressão, ***add*** para adicionar elementos a uma sequência, ***inc*** para aumentar o valor de um inteiro, ***len*** para obter o comprimento de um contêiner, etc. Agora veremos Como criar e usar nossos próprios procedimentos.

Algumas das vantagens de usar procedimentos são:

- Reduzindo a duplicação de código
- Mais fácil de ler o código, pois podemos nomear as peças pelo que fazem
- Decompor uma tarefa complexa em etapas mais simples

Conforme mencionado no início desta seção, os procedimentos são freqüentemente chamados de funções em outras linguagens. Na verdade, esse é um nome um tanto impróprio se considerarmos a definição matemática de uma função. As funções matemáticas recebem um conjunto de argumentos (como f (x, y), onde f é uma função e x e y são seus argumentos) e sempre retornam a mesma resposta para a mesma entrada.

Os procedimentos programáticos, por outro lado, nem sempre retornam a mesma saída para uma determinada entrada. Às vezes, eles não devolvem nada. Isso ocorre porque nossos programas de computador podem armazenar estados nas variáveis mencionadas anteriormente, que os procedimentos podem ler e alterar. No Nim, a palavra ***func*** é atualmente reservada para ser usada como o tipo de função mais matematicamente correto, sem forçar efeitos colaterais.

### Declarando um procedure

Antes de podermos usar (chamar) nosso procedimento, precisamos criá-lo e definir o que ele faz.

Um procedimento é declarado usando a palavra-chave ***proc*** e o nome do procedimento, seguido pelos parâmetros de entrada e seu tipo entre parênteses, e a última parte são dois pontos e o tipo do valor retornado de um procedimento, como este:

```nim
proc <name>(<p1>: <type1>, <p2>: <type2>, ...): <returnType>
```

O corpo de um procedimento é escrito no bloco recuado seguindo a declaração anexada com um sinal **=**.

***callProcs.nim***

```nim
proc encontreOhMaior(x: int, y: int): int = # Procedimento de declaração
                                            # denominado `encontreOhMaior`, que
                                            # possui dois parâmetros, `x` e `y`,
                                            # e retorna um tipo `int`.
  if x > y:
    return x # Para retornar um valor de um procedimento,
             # usamos a palavra-chave return.  
  else:
    return y
  # isso está dentro do procedimento
# isso está fora do procedimento
```

```nim
# O procedimento `ecoaClassificacaoLinguagem` apenas ecoa o nome fornecido,
# não retorna nada, então o tipo de retorno não é declarado.
proc ecoaClassificacaoLinguagem(linguagem: string) =
  case linguagem
  of "Nim", "nim", "NIM":
    echo linguagem, " é a melhor linguagem!"
  else:
    echo linguagem, " pode ser uma segunda melhor linguagem."

ecoaClassificacaoLinguagem("Nim") # --> saída: Nim é a melhor linguagem!
```

Normalmente não temos permissão para alterar nenhum dos parâmetros que recebemos. Fazer algo assim gerará um erro:

```nim
proc alterarArgumento(argumento: int) =
  argumento += 5

var nossaVariavel = 10
alterarArgumento(nossaVariavel)
```

Para que isso funcione, precisamos permitir que Nim e o programador usando nosso procedimento alterem o argumento declarando-o como uma variável:

```nim
# Observe como o argumento agora é declarado
# como `var int` e não apenas como `int`.
proc alterarArgumento(argumento: var int) =
  argumento += 5

var nossaVariavel = 10
alterarArgumento(nossaVariavel)
echo nossaVariavel # --> saída: 15
alterarArgumento(nossaVariavel)
echo nossaVariavel # --> saída: 20
```

Isso, é claro, significa que o nome que passamos deve ser declarado também como uma variável, <strong style="text-decoration: underline; font-style: italic;">passando algo atribuído como `const` ou `let` gerará um erro</strong>.

Embora seja uma boa prática passar as coisas como argumentos, também é possível usar nomes declarados fora do procedimento, tanto variáveis quanto constantes:

```nim
var x = 100

proc echoX() =
  echo x # Aqui, acessamos a variável externa `x`.
  x += 1 # Também podemos atualizar seu valor, uma
         # vez que é declarado como uma variável.

echoX() # --> saída: 100
echoX() # --> saída: 101
```

### Chamando os procedimentos

Depois de declarar um procedimento, podemos chamá-lo. A maneira usual de chamar procedimentos / funções em muitas linguagens de programação é declarar seu nome e fornecer os argumentos entre parênteses, como este:

```nim
<procName>(<arg1>, <arg2>, ...)
```

O resultado da chamada de um procedimento pode ser armazenado em uma variável.

Se quisermos chamar nosso procedimento ***encontrarOhMaior*** do exemplo acima e salvar o valor de retorno em uma variável, podemos fazer isso com:

***callProcs.nim***

```nim
let
  a = encontrarOhMaior(987, 789)
  b = encontrarOhMaior(123, 321)
  c = encontrarOhMaior(a, b) # O resultado da função `encontrarOhMaior`
                             # é aqui denominado `c`, e é chamado com
                             # os resultados de nossas duas primeiras
                             # chamadas `encontrarOhMaior (987, 321)`.  

echo a # --> saída: 987
echo b # --> saída: 321
echo c # --> saída: 987
```

O Nim, ao contrário de muitas outras linguagens, também oferece suporte à Sintaxe de Chamada de Função Uniforme, que permite muitas maneiras diferentes de chamar procedimentos.

Esta é uma chamada em que o primeiro argumento é escrito antes do nome da função e o resto dos parâmetros são declarados entre parênteses:

```nim
<arg1>.<procName>(<arg2>, ...)
```

Usamos essa sintaxe quando adicionamos elementos a uma sequência existente (**`<seq>.add(<element>)`**), pois isso a torna mais legível e expressa nossa intenção de maneira mais clara do que escrever **`add(<seq>, <element>)`**. Também podemos omitir os parênteses em torno dos argumentos:

```nim
<procName> <arg1>, <arg2>, ...
```

Vimos esse estilo sendo usado quando chamamos o procedimento ***echo*** e ao chamar o procedimento ***len*** sem nenhum argumento. Esses dois também podem ser combinados dessa forma, mas essa sintaxe, entretanto, não é vista com muita frequência:

```nim
<arg1>.<procName> <arg2>, <arg3>, ...
```

A sintaxe de chamada uniforme permite um encadeamento mais legível de vários procedimentos:

***ufcs.nim***

```nim
# Se vários parâmetros forem do mesmo tipo, podemos
# declarar seu tipo desta forma compacta.
proc soma(x, y: int): int =  
  return x + y

proc produto(x, y: int): int =
  return x * y

let
  a = 2
  b = 3
  c = 4

echo a.soma(b) == soma(a, b) # --> saída: true
echo c.produto(a) == produto(c, a) # --> saída: true

# Primeiro adicionamos `a` e `b`, então o resultado dessa
# operação `2 + 3 = 5` é passado como o primeiro parâmetro
# para o procedimento `produto`, onde é multiplicado por c
# `5 * 4 = 20`.
echo a.soma(b).produto(c) # --> saída: 20

# Primeiro, multiplicamos `c` e `b`, então o resultado
# dessa operação `4 * 3 = 12` é passado como o primeiro
# parâmetro para o procedimento `soma`, onde é adicionado
# com `a` `12 + 2 = 14`.
echo c.produto(b).soma(a) # --> saída: 14
```

### Variável de resultado

No Nim, todo procedimento que retorna um valor tem uma <strong style="text-decoration: underline; font-style: italic;">variável de resultado declarada implicitamente e inicializada (com um valor padrão)</strong>. O procedimento retornará o valor desta variável de resultado quando atingir o final de seu bloco indentado, mesmo sem declaração de retorno.

***result.nim***

```nim
# O tipo de retorno é `int`. A variável `result`
# é inicializada com o valor padrão para `int: 0`.  
proc encontreOhMaior(a: seq[int]): int =
  for numero in a:
    if numero > result:
      result = numero
  # fim do procedimento.
  # Quando o final do procedimento é alcançado,
  # o valor do `result` é retornado.

let d = @[3, -5, 11, 33, 7, -15]
echo encontreOhMaior(d) # --> saída: 33
```

Observe que este procedimento serve para demonstrar a variável ***result***, e não está 100% correto: se você passasse uma sequência contendo apenas números negativos, este procedimento retornaria ***0*** (que não está contido na sequência).

> ***Cuidado!*** Em versões mais antigas do Nim (antes do ***Nim 0.19.0***), o valor padrão de ***strings*** e ***sequências*** era nulo, e quando íamos usá-los como tipos de retorno, a variável ***result*** precisaria ser inicializada como uma ***string*** vazia (***""***) ou como uma sequência vazia (***@ []***).

***result.nim***

```nim
proc manterOsImpares(a: seq[int]): seq[int] =
  # Na versão `Nim 0.19.0` e mais recente, esta linha
  # não é necessária as sequências são inicializadas
  # automaticamente como sequências vazias:
  # result = @[]
  for numero in a:
    if numero mod 2 == 1:
      result.add(numero)


let f = @[1, 6, 4, 43, 57, 34, 98]
echo manterOsImpares(f) # --> saída: @[1, 43, 57]
```

> **Obs.:** Em versões mais antigas do Nim, as sequências devem ser inicializadas e, sem essa esta instrução ***result = @[]***, o compilador geraria um erro. (Observe que ***var*** não deve ser usado, pois o resultado já está declarado implicitamente.)

Dentro de um procedimento, também podemos chamar outros procedimentos.

***filterOdds.nim***

```nim
proc ehDivisivelPor3(x: int): bool =
  return x mod 3 == 0

proc filtraMultiplosDe3(a: seq[int]): seq[int] =
  # result = @[] # Mais uma vez, esta linha não é necessária
                 # nas versões mais recentes do Nim.
  for i in a:
    if i.ehDivisivelPor3(): # Chamando o procedimento declarado anteriormente.
                            # Seu tipo de retorno é `bool` e pode ser usado na
                            # instrução `if`.
      result.add(i)

let
  g = @[2, 6, 5, 7, 9, 0, 5, 3]
  h = @[5, 4, 3, 2, 1]
  i = @[626, 45390, 3219, 4210, 4126]

echo filtraMultiplosDe3(g)
echo h.filtraMultiplosDe3()
echo filtraMultiplosDe3 i # A terceira forma de chamar um procedimento,
                          # como vimos acima (sem uso de parentêsis).

# --> saídas:
# @[6, 9, 0, 3]
# @[3]
# @[45390, 3219]
```

### Declaração de encaminhamento

Conforme mencionado no início desta seção, podemos declarar um procedimento sem um bloco de código. A razão para isso é que temos que declarar os procedimentos antes de podermos chamá-los, fazer isso não funcionará:

```nim
echo 5.soma(10) # error - Isso gerará um erro, pois
                # o procedimento `soma` ainda não
                # foi definido.      

proc soma(x, y: int): int = # Aqui nós definimos `soma`,
                            # mas como é depois de usá-lo,
                            # Nim ainda não sabe sobre isso.  
  return x + y
```

A maneira de contornar isso é chamada de declaração de encaminhamento:

```nim
proc soma(x, y: int): int # Aqui, dizemos a Nim que ele deve
                          # considerar que o procedimento
                          # `soma` existe com essa definição.   

echo 5.soma(10) # Agora estamos livres para usá-lo em nosso
                # código, isso vai funcionar.              

proc soma(x, y: int): int = # Isso é onde o `soma` é realmente
                            # implementado, isso deve corresponder
                            # à nossa definição anterior.  
  return x + y
```

## Módulos

Até agora, usamos a funcionalidade que está disponível por padrão sempre que iniciamos um novo arquivo Nim. Isso pode ser estendido com módulos, que fornecem mais funcionalidade para algum tópico específico.

Alguns dos módulos Nim mais usados são:

- ***strutils***: funcionalidade adicional ao lidar com strings
- ***sequtils***: funcionalidade adicional para sequências
- ***math***: funções matemáticas (logaritmos, raízes quadradas, ...), trigonometria (sen, cos, ...)
- ***times***: medir e lidar com o tempo

Mas há muitos mais, tanto na chamada biblioteca padrão quanto no gerenciador de pacotes ***nimble***.

### Importando um módulo

Se quisermos importar um módulo e todas as suas funcionalidades, basta colocar ***import \<nomeDoModulo\>*** em nosso arquivo. Isso geralmente é feito na parte superior do arquivo para que possamos ver facilmente o que nosso código usa.

***stringutils.nim***

```nim
import strutils # Importando `strutils`

let
  a = "Minha string com espaços em branco."
  b = '!'

echo a.split() # Usando o módulo `split` de `strutils`. Ele divide
               # a `string` em uma sequência de palavras.

echo a.toUpperAscii() # `toUpperAscii` converte todas as letras
                      # `ASCII` em maiúsculas.

echo b.repeat(5) # `repeat` também é do módulo `strutils` e repete
                 # um caractere ou uma `string` inteira a quantidade
                 # de vezes solicitada.

# --> saídas:
# @["Minha", "string", "com", "espaços", "em", "branco."]
# MINHA STRING COM ESPAçOS EM BRANCO.
# !!!!!
```

> Para os usuários vindos de outras linguagens de programação (especialmente Python), a maneira como as importações funcionam no Nim pode parecer 'errada'. Se for esse o caso, [esta](https://narimiran.github.io/2019/07/01/nim-import.html) é a leitura recomendada.

***maths.nim***

```nim
import math # Importando `math`.

let
  c = 30.0 # graus
  cRadians = c.degToRad() # Convertendo graus em radianos com `degToRad`.

echo("30° em radianos = ", cRadians)
echo("Seno de 30° = ", sin(cRadians).round(2)) # O seno recebe radianos.
                                               # Arredondamos (também
                                               # a partir do módulo de `math`)
                                               # o resultado para no máximo 2ªs
                                               # casas decimais. (Caso contrário,
                                               # o resultado seria:
                                               # `0.4999999999999999`).

echo("2⁵ = ", 2^5) # O módulo `math` também possui o operador `^`
                   # para calcular as potências de um número.

# --> saídas:
# 30° em radianos = 0.5235987755982988
# Seno de 30° = 0.5
# 2⁵ = 32
```

### Criando o nosso próprio

Frequentemente, temos tanto código em um projeto que faz sentido dividi-lo em partes para que cada um faça uma determinada coisa. Se você criar dois arquivos lado a lado em uma pasta, vamos chamá-los de ***primeiroArquivo.nim*** e ***segundoArquivo.nim***, você pode importar um do outro como um módulo:

***primeiroArquivo.nim***

```nim
proc soma*(a, b: int): int = # Observe como o procedimento de `soma`
                             # agora tem um asterisco (*) após seu
                             # nome, isso informa ao Nim que outro
                             # arquivo importando este poderá usar
                             # este procedimento. 
  return a + b

proc diferenca(a, b: int): int = # Por outro lado, isso não estará
                                 # visível ao importar este arquivo. 
  return a - b
```

***segundoArquivo.nim***

```nim
import primeiroArquivo # Aqui, importamos `primeiroArquivo.nim`. Não precisamos
                       # colocar a extensão `.nim` aqui.          

echo soma(5, 10) # Isso funcionará bem e produzirá `15`, conforme
                 # declarado no `primeiroArquivo` e visível para nós.

echo diferenca(10, 5) # error - No entanto, isso gerará um erro,
                      # pois o procedimento `diferenca` não é visível,
                      # pois não tem um asterisco seguido ao seu
                      # nome em `primeiroArquivo.nim`.
```

Caso você tenha mais do que esses dois arquivos, convém organizá-los em um subdiretório (ou mais de um subdiretório). Com a seguinte estrutura de diretório:

```
.
├── meuOutroSubdiretorio
│   ├── quintoArquivo.nim
│   └── quartoArquivo.nim
├── meuSubdiretorio
│   └── terceiroArquivo.nim
├── primeiroArquivo.nim
└── segundoArquivo.nim
```

Se você quiser importar todos os outros arquivos em seu ***segundoArquivo.nim***, faça o seguinte:

***segundoArquivo.nim***

```nim
import primeiroArquivo
import meuSubdiretorio/terceiroArquivo
import meuOutroSubdiretorio/[quartoArquivo, quintoArquivo]
```

## Interagindo com a entrada do usuário

Usar as coisas que apresentamos até agora (tipos de dados básicos e contêineres, fluxo de controle, loops) nos permite fazer alguns programas simples.

Neste capítulo, aprenderemos como tornar nossos programas mais interativos. Para isso, precisamos de uma opção para ler os dados de um arquivo ou solicitar uma entrada do usuário.

### Lendo de um arquivo

Digamos que temos um arquivo de texto chamado ***pessoas.txt*** no mesmo diretório que nosso código Nim. O conteúdo desse arquivo é parecido com este:

***pessoas.txt***

```
Alice A.
Bob B.
Carol C.
```

***lendoDoArquivo.nim***

```nim
import strutils

let conteudo = readFile("pessoas.txt") # Para ler o conteúdo de um arquivo,
                                       # usamos o procedimento `readFile`
                                       # e fornecemos um caminho para o
                                       # arquivo a ser lido (se o arquivo
                                       # estiver no mesmo diretório de nosso
                                       # programa Nim, basta fornecer um nome
                                       # de arquivo). O resultado é uma `string`
                                       # multilinha. 
echo conteudo
# --> saída:
# Alice A.
# Bob B.
# Carol C.
# `\n`      # Havia uma nova linha final (última linha vazia)
            # no arquivo original, que também está presente aqui.

let pessoa = conteudo.splitLines() # Para dividir uma `string` multilinha em uma
                                   # sequência de `strings` (cada string contém
                                   # todo o conteúdo de uma única linha), usamos
                                   # `splitLines` do módulo `strutils`.
echo pessoa
# --> saída:
# @["Alice A.", "Bob B.", "Carol C.", ""] # Por causa da nova linha final,
                                          # nossa sequência é mais longa
                                          # do que esperávamos.
```

Para resolver o problema de uma nova linha final, podemos usar o procedimento de ***strip()*** de ***strutils*** depois de ler um arquivo. Tudo o que isso faz é remover os chamados espaços em branco do início e do final de nossa string. O espaço em branco é simplesmente qualquer caractere que crie algum espaço, novas linhas, espaços, tabulações, etc.

***lendoDoArquivo2.nim***

```nim
import strutils

let conteudo = readFile("pessoas.txt").strip() # O uso de `strip()`
                                               # fornece os resultados
                                               # esperados. 
echo conteudo

let pessoa = conteudo.splitLines()
echo pessoa

# --> saídas:
# Alice A.
# Bob B.
# Carol C.
# @["Alice A.", "Bob B.", "Carol C."]
```

### Lendo a entrada do usuário

Se quisermos interagir com um usuário, devemos ser capazes de pedir-lhe uma entrada e, em seguida, processá-la e usá-la. Precisamos ler a entrada padrão (***stdin***) passando ***stdin*** para o procedimento ***readLine***.

***interacao1.nim***

```nim
echo "Por favor, insira seu nome:"
let nome = readLine(stdin) # O tipo de `nome` é considerado
                           # uma `string`. 

echo "Olá ", nome, ", prazer em conhecê-la!"

# --> saídas:
# Por favor, insira seu nome:
# Alice
# --> Após precionar <enter>:
# Olá Alice, prazer em conhecê-la!
```

### Lidando com números

A leitura de um arquivo ou de uma entrada do usuário sempre fornece uma ***string*** como resultado. Se quisermos usar números, precisamos converter ***strings*** em números: novamente usamos o módulo ***strutils*** e ***parseInt*** para converter para ***int*** ou ***parseFloat*** para converter em um ***float***.

***interacao2.nim***

```nim
import strutils

echo "Por favor insira o seu ano de nascimento:"
let anoDeNascimento = readLine(stdin).parseInt() # Converta uma `string` em um inteiro.
                                                 # Quando escrito assim, confiamos em
                                                 # nosso usuário para fornecer um número
                                                 # inteiro válido. O que aconteceria se
                                                 # um usuário inserisse `'79 ou noventa
                                                 # e três?` Tente você mesmo. 

let idade = 2018 - anoDeNascimento

echo "Você tem ", idade, " anos."

# --> saída:
# Por favor insira o seu ano de nascimento:
# 1934
# --> Após precionar <enter>:
# Você tem 84 anos.
```

Se tivermos o arquivo ***numbers.txt*** no mesmo diretório que nosso código Nim, com o seguinte conteúdo:

***numbers.txt***

```
27.3
98.24
11.93
33.67
55.01
```

E queremos ler esse arquivo e encontrar a soma e a média dos números fornecidos, podemos fazer algo assim:

***interacao3.nim***

```nim
import strutils, sequtils, math # Importamos vários módulos.
                                # `strutils` fornece `strip` e `splitLines`,
                                # `sequtils` fornece `map` e
                                # `math` fornece `sum`.
let
  strNums = readFile("numeros.txt").strip().splitLines() # Retiramos a nova
                                                         # linha final e
                                                         # dividimos as linhas
                                                         # para criar uma
                                                         # sequência de `strings`.
  nums = strNums.map(parseFloat) # `map` funciona aplicando um
                                 # procedimento (neste caso,
                                 # `parseFloat`) a cada membro
                                 # de um `contêiner`. Em outras
                                 # palavras, convertemos cada
                                 # `string` em um `float`,
                                 # retornando uma nova sequência
                                 # de `floats`.       

let
  somaNums = sum(nums) # Usando `sum` do módulo `math` para nos
                       # dar a soma de todos os elementos em uma
                       # sequência.
    
  media = somaNums / float(nums.len) # Precisamos converter o
                                     # comprimento de uma sequência
                                     # em `float`, porque `sumNums`
                                     # é um `float`.  

echo somaNums # --> saída: 226.15
echo media # --> saída: 45.23
```

## Entrada e saída de dados

### 1º - Imprimindo no terminal

```nim
# Com retorno de linha embutido (\n)
echo "Olá Mundo!"
# Para incluir um salto de linha inclua (\n)
stdout.write("Olá Mundo!\n")
# ou
write(stdout, "Olá Mundo!\n")
```

### 2º - Recebendo texto pelo terminal

```nim
stdout.write "Qual o seu nome: "
var
  nome = stdin.readline()
  sobrenome: string

write(stdout, "E o sobrenome: ")
sobrenome = readline(stdin)

echo "Bem vindo ao Nim ", nome & " " & sobrenome & "!"

# --> saída:
# Qual o seu nome: Fulano
# E o sobrenome: de Tal
# Bem vindo ao Nim Fulano de Tal!
```

### 3º - Receber números

```nim
# `strutils` para lidar com strings
# `math` para acessar funções matemáticas
import strutils, math

# Para operação de exponenciação
# os elementos devem ser float's
var
  base: float
  expoente: float

stdout.write("Entre com número: ")
base = stdin.readline().parseFloat()

stdout.write("Entre com expoente: ")
expoente = stdin.readline().parseFloat()

echo pow(base, expoente)

# --> saída:
# Entre com número: 2
# Entre com expoente: 5
# 32.0
```

### 4º Validando se entrada é um número

```nim
import times, strutils

let
  time = getTime()
  ano = time.format("yyyy").parseInt()

# Apenas para conhecimento:
#echo time.format("dd-MM-yyyy")  # --> 25-05-2024
#echo time.format("dd-MMM-yyyy") # --> 25-May-2024

# Validando se a entrada é um número
try:
  stdout.write("Qual sua idade?: ")
  let idade = stdin.readline().parseInt()
  echo "Seu ano de nascimento: ", ano - idade
except ValueError as e:
  echo "Erro: " & e.msg

# --> saída:
# Qual sua idade?: 55
# Seu ano de nascimento: 1969

# --> saída c/ exceção:
# Qual sua idade?: A
# Erro: invalid integer: A
```

> Refatorando usando regex e lançando nossa própria exceção:
>
> ```nim
> import times, strutils, re
> 
> let
>   time = getTime()
>   ano = time.format("yyyy").parseInt()
>   pattern = re"^\d+$" # Regex que verifica se a string contém apenas dígitos
> 
> # Função para verificar se a string contém apenas números
> proc isNumber(s: string): bool =
>   return s.len > 0 and s.match(pattern)
> 
> try:
>   stdout.write("Qual sua idade?: ")
>   let input = stdin.readLine()
> 
>   if isNumber(input):
>     let idade = input.parseInt()  # Agora é seguro converter para número
>     echo "Seu ano de nascimento: ", ano - idade
>   else:
>     raise newException(
>       ValueError,
>       "Você entrou com um valor inválido que não é um número: " & input
>     )
> 
> except ValueError as e:
>   echo "Erro: " & e.msg
> 
> # --> saída:
> # Qual sua idade?: 55
> # Seu ano de nascimento: 1969
> 
> # --> saída c/ exceção personalizada:
> # Qual sua idade?: A
> # Erro: Você entrou com um valor inválido que não é um número: A
> ```
>
> <img src="icons/location01.png" width=48/> Mais sobre regex no [Anexo II](#anexo-ii). <a name="ancora-ii"><img src="icons/ancora01.png" width=48/></a>

```nim
import strutils

# Procedimento de entrada de dados
# do terminal
proc input(label: string): string =
  stdout.write(label)
  stdin.readline()

# Loop infinito
while true:
  try:
    let qtde = input("Quantidade desejada em Kilos: ").parseInt()
    if qtde < 0:
      echo "Valores negativos não são aceitos!"
    else:
      echo "Pedido recebido, e sendo enviado"
  except ValueError:
    echo "Não foi possível converter string em inteiro"
  # De qualquer forma finaliza o loop
  finally:
    break
```

## Controle de fluxo e loops

### 1º - `if`, `elif` e `else`

```nim
import strutils

proc print(texto: string) =
  stdout.write(texto)

proc input(texto: string): string =
  stdout.write(texto)
  stdin.readline()

var flag: bool = true

while flag:
  try:
    let num = input("\tEntre com um número de 1 à 100: ").parseInt()

    if num < 0:
      flag = false
    elif (num >= 0) and (num <= 50):
      print "É um número >= 0 e <= 50.\n"
    elif (num >= 50) and (num <= 100):
      print "É um número >= 50 e <= 100.\n"
    else:
      echo "Você entrou com um número maior que 100.\n"
  except ValueError:
    print "Não foi possível converter string em inteiro\n"
print("Tchau!!!\n")

# --> saída:
# 	Entre com um número de 1 à 100: a
# Não foi possível converter string em inteiro
# 	Entre com um número de 1 à 100: 25
# É um número >= 0 e <= 50.
# 	Entre com um número de 1 à 100: 63
# É um número >= 50 e <= 100.
# 	Entre com um número de 1 à 100: -1
# Tchau!!!
```

### 2º - `switch case`

```nim
stdout.write("Qual sua fruta preferida? ")
let res = stdin.readLine()

case res:
  of "kaki", "pêra", "uva":
    echo "Você tem um gosto refinado"
  of "banana", "maçã", "goiaba":
    echo "Você é mais tradicional"
  of "abacate", "abacaxi", "laranja":
    echo "Esta é para os fortes"
  else: echo "Esta é menos comum"

# --> saída:
# Qual sua fruta preferida? pêra
# Você tem um gosto refinado
```

### 3º - `for` loop

```nim
for i in 0..100:
  stdout.write i, " "

echo '\n'

var sequencia = @[1, 2, 3]
for i, v in sequencia:
  echo("Indice: ", i, " Valor: ", v)

echo '\n'

for v in sequencia:
  stdout.write(v, " ")

echo '\n'

# --> saídas:
# 0 1 2 3 ... 98 99 100
#
# Indice: 0 Valor: 1
# Indice: 1 Valor: 2
# Indice: 2 Valor: 3
#
# 1 2 3
```

## Coleções

### 1º - array

```nim
# De tamanho fixo de elementos homogêneos.
# Deve ser definido todos
# os elementos ou nenhum.
var
  lista: array[4, int] = [1, 2, 3, 4]
  # lista2: array[12, int] = [1, 2, 3] # Erro
  lista2: array[12, int]
  # Cria um array de 12 posições contendo zeros:
  # [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
```

### 2º - sequências

```nim
# De tamanho dinâmico de elementos homogêneos.
# Formas de criar:
var
  sequencia: seq[int]
  fabricantes: seq[string] = @["Wolksvagen", "Ford", "Fiat"]
  strings: newSeq[string](6) # Resulta em: @['', '', '', '', '', '']
  strings2: newSeqOfCap[string](10) # Resulta em: @[]
```

> <img src="icons/sticky-notes01.png" width=48/> **Obs.:** A diferença entre `newSeq` e `newSeqOfCap` é que, em `newSeq`, os espaços são alocados previamente, tornando a adição de novos elementos mais rápida, pois não é necessário alocar mais memória. Outro detalhe é que, em `newSeq`, os elementos são adicionados ao final, enquanto, em `newSeqOfCap`, eles são adicionados no início.
>
> **Exemplo:**
>
> ```nim
> var
> sequencia = newSeq[int](6)
> sequencia2 = newSeqOfCap[int](6)
> 
> sequencia.add(3)
> sequencia.add(4)
> 
> echo "Adicionando novos elementos em um `newSeq`:"
> echo sequencia
> 
> sequencia2.add(3)
> sequencia2.add(4)
> 
> echo "Adicionando novos elementos em um `newSeqOfCap`:"
> echo sequencia2
> 
> # --> saída:
> # Adicionando novos elementos em um `newSeq`:
> # @[0, 0, 0, 0, 0, 0, 3, 4]
> # Adicionando novos elementos em um `newSeqOfCap`:
> # @[3, 4]
> ```

### Biblioteca para sequências

```nim
import sequtils

var frutas = newSeqOfCap[string](10)

frutas = concat(frutas, @["kaki", "pêra", "maça"])

echo frutas # --> @["kaki", "pêra", "maça"]
```

### Array para Seq e vice-versa

```nim
var
  lista: array[5, int] = [1, 2, 3, 4, 5]
  sequencia: seq[int] = @lista
  lista2: array[5, int]

for i, el in sequencia:
  lista2[i] = el

echo lista # [1, 2, 3, 4, 5]

echo sequencia # @[1, 2, 3, 4, 5]

echo lista2 # [1, 2, 3, 4, 5]
```

### Acessando, tanto em arrays como em sequências, por índice ou slices

```nim
var
  lista: array[5, int] = [1, 2, 3, 4, 5]
  sequencia: seq[int] = @lista

echo lista[1]
echo sequencia[1]

echo lista[2..3]
echo sequencia[2..3]

# --> saída:
# 2
# 2
# @[3, 4]
# @[3, 4]
```

### Adicionar e Remover elementos

```nim
var metaisBasicos = newSeqOfCap[string](12)
metaisBasicos.add("Alumínio") # @["Alumínio"]
metaisBasicos.add(@["Índio", "Lata"]) # @["Alumínio", "Índio", "Lata"]
metaisBasicos.insert("Gálio", 1) # @["Alumínio", "Gálio", "Índio", "Lata"]
```

### Remover

```nim
var metaisBasicos = @["Alumínio", "Gálio", "Índio", "Lata"]
# Para remover um elemento primeiro encontra-se
# o índice para só depois remover, caso o índice
# não exista ele retorna -1.
let indice = metaisBasicos.find("Índio") # 2
metaisBasicos.del(indice) # @["Alumínio", "Gálio", "Lata"]
```

ou

```nim
var metaisBasicos = @["Alumínio", "Gálio", "Índio", "Lata"]

metaisBasicos.del(metaisBasicos.find("Índio")) # @["Alumínio", "Gálio", "Lata"]
```

Melhor forma de deletar:

```nim
# Sobrescrevendo o procedimento del.
# Como a sequência `s` que aqui vem como parâmetro foi
# declarada com `var` possibilita a alteração de seu
# conteúdo dentro do procedimento.
proc del(s: var seq[string], item: string) =
  let indice = s.find(item)
  if indice != -1:
    s.del(indice)

var metaisBasicos = @["Alumínio", "Gálio", "Índio", "Lata"]
metaisBasicos.del("Lata") # @["Alumínio", "Gálio", "Índio"]
```

## Mapeando listas

### Map

```nim
import sequtils, sugar

var
  lista = @[1, 2, 3, 4, 5, 6]
  quadrados1: seq[int]
  quadrados2: seq[int]
  quadrados3: seq[int]

proc quadrado(x: int): int = x * x

quadrados1 = lista.map(quadrado) # @[1, 4, 9, 16, 25, 36]
# Ou sem declarar o procedimento `quadrado`:
quadrados2 = lista.map(proc (x: int): int = x * x)
# Ou de forma mais limpa:
quadrados3 = lista.map((x: int) => int x * x) # Usa o módulo `sugar`

echo quadrados1
echo quadrados2
echo quadrados3
# --> saída:
# @[1, 4, 9, 16, 25, 36]
# @[1, 4, 9, 16, 25, 36]
# @[1, 4, 9, 16, 25, 36]
```

> <img src="icons/sticky-notes01.png" width=48/> **Obs.:** O módulo `sugar` facilita a escrita de funções a serem passadas como parâmetro que são mais simples e são usadas apenas 1 vês, então ter uma forma simples de declarar uma função é útil nesses casos.

###  Apply

```nim
# Quando não queremos retornar uma nova
# sequência, e sim alterar a sequência
# original (apply não tem retorno):
import sequtils, sugar

var lista = @[1, 2, 3, 4, 5, 6]
proc quadrado(x: int): int = x * x

lista.apply(quadrado) # @[1, 4, 9, 16, 25, 36]
# Ou sem declarar o procedimento `quadrado`:
lista.apply(proc (x: int): int = x * x)
# Ou de forma mais limpa:
lista.apply((x: int) => int x * x)
```

### Filter

```nim
import sequtils
proc par(num: int): bool = num mod 2 == 0

var
  numeros = @[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]
  pares = numeros.filter(par)

echo pares # --> @[2, 4, 6, 8, 10, 12]
```

ou

```nim
import sequtils, sugar

var
  numeros = @[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]
  pares = numeros.filter((num: int) => bool num mod 2 == 0) # Usa o módulo `sugar`

echo pares # --> @[2, 4, 6, 8, 10, 12]
```

### KeepIf

```nim
# Quando não queremos retornar
# uma nova sequência.
import sequtils
proc par(num: int): bool = num mod 2 == 0

var numeros = @[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]

numeros.keepIf(par)

echo numeros # --> @[2, 4, 6, 8, 10, 12]
```

ou

```nim
import sequtils, sugar

var numeros = @[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]

numeros.keepIf((num: int) => bool num mod 2 == 0) # Usa o módulo `sugar`

echo numeros # --> @[2, 4, 6, 8, 10, 12]
```

### Foldl (similar ao Reduce de outras linguagens)

```nim
import sequtils

var
  numeros = @[1, 2, 3, 4, 5, 6, 7, 8]
  soma = numeros.foldl(a + b, 0) # 36
  maior = numeros.foldl(if a > b: a else: b, 0) # 8
```

## Tuplas

```nim
# De tamanho fixo de elementos
# tanto homogêneos como heterogêneos.
# elementos podem ser nomeados
# ou não e nesse caso são
# acessados por indice.
var
  pessoa = (nome: "Fulano", idade: 25)
  pessoa2 = ("Sicrano", 30)

echo("Nome: ", pessoa.nome, " - Idade: ", pessoa.idade)

echo("Nome: ", pessoa2[0], " - Idade: ", pessoa2[1])

# --> saída:
# Nome: Fulano - Idade: 25
# Nome: Sicrano - Idade: 30
```

### Combinando tuplas com sequências

```nim
var
  pessoas = @[
    (nome: "Fulano", idade: 25),
    (nome: "Sicrano", idade: 30),
    (nome: "Beltrano", idade: 20)
  ]

for i, pessoa in pessoas:
  echo(i, ": Nome: ", pessoa.nome, " - Idade: ", pessoa.idade)

echo("\nTotal de registros: ", pessoas.len)

# --> saída:
# 0: Nome: Fulano - Idade: 25
# 1: Nome: Sicrano - Idade: 30
# 2: Nome: Beltrano - Idade: 20
# Total de registros: 3
```

### Extras

### Zip

Pega duas sequências e retorna uma sequência de tuplas:

```nim
import sequtils

var
  simbolos = @["Cu", "Al", "Ag", "Au", "Fe", "Ni"]
  metais = @["Cobre", "Alumínio", "Prata", "Ouro", "Ferro", "Níquel"]
  seqTuplas = simbolos.zip(metais)

echo seqTuplas
# --> saída:
# @[("Cu", "Cobre"), ("Al", "Alumínio"), ("Ag", "Prata"),
#   ("Au", "Ouro"), ("Fe", "Ferro"), ("Ni", "Níquel")]
```

### Unzip

Faz o inverso, pega uma sequência de tuplas e divide em duas sequências:

```nim
import sequtils

var
  seqTuplas = @[
    ("Cu", "Cobre"), ("Al", "Alumínio"), ("Ag", "Prata"),
    ("Au", "Ouro"), ("Fe", "Ferro"), ("Ni", "Níquel")
  ]
  (simbolos, metais) = seqTuplas.unzip()

echo simbolos # --> @["Cu", "Al", "Ag", "Au", "Fe", "Ni"]
echo metais # --> @["Cobre", "Alumínio", "Prata", "Ouro", "Ferro", "Níquel"]
```

### Count

```nim
# Conta quantas vezes um elemento
# aparece em uma sequência.
import sequtils
var
  numeros = @[1, 1, 3, 1, 5, 7, 4, 4]
  repeticoesDoUm = numeros.count(1)

echo repeticoesDoUm # --> 3
```

### MaxIndex e MinIndex

```nim
# Retornam o índice do
# maior e menor valor
# de uma sequência.
import sequtils
var
  numeros = @[1, 2, 3, 9, 4, 5, 6, 0]
  indiceDoMaior = numeros.maxIndex()
  indiceDoMenor = numeros.minIndex()

echo "numeros[", indiceDoMaior, "] = ", numeros[indiceDoMaior]
echo "numeros[", indiceDoMenor, "] = ", numeros[indiceDoMenor]

# --> saída:
# numeros[3] = 9
# numeros[7] = 0
```

## Funções impuras (proc) e funções puras (func)

### 1º - Funções impuras (proc)

- As funções impuras, também conhecidas como procedimentos (proc), são aquelas que podem causar efeitos colaterais, como modificar variáveis globais, alterar valores passados, imprimir algo na tela, receber input, interagir com o sistema de arquivos, realizar I/O, etc.
- Elas não possuem um valor de retorno definido, pois seu objetivo é realizar alguma ação, não necessariamente retornar um valor.
- As funções impuras podem ser úteis quando você precisa realizar tarefas que envolvem modificações no estado do programa, como atualizar uma variável ou escrever em um arquivo.

Exemplo de uma função impura (proc):

```nim
var contadorGlobal = 0

proc incrementaContador() =
  contadorGlobal += 1 # Efeito colateral modifica variável global

incrementaContador()
echo contadorGlobal # --> 1
```

```nim
# Parâmetro imutável.
proc soma(a, b: int) =
  echo a, " + ", b, " = ", a + b # Efeito colateral Imprime algo na tela

soma(10, 20) # --> 10 + 20 = 30
```

```nim
var flag: bool = true

# Parâmetro mutável
proc mudaEstado(f: var bool) =
  f = if f: false else: true # Efeito colateral alterou valor de
                             # variável passado como parâmetro.

mudaEstado(flag)

echo flag # --> false
```

### 2º - Funções puras (func)

- As funções puras são aquelas que não causam efeitos colaterais e sempre retornam o mesmo valor para os mesmos argumentos de entrada.
- Elas não modificam variáveis globais, não realizam I/O e não dependem de nenhum estado externo ao escopo da função.
- As funções puras são determinísticas, ou seja, sempre produzem o mesmo resultado para os mesmos argumentos de entrada.
- Elas são úteis quando você precisa realizar cálculos ou transformações de dados de uma maneira segura e previsível.

Exemplo de uma função pura (func):

```nim
func add(a, b: int): int =
  return a + b

let result = add(2, 3)
echo result # --> 5
```

```nim
from math import sqrt, pow

func calculaRaizes(a, b, c: float): (float, float) =
  let delta = sqrt(pow(b, 2) - (4*a*c))
  return (((-b + delta) / (2*a)), ((-b - delta) / (2*a)))

let (r1, r2) = calculaRaizes(1, -3, -10)

echo "r1: ", r1
echo "r2: ", r2

# --> saída:
# r1: 5.0
# r2: -2.0
```

Funções puras são definitivamente mais fáceis de serem escritas, testadas e alteradas, pois elas estão sempre contidas nelas mesmas, e quando precisamos realizar efeitos colaterais utilizamos funções impuras.

### 3º - Funcões de primeira classe

São funções que criam funções ou retornam funções. E é aqui onde **funcs** não são aceitas, apenas **procs** podem ser passadas como parâmetros, assim temos que usar o `{.noSideEffect.}`.

> <img src="icons/sticky-notes01.png" width=48/> **Obs.:**
>
> `{.noSideEffect}` => Não causar efeito colateral.
>
> `{.closure.}` => Fechado, encapsulado.

```nim
proc dobrar(n: int): int {.noSideEffect.} = n * 2

proc opera(lista: var seq[int], f: proc(n: int): int {.closure.}) =
  for i, v in lista:
    lista[i] = f(v)

var lista = @[1, 2, 3, 4, 5]
opera(lista, dobrar)

echo lista # --> @[2, 4, 6, 8, 10]
```

Refatorando o exemplo anterior:

```nim
import sugar

proc opera(lista: var seq[int], f: (int) -> int) =
  for i, v in lista:
    lista[i] = f(v)

var lista = @[1, 2, 3, 4, 5]
opera(lista, (x) => x * 2) # lista.apply((x) => x * 2)

echo lista # --> @[2, 4, 6, 8, 10]
```

### 4º - Generics

Generics em Nim são uma maneira de escrever código que pode funcionar com diferentes tipos de dados, sem precisar repetir o mesmo código para cada tipo. Isso torna o código mais genérico, flexível e reutilizável.

Aqui está um exemplo simples de uma função genérica em Nim que encontra o valor máximo entre dois valores:

```nim
proc max[T](a, b: T): T =
  if a > b:
    return a
  else:
    return b

echo max[int](10, 20) # --> 20
# ou echo max(10, 20)
echo max[float](3.14, 2.71) # --> 3.14
# ou echo max(3.14, 2.71)
echo max[string]("apple", "banana") # --> "banana"
# ou echo max("apple", "banana")
```

Neste exemplo, `T` é um parâmetro de tipo genérico. Quando você chama a função `max`, você pode passar qualquer tipo de dado que seja comparável (como `int`, `float`, `string`, etc.) e a função irá retornar o valor máximo desses dois valores.

```nim
func soma[T](x, y: T): T = x + y

echo soma(10, 10) # --> 20
echo soma(3.14, 2.5) # --> 5.640000000000001

func somaSequencias[T](seq1, seq2: seq[T]): seq[T] =
  result = newSeq[T](len(seq1))
  for i, v in seq1:
    result[i] = v + seq2[i]

let
  seqInt = somaSequencias(@[1, 2, 3], @[3, 2, 1])
  seqFloat = somaSequencias(@[1.0, 2.0, 3.0], @[3.0, 2.0, 1.0])

echo seqInt # --> @[4, 4, 4]
echo seqFloat # --> @[4.0, 4.0, 4.0]
```

Você também pode usar generics em tipos de dados personalizados, como estruturas e objetos. Aqui está um exemplo de uma lista genérica em Nim:

```nim
type
  LinkedList[T] = ref object
    value: T
    next: LinkedList[T]

proc newNode[T](value: T): LinkedList[T] =
  LinkedList[T](
    value: value,
    next: nil
  )

proc append[T](list: var LinkedList[T], value: T) =
  if list == nil:
    list = newNode[T](value)
  else:
    var current = list
    while current.next != nil:
      current = current.next
    current.next = newNode[T](value)

var l: LinkedList[char] = nil

append(l, 'A')
append(l, 'B')

# Usando um loop while para percorrer a lista
var current = l
while current != nil:
  echo current.value # --> Em linhas diferentes: A B
  current = current.next
# Se você quiser usar um loop for, você precisará
# implementar um iterador personalizado. Aqui
# está um exemplo de como fazer isso:

iterator items[T](list: LinkedList[T]): T =
  var current = list
  while current != nil:
    yield current.value
    current = current.next

# Agora você pode usar um loop for
for v in items(l):
  echo v # --> Em linhas diferentes: A B
```

Neste exemplo, `LinkedList[T]` é um tipo de dado genérico que representa uma lista ligada. A função `newNode` cria um novo nó da lista, e a função `append` adiciona um novo nó ao final da lista.

Generics em Nim são muito úteis para criar código reutilizável e flexível, que pode funcionar com diferentes tipos de dados sem precisar escrever o mesmo código várias vezes.

## Versatilidade da sintaxe

O que a sintaxe possibilita fazer:

```nim
proc adicionar(alvo: var (int, int), aSomar: (int, int)) =
    alvo[0] += aSomar[0]
    alvo[1] += aSomar[1]

var coordenadas = (x: 0, y: -100)

coordenadas.adicionar((x: 20, y: 10))

echo coordenadas.x # --> 20
echo coordenadas.y # --> -90
```

## Mais coleções

Duas coleções diretamente ligadas a funções (`openArray` e `varargs`) e sobre `hashMap`.

### 1 - OpenArray's

Como a capacidade de um `array` precisa ser conhecido em tempo de compilação temos como solução o `openArray` para passar um `array` como parâmetro para uma função:

```nim
func somaArrays(lista: openArray[int]): int =
  for item in lista:
    result += item

echo somaArrays([1, 2, 3, 4]) # --> 10
echo somaArrays([1, 2, 3, 4, 6]) # --> 16
```

> <img src="icons/sticky-notes01.png" width=48/> **Obs.:** Usar `seq` ou `openArray` resulta no mesmo, porém, a vantagem de utilizar `openArray` é que `array` sempre oferece melhor *memory safety* e *performance* em comparação a `seq`. Além disso, `openArray` é compatível com `seq`.

### 2 - Varargs

Usada quando a função recebe uma quantidade arbitrária de parâmetros.

```nim
func concatena(palavras: varargs[string]): string =
  for palavra in palavras:
    result &= palavra & " "

echo concatena("Nim", "uma", "linguagem", "de", "uso", "geral")
echo concatena("A", "melhor", "linguagem")

# --> saída:
# Nim uma linguagem de uso geral
# A melhor linguagem
```

A diferença dos `varargs` com os `openArray's` é que pode ser aplicado uma função aos valores:

```nim
func quadrado(x: int): int = x * x

func somaQuadrados(numeros: varargs[int, quadrado]): int =
  for num in numeros: result += num

echo somaQuadrados(1, 2, 3, 4) # --> 30
echo somaQuadrados(@[1, 2, 3]) # --> 6
```

### 3 - HashMap que em Nim é chamado de `table`

Estrutura de dados do tipo par {chave, valor}. Podem ser criadas de duas formas:

```nim
import tables

var
  # 1ª forma:
  pessoas = {
    1: "Fulano", 2: "Sicrano", 3: "Beltrano"
  }.toTable
  # 2ª forma, criando sem dar nenhum valor:
  vogais = initTable[int, string]()

# Inserindo valores:
vogais[1] = "a"
vogais[2] = "e"
vogais[3] = "i"
vogais[4] = "o"
vogais[5] = "u"

echo pessoas # --> {3: "Beltrano", 2: "Sicrano", 1: "Fulano"}
echo vogais  # --> {4: "o", 5: "u", 3: "i", 2: "e", 1: "a"}
```

Outro exemplo:

```nim
import tables, sequtils

let
  posicao = @[1, 2, 3]
  personagem = @["Chaves", "Seu Madruga", "Chiquinha"]

var vila = initTable[int, string]()
for (key, value) in zip(posicao, personagem):
  vila[key] = value
    
echo vila[3] # --> "Chiquinha"

vila[4] = "Kiko"

# Se o valor não existir será adicionado e retorna false.
echo vila.hasKeyOrPut(5, "Dona Florinda") # --> false

# Se o valor existir apenas retorna true
echo vila.hasKeyOrPut(4, "Kiko") # --> true

# Tenta retornar um valor,
# caso não exista retorna
# 0 para inteiros ou string
# vazia string's.
echo vila.getOrDefault(2) # --> "Seu Madruga"
echo vila.getOrDefault(6) # --> ""

# Remove uma chave, caso não exista não acontece nada
vila.del(4)

# Os table's são naturalmente desordenados.
# Listando um table:
for (key, value) in vila.mpairs:
  echo "Personagem nº ", key, " - ", value
  # --> saida:
  # Personagem nº 5 - Dona Florinda
  # Personagem nº 3 - Chiquinha
  # Personagem nº 2 - Seu Madruga
  # Personagem nº 1 - Chaves

vila.clear()
echo vila.len() # --> 0
```

Se for necessário que os elementos estejam ordenados crie usando `initOrderedTable`:

```nim
import tables, sequtils

let
  posicao = @[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]
  personagem = @[
    "Chaves", "Seu Madruga", "Chiquinha", "Quico",
    "Dona Florinda", "Professor Girafales",
    "Senhor Barriga", "Dona Clotilde", "Jaiminho",
    "Nhonho", "Popis"
  ]

var vila = initOrderedTable[int, string]()
for (key, value) in zip(posicao, personagem):
  vila[key] = value

# Agora listando de forma ordenada:
for (key, value) in vila.mpairs:
  echo "Personagem nº ", key, " - ", value

# --> saída:
# Personagem nº 1 - Chaves
# Personagem nº 2 - Seu Madruga
# Personagem nº 3 - Chiquinha
# Personagem nº 4 - Quico
# Personagem nº 5 - Dona Florinda
# Personagem nº 6 - Professor Girafales
# Personagem nº 7 - Senhor Barriga
# Personagem nº 8 - Dona Clotilde
# Personagem nº 9 - Jaiminho
# Personagem nº 10 - Nhonho
# Personagem nº 11 - Popis
```

> <img src="icons/sticky-notes01.png" width=48/> **Obs.:** Além de `table`, existe `countTable`, que basicamente utiliza cada valor de uma lista ou sequência como *key* e conta quantas vezes esse valor se repete.

## Options e Results

### Options

Pegar valor de uma lista referente a um índice checando se o índice existe ou não:

```nim
import options

func getItem(lista: seq[int], i: int): Option[int] =
  if (i >= lista.len):
    return none(int) # none = nenhum
  return some(lista[i])

if (let valor = @[1, 2, 3, 4, 5].getItem(3); valor.isSome): # isSome = é algum
  echo "isSome: ", valor.isSome
  echo "Valor = ", valor.get

echo '\n'

if (let valor2 = @[1, 2, 3, 4, 5].getItem(10); valor2.isNone): # isNone = é nenhum
  echo "isNone: ", valor2.isNone
  echo "Índice fora dos limites!"

# --> saída:
# isSome: true
# Valor = 4
#
#
# isNone: true
# Índice fora dos limites!
```

Instalar a biblioteca results:

```bash
$ nimble install results
```

> <img src="icons/location01.png" width=48/> Mais sobre o gestor de pacotes nimble no [Anexo I](#anexo-i). <a name="ancora-i"><img src="icons/ancora01.png" width=48/></a>

Agora refatorando o código anterior:

```nim
import results

type Resultado = Result[int, string]
func getItemResults(lista: seq[int], i: int): Resultado =
  if (i >= lista.len):
    return Resultado.err "Índice fora dos limites!"
  return Resultado.ok lista[i]

if (let valor = @[1, 2, 3, 4, 5].getItemResults(3); valor.isOk):
  echo "isOk: ", valor.isOk
  echo "Valor = ", valor.value

echo '\n'

if (let valor = @[1, 2, 3, 4, 5].getItemResults(10); valor.isErr):
  echo "isErr: ", valor.isErr
  echo valor.error

# --> saída:
# isOk: true
# Valor = 4
#
#
# isErr: true
# Índice fora dos limites!
```

## Tipos Complexos

> <img src="icons/sticky-notes01.png" width=48/> **Obs.:** Em Nim, não existem objetos propriamente ditos, pois tudo é convertido para estruturas. No entanto, a linguagem possui algumas sintaxes que fazem com que pareça que há objetos.

### Tipo simples

```nim
type age = int

var idade: age = 17

proc aniversario(idade: var int) =
  idade += 1

idade.aniversario()
echo idade # --> 18
```

### Tuplas

Usando tupla em lugar de objeto (preferível usar objeto):

```nim
type Pessoa = tuple
  nome: string
  sobrenome: string
  idade: int

func newPessoa(nome, sobrenome: string, idade: int): Pessoa =
  (nome, sobrenome, idade)

let p = newPessoa("Fulano", "de Tal", 30)
echo p.nome # --> Fulano
```

## Objetos

Nim não possui objetos, porem para ser mais palpável para programadores de OO, Nim permite alguns conceitos de OO como herança e polimorfismo.

```nim
type Pessoa = ref object
  nome: string
  sobrenome: string
  idade: int

func newPessoa(nome, sobrenome: string, idade: int): Pessoa =
  result = Pessoa(
    nome: nome,
    sobrenome: sobrenome,
    idade: idade
  )

let p = newPessoa("Fulano", "de Tal", 30)
echo p.nome # --> Fulano
```

> <img src="icons/sticky-notes01.png" width=48/> **Obs.:** 
>
> 1. O uso de `ref` na declaração de um objeto significa que, sempre que esse objeto for utilizado, ele será passado como uma referência, ou seja, será mutável por padrão.
>
> 2. Uma função pura é aquela que:
>    - Sempre retorna o mesmo resultado para os mesmos argumentos de entrada.
>    - Não tem efeitos colaterais (*side effects*), ou seja, não modifica o estado global nem realiza operações de I/O.
> 3. No caso da sua função `newPessoa`, ela simplesmente cria e retorna um novo objeto `Pessoa` com base nos argumentos fornecidos, sem modificar nenhum estado externo ou realizar operações de I/O.
>
> 
>
> Portanto, use `proc` para modificar o objeto ou `func` para retornar algo.

## POO e Classes

É importante notar que Nim não tem uma sintaxe específica para "classes" como em algumas outras linguagens orientadas a objetos, mas podemos alcançar funcionalidade semelhante usando tipos de objetos e métodos. Vamos explorar isso em detalhes:

1. **Definindo uma "Classe" (Tipo de Objeto)**

Em Nim, uma "classe" é definida usando a palavra-chave `object`:

```nim
type
  Pessoa = object
    nome: string
    idade: int
```

Para criar uma instância de um objeto:

```nim
var p = Pessoa(nome: "Alice", idade: 30)
```

2. **Construtores**

Nim não tem construtores integrados, mas podemos criar funções que retornam objetos inicializados:

```nim
proc newPessoa(nome: string, idade: int): Pessoa =
  result = Pessoa(nome: nome, idade: idade)

var p = newPessoa("Alice", 30)
```

3. **Métodos**

Métodos são funções associadas a um tipo. Em Nim, usamos a palavra-chave `self` (ou qualquer outro nome) para se referir à instância:

```nim
proc apresentacao(self: Pessoa) =
  echo "Olá, meu nome é ", self.nome, " e tenho ", self.idade, " anos."

p.apresentacao()
```

> Juntando objeto, construtor, instância e método
>
> ```nim
> # Objeto
> type
> Pessoa = object
>  nome: string
>  idade: int
> 
> # Construtor
> proc newPessoa(nome: string, idade: int): Pessoa =
> result = Pessoa(nome: nome, idade: idade)
> 
> # Instância
> var p = newPessoa("Alice", 30)
> 
> # Método
> proc apresentacao(self: Pessoa) =
> echo "Olá, meu nome é ", self.nome,
>  " e tenho ", self.idade, " anos."
> 
> p.apresentacao()
> # --> Olá, meu nome é Alice e tenho 30 anos.
> ```

4. **Métodos que Modificam o Objeto**

Para métodos que modificam o objeto, use `var` antes do tipo:

```nim
type
  Pessoa = object
    nome: string
    idade: int

proc newPessoa(nome: string, idade: int): Pessoa =
  result = Pessoa(nome: nome, idade: idade)

proc aniversario(self: var Pessoa) =
  self.idade += 1

var p = newPessoa("Bob", 25)
p.aniversario()
echo p.idade  # --> 26
```

> Refatorando código anterior
>
> ```nim
> type
>   # Declarando um objeto com `ref` sempre que esse objeto
>   # for utilizado, ele será passado como uma referência,
>   # ou seja, será mutável por padrão.
>   Pessoa = ref object # Foi acrescentado `ref`
>     nome: string
>     idade: int
> 
> proc newPessoa(nome: string, idade: int): Pessoa =
>   result = Pessoa(nome: nome, idade: idade)
> 
> proc aniversario(self: Pessoa) = # Foi removido `var` onde
>   self.idade += 1                  # antes havia `var Pessoa`
> 
> var p = newPessoa("Bob", 25)
> p.aniversario()
> echo p.idade  # --> 26
> ```
>
> 

5. **Encapsulamento**

Nim não tem modificadores de acesso como `private` ou `public`, mas podemos usar convenções de nomenclatura e módulos para controlar o acesso:

```nim
# Arquivo: pessoa.nim
type
  Pessoa* = ref object  # O asterisco torna o tipo público
    nome*: string       # Campo público
    idade: int          # Campo "privado" (acessível apenas dentro deste módulo)

proc newPessoa*(nome: string, idade: int): Pessoa =
  result = Pessoa(nome: nome, idade: idade)

proc getIdade*(self: Pessoa): int =
  result = self.idade

# Em outro arquivo (por exemplo: main.nim)
import pessoa

var p = newPessoa("Charlie", 35)
echo p.nome       # --> Charlie
echo p.getIdade() # --> 35
# echo p.idade   # Erro! 'idade' não é acessível fora do módulo pessoa
```

6. **Herança**

Nim suporta herança simples usando `object of`:

```nim
type
  Animal = ref object of RootObj
    nome: string

  Cachorro = ref object of Animal
    raca: string

proc newCachorro(nome, raca: string): Cachorro =
  result = Cachorro(nome: nome, raca: raca)

let cachorro = newCachorro("Rex", "Labrador")

echo cachorro.nome # --> Rex
echo cachorro.raca # --> labrador
```

> <img src="icons/sticky-notes01.png" width=48/> **Obs.:** O objeto do qual será herdado precisa herdar de `RootObj`.
>
> 
>
> Em Nim, todos os tipos de referência (aqueles declarados com `ref`) devem herdar, direta ou indiretamente, de `RootObj`. Isso garante que todos os objetos alocados na pilha compartilhem um cabeçalho comum, permitindo que o coletor de lixo funcione corretamente.
>
> 
>
> **Por que `RootObj` é importante?**
>
> 
>
> - **Gerenciamento de memória**: `RootObj` fornece um cabeçalho comum para todos os objetos na pilha, contendo informações que o coletor de lixo utiliza para rastrear e liberar objetos não utilizados.
> - **Introspecção de tipo**: `RootObj` permite verificar o tipo de um objeto em tempo de execução usando a função `is`.
>
> 
>
> **Em resumo:**
>
> 
>
> A herança de `RootObj` é essencial para o funcionamento adequado do coletor de lixo e para a introspecção de tipos em Nim.

7. **Polimorfismo**

Para suportar polimorfismo, use a palavra-chave `method` em vez de `proc`:

```nim
type
  Animal = ref object of RootObj
    nome: string

  Cachorro = ref object of Animal
    raca: string

method somQueProduz(self: Animal) {.base.} =
  echo "Som genérico de animal"

method somQueProduz(self: Cachorro) =
  echo "Au au!"

var a: Animal = Cachorro(nome: "Rex", raca: "Labrador")
a.somQueProduz()  # --> "Au au!"
```

8. **Propriedades**

Podemos simular propriedades usando procedimentos:

```nim
type
  Retangulo = ref object
    base, altura: float

proc area(self: Retangulo): float =
  result = self.base * self.altura

var r = Retangulo(base: 5, altura: 3)

# `r.area` é usado com uma propriedade
echo "Base: ", r.base,
  " X Altura: ", r.altura,
  " = ", r.area

# --> Base: 5.0 X Altura: 3.0 = 15.0
```

9. **Métodos Estáticos**

Nim não tem métodos estáticos, mas podemos usar procedimentos regulares para funcionalidade semelhante:

```nim
type
  MathUtils = ref object

# associando o procedimento `quadrado` ao objeto `MatUtils`
proc quadrado*(self: MathUtils, n: int): int =
  result = n * n

echo MathUtils().quadrado(5)  # --> 25

discard """
# Ou:

let math = MathUtils()

echo math.quadrado(5)  # --> 25
"""
```

10. **Interfaces**

Embora Nim não tenha interfaces explícitas, podemos simular comportamento semelhante:

```nim
type
  FiguraGeometrica = ref object of RootObj

method desenhar(self: FiguraGeometrica) {.base.} =
  raise newException(CatchableError,
    "Método desenhar não implementado")

type
  Circulo = ref object of FiguraGeometrica
    raio: float

method desenhar(self: Circulo) =
  echo "Desenhando um círculo de raio ", self.raio

var circ = Circulo(raio: 5.0)
circ.desenhar()
# --> Desenhando um círculo de raio 5.0

discard """
var circ: FiguraGeometrica = Circulo(raio: 5.0)
d.desenhar()
# --> Desenhando um círculo de raio 4.940656458412465e-324
"""
```

11. **Construtores com Herança**

Ao trabalhar com herança, podemos criar construtores que inicializam tanto os campos da classe base quanto os da classe derivada:

```nim
type
  Veiculo = ref object of RootObj
    marca: string

  Carro = ref object of Veiculo
    modelo: string

proc newCarro(marca, modelo: string): Carro =
  result = Carro(marca: marca, modelo: modelo)

var meuCarro = newCarro("Toyota", "Corolla")

echo "Marca: ", meuCarro.marca,
  ", Modelo: ", meuCarro.modelo

# --> Marca: Toyota, Modelo: Corolla
```

12. **Genéricos**

Nim suporta genéricos, que podem ser usados com objetos:

```nim
type
  Caixa[T] = ref object
    valor: T

proc getValor[T](self: Caixa[T]): T =
  result = self.valor

var intCaixa = Caixa[int](valor: 42)
echo intCaixa.getValor() # --> 42
```

13. **Operadores Personalizados**

Podemos definir operadores personalizados para nossas "classes":

```nim
type
  Vector = ref object
    x, y: float

proc `+`(a, b: Vector): Vector =
  result = Vector(x: a.x + b.x, y: a.y + b.y) # x: 1 + 3, y: 2 + 4

var v1 = Vector(x: 1, y: 2)
var v2 = Vector(x: 3, y: 4)
var v3 = v1 + v2
echo v3.x, ", ", v3.y # --> 4.0, 6.0
```

Este tópico cobre os principais aspectos de como criar e usar "classes" em Nim. Lembre-se de que, embora Nim não tenha uma sintaxe específica para classes, podemos alcançar funcionalidade semelhante usando tipos de objetos, métodos e outras características da linguagem. A abordagem de Nim para orientação a objetos é mais flexível e permite misturar paradigmas de programação conforme necessário.

# Anexos

---

## <img src="icons/annex01.png" width=48/> <a name="anexo-i">Anexo I</a> [<img src="icons/up-arrow01.png" width=48/>](#ancora-i)

# Nimble

[fonte](https://nim-lang.github.io/nimble/use-packages.html)

Embora o Nim tenha uma biblioteca padrão relativamente grande, é provável que em algum momento você queira usar alguma biblioteca de terceiros. Nas seções a seguir, mostraremos os comandos `nimble` mais usados para essa finalidade.

## `nimble install`

O comando `install` fará o download e instalará um pacote. Você precisa passar o nome do pacote (ou pacotes) que deseja instalar. Se algum dos pacotes depender de outros pacotes do Nimble, o Nimble também os instalará. Exemplo:

```sh
$ nimble install nake
Downloading https://github.com/fowlmouth/nake using git
      ...
  Success:  nake installed successfully.
```

O Nimble sempre busca e instala a versão mais recente de um pacote. Observe que a versão mais recente é definida como a versão marcada mais recente no repositório Git (ou Mercurial). Se o pacote não tiver versões marcadas, o commit mais recente no repositório remoto será instalado. Se você já tiver essa versão instalada, o Nimble perguntará se você deseja substituir sua cópia local.

### Instalando uma versão específica

Você pode forçar o Nimble a baixar o commit mais recente do repositório do pacote, por exemplo:

```sh
$ nimble install nimgame@#head
```

É claro que isso é específico do Git, para o Mercurial, use `tip` em vez de `head`. Um hash de ramificação, tag ou commit também pode ser especificado no lugar de `head`.

Em vez de especificar uma ramificação VCS, você também pode especificar uma versão concreta ou um intervalo de versões, por exemplo:

```sh
$ nimble install nimgame@0.5
$ nimble install nimgame@"> 0.5"
```

Os seguintes operadores de seletor de versão estão disponíveis:

| Operador                 | Significado                                                  |
| :----------------------- | :----------------------------------------------------------- |
| `==`                     | Instale a versão exata.                                      |
| `>`                      | Instale uma versão superior.                                 |
| `<`                      | Instale a versão inferior.                                   |
| `>=`                     | Instale *pelo menos* a versão fornecida.                     |
| `<=`                     | Instale *no máximo* a versão fornecida.                      |
| `^=`                     | Instale a versão compatível mais recente de acordo com [semver](https://semver.npmjs.com/). |
| `~=`                     | Instale a versão mais recente aumentando o último dígito fornecido |
| para a versão mais alta. |                                                              |

Os sinalizadores Nim fornecidos para `nimble install` serão encaminhados ao compilador ao criar binários. Esses sinalizadores do compilador podem ser tornados persistentes usando arquivos de [configuração](https://nim-lang.org/docs/nimc.html#compiler-usage-configuration-files) Nim.

### URLs de pacotes

Um URL válido para um repositório Git ou Mercurial também pode ser especificado, o Nimble detectará automaticamente o tipo de repositório para o qual o URL aponta e o instalará. Desta forma, os pacotes que não estão na lista oficial de pacotes podem ser instalados.

Para repositórios que contêm o pacote Nimble em um subdiretório, você pode instruir o Nimble sobre a localização do seu pacote usando o parâmetro de consulta `?subdir=<path>`. Por exemplo:

```sh
$ nimble install https://github.com/nimble-test/multi?subdir=alpha
```

### Desenvolvimento de pacotes locais

O comando `install` também pode ser usado para testar ou desenvolver localmente um pacote Nimble, deixando de fora o parâmetro de nome do pacote. Seu diretório de trabalho atual deve ser um pacote Nimble e conter um arquivo `package.nimble` válido.

O Nimble instalará o pacote que reside no diretório de trabalho atual quando você não especificar um nome de pacote e o diretório contiver um arquivo `package.nimble`. Isso pode ser útil para desenvolvedores que estão testando localmente seus arquivos `.nimble` antes de enviá-los para a lista oficial de pacotes. Consulte o [guia Criar pacotes](https://nim-lang.github.io/nimble/create-packages.html) para obter mais informações sobre isso.

As dependências necessárias para desenvolver ou testar um projeto podem ser instaladas passando `--depsOnly` sem especificar um nome de pacote. O Nimble instalará todas as dependências ausentes listadas no arquivo `package.nimble` do pacote no diretório de trabalho atual. Observe que as dependências serão instaladas globalmente.

Por exemplo, para instalar as dependências de um projeto `myPackage`:

```sh
$ cd myPackage
$ nimble install --depsOnly
```

## `nimble list`

Se você quiser listar *todos os* pacotes disponíveis, você pode usar a `nimble list` mas cuidado: é uma lista muito longa (e não muito útil). Pode ser melhor usar a `nimble search` (explicada abaixo) para pesquisar um pacote específico.

Se você quiser ver uma lista de pacotes instalados localmente e suas versões, use `--installed`, ou `-i` para abreviar:

```sh
$ nimble list -i
```

## `nimble search`

Se você não quiser passar por toda a saída do comando `list`, você pode usar o comando `search` especificando como parâmetros o nome do pacote e/ou tags que deseja filtrar. O Nimble examinará a lista conhecida de pacotes disponíveis e exibirá apenas aqueles que correspondem às palavras-chave especificadas (que podem ser substrings). Exemplo:

```sh
$ nimble search math

linagl:
url:         https://bitbucket.org/BitPuffin/linagl (hg)
tags:        library, opengl, math, game
description: OpenGL math library
license:     CC0
website:     https://bitbucket.org/BitPuffin/linagl

extmath:
url:         git://github.com/achesak/extmath.nim (git)
tags:        library, math, trigonometry
description: Nim math library
license:     MIT
website:     https://github.com/achesak/extmath.nim

glm:
url:         https://github.com/stavenko/nim-glm (git)
tags:        opengl, math, matrix, vector, glsl
description: Port of c++ glm library with shader-like syntax
license:     MIT
website:     https://github.com/stavenko/nim-glm

...
```

As pesquisas não diferenciam maiúsculas de minúsculas.

Um parâmetro `--ver` pode ser especificado para dizer ao Nimble para consultar repositórios Git remotos para obter a lista de versões dos pacotes e, em seguida, imprimir as versões. No entanto, observe que isso pode ser lento, pois cada pacote deve ser consultado separadamente.

### nimble.diretório

Como alternativa ao comando `nimble search`, você pode usar o [site do Nimble Directory](https://nimble.directory/) para pesquisar pacotes.

## `nimble uninstall`

O comando `uninstall` removerá um pacote instalado.

> <img src="icons/exclamation01.png" width=48/> **Aviso**
>
> A tentativa de remover um pacote do qual outros pacotes dependem resultará em um erro.
>
> Você pode usar o sinalizador `--inclDeps` ou `-i` para remover todos os pacotes dependentes junto com o pacote.

Semelhante ao comando `install`, você pode especificar um intervalo de versão, por exemplo:

```sh
$ nimble uninstall nimgame@0.5
```

## `nimble refresh`

O comando `refresh` é usado para buscar e atualizar a lista de pacotes do Nimble. Não há mecanismo de atualização automática, portanto, você precisa executá-lo sozinho se precisar *atualizar* sua lista local de pacotes Nimble disponíveis conhecidos. Exemplo:

```sh
$ nimble refresh
    Copying local package list
    Success Package list copied.
Downloading Official package list
    Success Package list downloaded.
```

As listas de pacotes podem ser especificadas na configuração do Nimble. Opcionalmente, você também pode fornecer esse comando com uma URL se quiser usar uma lista de pacotes de terceiros.

Alguns comandos podem lembrá-lo de executar a `nimble refresh` ou executá-la para você se falharem.

## `nimble path`

O comando `nimble path` mostrará o caminho absoluto para os pacotes instalados que correspondem aos parâmetros especificados. Como pode haver muitas versões do mesmo pacote instaladas, este comando listará todas elas, por exemplo:

```sh
$ nimble path itertools
/home/user/.nimble/pkgs2/itertools-0.4.0-5a3514a97e4ff2f6ca4f9fab264b3be765527c7f
/home/user/.nimble/pkgs2/itertools-0.2.0-ab2eac22ebda6512d830568bfd3052928c8fa2b9
```



---

## <img src="icons/annex01.png" width=48/> <a name="anexo-ii">Anexo II</a> [<img src="icons/up-arrow01.png" width=48/>](#ancora-ii)

### Regex

Aqui está um tutorial completo sobre expressões regulares (regex) na linguagem Nim:

1. **Introdução**

Nim suporta expressões regulares através do módulo `re`. Para usar regex em Nim, você precisa importar este módulo:

```nim
import re
```

2. **Criando um Regex**

Você pode criar um objeto regex usando a função `re()`:

```nim
let pattern = re"[a-z]+"
```

3. **Funções Básicas**

a) *match*: Verifica se uma string corresponde completamente ao padrão

```nim
if match("hello", re"[a-z]+"):
  echo "Matched!"
```

b) *find*: Procura o padrão na string

```nim
if re.find("Hello, World!", re"World", 0) != -1:
  echo "Found!"
```

c) *contains*: Verifica se a string contém o padrão

```nim
if "example@email.com".contains(re"\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b"):
  echo "Valid email!"
```

4. **Capturando Grupos**

Você pode usar parênteses para criar grupos de captura:

```nim
import strutils, re

let pattern = re"(\d{2})/(\d{2})/(\d{4})"
let dataSeq = re.findAll("24/12/202325/12/2023", pattern, 0)

# Itera sobre cada data encontrada
for data in dataSeq:
  let partes = data.split('/')
  echo "Dia: ", partes[0]
  echo "Mês: ", partes[1]
  echo "Ano: ", partes[2]
  echo "---"

# --> saída:
# Dia: 24
# Mês: 12
# Ano: 2023
# ---
# Dia: 25
# Mês: 12
# Ano: 2023
# ---
```

5. **Substituições**

A função `replace` permite substituir ocorrências do padrão:

```nim
let text = "Olá, Mundo!"
let replaced = text.replace(re"Mundo", "Nim")
echo replaced  # --> Olá, Nim!
```

6. **Opções de Regex**

Você pode usar opções para modificar o comportamento do regex:

```nim
let pattern = re("(?i)hello")  # (?i) torna a busca case-insensitive
if "HELLO".match(pattern):
  echo "Matched!"
```

7. **Metacaracteres Comuns**

- `.`: Qualquer caractere exceto nova linha
- `^`: Início da string
- `$`: Fim da string
- `*`: Zero ou mais ocorrências
- `+`: Uma ou mais ocorrências
- `?`: Zero ou uma ocorrência
- `\d`: Dígito
- `\w`: Caractere de palavra (letras, dígitos, underscore)
- `\s`: Espaço em branco

8. **Classes de Caracteres**

- `[abc]`: Qualquer caractere a, b ou c
- `[^abc]`: Qualquer caractere exceto a, b ou c
- `[a-z]`: Qualquer caractere de a z

9. **Quantificadores**

- `{n}`: Exatamente n ocorrências
- `{n,}`: n ou mais ocorrências
- `{n,m}`: Entre n e m ocorrências

10. **Exemplo Prático**

Vamos criar uma função para validar um número de telefone:

```nim
import re

proc isValidPhoneNumber(phone: string): bool =
  let pattern = re"^\+?1?\s*\(?[2-9]\d{2}\)?[-.\s]?\d{3}[-.\s]?\d{4}$"
  result = phone.match(pattern)

# Teste
let phones = @["+1 (555) 555-5555", "1234567890", "555-5555", "(123) 456-7890"]
for phone in phones:
  if isValidPhoneNumber(phone):
    echo phone, " é válido"
  else:
    echo phone, " não é válido"

# --> saída:
# +1 (555) 555-5555 é válido
# 1234567890 não é válido
# 555-5555 não é válido
# (123) 456-7890 não é válido
```

11. **Dicas Finais**

- Use `r"raw string"` para strings brutas, úteis para padrões complexos
- O módulo `re` também oferece funções como `split` e `findAll` para operações mais avançadas
- Para regex mais complexos, considere usar a biblioteca PCRE (Perl Compatible Regular Expressions) disponível em Nim

Este tutorial cobre os conceitos básicos e intermediários de regex em Nim. Para casos mais avançados ou específicos, consulte a documentação oficial do módulo `re` de Nim.
