# Nim-Lang

## Compilação

**Exemplo:**

*ola.nim*

```php
echo "Olá Mundo!"
```

**Compilando:**

```sh
# Para compilar
nim c ola.nim
# Para compilar e executar
nim c -r ola.nim # Olá Mundo!
```

*saída:*

```sh
$ ./ola 
Olá Mundo!
```

## Variáveis e Constantes

```php
# ---
# Variáveis mutáveis.
# Tipagem estática:
var valor1: int = 10
# ---
# Declarando sem valor a prióri:
var valor2: int
# ---
# Atribuindo valor:
valor2 = 20
# ---
# Inferindo o tipo:
var valor3 = 30
# ---
# Declarando em bloco.
# Obs.: No Nim, a indentação por tabulação
# não é permitido mas somente por espaço.
var
  valor4 = -10 # Tipo 'int'
  valor5 = "Olá" # Tipo 'string'
  valor6 = '!' # Tipo 'character'
# Variáveis acima são mutáveis mas seu tipo não,
# logo, a reatribuição: valor5 = 50 causará erro.
# ---
# Nim não faz distinção entre maiúsculas,
# minúsculas e sublinhados portanto as
# variáveis: 'contaRegistros' e
# 'conta_registros' são a mesma.
var contaRegistros: int = 5
echo conta_registros # 5
# ---
# Variáveis imutáveis.
# 'const' e 'let'.
# Na instrução 'const' o valor tem de ser
# conhecido em tempo de compilação.
const pi = 3.14
# Na instrução 'let' o valor não precisa ser conhecido
# em tempo de compilação, mas, uma vez atribuído
# um valor este não muda (nem o seu tipo).
var cem = 100
let porcento = 1 / cem
echo 4 * porcento # 0.04
```

## Tipos de dados básicos

**Inteiros:**

```php
# Underline (_) pode ser usado para separar milhares
var dezMilhoes = 10_000_000
echo dezMilhoes # 10000000
let
  a = 11
  b = 4
echo "a + b = ", a + b # a + b = 15
echo "a - b = ", a - b # a - b = 7
echo "a * b = ", a * b # a * b = 44
echo "a / b = ", a / b # a / b = 2.75
echo "a div b = ", a div b # a div b = 2
echo "a mod b = ", a mod b # a mod b = 3
```

**Flutuantes:**

```php
# 2e3 = 2*10³
echo 2e3 # 2000.0
# Operadores 'div' e 'mod' não
# são definidos para flutuadores.
let
  c = 3.5
  d = 2.5
echo "c + d = ", c + d # c + d = 6.0
echo "c - d = ", c - d # c - d = 1.0
echo "c * d = ", c * d # c * d = 8.75
echo "c / d = ", c / d # c / d = 1.4
# multiplicação e divisão têm prioridade
# mais alta do que adição e subtração.
echo 2 + 3 * 4 # 14
echo 24 - 8 / 4 # 22.0
```

## Convertendo floats e inteiros

```php
let
  e = 5
  f = 2.6
# echo e + f # error
echo "float(e) = ", float(e) # 5.0
echo "int(f) = ", int(f) # 2 (não faz arredondamento)
echo "---"
echo "e + int(f) = ", e + int(f) # e + int(f) = 7
echo "float(e) + f = ", float(e) + f # float(e) + f = 7.6
```

## Characters

```php
# Caracteres são escritos entre aspas simples
let
  h = 'z'
  i = '+'
  j = '2'
  newLine = '\n'
  tab = '\t'
  k = '35' # error
  l = 'xy' # error
```

## Strings

```php
let
  m = "palavra"
  n = "Esta é uma frase."
  o = "" # String vazia
  p = "32" # Não é um número
  q = "!" # Embora contenha um só caracter é uma string
```

## Concatenação de string

```php
var
  frase = "Ser ou não ser "
  continuacao = "eis a questão?"
  frase2 = "Vida longa "
  continuacao2 = "ao rei!"
# Integrando o conteúdo de
# 'continuacao' a 'frase'
frase.add(continuacao)
echo "Frase: ", frase # Frase: Ser ou não ser eis a questão?
# Imprimindo 'frase2' e sua
# 'continuacao2'
echo "Concat: ", frase2 & continuacao2 # Concat: Vida longa ao rei!
```

## Operadores relacionais

```php
let
  n1 = 10
  n2 = 20
echo "n1 > n2: ", n1 > n2 # false
echo "n1 < n2: ", n1 < n2 # true
echo "n1 igual n2: ", n1 == n2 # false
echo "n1 diferente n2: ", n1 != n2 # true
echo "n1 maior igual n2: ", n1 >= n2 # false
echo "n1 menor igual n2: ", n1 <= n2 # true
# Comparação letras e strings
let
  l1 = 'a'
  l2 = 'b'
  s1 = "Fulano"
  s2 = "Cicrano"
echo "l1 < l2: ", l1 < l2 # true
echo "s1 < s2: ", s1 < s2 # false
```

