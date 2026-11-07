<p align="center"><b>МОНУ НТУУ КПІ ім. Ігоря Сікорського ФПСПМ СПіСКС</b></p>
<p align="center">
<b>Звіт з лабораторної роботи 4</b><br/>
"Функції вищого порядку та замикання"<br/>
дисципліни "Вступ до фугкціонального програмування"
</p>
<p align="right"><b>Студентка</b>: Швидько Крістіна Романівна КВ-31</p>
<p align="right"><b>Рік</b>: 2026</p>

### Загальне завдання

```lisp
;; Завдання складається з двох частин:
;; 1. Переписати функціональну реалізацію алгоритму сортування з лабораторної
;; роботи 3 з такими змінами:
;; використати функції вищого порядку для роботи з послідовностями (де/якщо
;; це доречно, в разі, якщо функції вищого порядку не були використані при
;; реалізації л.р. No3);
;; додати до інтерфейсу функції (та використання в реалізації) два ключових
;; параметра: key та test , що працюють аналогічно до того, як працюють
;; параметри з такими назвами в функціях, що працюють з послідовностями (р.
;; 12). При цьому key має виконатись мінімальну кількість разів.

;; 2. Реалізувати функцію, що створює замикання, яке працює згідно із
;; завданням за варіантом (див. п 4.1.2). Використання псевдофункцій не
;; забороняється, але, за можливості, має бути зменшене до необхідного
;; мінімуму.
```

### Варіант 17

```lisp
;; 4.1.1 Варіанти завдань для частини першої
;; Варіанти завдань для першої частини завдання обираються ті ж, що і для третьої л.р.

;; 4.1.2 Варіанти завдань для частини другої
;; Написати функцію propagator-fn , яка має один ключовий параметр — функцію
;; comparator . propagator-fn має повернути функцію, яка при застосуванні в
;; якості
;; першого аргументу mapcar разом з одним списком-аргументом робить наступне:
;; якщо елемент не "кращий" за попередній згідно з comparator , тоді він
;; заміняється на значення попереднього, тобто "кращого", елемента. Якщо ж
;; він "кращий" за попередній
;; елемент згідно comparator , тоді заміна не відбувається. Функція
;; comparator за замовчуванням має значення #'> .
;; CL-USER> (mapcar (propagator-fn) '(1 2 3))
;; (1 2 3)
;; CL-USER> (mapcar (propagator-fn) '(3 1 4 2))
;; (3 3 4 4)
;; CL-USER> (mapcar (propagator-fn :comparator #'<) '(1 2 3))
;; (1 1 1)
```

### Лістинг реалізації першої частини завдання

```lisp
(defun bubblepass (lst test)
  (cond
    ((null lst)
     nil)

    ((null (cdr lst))
     (cons (car lst) nil))

    ((funcall test
              (car (cadr lst))
              (car (car lst)))
     (cons (cadr lst)
           (bubblepass
            (cons (car lst) (cddr lst))
            test)))

    (t
     (cons (car lst)
           (bubblepass (cdr lst) test)))))


(defun bubblepasses (lst passes test)
  (if (<= passes 0)
      lst
      (bubblepasses
       (bubblepass lst test)
       (1- passes)
       test)))

(defun bubblesortf (lst &key (key #'identity) (test #'<))
  (let* ((keyfn (or key #'identity))

         (keyed
           (mapcar (lambda (item)
                     (cons (funcall keyfn item) item))
                   lst))

         (sorted
           (bubblepasses keyed
                         (1- (length keyed))
                         test)))

    (mapcar #'cdr sorted)))
```

### Тестові набори та утиліти першої частини

