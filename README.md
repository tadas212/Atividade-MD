# Verificação de Programas com Assertivas

**Disciplina:** Verificação de Programas  
**Professor:** Edjard Mota

---

# Introdução

A verificação de programas tem como objetivo demonstrar formalmente que um algoritmo satisfaz determinadas propriedades especificadas antes de sua execução. Entre essas propriedades destacam-se a correção parcial, a correção total e a terminação.

Neste trabalho foram analisados dois algoritmos contendo falhas propositais. Para cada um deles foram adicionadas assertivas responsáveis por verificar propriedades importantes do programa durante sua execução.

As verificações realizadas envolvem:

- Pré-condição;
- Inicialização do invariante;
- Manutenção do invariante;
- Função variante;
- Pós-condição.

A partir da execução de casos de teste foi possível identificar automaticamente os pontos onde os algoritmos deixam de satisfazer sua especificação formal.

---

# Metodologia de Verificação

A estratégia utilizada foi baseada nos conceitos estudados na disciplina.

Inicialmente foi definida uma especificação formal para cada problema, contendo pré-condições e pós-condições.

Em seguida foi escolhido um invariante de laço capaz de representar uma propriedade que deve permanecer verdadeira durante toda a execução.

Também foi definida uma função variante responsável por demonstrar o progresso do algoritmo em direção à condição de parada.

Por fim, assertivas foram inseridas em pontos estratégicos do código para verificar automaticamente essas propriedades durante a execução.

---

# Problema 6 – Busca do Maior Divisor Próprio

## Descrição

O objetivo do algoritmo é encontrar o maior divisor próprio de um número inteiro positivo.

Um divisor próprio é qualquer divisor positivo menor que o próprio número.

Por exemplo:

| Número | Maior divisor próprio |
|----------|----------|
| 8 | 4 |
| 9 | 3 |
| 12 | 6 |
| 15 | 5 |

---

## Especificação Formal

### Pré-condição

O algoritmo aceita apenas números inteiros positivos.

```
n > 0
```

### Pós-condição

O valor retornado deve satisfazer simultaneamente:

```
resultado < n
```

e

```
n % resultado = 0
```

Isso garante que o resultado é realmente um divisor próprio de n.

---

## Invariante Utilizado

Foi escolhido o seguinte invariante:

```
0 < d < n
```

onde d representa o candidato atual a divisor próprio.

Esse invariante garante que durante toda a execução o valor analisado permanece dentro do intervalo válido para a busca.

---

## Função Variante

A função variante escolhida foi:

```
V(state) = d
```

A cada repetição do laço o valor de d deveria diminuir.

Como d pertence ao conjunto dos números naturais e possui limite inferior igual a zero, sua redução contínua garante a terminação do algoritmo.

---

## Estratégia de Verificação

Foram adicionadas assertivas para verificar:

### Validade da entrada

```python
assert n > 0
```

### Inicialização correta do invariante

```python
assert 0 < d < n
```

### Preservação do invariante

```python
assert 0 < d < n
```

### Limite inferior da função variante

```python
assert velha_variante >= 0
```

### Decremento da função variante

```python
assert d < velha_variante
```

### Verificação da pós-condição

```python
assert resultado < n and n % resultado == 0
```

---

## Casos de Teste

| Entrada | Resultado Esperado |
|----------|----------|
| n = 8 | 4 |
| n = 9 | 3 |
| n = 15 | 5 |

---

## Resultado Observado

### Entrada

```
n = 8
```

### Saída

```
AssertionError:
Erro: Loop em execucao infinita (sem progresso)!
```

O mesmo comportamento ocorre para os demais testes.

---

## Discussão do Erro

A primeira assertiva que falha é:

```python
assert d < velha_variante
```

Essa assertiva verifica se a função variante está diminuindo.

O algoritmo original deveria executar:

```python
d = d - 1
```

sempre que o valor atual de d não fosse divisor de n.

Entretanto, essa instrução foi removida.

Consequentemente, o valor de d permanece constante durante toda a execução.

---

## Demonstração da Falha

Considere:

```
n = 9
```

Inicialmente:

```
d = 8
```

Como:

```
9 % 8 ≠ 0
```

o algoritmo deveria continuar a busca utilizando:

```
d = 7
```

Porém isso não acontece.

Na próxima iteração temos novamente:

```
d = 8
```

Logo:

```
V(estado atual) = 8
```

e

```
V(próximo estado) = 8
```

Mas a definição da função variante exige:

```
V(próximo estado) < V(estado atual)
```

Temos:

```
8 < 8
```

o que é falso.

Por isso a assertiva dispara um erro.

---

## Interpretação Formal

É importante observar que o invariante continua sendo satisfeito.

O valor de d permanece entre 0 e n durante toda a execução.

Portanto o problema não está relacionado à correção parcial.

A falha está associada exclusivamente à propriedade de terminação, uma vez que não existe progresso em direção ao fim do laço.

---

## Conclusão do Problema 6

As assertivas mostraram que o algoritmo perde sua garantia de terminação devido à ausência do decremento da variável d.

A função variante foi essencial para identificar automaticamente essa falha.