## Operadores lógicos

```php
echo "true and true: ", true and true # true
echo "true and false: ", true and false # false
echo "false and false: ", false and false # false
echo "---"
echo "true or true: ", true or true # true
echo "true or false: ", true or false # true
echo "false or false: ", false or false # false
echo "---"
echo "true xor true: ", true xor true # false
echo "true xor false: ", true xor false # true
echo "false xor false: ", false xor false # false
echo "---"
echo "not true: ", not true # false
echo "not false: ", not false # true
```

## Controle de fluxo

### *if*

```php
let
  a = 10
  b = 20
  c = 30
if a < b: # true
  echo "a é menor que b" # a é menor que b
  if 10*a < b: # false
    echo "10*a é menor que b"
if b < c: # true
  echo "b é menor que c" # b é menor que c
  if 10*b < c: # false
    echo "10*b é menor que c"
if a+b == c: # true
  echo "Sim! a+b é igual a c" # Sim! a+b é igual a c
  if a+b <= b+c: # true
    echo "a+b é menor igual a b+c" # a+b é menor igual a b+c
```

### *else*

```php
let
  a = 15
  b = 5
if a < 10: # false
  echo "a é menor que 10"
else:
  echo "a é maior que 10" # a é maior que 10
if b < 10: # true
  echo "b é menor que 10" # b é maior que 10
else:
  echo "b é maior que 10"
```

### *elif*

```php
let
  a = 3000
  b = 7
if a < 10: # false
  echo "a é menor que 10"
elif a < 100: # false
  echo "a esta entre 10 e 100"
elif a < 1000: # false
  echo "a esta entre 100 e 1000"
else:
  echo "a é maior que 1000" # a é maior que 1000
if b < 1000: # true (entra neste bloco 'if' e ignora o resto)
  echo "b é menor que 1000" # b é menor que 1000
elif b < 100:
  echo "b é menor que 100"
elif b < 10: 
  echo "b é menor que 10"
```

### *case*

```php
let x = 7
case x
of 5:
  echo "Cinco!"
of 7:
  echo "Sete!" # Sete!
of 10:
  echo "Dez!"
else:
  echo "Número desconhecido"
```

### *case* - Escolha fechada (discartando alternativa de acão)

```php
let h = 'y'
case h
of 'x':
  echo "Você escolheu x"
of 'y':
  echo "Você escolheu y" # Você escolheu y
of 'z':
  echo "Você escolheu z"
else: discard
```

### *multiple Case*

```php
let i = 7
case i
of 0:
  echo "i é zero"
of 1, 3, 5, 7, 9:
  echo "i é ímpar" # i é ímpar
of 2, 4, 6, 8:
  echo "i é par"
else:
  echo "i é muito grande"
```

## Loops

### *for*

```php
for n in 5 .. 9: # [5, 9]
  echo n # Em cada linha: 5, 6, 7, 8, 9
echo "---"
for n in 5 ..< 9: # [5, 9[
  echo n # Em cada linha: 5, 6, 7, 8
echo "---"
for n in countup(0, 16, 4): # [0, 16] de 4 em 4
  echo n # Em cada linha: 0, 4, 8, 12, 16
echo "---"
for n in countdown(4, 0): # [4, 0]
  echo n # Em cada linha: 4, 3, 2, 1, 0
echo "---"
for n in countdown(-3, -9, 2): # [-3, -9] de 2 em 2
  echo n # Em cada linha: -3, -5, -7, -9
echo "---"
let palavra = "alfabeto"
for letra in palavra:
  echo letra # Em cada linha: a, l, f, a, b, e, t, o
echo "---"
# for incluindo contador (i)
for i, letra in palavra:
  echo "letra ", i, " é: ", letra # letra 0 é: a
                                  # letra 1 é: l
                                  # letra 2 é: f
                                  # letra 3 é: a
                                  # letra 4 é: b
                                  # letra 5 é: e
                                  # letra 6 é: t
                                  # letra 7 é: o
```

### *while*

```php
var a = 1
while a*a < 10:
  echo "a é: ", a # a é: 1
                  # a é: 2
                  # a é: 3
  inc a # incremento de a, poderia ser:
        # a = a + 1 ou a += 1
echo "valor final de a: ", a # valor final de a: 4
```

## break e continue

### *break*

```php
var i = 1
while i < 1000:
  if i == 3:
    break
  echo i # 1
         # 2
  inc i
```

### *continue*

```php
for i in 1 .. 5:
  if (i == 2) or (i == 4):
    continue
  echo i # 1
         # 3
         # 5
```

## Containers

