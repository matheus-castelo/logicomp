# LISTA 1 -  FÓRMULAS DA LÓGICA PROPOSICIONAL
## 1- Defina um pseudocódigo recursivo para a função number_of_connectives(A) que retorna a quantidade de conectivos da fórmula de entrada A. Por exemplo, number_of_connectives(((¬p) → (¬q))) = 3. Em seguida, você deve usar o código disponível em https://github.com/thiagoalvesifce/logicomp e escrever um código para a função number_of_connectives(formula).
- Definindo em pseudocódigo:
![alt text](image.png)
- Código para função number_of_connectives(formula)
`````python
def formula_size(formula):
    if isinstance(formula, Atom):
        return 1
    elif isinstance(formula, Not):
        return 1 + formula_size(formula.inner)
    elif isinstance(formula, (And, Or, Implies)):
        return 1 + formula_size(formula.left) + formula_size(formula.right)

print(formula_size(formula))
``````
## Conforme a definição de fórmula da lógica proposicional, os conectivos binários devem ser escritos na forma infixa, ou seja, devem ser escritos entre duas fórmulas. Essa definição poderia ser modificada possibilitando escrever os conectivos na notação polonesa, conforme indicado pelas correspondências a seguir:

- A fórmula A atômica corresponde à fórmula A na notação polonesa,
- (¬A) corresponde a ¬A,
- (A ∧ B) corresponde a ∧AB,
- (A ∨ B) corresponde a ∨AB,
- (A → B) corresponde a →AB.

Escreva as fórmulas a seguir utilizando a notação polonesa:

(a) ¬(p → ¬q)
- Resposta: ¬→p¬q

(b) ((¬¬p ∨ q) → (p → q))
- Resposta: →∨¬¬pq→pq
