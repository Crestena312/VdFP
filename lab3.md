<p align="center"><b>МОНУ НТУУ КПІ ім. Ігоря Сікорського ФПСПМ СПіСКС</b></p>
<p align="center">
<b>Звіт з лабораторної роботи 3</b><br/>
"Функціональний і імперативний підходи до роботи зі списками"<br/>
дисципліни "Вступ до Functionallyго програмування"
</p>
<p align="right"><b>Студентка</b>: Швидько Крістіна Романівна КВ-31</p>
<p align="right"><b>Рік</b>: 2026</p>

### Загальне завдання

```lisp
;; Реалізуйте алгоритм сортування чисел у списку двома способами: Functionally і
;; Imperatibely.
;; 1. Функціональний варіант реалізації має базуватись на використанні рекурсії і
;; конструюванні нових списків щоразу, коли необхідно виконати зміну вхідного
;; списку. Не допускається використання: псевдо-функцій, деструктивних операцій,
;; циклів . Також реалізована функція не має бути функціоналом (тобто приймати на
;; вхід функції в якості аргументів).

;; 2. Імперативний варіант реалізації має базуватись на використанні циклів і
;; деструктивних функцій (псевдофункцій). Не допускається використання функцій
;; вищого порядку або функцій для роботи зі списками/послідовностями, що
;; використовуються як функції вищого порядку. Тим не менш, оригінальний список
;; цей варіант реалізації також не має змінювати, тому перед виконанням
;; деструктивних змін варто застосувати функцію copy-list (в разі необхідності).
;; Також реалізована функція не має бути функціоналом (тобто приймати на вхід
;; функції в якості аргументів).
;; Алгоритм, який необхідно реалізувати, задається варіантом (п. 3.1.1). Зміст і шаблон
;; звіту наведені в п. 3.2.
;; Кожна реалізована функція має бути протестована для різних тестових наборів. Тести
;; мають бути оформленні у вигляді модульних тестів (наприклад, як наведено у п. 2.3).
```

### Варіант 17

### Алгоритм сортування обміном No1 (без оптимізацій) за незмeншенням

### Функція з використанням Functionallyго сортування

```lisp
(defun bubblepass (lst)
  (cond
    ((null lst)
     nil)

    ((null (cdr lst))
     (cons (car lst) nil))

    ((> (car lst) (cadr lst))
     (cons (cadr lst)
           (bubblepass
            (cons (car lst) (cddr lst)))))

    (t
     (cons (car lst)
           (bubblepass (cdr lst))))))


(defun bubblepasses (lst passes)
  (if (<= passes 0)
      lst
      (bubblepasses
       (bubblepass lst)
       (1- passes))))


(defun bubblesortf (lst)
  (bubblepasses lst
                (1- (length lst))))

```

### Тестові набори та утиліти

```lisp
(defun tsorting (input expected)
  (let* ((original (copy-list input))
         (functres (bubblesortf input)))

    (assert (equal functres expected))

    (assert (equal input original)))

  t)


(defun tbubblesorts ()
  (tsorting nil nil)

  (tsorting (list 7)
            (list 7))

  (tsorting (list 2 1)
            (list 1 2))

  (tsorting (list 1 2 3 4 5)
            (list 1 2 3 4 5))

  (tsorting (list 5 4 3 2 1)
            (list 1 2 3 4 5))

  (tsorting (list 5 3 4 1 2)
            (list 1 2 3 4 5))

  (tsorting (list 3 1 3 2 1)
            (list 1 1 2 3 3))

  (tsorting (list 4 4 4 4)
            (list 4 4 4 4))

  (tsorting (list -2 5 0 -7 3)
            (list -7 -2 0 3 5))

  (tsorting (list 2.5 -1.5 0.0 1.25)
            (list -1.5 0.0 1.25 2.5))

  (format t "Passed.~%")
  t)
```

### Функція з використанням Imperatibelyго сортування

```lisp
(defun bubblesortimp (lst)
  (let* ((result (copy-list lst))
         (n (length result)))

    (loop repeat (max 0 (1- n)) do
      (loop for tail on result
            while (cdr tail)
            do
               (when (> (car tail) (cadr tail))
                 (rotatef (car tail)
                          (cadr tail)))))

    result))
```

### Тестові набори та утиліти

```lisp
(defun tsortingimp (input expected)
  (let* ((original (copy-list input))
         (result (bubblesortimp input)))

    (assert (equal result expected))

    (assert (equal input original))

    (when input
      (assert (not (eq result input)))))

  t)
(defun tbubblesortimp ()
  (tsortingimp nil nil)

  (tsortingimp (list 7)
               (list 7))

  (tsortingimp (list 2 1)
               (list 1 2))

  (tsortingimp (list 1 2 3 4 5)
               (list 1 2 3 4 5))

  (tsortingimp (list 5 4 3 2 1)
               (list 1 2 3 4 5))

  (tsortingimp (list 5 3 4 1 2)
               (list 1 2 3 4 5))

  (tsortingimp (list 3 1 3 2 1)
               (list 1 1 2 3 3))

  (tsortingimp (list 4 4 4 4)
               (list 4 4 4 4))

  (tsortingimp (list -2 5 0 -7 3)
               (list -7 -2 0 3 5))

  (tsortingimp (list 2.5 -1.5 0.0 1.25)
               (list -1.5 0.0 1.25 2.5))

  (format t "Passed.~%")
  t)
```

### Тестування

```lisp
(tbubblesorts)
(format t "Functionally: ~a~%"
        (bubblesortf '(5 3 4 1 2)))
(tbubblesortimp)
(format t "Imperatibely: ~a~%"
        (bubblesortimp '(5 3 4 1 2)))
```

### Результат виконання:

```text
Passed.
Functionally: (1 2 3 4 5)
Passed.
Imperatibely: (1 2 3 4 5)
```
