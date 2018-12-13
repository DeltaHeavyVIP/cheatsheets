# Что такое анонимные страницы? 

Анонимные страницы - это данные, используемые программами и не ассоциированные с каким-либо файлом.   
Число анонимных страниц для конкретного процесса можно вычислить как разницу между размером резидентной части (resident) 
и разделяемыми страницами (share) в выводе /proc/PID/statm (информация о состоянии памяти в страницах).

# Что такое таблица страниц?

Таблица страниц состоит из 4-байтовых элементов. Эти элементы называются PTE (Page Table Entries)
и представляют собой по сути - указатели на страницы, по формату - структуры данных.

Структура таблиц:

- P (Present - присутствие). Если 0, то страница не отображена на физическую память. 
- R / W (Read / Write - Чтение / Запись). 
- U / S (User / Supervisor - Пользователь / Система). Если 0, то доступ к странице разрешён только с нулевого уровня привилегий, если 1 - то со всех.
- PWT (Write-Through - Сквозная запись). Когда этот флаг установлен, разрешено кэширование сквозной записи для данной страницы, когда сброшен - кэширование обратной записи.
- PCD (Cache Disabled - Кэширование запрещено). 
- A (Accessed - Доступ). Устанавливается процессором каждый раз, когда он производит обращение к данной странице. Процессор не сбрасывает этот флаг - его может может сбросить программа, чтобы потом, через некоторое время определить, был ли доступ к этой странице, или нет.
- D (Dirty - Грязный). Устанавливается каждый раз, когда процессор производит запись в данную страницу. Этот флаг также не сбрасывается процессором.
- PAT (Page Table Attribute Index - Индекс атрибута таблицы страниц). Для процессоров, которые используют таблицу атрибутов страниц, этот флаг используется совместно с флагами PCD и PWT для выбора элемента в PAT, который выбирает тип памяти для страницы.
- G (Global Page - Глобальная страница).  Такая страница остаётся достоверной в кэшах TLB при перезагрузке регистра CR3 или переключении задач. 
- Биты с 9 по 11 не используются процессором.
- Биты с 12 по 31 - это физический адрес

Если страница не присутствует в памяти (бит P=0), то процессор не использует все остальные биты элемента PTE и программа может их использовать по своему усмотрению.

# Какие аргументы принимает longjmp?

`void longjmp(jmp_buf env, int val)` (Си) - переход к заранее сохраненному состоянию. (Вообще, эта штука - аналогия try/catch. `setjmp` 
сохраняет все необходимые указатели в структуре jmp_bu, а `longjmp` получает на вход структуру, 
извлекает из неё указатель следующей инструкции и указатель на вершину стека, и восстанавливает их)

`jmp_buf env` - указатель на структуру, int val - значение, которое вернет setjmp() при восстановлении состояния программы (val нельзя 
присвоить 0, если вы это сделали, то автоматически присвоится 1)

# Что такое побочный эффект?

Побочный эффект возникает, когда вычисление выражение влечёт за собой изменение значения данных в приложении.

x + 15 не дает побочный эффект, так как даст новое значение, но не поменяет переменную, а х++ даст побочный эффект

# Опишите функцию fread() и ее аргументы.

`size_t fread(void *buf, size_t size, size_t count, FILE *stream)`

`void *buf` - указатель на буфер для записи считанных символов, `size_t size` - размер объекта, `size_t count` - количество объектов,
`FILE *stream` - файл для чтения.
Возвращает количество действительно считанных объектов.

# Что такое дескриптор?

Дескриптор представляет собой "ключ", который обеспечивает абстракцию ресурса, по сути он может быть чем угодно:
от целочисленного индекса до указателя на ресурс в пространстве ядра.
Например, ко всем потокам ввода-вывода можно получить доступ через так называемые файловые дескрипторы. 
- 0	Стандартный ввод.
- 1	Стандартный вывод.
- 2	Стандартный вывод сообщений об ошибках.

# Что такое заголовочный файл? Зачем они используются? Объясните подробно и приведите пример.

заголовочный файл — файл, содержимое которого автоматически добавляется препроцессором в исходный текст в том месте, 
где располагается директива  #include <file.h> — основной способ подключить к программе типы данных, структуры, прототипы функций, перечислимые типы и макросы, используемые в другом 
модуле.

Чтобы избежать повторного включения одного и того же кода, используются директивы `#ifndef`, `#define`, `#endif`.

Используется для того, чтобы создавать программы из нескольких модулей, компилируемых по отдельности.

```c
 #ifndef ADD_H
 #define ADD_H
 
 int add(int, int);
 
 #endif
```

# Чему равны старшие биты rax после инструкции mov eax, aah?

Леха сказал, что обнуляет.

# Что делает команда mul?

Для умножения чисел без знака предназначена команда MUL. 
У этой команды только один операнд — второй множитель, который должен находиться в регистре или в памяти.

# Определите тип беззнаковых целых однобайтных чисел num_t?

```c
typedef unsigned char num_t;
```

# Что такое интернирование строк?

Интернирование строк — это механизм, при котором одинаковые литералы представляют собой один объект в памяти.

(В си при любом действии над строкой у нас получается новая строка)






