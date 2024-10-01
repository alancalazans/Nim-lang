# Nim - notas

## Introdução

Este documento é uma coletânea de conteúdos provenientes de diversas fontes sobre a linguagem Nim (como o guia oficial da linguagem, tutoriais de terceiros, o ebook de Stefan Salewski, IA e etc...), além de experimentos próprios. Embora seja uma verdadeira colcha de retalhos, está organizado de forma lógica, buscando cobrir a linguagem desde o nível introdutório até o avançado, tanto quanto possível. Este trabalho tem como objetivo ser continuamente atualizado, com o intuito de aprimorá-lo e trazer novos conteúdos sobre essa linguagem, que é, no mínimo, fascinante.

## Instalação do Nim no sistema

### choosenim

[página do projeto](https://github.com/dom96/choosenim)

choosenim instala a [linguagem de programação Nim](https://nim-lang.org/) a partir de downloads oficiais e fontes, permitindo que você alterne facilmente entre estável e compiladores de desenvolvimento.

O objetivo desta ferramenta é duplo:

- Forneça uma maneira fácil de instalar o compilador e as ferramentas Nim.
- Gerencie várias instalações Nim e permita que elas sejam selecionadas sob demanda.

### Uso típico

Para selecionar a versão atual do Nim:`stable`

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

Para atualizar para a versão mais recente do Nim:`stable`

```bash
$ choosenim update stable
```

Para exibir quais versões estão instaladas no momento:

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

As versões podem ser selecionadas via ou por nome de ramificação/tag via (observe que a seleção de ramificações provavelmente exigirá que o Nim seja inicializado, o que pode ser lento).`choosenim 1.6.6``choosenim #devel`

## Nosso primeiro programa Nim

test.nim

```nim
var
  soma: int = 0  # Ou:
  num: int = 0   # `soma, num: int = 0`
 
while num < 4:
  inc(num, 1) # Ou `inc(num)`
  echo "Nº: ", num
  inc(soma, num)

echo "\nA soma destes: ", soma

# --> saída:
# Nº: 1
# Nº: 2
# Nº: 3
# Nº: 4
#
# A soma destes: 10
```

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

## 1º - Números

### A) Tipos numéricos

```nim
var
  # Com sinal - int ~ int8 ~ int16 ~ int32 ~ int64
  num1: int      = 0
  num2: int8     = -128
  num3: int16    = 12314
  num4: int32    = -124
  num5: int64    = 1235
  # Sem sinal - uint ~ uint8 ~ uint16 ~ uint32 ~ uint64
  num6:  uint    = 0
  num7:  uint8   = 128
  num8:  uint16  = 12314
  num9:  uint32  = 124
  num10: uint64  = 1235
  # Floats - float ~ float32 ~ float64 (Possui somente estes tipos)
  fnum1: float   = 2.71
  fnum2: float32 = 3.1415
  fnum3: float64 = 14.352
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

### 2º - Strings

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

### 3º `var`, `let` e `const`

- `var`: São variáveis que podem ser criadas em <u>*runtime*</u> (tempo de execução) - quando valores são solicitados durante a execução do programa ou em <u>*compiletime*</u> (tempo de compilação) - valores são conhecidos durante a compilação e em ambos os casos estas variáveis podem ser atualizadas seu valor durante a execução do programa.

- `let` e `const`: São constantes, o simbolo definido como `const` seu valor deve ser conhecido em <u>*compiletime*</u> enquanto `let` seu valor é atribuído uma única vez tanto em <u>*runtime*</u> quanto em <u>*compiletime*</u>.

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

## Saída e entrada de dados

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
>  time = getTime()
>  ano = time.format("yyyy").parseInt()
>  pattern = re"^\d+$" # Regex que verifica se a string contém apenas dígitos
> 
> # Função para verificar se a string contém apenas números
> proc isNumber(s: string): bool =
>  return s.len > 0 and s.match(pattern)
> 
> try:
>  stdout.write("Qual sua idade?: ")
>  let input = stdin.readLine()
> 
>  if isNumber(input):
>     let idade = input.parseInt()  # Agora é seguro converter para número
>     echo "Seu ano de nascimento: ", ano - idade
>  else:
>     raise newException(ValueError, "Você entrou com um valor inválido que não é um número: " & input)
> 
> except ValueError as e:
>  echo "Erro: " & e.msg
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
> <img src="icons/location01.png" width=48/> Mais sobre regex no Anexo I.

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
import std/sequtils

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
# @[("Cu", "Cobre"), ("Al", "Alumínio"), ("Ag", "Prata"), ("Au", "Ouro"), ("Fe", "Ferro"), ("Ni", "Níquel")]
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
# Se você quiser usar um loop for, você precisará implementar um iterador personalizado. Aqui está um exemplo de como fazer isso:

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
  personagem = @["Chaves", "Seu Madruga", "Chiquinha", "Quico", "Dona Florinda", "Professor Girafales", "Senhor Barriga", "Dona Clotilde", "Jaiminho", "Nhonho", "Popis"]

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

### Herança

Sobre herança em Nim, é muito simples, a principal diferença é a *key word* `of` a mais na declaração do objeto, isso vai indicar de qual objeto sera herdado os atributos. 

```nim
type
  Pessoa = ref object of RootObj # ou Pessoa {.inheritable.} = object
    nome: string
    idade: int

  Estudante = ref object of Pessoa
    curso: string
    id: int

discard """
# Pessoa contém apenas os atribuitos `nome` e `idade`
# e são estes que a função abaixo pode retornar.
func `$`(p: Pessoa): string =
  return "Nome: " & p.nome & ", Idade: " & $p.idade
"""

# Estudante como herda de `Pessoa` pode retornar tanto
# os atributos de `Pessoa` (`nome` e `idade`) mais os
# de `Estudante` (`curso` e `id`).
func `$`(e: Estudante): string =
  return "Nome: " & e.nome & ", Idade: " & $e.idade & ", Curso: " & e.curso & ", ID: " & $e.id

func newEstudante(nome, curso: string, idade, id: int): Estudante =
  Estudante(nome: nome, idade: idade, curso: curso, id: id)

var estudante = newEstudante("Sicrano", "Computação", 21, 123)

echo estudante
# --> saída:
# Nome: Sicrano, Idade: 21, Curso: Computação, ID: 123
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

O símbolo `$` em Nim é usado para definir a representação em `string` de um objeto, sendo uma convenção da linguagem para implementar o método de conversão para `string`.



Aqui estão os principais pontos sobre o uso de `$`:



1. **Sobrecarga do operador de conversão para `string`**:
   - `func `$`(p: Pessoa): string` define como um objeto `Pessoa` deve ser convertido para uma `string`.
2. **Uso implícito**:
   - Quando você usa `echo estudante`, o Nim automaticamente busca uma função `$` definida para o tipo do objeto (neste caso, `Estudante`, que herda de `Pessoa`).
3. **Personalização da saída**:
   - Permite que você controle como o objeto é representado como `string`, útil para depuração e exibição.
4. **Herança**:
   - Como `Estudante` herda de `Pessoa`, ele também herda a implementação de `$`, a menos que seja sobrescrita.
5. **Convenção em Nim**:
   - É uma prática comum em Nim usar `$` para definir a representação em `string` de objetos personalizados.
6. **Uso em outras funções**:
   - Muitas funções em Nim que manipulam `strings` (como `echo` ou `&` para concatenação) usam implicitamente `$` para converter objetos em `strings`.

### Objetos externos

#### Módulo principal: main.nim

```nim
import entidades_class

func `$`*(al: Aluno): string =
  return "ID: " & $al.id & ", Nome: " & al.nome & ", DataDeNasc.: " & $al.dataDeNasc & ", Idade: " & $al.idade

let novoAluno = newAluno(1, "Beltrano", "06/08/1969")

echo novoAluno
# --> saída:
# ID: 1, Nome: Beltrano, DataDeNasc.: 06/08/1969, Idade: 54
```

#### Refatorando: main.nim

```nim
import io_class
clear()

import entidades_class

let al = newAluno(1, "Beltrano", "06/08/1969")

echo "ID: ", al.id, ", Nome: ", al.nome, ", DataDeNasc.: ", al.dataDeNasc, ", Idade: ", al.idade
# --> saída:
# ID: 1, Nome: Beltrano, DataDeNasc.: 06/08/1969, Idade: 54
```

#### Módulo: entidades_class.nim

```nim
import times, strutils

let
  time        = getTime()
  anoCorrente = time.format("yyyy").parseInt()
  mesCorrente = time.format("MM").parseInt()
  diaCorrente = time.format("dd").parseInt()

type
  Pessoa = ref object of RootObj # Objeto privado somente acessível neste módulo.
    # A presença do asterisco torna os atributos públicos acessíveis
    # por outros módulos.
    nome*: string
    dataDeNasc*: string
    idade*: int

  # A presença do asterisco torna o objeto público acessível
  # por outros módulos.
  Aluno* = ref object of Pessoa # `Aluno` herda de `Pessoa`.
    # A presença do asterisco torna o atributo público acessível
    # por outros módulos.
    id*: int

# Função que retorna uma instância de `Aluno` c/ os dados
# enviados via parâmetros.
proc newAluno*(idNovo: int, nomeNovo, dataDeNascNovo: string): Aluno =
  # Atributos obtidos a partir de `dataDeNascNovo` a
  # serem utilizados p/ determinar a idade do aluno.
  let
    formato   = "dd/MM/yyyy"
    data      = parse(dataDeNascNovo, formato)
    anoDeNasc = int(data.year)
    mesDeNasc = int(data.month)
    diaDeNasc = int(data.monthday)

  var
    idade: int

  # Definindo a idade a partir dos atributos definidos
  # nesta função.
  if anoDeNasc < anoCorrente:
    idade = anoCorrente - anoDeNasc
    if mesCorrente < mesDeNasc:
      idade -= 1
    elif mesCorrente == mesDeNasc and diaCorrente < diaDeNasc:
      idade -= 1

# Duas formas de instânciar `Aluno`
# retornar esta instância:
# ---
# 1ª Forma:
# ---
  let aluno = new(Aluno)
  aluno.id         = idNovo
  aluno.nome       = nomeNovo
  aluno.dataDeNasc = dataDeNascNovo
  aluno.idade      = idade

  return aluno
# ---
# 2ª Forma: Não é necessário
# a instrução `return`:
# ---
discard """
  new(result)
  result.id         = idNovo
  result.nome       = nomeNovo
  result.dataDeNasc = dataDeNascNovo
  result.idade      = idade
"""
```

# Anexos

---

## <img src="icons/annex01.png" width=48/> Anexo I

### Nim - Regex

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
