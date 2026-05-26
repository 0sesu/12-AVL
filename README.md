# Árvores AVL (Adelson-Velsky e Landis)
---

## 📚 Introdução

Uma **Árvore AVL** é uma árvore binária de busca **auto-balanceada**, onde a diferença de altura entre as subárvores esquerda e direita de qualquer nó (chamada de **fator de balanceamento**) não pode ser maior que 1.

Esta propriedade garante que operações de busca, inserção e remoção sejam realizadas em tempo **O(log n)**, mesmo no pior caso.

**Escopo desta atividade:** este roteiro trabalha a **inserção em AVL** e as rotações necessárias para rebalancear a árvore após novas inserções. A remoção é citada como operação da estrutura, mas não será implementada neste exercício.

---

## 🎯 Objetivos de Aprendizagem

Ao concluir esta atividade, você deverá compreender:

1. **Árvore Binária de Busca (ABB)**: Estrutura base onde cada nó tem no máximo dois filhos, com valores à esquerda menores e à direita maiores.

2. **Altura de um Nó**: A distância do nó até a folha mais distante em sua subárvore.
   - Nó folha: altura = 0
   - Nó NULL: altura = -1

3. **Fator de Balanceamento (FB)**: Diferença entre a altura da subárvore esquerda e direita.
   ```
   FB = altura(subárvore esquerda) - altura(subárvore direita)
   ```
   - FB > 1: árvore "pesada" à esquerda
   - FB < -1: árvore "pesada" à direita
   - -1 ≤ FB ≤ 1: árvore balanceada

4. **Rotações**: Operações para rebalancear a árvore após inserções.

5. **Rastreamento Manual**: Acompanhar a volta da recursão, atualizando alturas e fatores de balanceamento antes de decidir se uma rotação é necessária.

---

## 🧭 Caminho da Inserção AVL

Antes de implementar as rotações, observe a ordem das etapas dentro de `insereArvore()`:

1. Inserir o valor seguindo as regras de uma Árvore Binária de Busca.
2. Ao retornar da chamada recursiva, recalcular a altura do nó atual.
3. Calcular o fator de balanceamento do nó atual.
4. Identificar se o caso é EE, DD, ED ou DE.
5. Aplicar a rotação simples ou dupla quando o FB sair do intervalo `-1`, `0`, `1`.
6. Retornar a nova raiz da subárvore analisada.

Esse caminho é importante porque o desbalanceamento costuma ser percebido na volta da recursão, não no momento exato em que o novo nó é criado.

---

## 🔄 Tipos de Rotações

> Nesta implementação de inserção, os casos são identificados comparando o `valor` inserido com o filho do nó desbalanceado. Em uma remoção AVL, que não faz parte desta atividade, a escolha da rotação costuma depender dos fatores de balanceamento dos filhos.

### 1. Rotação Simples à Direita

Utilizada quando ocorre desbalanceamento **Esquerda-Esquerda** (FB > 1 no nó y e valor inserido à esquerda do filho esquerdo).

**Visualização:**
```
        y                    x
       / \                  / \
      x   T3    =>         T1  y
     / \                      / \
    T1  T2                   T2 T3
```

**Passos da implementação:**
1. Armazene o filho esquerdo de `y` em `x`
2. Transfira a subárvore direita de `x` (T2) para a esquerda de `y`
3. Faça `y` se tornar filho direito de `x`
4. Recalcule as alturas de `y` e depois de `x`
5. Retorne `x` como nova raiz da subárvore

**Perguntas-guia:**
- Onde estão `T1`, `T2` e `T3` antes da rotação?
- Por que `T2` pode se tornar filho esquerdo de `y` sem quebrar a regra da ABB?
- Qual nó deve ter sua altura atualizada primeiro: `x` ou `y`?

### 2. Rotação Simples à Esquerda

Utilizada quando ocorre desbalanceamento **Direita-Direita** (FB < -1 no nó x e valor inserido à direita do filho direito).

**Visualização:**
```
      x                      y
     / \                    / \
    T1  y        =>        x   T3
       / \                / \
      T2 T3              T1 T2
```

**Passos da implementação:**
1. Armazene o filho direito de `x` em `y`
2. Transfira a subárvore esquerda de `y` (T2) para a direita de `x`
3. Faça `x` se tornar filho esquerdo de `y`
4. Recalcule as alturas de `x` e depois de `y`
5. Retorne `y` como nova raiz da subárvore

**Perguntas-guia:**
- Onde estão `T1`, `T2` e `T3` antes da rotação?
- Por que `T2` pode se tornar filho direito de `x` sem quebrar a regra da ABB?
- Qual nó deve ter sua altura atualizada primeiro: `x` ou `y`?

### 3. Rotação Dupla Esquerda-Direita

Usada em casos **Esquerda-Direita** (FB > 1 e valor inserido à direita do filho esquerdo).

**Solução:** 
1. Rotação esquerda no filho esquerdo
2. Rotação direita no nó desbalanceado

