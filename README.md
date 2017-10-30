# stack@0.0.1

### Описание

Написать программу работы со стеком. Стек должен иметь такое же перераспределение памяти как у вектора. При этом класс должен обладать следующим интерфейсом:

```
template <typename T>
class stack
{
public:
  stack();
  size_t count() const;
  void push(T const &);
  T pop();
private:
  T * array_;
  size_t array_size_;
  size_t count_;
};
```
### Ссылки

- https://github.com/justcppdev/palindrome - образец проекта с тестами и примером

### Входные/Выходные данные
Во входных данных задана строка, следующего формата:
- `+ 5` - вставить в стэк значение **5**
- `?` - вывести значение последнего элемента
- `-` - достать из стека последний элемент
- `=` - вывести на экран содержимое стека

### Примеры
#### входные данные
`+ 4`

`+ 8`

`?`

`=`

`+ 15`

`?`

`-`

`=`
#### выходные данные
`8`

`4 8`

`15`

`4 8`
#### входные данные
`-`

`+`
#### выходные данные
`Stack is empty`

`An error has occurred while reading arguments`

# stack@0.0.2

- Написать тесты для всех методов.
- Заменить циклы на std::copy. 
- Избавиться от дублирования кода (new + copy) в операторе копирующего присваивания, используя метод swap.

# stack@0.0.3

- Пометить все методы одной из 4 меток:
  - `/*unsafe*/`- может возникнуть исключение, при котором объекты могут перейти в невалидное состояние или возможна утечка ресурсов
  - `/*basic*/` - может возникнуть исключение, но объекты останутся в валидном состоянии и не будет утечки ресурсов
  - `/*strong*/` - может возникнуть исключение, но объекты останутся в прежнем состоянии и не будет утечки ресурсов
  - `/*noexcept*/` - исключение не возникает
  
  Пример:
```
template <typename T>
class stack
{
public:
  stack(); /*strong*/
  size_t count() const /*noexcept*/;
  void push(T const &) /*basic*/;
  T pop() /*unsafe*/;
private:
  swap( stack & ) /**/;
  T * array_;
  size_t array_size_;
  size_t count_;
};
```
- Добавить метод empty().

# stack@0.0.4

- Сделать класс `stack` более безопасным относительно исключений (обеспечивающим строгую гарантию), заменив метод `pop` на два соответствующих `pop` и `top`.
```
template <typename T>
class stack
{
public:
  stack(); /*noexcept || strong*/
  size_t count() const /*noexcept || strong*/;
  void push(T const &) /*noexcept || strong*/;
  void pop() /*noexcept || strong*/;
  T top() /*noexcept || strong*/
private:
  swap( stack & ) /*noexcept || strong*/
  T * array_;
  size_t array_size_;
  size_t count_;
};
```

# stack@0.0.5

- Сделать класс `stack` потокобезопасным,  устранив:
  - гонку за данными, используя объект типа `std::mutex`
  - архитектурную ошибку, объеденив методы `pop`  и `top` в один `pop`
```
template <typename T>
class stack /*thread-safe*/
{
public:
  stack();
  size_t count() const;
  void push(T const &);
  auto pop() -> std::shared_ptr<T>;
private:
  swap( stack & other )
  T * array_;
  size_t array_size_;
  size_t count_;
};
```


