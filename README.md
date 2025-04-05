OBS: As imagens que estão no repositório podem ser ignoradas. Elas só servem para fazer referência ao Markdown. Costumo utilizar o Notion que já cuida dessa parte, mas para botar no Github ficaria mais fácil fazer direto pelo Markdown mesmo.
----
# LISTA 1 -  FÓRMULAS DA LÓGICA PROPOSICIONAL
----
## Questão 1
1- Defina um pseudocódigo recursivo para a função number_of_connectives(A) que retorna a quantidade de conectivos da fórmula de entrada A. Por exemplo, number_of_connectives(((¬p) → (¬q))) = 3. Em seguida, você deve usar o código disponível em https://github.com/thiagoalvesifce/logicomp e escrever um código para a função number_of_connectives(formula).
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
----
## Questão 2
Conforme a definição de fórmula da lógica proposicional, os conectivos binários devem ser escritos na forma infixa, ou seja, devem ser escritos entre duas fórmulas. Essa definição poderia ser modificada possibilitando escrever os conectivos na notação polonesa, conforme indicado pelas correspondências a seguir:

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
----
## Questão 3
Defina recursivamente um pseudocódigo para a função `atoms(A)` que retorna o  
conjunto de todas as fórmulas atômicas que ocorrem em A. Por exemplo,  
`atoms(p ∧ ¬(p → ¬q) ∨ ¬q) = {p, q}`.  

Em seguida, você deve usar o repositório disponível em  
https://github.com/thiagoalvesifce/logicomp  
e escrever um código para definir a função `atoms(formula)`. Por exemplo,  