### 4. Rotação Dupla Direita-Esquerda

Usada em casos **Direita-Esquerda** (FB < -1 e valor inserido à esquerda do filho direito).

**Solução:**
1. Rotação direita no filho direito
2. Rotação esquerda no nó desbalanceado

---

## 💻 Estrutura do Código

### Estrutura do Nó
```cpp
struct NO {
    int valor;      // Dado armazenado
    NO* esq;        // Ponteiro para filho esquerdo
    NO* dir;        // Ponteiro para filho direito
    int altura;     // Altura do nó (para balanceamento)
};
```

### Funções Principais

- `insereArvore()`: Insere um elemento e rebalanceia se necessário
- `alturaNo()`: Retorna a altura de um nó
- `fatorBalanceamento()`: Calcula o FB de um nó
- `girarDireita()`: **[A IMPLEMENTAR]** Rotação à direita
- `girarEsquerda()`: **[A IMPLEMENTAR]** Rotação à esquerda
- `exibirElementosArvore()`: Exibe cada nó com valor, altura e FB para facilitar a conferência
- `liberarArvore()`: Libera os nós da árvore ao reinicializar ou encerrar o programa

---

## ✅ Atividade Proposta

Faça um fork deste repositório e realize as seguintes atividades:

### Tarefa 0: Rastrear uma inserção no papel

- [ ] Desenhe a inserção dos valores `30`, `20`, `10`
- [ ] Marque a altura de cada nó após cada inserção
- [ ] Calcule o FB de cada nó visitado na volta da recursão
- [ ] Identifique em qual nó aparece o primeiro desbalanceamento
- [ ] Só depois implemente a rotação correspondente

### Tarefa 1: Implementar `girarDireita()`

- [ ] Localize a função `NO* girarDireita(NO* y)` no arquivo `AVL.cpp`
- [ ] Siga os 5 passos comentados no código
- [ ] **Dica**: Use a visualização do diagrama como referência
- [ ] **Importante**: Não esqueça de recalcular as alturas após as rotações
- [ ] **Conferência**: Depois da rotação, o antigo nó raiz da subárvore deve ficar abaixo do novo nó raiz

**Exemplo de cálculo de altura:**
```cpp
no->altura = maior(alturaNo(no->esq), alturaNo(no->dir)) + 1;
```

### Tarefa 2: Implementar `girarEsquerda()`

- [ ] Localize a função `NO* girarEsquerda(NO* x)` no arquivo `AVL.cpp`
- [ ] Siga os 5 passos comentados no código
- [ ] **Dica**: Esta função é espelhada em relação à `girarDireita()`
- [ ] **Importante**: Mantenha a lógica simétrica
- [ ] **Conferência**: Depois da rotação, as alturas devem ser recalculadas de baixo para cima

---

## 🧪 Como Testar

1. Compile o programa:
 - Utilize o Visual Studio 2022 ou superior para compilar o código


2. Teste primeiro as rotações simples:
   - **Caso EE**: Inserir `30`, `20`, `10`. Resultado esperado: raiz `20`, com `10` à esquerda e `30` à direita.
   - **Caso DD**: Reinicializar e inserir `10`, `20`, `30`. Resultado esperado: raiz `20`, com `10` à esquerda e `30` à direita.

3. Depois teste as rotações duplas:
   - **Caso ED**: Reinicializar e inserir `30`, `10`, `20`. Resultado esperado: raiz `20`, com `10` à esquerda e `30` à direita.
   - **Caso DE**: Reinicializar e inserir `10`, `30`, `20`. Resultado esperado: raiz `20`, com `10` à esquerda e `30` à direita.

4. Use a opção "4 - Exibir árvore" após cada inserção. A saída mostra `valor (h=altura, FB=fator de balanceamento)`, o que permite conferir se todos os nós ficaram com FB entre `-1` e `1`.

---

## 📖 Referências e Recursos

- **Complexidade AVL**: Todas as operações em O(log n)
- **Inventores**: Georgy Adelson-Velsky e Evgenii Landis (1962)
- **Comparação**: AVL mantém balanceamento mais rígido que Red-Black Trees, resultando em buscas mais rápidas mas inserções/remoções ligeiramente mais lentas
- **Próximo passo natural**: Depois de dominar inserção e rotações, implemente remoção em AVL e compare como os critérios de rotação mudam

### Material Complementar
- [Visualização interativa de AVL](https://www.cs.usfca.edu/~galles/visualization/AVLtree.html)
- Desenhe cada rotação no papel para melhor compreensão
- Teste com diferentes sequências de inserção

---

## 🤔 Perguntas para Reflexão

1. Por que a complexidade permanece O(log n) mesmo no pior caso?
2. O que aconteceria se não fizéssemos o balanceamento?
3. Como as rotações preservam a propriedade de árvore binária de busca?
4. Quando precisamos de rotações duplas ao invés de simples?

---

**Boa sorte com a implementação! 🚀**
