<p align="center"><b>МОНУ НТУУ КПІ ім. Ігоря Сікорського ФПСПМ СПіСКС</b></p>
<p align="center">
<b>Звіт з лабораторної роботи 2</b><br/>
"Рекурсія"<br/>
дисципліни "Вступ до функціонального програмування"
</p>
<p align="right"><b>Студентка</b>: Швидько Крістіна Романівна КВ-31</p>
<p align="right"><b>Рік</b>: 2026</p>

### Загальне завдання

```lisp
;; Реалізуйте дві рекурсивні функції, що виконують деякі дії з вхідним(и)
;; списком(-ами), за можливості/необхідності використовуючи різні види
;; рекурсії. Функції, які необхідно реалізувати, задаються варіантом (п. 2.1.
;; 1). Вимоги до функцій:
;; 1. Зміна списку згідно із завданням має відбуватись за рахунок
;; конструювання нового списку, а не зміни наявного (вхідного).
;; 2. Не допускається використання функцій вищого порядку чи стандартних
;; функцій для роботи зі списками, що не наведені в четвертому розділі
;; навчального посібника.
;; 3. Реалізована функція не має бути функцією вищого порядку, тобто приймати
;; функції в якості аргументів.
;; 4. Не допускається використання псевдофункцій (деструктивного підходу).
;; 5. Не допускається використання циклів.
;; Кожна реалізована функція має бути протестована для різних тестових
;; наборів. Тести мають бути оформленні у вигляді модульних тестів (див. п. 2.
;; 3). Додатковий бал за лабораторну роботу можна отримати в разі виконання
;; всіх наступних умов:
;; робота виконана до дедлайну (включно з датою дедлайну)
;; крім основних реалізацій функцій за варіантом, також реалізовано додатковий
;; варіант однієї чи обох функцій, який працюватиме швидше за основну
;; реалізацію, не порушуючи при цьому перші три вимоги до основної реалізації
;; (вимоги 4 і 5 можуть бути порушені), за виключенням того, що в разі
;; необхідності можна також використати стандартну функцію copy-list
```

### Варіант 17

### Лістинг функції remove-seconds-and-thirds

```lisp
;; 1. Написати функцію remove-seconds-and-thirds , яка видаляє зі списку
;; кожен другий і третій елементи:
;; CL-USER> (remove-seconds-and-thirds '(a b c d e f g))
;; (A D G)
(defun remove-seconds-and-thirds (lst)
  (cond
    ((null lst)
     nil)

    (t
     (cons (car lst)
           (remove-seconds-and-thirds
            (cdr
             (cdr
              (cdr lst))))))))

```

### Тестові набори та утиліти

```lisp
     (defun test-remove-seconds-and-thirds ()
  (assert
   (equal (remove-seconds-and-thirds nil)
          nil))

  (assert
   (equal (remove-seconds-and-thirds '(a))
          '(a)))

  (assert
   (equal (remove-seconds-and-thirds '(a b))
          '(a)))

  (assert
   (equal (remove-seconds-and-thirds '(a b c))
          '(a)))

  (assert
   (equal (remove-seconds-and-thirds '(a b c d))
          '(a d)))

  (assert
   (equal (remove-seconds-and-thirds '(a b c d e f g))
          '(a d g)))

  (assert
   (equal (remove-seconds-and-thirds '(1 2 3 4 5 6 7 8 9 10))
          '(1 4 7 10)))

  t)
```

### Лістинг функції list-set-intersection

```lisp
;; 2. Написати функцію list-set-intersection , яка визначає перетин двох множин,
;; заданих списками атомів:
;; CL-USER> (list-set-intersection '(1 2 3 4) '(3 4 5 6))
;; (3 4) ; порядок може відрізнятись

(defun estlist (item lst)
  (cond
    ((null lst)
     nil)

    ((eql item (car lst))
     t)

    (t
     (estlist item (cdr lst)))))
    (defun list-set-intersection (set1 set2)
  (cond
    ((null set1)
     nil)

    ((estlist (car set1) set2)
     (cons (car set1)
           (list-set-intersection (cdr set1) set2)))
    (t
     (list-set-intersection (cdr set1) set2))))
```

### Тестові набори та утиліти

```lisp
(defun test-list-set-intersection ()
  (assert
   (equal (list-set-intersection nil '(1 2 3))
          nil))

  (assert
   (equal (list-set-intersection '(1 2 3) nil)
          nil))

  (assert
   (equal (list-set-intersection '(1 2 3) '(4 5 6))
          nil))

  (assert
   (equal (list-set-intersection '(1 2 3 4) '(3 4 5 6))
          '(3 4)))

  (assert
   (equal (list-set-intersection '(a b c d) '(b d e))
          '(b d)))

  (assert
   (equal (list-set-intersection '(a b c) '(a b c))
          '(a b c)))
  t)
```

### Тестування

```lisp
 (defun run-tests ()
  (format t "Testing REMOVE-SECONDS-AND-THIRDS...~%")
  (test-remove-seconds-and-thirds)
  (format t "Tests passed.~%")

  (format t "Testing LIST-SET-INTERSECTION...~%")
  (test-list-set-intersection)
  (format t "Tests passed.~%")

  t)
  (run-tests)
  (format t "~%First result: ~A~%"
        (remove-seconds-and-thirds '(a b c d e f g)))

(format t "Second result: ~A~%"
        (list-set-intersection '(1 2 3 4) '(3 4 5 6)))
```

### Результат виконання:

```text
Testing REMOVE-SECONDS-AND-THIRDS...
Tests passed.
Testing LIST-SET-INTERSECTION...
Tests passed.

First result: (A D G)
Second result: (3 4)
```
