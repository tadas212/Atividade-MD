# Verificação de Programas com Assertivas

**Disciplina:** Verificação de Programas  
**Professor:** Edjard Mota

## Integrantes

- Nome 1
- Nome 2
- Nome 3
- Nome 4

---

# Objetivo

Este trabalho tem como objetivo aplicar conceitos de Verificação de Programas estudados na disciplina, utilizando assertivas para verificar automaticamente propriedades de correção e terminação de algoritmos.

Para cada problema foram adicionadas verificações referentes a:

- Pré-condição;
- Inicialização do invariante;
- Manutenção do invariante;
- Função variante;
- Pós-condição.

Após a instrumentação dos algoritmos, foram executados casos de teste para identificar e localizar os erros presentes nos programas originais.

---

# Estrutura do Projeto

```
Projeto/

├── problema6.py
├── ProblemaAlternativo.py
└── README.md
```

---

# Problema 6 — Busca do Maior Divisor Próprio

## Descrição do Problema

O objetivo deste algoritmo é encontrar o maior divisor próprio de um número inteiro positivo.

Um divisor próprio de um número n é qualquer divisor positivo menor que n.

Exemplos:

| n | Maior divisor próprio |
|---|---|
| 8 | 4 |
| 9 | 3 |
| 15 | 5 |
| 25 | 5 |

---

## Especificação Formal

### Pré-condição

O algoritmo só aceita valores inteiros positivos.

```
n > 0
```

### Pós-condição

O resultado retornado deve satisfazer simultaneamente:

```
resultado < n
```

e

```
n % resultado = 0
```

Ou seja, o valor retornado deve ser um divisor próprio de n.

---

## Invariante de Loop

Foi utilizado o seguinte invariante:

```
0 < d < n
```

onde d representa o candidato atual a maior divisor próprio.

O invariante garante que durante toda a execução do laço o valor de d permanece válido para a busca.

---

## Função Variante

A função variante escolhida foi:

```
V(state) = d
```

Essa função mede o progresso do algoritmo.

A cada repetição do laço o valor de d deveria diminuir em uma unidade.

Como d é um número natural limitado inferiormente por 0, sua redução contínua garante que o laço eventualmente termine.

---

## Assertivas Inseridas

### 1. Pré-condição

Verifica se a entrada é válida.

```python
assert n > 0
```

---

### 2. Inicialização do Invariante

Após a atribuição:

```python
d = n - 1
```

deve ser verdadeiro que:

```python
assert 0 < d < n
```

Isso garante que o invariante é válido antes da primeira iteração.

---

### 3. Manutenção do Invariante

Ao final de cada iteração é verificado se o invariante continua verdadeiro.

```python
assert 0 < d < n
```

---

### 4. Função Variante

São verificadas duas propriedades.

#### Limite Inferior

```python
assert velha_variante >= 0
```

Garante que a variante nunca assume valores negativos.

#### Decremento

```python
assert d < velha_variante
```

Garante que existe progresso em direção à condição de parada.

---

### 5. Pós-condição

Quando um resultado é encontrado, verifica-se:

```python
assert resultado < n and n % resultado == 0
```

Garantindo que o valor retornado realmente satisfaz a especificação.

---

## Data Set Utilizado

| Entrada | Resultado Esperado |
|----------|----------|
| n = 8 | 4 |
| n = 9 | 3 |
| n = 15 | 5 |

---

## Resultado da Execução

### Caso 1

Entrada:

```
n = 8
```

Resultado:

```
AssertionError:
Erro: Loop em execucao infinita (sem progresso)!
```

---

### Caso 2

Entrada:

```
n = 9
```

Resultado:

```
AssertionError:
Erro: Loop em execucao infinita (sem progresso)!
```

---

### Caso 3

Entrada:

```
n = 15
```

Resultado:

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

responsável pela verificação do progresso da função variante.

O algoritmo deveria executar:

```python
d = d - 1
```

sempre que o valor atual de d não fosse divisor de n.

Entretanto, essa instrução foi removida do código.

Como consequência, o valor de d permanece constante.

---

## Demonstração da Falha

Considere a entrada:

```
n = 9
```

Inicialmente:

```
d = 8
```

Temos:

```
9 % 8 ≠ 0
```

Portanto o algoritmo deveria reduzir d.

Entretanto, o decremento não acontece.

Na próxima iteração:

```
d = 8
```

novamente.

Logo:

```
V(state atual) = 8
V(próximo estado) = 8
```

Mas a definição da variante exige:

```
V(próximo estado) < V(state atual)
```

Temos então:

```
8 < 8
```

o que é falso.

Por isso a assertiva dispara um erro.

---

## Razão Matemática do Bug

