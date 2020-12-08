# Математические модели вычислений

## Индуктивные и коиндуктивные типы
## Языки программирования на основе Исчисления Конструкций
## Работа в среде Coq


maxim.krivchikov@gmail.com

https://maxxk.github.io/formal-models/
<style>
.inference table {
    display: inline-block;
    padding: 1em;
}

.inference table th {
    font-weight: normal;
    border-bottom: 1px solid black;
}
</style>

# Индукция
Ранее мы определили натуральные числа в системе Fω, которые кодируют принцип итерации:

ℕ' ≡ Π (T : Type). T → (T → T) → T

0 : ℕ', S : ℕ' → ℕ'

Можно ли описать принцип индукции по натуральным числам в терминах зависимых типов?

Для того, чтобы доказать утверждение о натуральном числе P : ℕ → Type по индукции, мы строим:

1. Базу индукции — доказательство p~0~ : P(0)
2. Шаг индукции — для произвольного k : ℕ способ получения P(k+1) из P(k)

Если записать это в виде типа:

ind~ℕ~ : Π (P : ℕ → Type). (p~0~ : P·0). (step : Π(k : ℕ). P·k → P·(S·k))

# Зависимое удаление {.inference}
Зависимое удаление для зависимой суммы позволяют получать доказательства свойств для пары:

  Γ ⊦ a : Σ(x:A).B  $\qquad$ Γ, s : Σ(x:A).B ⊦ C : Type $\qquad$ Γ, a : A, b : B ⊦ r : C · pair(a, b)
 ------------------------------------------------------------------------------------------------------------
                Γ ⊦ split(C, s, r) : C · s

«Для любого элемента a : A, такого, что b : B(a), верно C(a, b)»

Принцип индукции описывает терм зависимого удаления для натуральных чисел:

ind~ℕ~ : Π (P : ℕ → Type). (p~0~ : P·0). (step : Π(k : ℕ). P·k → P·(S·k))

К сожалению, натуральные числа с индукцией невозможно закодировать в исчислении конструкций так же, как итерацию:

ℕ'' ≡ Π (P : **ℕ?** → Type). (p~0~ : P·**0?**). (step : Π(k : **ℕ?**). P·k → P·(**S?**·k))

Легко заметить, что итерация следует из индукции при P ≡ λ(n : ℕ). Type.

# Индуктивные типы
— средства для создания новых типов, имеющих рекурсивную структуру. Такие средства, как правило, включают конструкторы для элементов типа и «принцип индукции» или «принцип итерации» — терм удаления элементов индуктивного типа.

Натуральные числа — простейший пример индуктивного типа, можно придумать и более сложные, такие как отношение «меньше или равно» на натуральных числах:

· ⩽ · : ℕ → ℕ → Type

x_leq_x : Π(x : ℕ). x ⩽ x

leq_S : Π(x y : ℕ). x ⩽ y → x ⩽ y + 1

По этому определению можно вывести принцип ограниченной индукции:

leq_ind : Π (n : ℕ) (P : ℕ → Type). P · n → (Π(m : ℕ). m ⩽ n → P m → P (S m)) → Π (n~0~ : ℕ). n ⩽ n~0~ → P n~0~


# Рекурсивные типы
Простейший подход к описанию индуктивных типов:

Функция F : Type → Type определяет индуктивный тип μF : Type, для которого заданы операторы:
- введения inj~F~ : F (μF) → μ F
- удаления (принцип итерации) rec~F~ : Π (A : Type). ((μF → A) → F ·(μ F) → A ) → μF → A,

и правило редукции:

rec~F~ · A · fn · (inj~F~ · x) ⟶ fn · (rec~F~ · A · fn) · x

Для натуральных чисел: F~ℕ~ ≡ λ N. T + N. 

