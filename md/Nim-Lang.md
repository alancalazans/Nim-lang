# Nim-Lang

## Compilação

**Exemplo:**

*ola.nim*

```ruby
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

```ruby
# -----------------------------------------
# Variáveis mutáveis.
# Tipagem estática:
var valor1: int = 10
# -----------------------------------------
# Declarando sem valor a prióri:
var valor2: int
# -----------------------------------------
# Atribuindo valor:
valor2 = 20
# -----------------------------------------
# Inferindo o tipo:
var valor3 = 30
# -----------------------------------------
# Declarando em bloco.
# Obs.: No Nim, a indentação por tabulação
# não é permitido mas somente por espaço.
var
  valor4 = -10
  valor5 = "Olá"
  valor6 = '!'
# Variáveis acima são mutáveis mas seu tipo não,
# logo, a reatribuição: valor5 = 50 causará erro.
# -----------------------------------------
# Nim não faz distinção entre maiúsculas,
# minúsculas e sublinhados portanto as
# variáveis: 'contaRegistros' e
# 'conta_registros' são a mesma.
var contaRegistros: int = 5
echo conta_registros # 5
# -----------------------------------------
# Variáveis imutáveis.
# 'const' e 'let'.
# No tipo 'const' o valor tem de ser conhecido
# em tempo de compilação.
const pi = 3.14
# No tipo 'let' o valor não precisa ser conhecido
# em tempo de compilação, mas, uma vez atribuído
# não muda.
var cem = 100
let porcento = 1 / cem
echo 4 * porcento # 0.04
```

