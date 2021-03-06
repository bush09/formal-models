# К системе типов языков семейства Рефал для промежуточного представления программ
## д.ф.-м.н., проф. В. А. Васенин, к.ф.-м.н. М. А. Кривчиков
### МГУ имени М. В. Ломоносова, Москва
#### Мальцевские чтения, Новосибирск, 21-25 ноября 2016 г.

<style>
.bsods-everywhere:after {
  background: url(images/bsods-everywhere.jpg);
  background-position: left, right;
  background-repeat: repeat-x, repeat-y;
  opacity: 0.3;
  width: 100%;
  height: 100%;
}
.bsods-everywhere {
  text-shadow: rgba(0.5,0.5,0.5,0.5) 0 0 1px;
}

span.small {
  font-size: 0.75em;
}
.inference table {
    display: inline-block;
    padding: 1em;
}

.inference table th {
    font-weight: normal;
    border-bottom: 1px solid black;
}

.slider > section {
  font-size: 20pt !important;
}

</style>
# Программы окружают нас {.center .bg .bsods-everywhere}

Например, в следующих сферах жизни общества:
- развлечения
- рабочие места
- коммуникации
- транспорт
- медицина
- наука
- производство
- энергетика
- вооружение

# Программы содержат ошибки
### Уязвимости в ПО
- Stuxnet, Иранская ядерная программа (2009) — предположительно, повреждены центрифуги, используемые при обогащении урана
- OpenSSL Heartbleed (CVE-2014-0160) — значительная часть TLS-серверов в Интернет (популярные дистрибутивы Linux — Debian-based, RedHat-based; OpenBSD, FreeBSD, NetBSD, роутеры Cisco, Juniper, …) имели уязвимость в течение более 1 года

### Некорректная работа ПО
- «Фобос-Грунт» (2011) — утрачена автоматическая межпланетная станция
- Панама, установки лучевой терапии (2000-2001)
- Therac-25, установки лучевой терапии (1985-1987)
- ExoMars, крушение зонда Schiaparelli (2016)

### Недокументированные возможности
- Volkswagen (2015) — занижались данные по выбросам вредных веществ дизельными двигателями, компанию ожидает штраф до 18 млрд. долл.

# Верификация
— процесс, целью которого является показать соответствие продукта, сервиса или системы требованиям, спецификациям и другим условиям, которые накладываются на продукт

* рецензирование (peer review), ручное тестирование
    - зависит от экспертного мнения;
    - не автоматизировано;
    - на больших объёмах кода сложно в применении.

* (динамическое автоматизированное) тестирование
    - большой объём кода тестов по сравнению с кодом продукта (SQLite: 80 тыс. строк кода / 90 млн. строк тестов);
    - тесты необходимо поддерживать в актуальном состоянии;
    - тесты не гарантируют отсутствия дефектов/ошибок.

* Статический и динамический анализ
    - нетривиальные свойства неразрешимы согласно теореме Райса;
    - большое количество ложных срабатываний;
    - на практике встречаются дефекты, необнаружимые средствами статического анализа (OpenSSL Heartbleed bug: информация раскрыта 7 апреля 2014 г., адаптированные средства статического анализа появились 18 апреля 2014 г.).

# Формальная верификация

— процесс, результатом которого является получение строгого математического доказательства соответствия программы требуемой спецификации, наличия у программы требуемых свойств.

### Недостатки

- является трудоёмким и наукоёмким процессом
- сложно получить строгую постановку
- не имеет широкого распространения

### Подходы

- теоретико-модельные — исчерпывающая автоматическая проверка пространства конфигураций модели программы (верификация моделей программ, model checking)^1^;
- **теоретико-доказательные** — представление программ и свойств в виде логических формул с дальнейшим построением доказательств;
    - корректные по построению программы на языках программирования с системами типов, которые предоставляют сильные гарантии корректности^2^.