```python
atoms(Or(Not(And(Atom('p'), Atom('choveu_ontem'))), Atom('p')))
````
deve retornar um conjunto com as atômicas `Atom('p')` e `Atom('choveu_ontem')`.
- Resposta: 
````python
def formula_atoms(formula):
    if isinstance(formula, Atom):
        return {formula.name} 
    elif isinstance(formula, Not):
        return formula_atoms(formula.inner)
    elif isinstance(formula, (And, Or, Implies)):
        return formula_atoms(formula.left) | formula_atoms(formula.right)


print("Questão 2 - Atoms da Fórmula:", formula_atoms(formula))
````
----
## Questão 4
Uma fórmula está na forma normal da negação (NNF - do inglês: negation normal  
form) se a negação só é aplicada diretamente nas atômicas e os outros únicos  
conectivos permitidos são a conjunção e a disjunção. Por exemplo,  
`((p ∧ (¬(q ∧ r))) ∧ (¬r)) ∨ s` não está na NNF e  
`((p ∧ ((¬q) ∧ r)) ∧ (¬r)) ∨ s` está na NNF.  

Defina um pseudocódigo para a função `is_negation_normal_form(A)` para verificar  
se A está na NNF. Em seguida, você deve usar o repositório disponível em  
https://github.com/thiagoalvesifce/logicomp  
e escrever um código para a função `is_negation_normal_form(formula)`.
- Resposta:
````python
def is_negation_formal_formula(formula):
    if isinstance(formula, Atom):
        return True
    elif isinstance(formula, Not):
        return isinstance(formula.inner, Atom)
    elif isinstance(formula, (And, Or, Implies)):
        return is_negation_formal_formula(formula.left) and is_negation_formal_formula(formula.right)
    else:
        return False


print("Questao 3 - Eh uma Formula Negativa? ", is_negation_formal_formula(formula))
````
----
## Questão 5
Conforme a definição de fórmula da lógica proposicional, os conectivos binários devem  
ser escritos na forma infixa, ou seja, devem ser escritos entre duas fórmulas. Essa  
definição poderia ser modificada possibilitando escrever os conectivos na notação  
polonesa, conforme indicado pelas correspondências a seguir:  

- A fórmula A atômica corresponde à fórmula A na notação polonesa,  
- (¬A) corresponde a ¬A,  
- (A ∧ B) corresponde a ∧AB,  
- (A ∨ B) corresponde a ∨AB,  
- (A → B) corresponde a →AB.  

As fórmulas a seguir estão na notação polonesa. Reescreva-as na notação convencional:  

(a) ∨ → p q → r → ∨ p q ¬ s  
- Resposta: (p→q)∨(r→((p∨q)→(¬s)))


(b) → → p q ∨ → p q → ¬ r r
- Resposta: (p→q)→((p→q)∨((¬r)→r))
----
# Questão 6
Defina um pseudocódigo recursivo que substitui toda ocorrência da subfórmula B  
dentro da fórmula A pela fórmula C.  

Observe que `substitution(((p ∧ ¬q) → r), (¬q), (r ∨ t))` deve retornar a fórmula  
`((p ∧ (r ∨ t)) → r)`.  

Em seguida, você deve usar o repositório disponível em  
https://github.com/thiagoalvesifce/logicomp  
e escrever um código para a função  
`substitution(formula, old_subformula, new_subformula)`.
- Resposta: 
````python
formula = And(Atom('p'), Not(Atom('q')))  # p AND NOT q
old_formula = Atom('p')  # p
new_formula = Or(Atom('r'), Atom('s'))  # r OR s

def substitution(formula, old_formula, new_formula):
    if formula == old_formula:
        return new_formula
    elif isinstance(formula, Atom):
        return formula
    elif isinstance(formula, Not):
        return Not(substitution(formula.inner, old_formula, new_formula))
    elif isinstance(formula, (And, Or, Implies)):
        return type(formula)(substitution(formula.left, old_formula, new_formula), substitution(formula.right, old_formula, new_formula))
    else:
        return formula

print("Formula:", str(formula))
print("Old Formula:", str(old_formula))
print("New Formula:", str(new_formula))
print("After Substitution:", str(substitution(formula, old_formula, new_formula)))
````
----
# Questão 7
Além das convenções para omitir os parênteses das fórmulas, também temos outras formas para melhorar a legibilidade de fórmulas grandes. Por exemplo, a fórmula `p1 ∧ p2 ∧ p3 ∧ p4 ∧ p5 ∧ p6 ∧ p7 ∧ p8 ∧ p9` pode ser representada de maneira mais compacta como:

![alt text](image-1.png)

Como outro exemplo, a fórmula `(¬p1,1 ∧ ¬p1,2) ∨ (¬p2,1 ∧ ¬p2,2) ∨ (¬p3,1 ∧ ¬p3,2)` pode ser apresentada de maneira mais compacta como:

![alt text](image-2.png)

Dessa forma, essas notações são definidas de forma semelhante à notação de somatório. Nesta questão, a cada item a seguir você vai criar a fórmula completa via código a partir da fórmula compacta:

![alt text](image-3.png)
- Resposta: 


![alt text](image-4.png)
- Resposta:
`p₁ ∧ p₂ ∧ p₃ ∧ ... ∧ p₂₀`  

![alt text](image-5.png)
- Resposta:
`p₁ ∧ p₂ ∧ ... ∧ pₙ`  

![alt text](image-6.png)
- Resposta:
 `(¬p₁,₁ ∧ ¬p₁,₂ ∧ ... ∧ ¬p₁,ₘ) ∨ (¬p₂,₁ ∧ ¬p₂,₂ ∧ ... ∧ ¬p₂,ₘ) ∨ ... ∨ (¬pₙ,₁ ∧ ... ∧ ¬pₙ,ₘ)`

![alt text](image-7.png)
- Resposta:
`((a₁ → (a₂ ∨ b₂)) ∧ (b₁ → (a₂ ∨ b₂))) ∧ ((a₂ → (a₃ ∨ b₃)) ∧ (b₂ → (a₃ ∨ b₃))) ∧ ... ∧ ((aₙ → (aₙ₊₁ ∨ bₙ₊₁)) ∧ (bₙ → (aₙ₊₁ ∨ bₙ₊₁)))`  

![alt text](image-8.png)
- Resposta:
`((p₁ ∨ q₁) ∧ (p₂ ∨ q₂) ∧ ... ∧ (pₙ ∨ qₙ)) → pₙ₊₁`  

![alt text](image-9.png)
- Resposta:
`(p₁,₁ ∨ p₁,₂ ∨ ⋯ ∨ p₁,ₙ) ∧ (p₂,₁ ∨ p₂,₂ ∨ ⋯ ∨ p₂,ₙ) ∧ ... ∧ (pₙ₊₁,₁ ∨ pₙ₊₁,₂ ∨ ⋯ ∨ pₙ₊₁,ₙ)`
----
Obs: A lista 1 tem umas questões extras. Devo tentar depois de resolver as outras 2 listas, caso dê tempo, até a prova.
----
# Lista 2
