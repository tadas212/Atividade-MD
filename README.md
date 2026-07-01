# Verificação de Programas

**Disciplina:** Verificação de Programas  
**Professor:** Edjard Mota

## Objetivo

Este trabalho tem como objetivo aplicar técnicas de verificação formal utilizando assertivas (`assert`) para analisar propriedades de correção e terminação de algoritmos.

Foram instrumentados dois programas contendo falhas propositais. Em ambos os casos foram adicionadas verificações de:

- Pré-condição;
- Inicialização do invariante;
- Manutenção do invariante;
- Função variante;
- Pós-condição.

A execução dos testes permitiu identificar automaticamente os erros presentes em cada algoritmo.

---

# Problema 6 – Busca do Maior Divisor Próprio

## Especificação

### Pré-condição

O valor de entrada deve ser um inteiro positivo.

n > 0

### Resultado esperado

A função deve retornar o maior divisor próprio de `n`, ou seja, o maior número menor que `n` que divide `n` sem deixar resto.