<span class="small">
^1^ Э.М. Кларк и др. Верификация моделей программ. Model checking. М.:МЦНМО, 2002.
^2^ Chen, Haogang, Daniel Ziegler, Tej Chajed, Adam Chlipala, M. Frans Kaashoek, Nickolai Zeldovich. «Using Crash Hoare Logic for Certifying the FSCQ File System», 18–37. ACM Press, 2015. [doi:10.1145/2815400.2815402](http://dx.doi.org/10.1145/2815400.2815402).
</span>

# Программы сложны и объёмны

<table>
<tr><td>
OpenSSL 1.1.0c, <span class="small">оптимизированные реализации криптографических алгоритмов (подкаталоги crypto, doc/crypto)</span>
</td>
<td>
<span class="small">410 тыс. строк
(код (300), документация (30), комментарии (40) и пустые строки (40)),</span> <div style="display:inline-block">около 10 000 страниц^1^</div>
</td></tr>
<tr><td>
Микроядро операционной системы SeL4
</td>
<td>
<span class="small">10 тыс. строк кода на C, 480 тыс. строк формальных доказательств</span>
<div style="display:inline-block">около 12 000 страниц^2^</div>
</td>
</tr>
<tr><td>
Классификация простых конечных групп <span class="small">(для сравнения, на основе информации в Daniel Gorenstein (1985), "The Enormous Theorem", Scientific American, vol. 253, no. 6, pp. 104–115.)</span>
</td><td>
<div style="display:inline-block">около 15 000 страниц</div>
</td></tr>
<tr><td>
Формальное доказательство в среде Coq теоремы Томпсона-Фейта о разрешимости всякой конечной группы нечётного порядка
</td>
<td>
<span class="small">150 тыс. строк в среде Coq</span>
<div style="display:inline-block">около 3 750 страниц^3^</div>
</td>
</tr>
</table>

<span class="small">
^1^ ГОСТ 7.32-2001: 40±3 строк на страницу.
^2^ Klein G. et al. Comprehensive Formal Verification of an OS Microkernel // ACM Trans. Comput. Syst. 2014. Vol. 32, № 1. P. 2:1–2:70.
^3^ G. Gonthier et al. A Machine-Checked Proof of the Odd Order Theorem. In Proc. of ITP 2013, LNCS 7998, pp.163-179, 2013.
</span>

# Системы типов и верификация
Система типов [языка программирования] — это гибко управляемый синтаксический метод доказательства отсутствия в программе определенных видов поведения при помощи классификаций выражений языка по разновидностям вычисляемых ими значений.^1^

Соответствие Карри-Говарда — типы в системе, обладающей свойством нормализации, можно рассматривать как утверждения интуиционистской логики, а термы, имеющие заданный тип — как доказательства утверждения.

Разновидности типизированного λ-исчисления, активно используются в исследованиях как в области формальной верификации, так и в области проверки доказательств, в особенности — Исчисление конструкций (среды Coq, Agda).

<span class="small">
^1^ Б. Пирс. Типы в языках программирования. М.: Лямбда-пресс: Добросвет, 2011.
</span>

# Существующие языки программирования
- системы типов распространённых языков программирования (C/C++, Java, Python) не могут представить строгие математические спецификации программ:
    * недостаточно мощны для описания точных спецификаций
      MatrixMultiply : ∀ (m, n, p : ℕ), Matrix m×n → Matrix n×p → Matrix m×p
    * противоречивы, т.к. допускают описание незавершимых программ и неограниченную рекурсию

```javascript
// omega : T → ⊥
function omega(a) { return omega(a); }
```

- языки программирования на основе исчисления конструкций сложны в применении для специалистов без математического образования (и для математиков, не имеющих специальной подготовки)

# Предметно-ориентированные языки программирования
 — языки программирования, адекватно отражающие специфику предметной области.

Примеры: SQL, языки шаблонов в Web-приложениях, языки конфигурации, языки описания входных данных для сред математического моделирования физических процессов.

## Задача
Разработать представление для описания семантики различных (в первую очередь, предметно-ориентированных) языков программирования и программ, записанных на наборах таких языков, предназначенное для формальной верификации функциональных свойств таких программ.

# Подход
![](images/DSL-scheme.svg)

<!--
# Исчисление конструкций {.inference}

   Γ ⊦ A : **Type** $\qquad$  Γ, x : A ⊦ B : **Type**
 -----------------------------------------------------
  Γ ⊦ Π (x : A). B : **Type**


  Γ ⊦ Π (x : A). B : **Type** $\qquad$ Γ, x : A ⊦ M : B
 -----------------------------------------------------------
  Γ ⊦ λ (x : A). M : Π (x : A). B


  Γ ⊦ F : Π (x : A). B $\qquad$ Γ ⊦ N : A
 --------------------------------------------
  Γ ⊦ F · N : B[x := N]

  (λ (x : A). M) · N ⟶~β~ M[x := N]

  **Type**~i~ : **Type**~j~, i⩽j
-->

# Индуктивные типы
Индуктивные типы — расширения исчисления конструкций — позволяют определять корректные рекурсивные программы, а также расширяют выразительные возможности для описания спецификации программ.

```coq
Inductive assign_loc (ty: type) (m: mem) (b: block) (ofs: int):
  val -> trace -> mem -> Prop :=
| assign_loc_value: forall v chunk m',
    access_mode ty = By_value chunk ->
    type_is_volatile ty = false ->
    Mem.storev chunk m (Vptr b ofs) v = Some m' ->
    assign_loc ty m b ofs v E0 m'
```

<span class="small">
Индуктивные типы в фрагменте описания семантики языка C для компилятора CompCert. Xavier Leroy. Formal verification of a realistic compiler. Communications of the ACM, 52(7):107-115, 2009.
</span>

# Высшие индуктивные типы
Гомотопическая теория типов устанавливает связь между теорией типов и алгебраической топологией.^1^

```
R : Type
document : R
patch (old new : String) (sequence : Fin n) : document = document
indep (i != j) -> patch s t i ∘ patch u v j  = patch u v i ∘ patch s t j
noop : patch s s i = refl
```

<span class="small">
Пример применения высших индуктивных типов для спецификации системы контроля версий: Angiuli, Carlo, Edward Morehouse, Daniel R. Licata, и Robert Harper. «Homotopical patch theory». В Proceedings of the 19th ACM SIGPLAN international conference on Functional programming, 243–56. ACM, 2014. doi:10.1145/2628136.2628158.
^1^ Homotopy Type Theory: Univalent Foundations of Mathematics. — Princeton: Institute for Advanced Study, 2013. — 603 p.
</span>

# Нетривиальные правила редукции
s =~A~ t ⇔ I — отношение структуры типа идентичности между элементами типа A (элементы типа идентичности s = t имеют структуру типа I(s, t)).
<table style="font-size: 0.8em">
<tr>
<td>
J(B, u, v, a =~A~ b, ξ, x)   ⟶   **let**
$\qquad$    A~1~      ≡       A(u, **refl** u)
$\qquad$    A~2~      ≡       A(v, ξ)
$\qquad$    s =~A2~ t   ⇔       I~2~
$\qquad$    a~1~      ≡       a(u, **refl** u) : A~1~
$\qquad$    a~2~      ≡       a(v, ξ) : A~2~
$\qquad$    a'~1~    ≡       J(B, u, v, A, ξ, a~1~) : A~2~
$\qquad$    b~1~      ≡       b(u, **refl** u) : A~1~
</td>
<td>
$\qquad$    b~2~      ≡       b(v, ξ) : A~2~
$\qquad$    b'~1~   ≡       J(B, u, v, A, ξ, b~1~) : A~2~
$\qquad$    x~1~      ≡       **itoe**(**refl** a'~1~) : I~2~(a'~1~, a'~1~)
$\qquad$    x~2~      :       I~2~(a'~1~, b'~1~)
$\qquad$    x~2~    ≡ J(A~1~, a~1~, b~1~, (r, ρ) ↦ I~2~(a'~1~, J(B, u, v, A, ξ, r)), x, x~1~)
$\qquad$    z       ≡       λ (w : B) (ω : v =~B~ w) (a~wω~ : A(w, ω)). J(B, w, v, A, ξ ∘ ω^-1^, a~wω~)
$\qquad$    x~3~      ≡       J(B, u, v, (r, ρ) ↦ I~2~(z(r, ρ, a(r, ρ)), b'~1~), ξ, x~2~)
$\qquad$  **in**      J(B, u, v, I~2~(a~2~, z · b), ξ, x~3~) $\qquad$ .
</td>
</tr>
</table>

Как исследовать поведение таких сложных правил?

<span class="small">
 Правило редукции переноса вдоль идентичности ξ : u =~B~ v элемента другого типа идентичности x : a(t, σ) =~A~ b(t, σ). <div style="display: inline-block">В. А. Васенин,</div> <div style="display: inline-block">М. А. Кривчиков.</div> «Предметно-ориентированные языки с заданной формальной семантикой на основе лямбда-исчисления с зависимыми типами». Международная конференция «Мальцевские чтения-2014», Новосибирск, 2014.
</span>


# Рефал
— [РЕкурсивных Функций АЛгоритмический] язык программирования, построенный на основе модели нормальных алгорифмов Маркова.

Функции задаются в терминах правил переписывания входных строк (деревьев) в выходные.
Последовательность правил, разделённых символом `;`. Слева от знака `=` записываются образцы, справа — результаты. После унификации входной строки с образцом выполняется подстановка полученных значений переменных в результат. Вызов функции — в угловых скобках, круглые скобки позволяют задавать и сопоставлять деревья и образцы на деревьях.

```
Beta {
   (λ s.var '.'  e.body) t.value e.rest =
      Step <Subst s.var t.value e.body> e.rest ;
    = Stuck;
}
```

<span class="small">
Турчин В. Ф. «Метаалгоритмический язык». Кибернетика, вып. 4 (1968 г.): 45–54.
</span>

# Расширения Базового Рефала
Условия: промежуточный результат сопоставляется с образцом (Рефал-5):

```
t.neutral e.rest,
  <Beta e.rest> : s.state e.result =
  s.state t.neutral e.result ;
```

Функции высшего порядка: функции передаются так же, как и данные (Рефал-7):

```
(e.1) e.rest,
  <Beta e.1> : e.betaResult =
  <{
    Step e.result = Step (e.result) e.rest ;
    Stuck e.result,
      <Beta e.rest> : s.state e.result2 =
      s.state (e.result) e.result2 ;
  } e.betaResult>;
```

<span class="small">
Рефал-5: В.Ф. Турчин. «Рефал-5. Руководство по программированию и справочник» http://refal.net/rf5_frm.htm
Рефал-7: С.Ю. Скоробогатов, А. М. Чеповский. «Разработка нового диалекта языка Refal». Информационные технологии, вып. 9 (2006 г.): 31–38.
</span>

# Метавычисления и суперкомпиляция
Метавычисления — это раздел теории и практики программирования, связанный с разработкой и использованием метапрограмм — конструктивных метасистем над программами.^1^

Суперкомпиляция — техника преобразования программ [в первую очередь — оптимизации], основанная на построении полной и самодостаточной модели программы.^2^

Две основные стадии суперкомпиляции:
1. *Прогонка* программы на параметризованных входных данных (частичная специализация).
2. *Свёртка* результата прогонки для получения остаточной программы (выделение рекурсии).

<span class="small">
^1^ С.М. Абрамов. Основы метавычислений. Курс НОУ ИНТУИТ. http://www.intuit.ru/studies/courses/1067/221/info
^2^ И.Г. Ключников. Суперкомпиляция: идеи и методы. Практика функционального программирования, № 7, 2011.
</span>

# Система типов для языков семейства Рефал
Ограничимся функциями без побочных эффектов. Тип значения — завершимая функция, принимающая или отвергающая значение.

```
Bool = { 0 = ; 1 = ; }
```

Тип функций задаёт допустимый набор типов аргумента и типов выходных значений (разделены знаком `--`) Суждение «функция f обладает типом g» означает, что для любого входного значения, распознаваемого g, f завершится без ошибок, и её возвращаемое значение будет распознано соответствующим типом для g.

```
BoolBinaryOperation = { s.1 s.2, <Bool s.1> <Bool s.2> -- s.3, <Bool s.3> }
BoolIdentity = { s.1, <Bool s.1> -- s.1 }
```

# Суждение типизации
— применение суперкомпиляции к типу и функции на произвольном выражении.

```
BoolBinaryOperation ≡ { t.FN = { s.1 s.2, <Bool s.1> <Bool s.2> : s.1 s.2 = <t.FN s.1 s.2> : s.3, <Bool s.3> = ;
  e.skip = ; } }
<HasType BoolBinaryOperation F> ≡ < Supercompile [<BoolBinaryOperation F> e.1] >
```

Последний образец `e.skip` срабатывает в случае, если входные данные не удовлетворяют условиям типа.
Возможные случаи:
1. Успешная типизация: Суперкомпиляция уничтожает вызов функции t.FN, результат — общезначимый тип `{ e.1 = }`.
2. Ошибка типизации: Суперкомпиляция приходит к возможному контрпримеру `{ … = < > ; … }`
3. Неопределённый результат: Суперкомпиляция не может уничтожить вызов функции и генерирует нетривиальную остаточную программу. `R ≡ { … ; … }`. В этом случае можно предоставить дополнительные данные в исходном определении, или в определении R':

```
<HasType T F> ⟶ R
<HasType T F R'> ⟶ <HasType R' <Supercompile [<T F> e.1]>> <HasType { e.1 = } R'>
```

# Подтипы
Наиболее точный тип можно получить из определения функции.

```
XorDef = { 0 0 = 0 ; 0 1 = 1; 1 0 = 1; 1 1 = 0; }
XorSpec = { 0 0 -- 0; 0 1 -- 1; 1 0 -- 1; 1 1 -- 0; }
```

Другие типы могут, например, более свободно трактовать возможные выходные значения, или же задавать дополнительные ограничения на входные значения и получать таким образом дополнительные свойства выходных значений. Фактически, тип — это свойство функции (утверждение о функции).

```
XorNeg = { 1 s.1 -- s.2, <Not s.1> : s.2 }
```

Можно попытаться получить XorNeg ⩽ XorSpec, в терминах пересечения спецификаций:

```
<HasType XorNeg XorSpec>
```

# Проверка на завершимость
Индуктивные типы.

```
ℕ {
  = ;
  1 e.1, <ℕ e.1> : = 1 e.1
}
```

Известные условия завершимости:
1. Длина аргумента: функция F завершима, если в рекурсивных вызовах используется только убывающий по длине аргумент.
2. Размер аргумента: функция F завершима, если на аргументах можно определить другую функцию G : dom F → ℕ, такую, что в рекурсивных вызовах используется аргумент меньшего размера.
3. Трансфинитная индукция.

# λ-исчисление с зависимыми типами
Для определения типов функций высшего порядка введём понятие функциональной переменной `f`, сопоставление которой успешно в точности для функций.
Определим виды, типы и термы λ-исчисления с зависимыми типами в виде трансляции
⟦ Type ⟧ = Type { f.FN, < Finite f.FN > -- f.FN }
В качестве контекстов используем простую последовательность переменных:
⟦ (x : A), (y : B), … ⟧ = t.x t.y …, < Type ⟦A⟧ > < Type ⟦B ⟧> … < HasType s.x ⟦A⟧ > < HasType s.y ⟦B⟧ > …
⟦ Var v ⟧ = t.v
⟦ Π(x : A).B ⟧ = { t.x, < HasType Type ⟦A⟧ > < HasType ⟦A⟧ t.x > -- t.b, < HasType Type ⟦B⟧ > < HasType ⟦B⟧ t.b > }
⟦ λ(x : A).N ⟧ = { t.x, < HasType Type ⟦A⟧ > < HasType ⟦A⟧ t.x > = ⟦ N ⟧ }
⟦ M N ⟧ = < ⟦M⟧ ⟦N⟧ >


# Промежуточное представление
В какой форме удобнее представлять рефал-программы для суперкомпиляции? Необходимо представление, определяющее последовательность разбора.

```
XorDef = { ... 1 0 = 1 ; 1 1 = 0;}
```

1. Рефал-графы. ^1^

```
:(
  ...
  e0 -> 1 e1; :( e1 -> 0; 1 <- e0;
     + e1 -> 1; 0 <- e0
     )
)
```

2. Язык сборки. ^2^

```
  ...
  RECOGNIZE CONST 1; BRANCH L1; RECOGNIZE CONST 0; EMIT CONST 1;
  L1: RECOGNIZE CONST 1; EMIT 0; RETURN;
```

Язык сборки можно понимать как аналог индексов де Брёйна для рефал-графов.

<span class="small">
^1^ Turchin V.F. The algorithm of generalization in the supercompiler. In Bjorner D., Ershov A.P., Jones N. (ed.), Partial Evaluation and Mixed Computation. (Gammel Avernaes, Denmark). pp.341--353, North-Holland, 1988.
^2^ Романенко, С. А. «Машинно независимый компилятор с языка рекурсивных функций». Диссертация на соискание учёной степени к.ф.-м.н. (01.01.10), ИПМ АН СССР, 1978.
</span>

# Расширение промежуточного представления
Мы предлагаем:
1. использовать явное построение выходной строки (в оригинале используется перестроение выходной строки из входной);
2. считать константные выражения последовательностью инструкций построения выходной строки;
3. представлять функции высшего порядка как последовательность инструкций;
4. представлять замыкания как инструкции, модифицирующие код функции.

Основные положения
1. Уровень цитирования отражает глубину функциональных скобок в выходной строке.
2. Инструкции распознавания на ненулевом уровне цитирования считаются инструкциями записи выходной строки.
3. Уровень выхода из цитаты инструкции `EMIT`: если при выполнении инструкции текущий уровень цитирования равен уровню выхода из цитаты, инструкция выполняется, в противном случае — записывается как данные.
4. Инструкция `PUSH` вычисляет текущее значение выходного выражения, записывает его значение в переменную и очищает выходное выражение.

# Пример
Распознать один символ и вернуть функцию, распознающую пару таких символов.

```
{ s.1 = { s.1 s.1 = 1 ; } }
```

```
RECOGNIZE LITERAL; EMIT CONST { 0;   // уровень цитирования 0 -> 1
  EMIT VARIABLE 1 1;                 // выход из цитаты 1, внешняя функция
  PUSH;                              // выходное выражение -> переменная 1 (внутр. функции)
  RECOGNIZE VARIABLE 1;
  RECOGNIZE VARIABLE 1;
  EMIT CONST 1 0;                    // выход из цитаты 0, внутренняя функция
EMIT CONST } 0;                      // уровень цитирования 1 -> 0
```



Код функции, возвращаемой при выполнении примера на аргументе `1`.

```
EMIT CONST 1;
PUSH;
RECOGNIZE VARIABLE 1;
RECOGNIZE VARIABLE 1;
EMIT CONST 1 0;
```


# Проверка типов
Методы суперкомпиляции требуют адаптации для работы с функциями высших порядков.
Схема обоснования существования метода суперкомпиляции:

1. Для типов-значений с ограничениями (без перебора и с раскрытием переменных типа `t`)^1^
Определен. Существует также суперкомпилятор SCP4, который работает на языке Рефал-5.^2^

2. Новые функциональные переменные `f.1` в образцах имеют семантику, аналогичную образцу-паре скобок `(e.1)`, с единственным отличием — распознаются функциональные скобки. Функциональные переменные непрозрачны для сопоставления с образцом.

3. Суждения типизации для функциональных переменных пока не раскрываются, записываются в контекст синтаксически.

Тип ложных высказываний — «пустая» функция с единственной инструкцией `FAIL`. Тип истинных высказываний — общезначимая функция `{ e.1 = }`.

<span class="small">
^1^ А.П. Немытых. О суперкомпиляции (к 80-летию со дня рождения В.Ф. Турчина). Международная конференция
"Современные проблемы математики, информатики и биоинформатики", Новосибирск, 2011.
^2^ А.П. Немытых. Суперкомпилятор SCP-4. Общая структура. М.: URSS. 2007. 152 с.
</span>

# Связанные работы

Автоматическая верификация с использованием методов суперкомпиляции (ближе к теоретико-модельным методам):
- Климов А.В., Ключников И.Г., Романенко С.А. Автоматизированная верификация счетчиковых систем посредством предметно-ориентированной многорезультатной суперкомпиляции.
- A. P. Lisitsa and A. P. Nemytykh. Towards verification via supercompilation. In Proceedings of the 29th Annual International Computer Software and Applications Conference (COMPSAC’05), 25-28 July 2005, Edinburgh, Scotland, UK, pages 9–10. IEEE Computer Society, 2005.

Методы суперкомпиляции для автоматизированного доказательства эквивалентности термов в λ-исчислении высших порядков (без зависимых типов):
- I. G. Klyuchnikov and S. A. Romanenko. Proving the equivalence of higher-order terms by means of supercompilation. In Pnueli et al. [33], pages 193–205. 2009


# Заключение
- предложен подход к построению системы типов для языков семейства Рефал, которые поддерживают функции высшего порядка
- система типов допускает последовательную детализацию спецификации, от максимально детальной (реализация функции в терминах Рефал) до более общих (типы с различными ограничениями на входные данные)
- продемонстрирована возможность применения системы типов для пошагового построения доказательств
- предложена трансляция λ-исчисления с зависимыми типами первого порядка в Рефал, сохраняющая типизацию

## Следующие шаги
- практическая реализация схемы проверки типов
- среда автоматизации доказательств на основе Рефал-подобных языков
- доказательства включения в систему типов других разновидностей типизированного λ-исчисления (в первую очередь — исчисления конструкций с индуктивными типами)


# Спасибо за внимание!

### Докладчик — Максим Александрович Кривчиков,
#### к.ф.-м.н., н.с. МГУ имени М. В. Ломоносова
#### maxim.krivchikov@gmail.com

Презентация online: https://goo.gl/THpvIu

<span class="small">
Исследования поддержаны грантом РФФИ № 16-07-01178 А «Методы и средства разработки верифицируемого программного обеспечения с использованием предметно-ориентированных языков, имеющих заданную формальную семантику».
</span>