O erro encontrado é um erro de terminação e não um erro de correção.

---

# Problema Alternativo – Soma dos Primeiros N Naturais

## Descrição

Este algoritmo tem como objetivo calcular a soma dos primeiros n números naturais.

Matematicamente:

```
1 + 2 + 3 + ... + n
```

O resultado esperado é dado pela fórmula:

```
n(n + 1)
---------
    2
```

conhecida como Fórmula de Gauss.

---

## Especificação Formal

### Pré-condição

```
n >= 0
```

### Pós-condição

Ao final da execução deve ser verdadeiro que:

```
s = n(n + 1)/2
```

---

## Invariante Utilizado

Foi escolhido o seguinte invariante:

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

Esse invariante descreve exatamente o significado matemático das variáveis utilizadas durante a execução.

---

## Função Variante

A função variante escolhida foi:

```
V(state) = n - i
```

Ela representa a quantidade de iterações restantes.

Como i aumenta a cada repetição, a variante diminui continuamente até atingir zero.

---

## Estratégia de Verificação

Foram adicionadas assertivas para verificar:

### Pré-condição

```python
assert n >= 0
```

### Inicialização do invariante

```python
assert 0 <= i <= n and s == i * (i + 1) // 2
```

### Manutenção do invariante

```python
assert 0 <= i <= n and s == i * (i + 1) // 2
```

### Limite inferior da variante

```python
assert velha_variante >= 0
```

### Decremento da variante

```python
assert (n - i) < velha_variante
```

### Pós-condição

```python
assert s == n * (n + 1) // 2
```

---

## Casos de Teste

| Entrada | Resultado Esperado |
|----------|----------|
| n = 0 | 0 |
| n = 1 | 1 |
| n = 4 | 10 |

---

## Resultado Observado

### Entrada

```
n = 0
```

Resultado:

```
Nenhuma assertiva falhou.
```

### Entrada

```
n = 1
```

Resultado:

```
AssertionError:
Erro: Invariante violado no corpo do loop!
```

### Entrada

```
n = 4
```

Resultado:

```
AssertionError:
Erro: Invariante violado no corpo do loop!
```

---

## Discussão do Erro

A primeira assertiva que falha é:

```python
assert 0 <= i <= n and s == i * (i + 1) // 2
```

responsável pela manutenção do invariante.

O trecho defeituoso é:

```python
s = s + i
i = i + 1
```

A atualização da soma utiliza o valor antigo de i.

Isso faz com que o acumulador deixe de representar corretamente a soma dos valores processados.

---

## Demonstração da Falha

Considere:

```
n = 4
```

Estado inicial:

```
s = 0
i = 0
```

Após executar:

```python
s = s + i
```

temos:

```
s = 0
```

Após:

```python
i = i + 1
```

obtemos:

```
i = 1
```

Segundo o invariante:

```
s = i(i + 1)/2
```

Deveríamos ter:

```
s = 1
```

Mas o valor calculado foi:

```
s = 0
```

Logo:

```
0 ≠ 1
```

e a assertiva falha.

---

## Interpretação Formal

Diferentemente do Problema 6, a função variante continua diminuindo normalmente.

O algoritmo progride em direção à condição de parada.

Portanto a terminação permanece garantida.

O problema ocorre porque a propriedade matemática descrita pelo invariante deixa de ser preservada entre duas iterações consecutivas.

Trata-se de uma falha de correção.

---

## Relação com o Método Indutivo

O invariante funciona como uma hipótese de indução.

A ideia é que, se ele for verdadeiro em uma determinada iteração, também deve permanecer verdadeiro na próxima.

Entretanto, devido ao erro na atualização da soma, essa propriedade deixa de ser preservada.

Assim, a prova indutiva falha exatamente na etapa de manutenção do invariante.

---

## Conclusão do Problema Alternativo

As assertivas permitiram identificar automaticamente uma violação da correção do algoritmo.

Embora o programa continue avançando até sua condição de parada, o resultado produzido deixa de satisfazer a especificação matemática esperada.

A quebra do invariante evidencia que a atualização das variáveis foi implementada de forma incorreta.

---

# Comparação dos Problemas

Os dois programas analisados apresentam falhas diferentes.

No Problema 6, o invariante permanece válido, mas a função variante deixa de diminuir. Isso caracteriza uma falha de terminação.

No Problema Alternativo, a função variante continua diminuindo normalmente, porém o invariante deixa de ser preservado. Nesse caso a falha está relacionada à correção do algoritmo.

Dessa forma, os exemplos ilustram duas situações distintas estudadas na disciplina: erros de terminação e erros de correção.

---

# Conclusão Geral

A utilização de assertivas mostrou-se uma ferramenta eficiente para a verificação automática de propriedades formais em programas.

A análise realizada permitiu identificar precisamente os pontos em que cada algoritmo deixa de satisfazer sua especificação.

Além disso, os experimentos demonstraram a importância dos invariantes de laço para a prova de correção e das funções variantes para a prova de terminação, evidenciando como esses conceitos podem ser utilizados para detectar erros de forma sistemática e fundamentada.