Подробнее:
<div style="font-size: 0.8em">
Abel, Andreas, Ralph Matthes, и Tarmo Uustalu. «Iteration and coiteration schemes for higher-order and nested datatypes». Theoretical Computer Science 333, вып. 1–2 (март 2005 г.): 3–66. doi:10.1016/j.tcs.2004.10.017.
Ahn, Ki Yung, и Tim Sheard. «A hierarchy of mendler style recursion combinators». ACM SIGPLAN Notices 46, вып. 9 (сентябрь 2011 г.): 234–46. doi:10.1145/2034574.2034807.
Fumex, Clément, Neil Ghani, и Patricia Johann. «Indexed Induction and Coinduction, Fibrationally». В Algebra and Coalgebra in Computer Science, 176–91. Springer, 2011. http://link.springer.com/chapter/10.1007/978-3-642-22944-2_13. 
По последней статье — лучше подождать несколько занятий, пока мы не рассмотрим введение в теорию категорий.
</div>

# Не все индуктивные типы допустимы
A : Type

intro~A~ : ((A → Type) → Type) → A

match~A~ : A → ((A → Type) → Type)

match~A~ (intro~A~ · x) ⟶ x

**Задача 8.1 \*\***
~ Показать, что этот набор термов и правило редукции позволяют получить терм типа ⊥, т.е. вывести парадокс.  

# Индуктивные типы в Исчислении Конструкций
Введём понятие «строго положительного» вхождения переменной X в терм τ.
- любое вхождение в терм без зависимых произведений (τ=X × X, τ = ΣX.X) строго положительное, если есть зависимые произведения — смотрим на структуру.
- для ΠA.B, если X не встречается в A, вхождения строго положительные
- ΠX.B — отрицательное вхождение X.
- ΠA.B — если X встречается в A в отрицательных позициях, то его вхождение в τ положительно, но не строго положительно.

<div class="fragment">
Т.е. X не должен стоять «слева» от стрелки вообще (строго положительное) или в самой левой части стрелки (положительное)

Примеры строго положительных вхождений: X × X, ℕ → X, X × (ℕ → X)
Пример нестрого положительного вхождения: (X → ℕ) → ℕ
Примеры отрицательных вхождений: X → ℕ, (X × ℕ) → X, F(X) → X, (ℕ → X) → X (?)
</div>

# Индуктивные типы в Исчислении Конструкций
μF — набор *конструкторов* — типов, в которых переменная итерации X может встречаться только в строго положительных позициях.

Строгая положительность позволяет определить терм удаления и не получить при этом возможность парадоксов.

<span style="font-size: 0.8em">
Paulin-Mohring, Christine. «Inductive Definitions in the System Coq - Rules and Properties». In TLCA ’93 Proceedings of the International Conference on Typed Lambda Calculi and Applications, 328–45. Lecture Notes in Computer Science. Springer-Verlag London, UK, 1993. doi:10.1007/BFb0037116.
</span>

# Коиндуктивные типы
## Двойственность индуктивных и коиндуктивных типов

# Простые расширения
## Взаимно индуктивные типы
Несколько типов A~1~, …, A~k~, определения которых могут ссылаться друг на друга.
Пример — типы списков с контролируемой чётностью длины:
μ(EvenList = T + (ℕ × OddList)
$\qquad$ OddList = ℕ × EvenList)

## Индексированные индуктивные типы
(индуктивные предикаты — ΠA.Type для некоторого A)
Чётность (T : Πℕ.Type) — в форме набора конструкторов:

μ( Even = 
even_zero : Even · 0
$\qquad$ even_plus_2 : Π(k : ℕ).Even(k) → Even (S · S · k)

# Нетривиальные расширения
## Индуктивно-рекурсивные и индуктивно-индуктивные типы
Одновременно определяется индуктивный тип U : Type и рекурсивная функция T : U → Type.

1. Forsberg F.N., Setzer A. A finite axiomatisation of inductive-inductive definitions. 2012.
2. Ghani N. et al. Fibred Data Types // Proceedings of the 2013 28th Annual ACM/IEEE Symposium on Logic in Computer Science. IEEE Computer Society, 2013. P. 243–252.


# Типизированное λ-исчисление в языках программирования

В распространённых языках программирования используется система типов Хиндли-Милнера — ограниченная версия системы F, типизация для которой разрешима.

Система типов языка Haskell основана на системе Fω (точнее:
- https://ghc.haskell.org/trac/ghc/wiki/Commentary/Compiler/FC
- http://stackoverflow.com/a/21220357
)

# Исчисление конструкций в языках программирования

На исчислении конструкций (полиморфном λ-исчислении с конструкторами типов и зависимыми типами) основан ряд языков программирования:

Coq, Agda — среды автоматизированного доказательства теорем (proof assistants)
Idris, ATS — языки программирования

Есть ещё большое количество примеров, которые мы не рассматриваем:
Matita, CubicalTypeTheory, Twelf, NuPRL — среды интерактивного доказательства теорем
Haskell — некоторые расширения языка допускают использование зависимых типов.

# Agda
Официальный сайт: http://wiki.portal.chalmers.se/agda/pmwiki.php
Пример на русском языке: https://habrahabr.ru/post/148769/
Книга: A. Stump. Verified Functional Programming in Agda. 2016.

Язык похож по синтаксису на Haskell, использует Unicode-символы (для нормального использования стандартной библиотеки нужно работать в редакторе, который поддерживает ввод математических Unicode-символов с использованием LaTeX-подобных последовательностей)

```
data huffman-tree : Set where
  ht-leaf : string → ℕ → huffman-tree
  ht-node : ℕ → huffman-tree → huffman-tree → huffman-tree

-- get the frequency out of a Huffman tree
ht-frequency : huffman-tree → ℕ
ht-frequency (ht-leaf _ f) = f
ht-frequency (ht-node f _ _) = f

-- lower frequency nodes are considered smaller 
ht-compare : huffman-tree → huffman-tree → 𝔹
ht-compare h1 h2 = (ht-frequency h1) < (ht-frequency h2)

{- return (just s) if Huffman tree is an ht-leaf with string s;
   otherwise, return nothing (this is a constructor for the maybe type -- see maybe.agda in the IAL) -}
ht-string : huffman-tree → maybe string
ht-string (ht-leaf s f) = just s
ht-string _ = nothing
```

# Idris
Официальный сайт, документация, примеры: http://www.idris-lang.org/
Пример — игра 2048 в терминале: https://github.com/KesterTong/idris2048 

Более новый, чем Agda, язык с зависимыми типами, нацеленный, в первую очередь, на использование в качестве полноценного языка программирования.

```
FILE_IO : Type -> EFFECT

data OpenFile : Mode -> Type

open : (fname : String)
       -> (m : Mode)
       -> Eff Bool [FILE_IO ()]
                   (\res => [FILE_IO (case res of
                                           True => OpenFile m
                                           False => ())])
close : Eff () [FILE_IO (OpenFile m)] [FILE_IO ()]

readLine  : Eff String [FILE_IO (OpenFile Read)]
```

# ATS
Официальный сайт, документация, примеры: http://www.ats-lang.org/
Императивный язык программирования с зависимыми типами. 

```
fun
fibats{n:nat}
  (n: int (n))
: [r:int] (FIB (n, r) | int r) = let
//
fun
loop
{i:nat | i <= n}{r0,r1:int}
(
  pf0: FIB(i, r0), pf1: FIB(i+1, r1)
| n_i: int(n-i), r0: int r0, r1: int r1
) : [r:int] (FIB(n, r) | int(r)) =
(
  if (n_i > 0)
    then
    loop{i+1}
    (
      pf1, FIB2(pf0, pf1) | n_i-1, r1, r0+r1
    ) (* then *)
    else (pf0 | r0)
  // end of [if]
) (* end of [loop] *)
in
  loop{0}(FIB0(*void*), FIB1(*void*) | n, 0, 1)
end // end of [fibats]
```

# Coq
Официальный сайт: https://coq.inria.fr/
Книга Certified Programming with Dependent Types: http://adam.chlipala.net/cpdt/ 

Наиболее широко используемый в настоящее время язык программирования и среда интерактивного доказательства теорем с поддержкой зависимых типов.

Известные результаты, полученные в Coq:
- [формализация доказательства теоремы о четырёх красках](http://research.microsoft.com/en-us/um/people/gonthier/4colproof.pdf)
- [формализация доказательства теоремы Томпсона-Фейта о классификации простых конечных групп нечётного порядка](https://hal.inria.fr/hal-00816699/en)
- [верифицированный компилятор языка C CompCert](http://compcert.inria.fr/)
- [файловая система FSCQ с доказательством устойчивости к сбоям при отключении питания](https://people.csail.mit.edu/nickolai/papers/chen-fscq.pdf) ([исходный код](https://github.com/mit-pdos/fscq-impl))

# Выделение программ
Среда Coq может транслировать программы с зависимыми типами в языки Haskell и OCaml. Такие программы потом можно скомпилировать в машинный код, и их производительность будет сравнима с распространёнными языками программирования.

## Стирание типов
Зависимые типы используются на этапе «компиляции» при проверке того, что программы удовлетворяют заданной спецификации. Если про программу доказано свойство её корректности, нет необходимости во время её выполнения на каждом шаге вычислять доказательства.

В Coq для этого *(на самом деле, не только для этого)* типы нижнего уровня разделены на «типы данных» Set и «типы утверждений» Prop.

Статья: https://eb.host.cs.st-andrews.ac.uk/drafts/dtp-erasure-draft.pdf

# Основные команды среды Coq
Предложения языка разделяются точкой; ключевые слова регистрозависимы и пишутся с большой буквы.
- `Variable имена переменных : тип.` — объявление переменных
- Определение функции; в круглых и фигурных скобках — аргументы.
```
Definition имя (переменная : тип) ... { переменная : тип } 
... : тип := терм.
```
- аналогично можно вводить определения ключевыми словами `Theorem`, `Lemma`.
- `Check (терм : тип)` — проверить, что терм имеет заданный тип.
- `Compute терм` — вычислить значение терма.
- тип «стрелки» — ->; (у меня шрифт с лигатурами, поэтому будет отображаться как →)
- тип зависимого произведения — `forall (x : A), ...` (или ∀ вместо `forall`)
- λ-абстракция — терм `fun (переменная : тип) ... , терм` (у меня — λ вместо fun)

# Индуктивные типы в Coq
Coq поддерживает определение индексированных индуктивных типов в виде набора конструкторов со строго положительными параметрами и «синтаксический сахар» над определением рекурсивных функций, который позволяет определять рекурсивные функции с явной структурной индукцией так же, как и в обычных функциональных языках программирования.

- Индуктивное определение:

```coq
Section ilist.
  Variable A : Set.

  Inductive ilist : nat -> Set :=
  | Nil : ilist O
  | Cons : forall n, A -> ilist n -> ilist (S n).

   Inductive fin : nat -> Set :=
  | First : forall n, fin (S n)
  | Next : forall n, fin n -> fin (S n). 
```

# Индуктивные типы в Coq
Рекурсивная функция:

```
Fixpoint get n (ls : ilist n) : fin n -> A :=
    match ls with
      | Nil => fun idx =>
        match idx in fin n' return (match n' with
                                        | O => A
                                        | S _ => unit
                                      end) with
          | First _ => tt
          | Next _ _ => tt
        end
      | Cons _ x ls' => fun idx =>
        match idx in fin n' return (fin (pred n') -> A) -> A with
          | First _ => fun _ => x
          | Next _ idx' => fun get_ls' => get_ls' idx'
        end (get ls')
    end.
End ilist.
```

# Другие команды среды Coq
`Search About nat` — выводит все функции стандартной библиотеки, работающие на натуральных числах.
`Print t` — выводит определение терма `t`.

Пример: автоматически сгенерированный принцип рекурсии для списков с заданной длиной
```
Print ilist_ind.
```

```
ilist_rect = 
fun (P : forall n : nat,  ilist n -> Type) (f : P 0 Nil)
  (f0 : forall (n : nat) (a : A) (i : ilist n), P n i -> P (S n) (Cons n a i)) =>
fix F (n : nat) (i : ilist n) {struct i} :
  P n i := match i as i0 in (ilist n0) return (P n0 i0)
    with
    | Nil => f
    | Cons n0 a i0 => f0 n0 a i0 (F n0 i0)
  end.
```

# Интерактивные доказательства
https://github.com/klao/coq-learning/tree/master/primes

# Задачи
**Задача 8.2а \***
Сформулировать несколько (хотя бы 5) простых утверждений арифметики в Coq.

**Задача 8.2б \*\***
Доказать утверждения, не используя тактику `omega`.

**Задача 8.3\*\*\***
Реализовать функцию сортировки списка в Coq с доказательством того, что она возвращает отсортированный список.  