```lisp

(tbubblesorts)

(defun tsorting (input expected
                 &key (key #'identity) (test #'<))
  (let* ((original (copy-tree input))
         (result
           (bubblesortf input :key key :test test)))

    (assert (equal result expected))
    (assert (equal input original)))

  t)


(defun tbubblesorts ()
  (tsorting nil nil)

  (tsorting '(7)
            '(7))

  (tsorting '(2 1)
            '(1 2))

  (tsorting '(1 2 3 4 5)
            '(1 2 3 4 5))

  (tsorting '(5 4 3 2 1)
            '(1 2 3 4 5))

  (tsorting '(5 3 4 1 2)
            '(1 2 3 4 5))

  (tsorting '(3 1 3 2 1)
            '(1 1 2 3 3))

  (tsorting '(4 4 4 4)
            '(4 4 4 4))

  (tsorting '(-2 5 0 -7 3)
            '(-7 -2 0 3 5))

  (tsorting '(2.5 -1.5 0.0 1.25)
            '(-1.5 0.0 1.25 2.5))

  (tsorting '(5 3 4 1 2)
            '(5 4 3 2 1)
            :test #'>)

  (tsorting '(-5 2 -1 4)
            '(-1 2 4 -5)
            :key #'abs)

  (tsorting '(-5 2 -1 4)
            '(-5 4 2 -1)
            :key #'abs
            :test #'>)

  (tsorting '((3 c) (1 a) (2 b))
            '((1 a) (2 b) (3 c))
            :key #'car)

  (tsorting '("bbbb" "a" "ccc" "dd")
            '("a" "dd" "ccc" "bbbb")
            :key #'length)

  (tsorting '((2 a) (1 b) (2 c) (1 d))
            '((1 b) (1 d) (2 a) (2 c))
            :key #'car)

  (tsorting '(3 1 2)
            '(1 2 3)
            :key nil)

  (let ((calls 0))
    (tsorting '(4 1 3 2)
              '(1 2 3 4)
              :key (lambda (item)
                     (incf calls)
                     item))

    (assert (= calls 4)))

  (let ((calls 0))
    (tsorting nil nil
              :key (lambda (item)
                     (incf calls)
                     item))

    (assert (= calls 0)))

  (format t "Sorting tests passed.~%")
  t)

```

### Тестування першої частини

```lisp
(tbubblesorts)

(format t "Ascending: ~a~%"
        (bubblesortf '(5 3 4 1 2)))

(format t "Descending: ~a~%"
        (bubblesortf '(5 3 4 1 2) :test #'>))

(format t "By absolute value: ~a~%"
        (bubblesortf '(-5 2 -1 4) :key #'abs))
```

### Лістинг реалізації другої частини завдання

```lisp
(defun propagator-fn (&key (comparator #'>))
  (let ((best nil)
        (initialized-p nil))

    (lambda (item)
      (cond
        ((not initialized-p)
         (setf best item
               initialized-p t))
        ((funcall comparator item best)
         (setf best item)))

      best)))
```

### Тестові набори та утиліти другої частини

```lisp
(defun tpropagator (input expected
                    &key (comparator #'>))
  (let* ((original (copy-tree input))
         (result
           (mapcar
            (propagator-fn :comparator comparator)
            input)))

    (assert (equal result expected))
    (assert (equal input original)))

  t)

(defun tpropagators ()
  (tpropagator '(1 2 3)
               '(1 2 3))

  (tpropagator '(3 1 4 2)
               '(3 3 4 4))

  (tpropagator '(1 2 3)
               '(1 1 1)
               :comparator #'<)
  (tpropagator nil nil)

  (tpropagator '(7)
               '(7))
  (tpropagator '(5 4 3 2 1)
               '(5 5 5 5 5))

  (tpropagator '(4 4 4)
               '(4 4 4))

  (tpropagator '(-3 -5 -1 -4)
               '(-3 -3 -1 -1))

  (tpropagator '(3 1 4 0 2)
               '(3 1 1 0 0)
               :comparator #'<)
  (tpropagator '("a" "bbb" "cc" "dddd")
               '("a" "bbb" "bbb" "dddd")
               :comparator
               (lambda (current previous)
                 (> (length current)
                    (length previous))))

  (tpropagator '(nil (a) nil (a b))
               '(nil (a) (a) (a b))
               :comparator
               (lambda (current previous)
                 (> (length current)
                    (length previous))))

  (let ((first-fn (propagator-fn))
        (second-fn (propagator-fn)))

    (assert (= (funcall first-fn 10) 10))
    (assert (= (funcall second-fn 2) 2))
    (assert (= (funcall first-fn 3) 10))
    (assert (= (funcall second-fn 5) 5)))

  (let ((fn (propagator-fn)))
    (assert
     (equal (mapcar fn '(3 1))
            '(3 3)))

    (assert
     (equal (mapcar fn '(2 4))
            '(3 4))))

  (format t "Closure tests passed.~%")
  t)
```

### Тестування другої частини

```lisp

(tpropagators)

(format t "Propagator >: ~a~%"
        (mapcar (propagator-fn)
                '(3 1 4 2)))

(format t "Propagator <: ~a~%"
        (mapcar (propagator-fn :comparator #'<)
                '(1 2 3)))
```

### Результат виконання:

```lisp
Sorting tests passed.
Closure tests passed.
Ascending: (1 2 3 4 5)
Descending: (5 4 3 2 1)
By absolute value: (-1 2 4 -5)
Propagator >: (3 3 4 4)
Propagator <: (1 1 1)
```