A função variante foi escolhida justamente para demonstrar que o laço está se aproximando de sua condição de parada.

Quando uma variante deixa de diminuir, perde-se a garantia de terminação.

Assim, embora o algoritmo continue satisfazendo o invariante, ele deixa de satisfazer a propriedade de progresso.

O erro encontrado é, portanto, um erro de terminação e não um erro de correção.

---

## Conclusão

As assertivas permitiram verificar automaticamente propriedades importantes do algoritmo.

A falha identificada ocorre porque a função variante deixa de diminuir devido à ausência do comando de decremento da variável d.

Dessa forma, o algoritmo perde sua garantia de terminação e pode entrar em execução infinita.

---

# Problema Alternativo — Soma dos Primeiros N Naturais

## Descrição do Problema

O objetivo do algoritmo é calcular a soma dos primeiros n números naturais.

Matematicamente:

```
1 + 2 + 3 + ... + n
```

cujo resultado é dado por:

```
n(n + 1)
---------
    2
```

---

## Especificação Formal

### Pré-condição

```
n >= 0
```

---

### Pós-condição

Ao término da execução deve ser verdadeiro que:

```
s = n(n + 1)/2
```

---

## Invariante de Loop

Foi utilizado o seguinte invariante:

```
0 <= i <= n
```

e

```
s = i(i + 1)/2
```

onde:

- i representa a quantidade de números já processados;
- s representa a soma acumulada.

---

## Justificativa do Invariante

Antes de iniciar o laço:

```
i = 0
s = 0
```

Logo:

```
s = 0(0 + 1)/2
```

```
s = 0
```

Portanto o invariante é verdadeiro inicialmente.

---

## Função Variante

A função variante escolhida foi:

```
V(state) = n - i
```

Ela representa quantas iterações ainda faltam para o término do algoritmo.

A cada repetição do laço o valor de i aumenta em uma unidade.

Consequentemente:

```
n - i
```

diminui continuamente até atingir zero.

---

## Assertivas Inseridas

### 1. Pré-condição

```python
assert n >= 0
```

---

### 2. Inicialização do Invariante

```python
assert 0 <= i <= n and s == i * (i + 1) // 2
```

---

### 3. Manutenção do Invariante

```python
assert 0 <= i <= n and s == i * (i + 1) // 2
```

---

### 4. Função Variante

#### Limite Inferior

```python
assert velha_variante >= 0
```

#### Decremento

```python
assert (n - i) < velha_variante
```

---

### 5. Pós-condição

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

### Caso 1

Entrada:

```
n = 0
```

Resultado:

```
Nenhuma assertiva falhou.
```

---

### Caso 2

Entrada:

```
n = 1
```

Resultado:

```
AssertionError:
Erro: Invariante violado no corpo do loop!
```

---

### Caso 3

Entrada:

```
n = 4
```

Resultado:

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

---

## Demonstração da Falha

Considere a entrada:

```
n = 4
```

Inicialmente:

```
s = 0
i = 0
```

O algoritmo executa:

```python
s = s + i
```

obtendo:

```
s = 0
```

Em seguida:

```python
i = i + 1
```

obtendo:

```
i = 1
```

Nesse momento o invariante exige:

```
s = 1(1 + 1)/2
```

ou seja:

```
s = 1
```

Entretanto:

```
s = 0
```

Logo:

```
0 ≠ 1
```

Portanto o invariante é violado.

---

## Razão Matemática do Bug

O erro ocorre porque a atualização do acumulador utiliza o valor antigo de i.

Na primeira iteração o algoritmo realiza:

```
0 + 0
```

quando deveria estar considerando o próximo elemento da sequência.

Como consequência, a soma acumulada deixa de representar corretamente os números processados.

A propriedade de manutenção do invariante é quebrada já na primeira passagem pelo laço.

---

## Relação com Indução Fraca

O invariante funciona como uma hipótese de indução.

Se ele é verdadeiro antes da iteração k, deveria continuar verdadeiro após a iteração k + 1.

Entretanto, o bug faz com que essa propriedade não seja preservada.

Assim, a prova indutiva falha exatamente no passo de manutenção do invariante.

---

## Conclusão

As assertivas permitiram identificar automaticamente uma falha de correção no algoritmo.

A função variante continua diminuindo normalmente, demonstrando que não existe problema de terminação.

O erro está exclusivamente na atualização incorreta das variáveis dentro do laço, o que faz com que o invariante deixe de ser preservado e o resultado produzido deixe de satisfazer a especificação matemática.

---

## Como Executar

No terminal, execute:

```bash
python problema6.py
```

ou

```bash
python ProblemaAlternativo.py
```

Os programas executarão os testes e exibirão as mensagens produzidas pelas assertivas.
