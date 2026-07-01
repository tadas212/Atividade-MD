# Verificação de Programas

**Disciplina:** Verificação de Programas  
**Professor:** Edjard Mota

## Objetivo

Este trabalho tem como objetivo aplicar técnicas de verificação formal utilizando assertivas em Python para verificar propriedades de correção e terminação de algoritmos.

Foram analisados dois programas contendo erros propositais. Em ambos os casos foram adicionadas assertivas para verificar:

- Pré-condição;
- Inicialização do invariante;
- Manutenção do invariante;
- Função variante;
- Pós-condição.

A execução dos testes permitiu identificar automaticamente as falhas presentes em cada algoritmo.

---

# Problema 6 – Busca do Maior Divisor Próprio

## Especificação

### Pré-condição

O algoritmo aceita apenas números inteiros positivos.

```
n > 0
```

### Resultado esperado

A função deve retornar o maior divisor próprio de `n`, ou seja, o maior número menor que `n` que divide `n` sem deixar resto.

Exemplos:

| Entrada | Resultado Esperado |
|----------|----------|
| n = 8 | 4 |
| n = 9 | 3 |
| n = 15 | 5 |

---

## Assertivas Inseridas

### 1. Pré-condição

Verifica se a entrada recebida é válida.

```python
assert n > 0
```

### 2. Inicialização do Invariante

Foi utilizado o seguinte invariante:

```
0 <= d < n
```

Após a inicialização:

```python
d = n - 1
```

o invariante deve ser verdadeiro.

Assertiva utilizada:

```python
assert 0 <= d < n
```

### 3. Manutenção do Invariante

Ao final de cada repetição do laço verifica-se se o valor de `d` continua satisfazendo o invariante.

```python
assert 0 <= d < n
```

### 4. Função Variante

A função variante escolhida foi:

```
V(state) = d
```

Foram verificadas duas propriedades.

#### Limite inferior

```python
assert velha_variante >= 0
```

#### Decremento

```python
assert d < velha_variante
```

### 5. Pós-condição

Ao término da execução verifica-se se o valor retornado é realmente um divisor próprio de `n`.

```python
assert resultado < n and n % resultado == 0
```

---

## Data Set Utilizado

| Entrada | Resultado Esperado |
|----------|----------|
| n = 8 | 4 |
| n = 9 | 3 |
| n = 15 | 5 |

---

## Resultado da Execução

### Entrada

```
n = 9
```

### Resultado

```
AssertionError:
Erro: Loop em execucao infinita (sem progresso)!
```

---

## Análise da Falha

A primeira assertiva que falha é:

```python
assert d < velha_variante
```

correspondente à verificação da função variante.

Quando `n % d != 0`, o algoritmo deveria executar:

```python
d = d - 1
```

Entretanto, essa instrução foi removida do programa.

Dessa forma, o valor de `d` permanece constante entre duas iterações consecutivas, impedindo o progresso do laço.

---

## Razão da Falha

A função variante escolhida foi:

```
V(state) = d
```

Para garantir a terminação do algoritmo, a variante deve diminuir estritamente a cada repetição.

Como o valor de `d` permanece inalterado, temos:

```
V(novo estado) = V(estado anterior)
```

Logo, a propriedade de progresso é violada.

A assertiva detecta essa situação e interrompe a execução, evitando um possível loop infinito.

---

## Conclusão

A utilização de assertivas permitiu verificar automaticamente propriedades importantes do algoritmo.

A falha encontrada está relacionada à terminação do programa. A ausência do decremento da variável `d` impede a redução da função variante e faz com que o algoritmo deixe de satisfazer as condições necessárias para garantir o término do laço.

---

# Problema Alternativo – Soma dos Primeiros N Naturais

## Objetivo

Este problema tem como objetivo verificar a corretude de um algoritmo que calcula a soma dos primeiros números naturais utilizando assertivas em Python.

Foram inseridas verificações de:

- Pré-condição;
- Inicialização do invariante;
- Manutenção do invariante;
- Função variante;
- Pós-condição.

---

## Especificação

### Pré-condição

O algoritmo aceita apenas números inteiros não negativos.

```
n >= 0
```

### Resultado esperado

A função deve calcular:

```
1 + 2 + 3 + ... + n
```

que pode ser representado pela fórmula:

```
n(n + 1)
---------
    2
```

---

## Assertivas Inseridas

### 1. Pré-condição

Verifica se o valor recebido é válido.

```python
assert n >= 0
```

### 2. Inicialização do Invariante

Foi utilizado o seguinte invariante:

```
s = i(i + 1) / 2
```

Inicialmente:

```python
s = 0
i = 0
```

Como:

```
0(0 + 1)/2 = 0
```

o invariante é verdadeiro.

Assertiva utilizada:

```python
assert 0 <= i <= n and s == i * (i + 1) // 2
```

### 3. Manutenção do Invariante

Ao final de cada repetição do laço é verificado se o invariante continua válido.

```python
assert 0 <= i <= n and s == i * (i + 1) // 2
```

### 4. Função Variante

A função variante escolhida foi:

```
V(state) = n - i
```

Foram verificadas duas propriedades.

#### Limite inferior

```python
assert velha_variante >= 0
```

#### Decremento

```python
assert (n - i) < velha_variante
```

### 5. Pós-condição

Ao final da execução verifica-se se o resultado corresponde à especificação matemática.

```python
assert s == n * (n + 1) // 2
```

---

## Data Set Utilizado

| Entrada | Resultado Esperado |
|----------|----------|
| n = 0 | 0 |
| n = 1 | 1 |
| n = 4 | 10 |

---

## Resultado da Execução

### Entrada

```
n = 4
```

### Resultado

```
AssertionError:
Erro: Invariante violado no corpo do loop!
```

---

## Análise da Falha

A primeira assertiva que falha é:

```python
assert 0 <= i <= n and s == i * (i + 1) // 2
```

correspondente à manutenção do invariante.

Na primeira iteração temos:

```
s = 0
i = 0
```

O algoritmo executa:

```python
s = s + i
i = i + 1
```

obtendo:

```
s = 0
i = 1
```

Nesse momento o invariante exige:

```
s = 1
```

Porém:

```
s = 0
```

Logo:

```
0 ≠ 1
```

e a assertiva dispara um `AssertionError`.

---

## Razão da Falha

O erro ocorre porque o acumulador é atualizado utilizando o valor antigo de `i`.

Na primeira repetição é realizada a operação:

```
0 + 0
```

quando deveria ser considerada a próxima parcela da sequência.

Como consequência, o valor acumulado deixa de representar corretamente a soma dos números processados, fazendo com que o invariante seja violado já na primeira iteração.

---

## Conclusão

A utilização de assertivas permitiu identificar automaticamente uma falha de correção no algoritmo.

A função variante continua diminuindo normalmente, demonstrando que a terminação do laço não é o problema. O erro está na atualização incorreta das variáveis dentro do corpo do algoritmo, o que faz com que o invariante deixe de ser satisfeito e o resultado produzido seja incorreto.

---

## Como Executar

Execute os arquivos pelo terminal:

```bash
python problema6.py
```

```bash
python ProblemaAlternativo.py
```

Os programas executarão os testes definidos e exibirão as mensagens geradas pelas assertivas.
