# Статические массивы

Массивы бывают двух типов: [статические](./c-array.md) и [динамические](dynamic-array.md). Давайте, рассмотрим статические.

Особенности статических массивов
===
>Главная особенность статических массивов - неизменяемый размер. Размер статического массива должен быть известен до компиляции, то есть считать с консоли переменную N и после создать массив размером N не получится, размер массива должен быть только константой, например, числом 2352. 

Давайте рассмотрим объявление и инициализацию массива:
```cpp
int my_second_array[5] = {1, 2, 3, 4, 5};
int my_first_array[25];
my_first_array[0] = 1;
my_first_array[1] = 2;
my_first_array[2] = 3;
```
В первой строке мы создали массив размером 5 и сразу задали значения всем пяти его ячейкам.
Далее мы объявили массив целочисленного типа данных int с именем my_first_array и вместимостью 25, а после инициализировали его первые три ячейки значениями 1, 2 и 3, соответственно.

Итерация по элементам массива
===
Доступ к элементам массива происходит по следующей форме: имя_Массива[индекс_нужного, элемента]
В примере выше по такой формуле мы задали значения элементам массива my_first_array.
```cpp
my_first_array[0] =
    1;  // имя массива - my_first_array, индекс нужного элемента - 0
my_first_array[1] =
    2;  // имя массива - my_first_array, индекс нужного элемента - 1
my_first_array[2] =
    3;  // имя массива - my_first_array, индекс нужного элемента - 2
```
Теперь самое время задаться вопросом, "А что делать, если у меня массив из 25 элементов, и я хочу каждый элемент сделать больше на 2, чем предыдущий. Мне что, 25 раз считать самому/самой и 25 раз писать процедуру присваивание?!". 
К счастью, нет. Тут нам на помощь приходят циклы, в частности цикл for(). 

Давайте воспользуемся циклом for(), чтобы считать N элементов в наш массив, а после вывести их на консоль.
```cpp
#include <iostream>

int main() {
  int N = 256;
  int my_array[N];  // создали массив целочисленного типа размера N. Строкой
                    // выше мы присвоили N значение константы 256, так что можем
                    // себе позволить использовать переменную как параметр
                    // размера. Размер массива будет, соответственно, 256

  for (int i = 0; i < N;
       i++) {  // переменная i будет увеличиваться каждую итерацию и принимать
               // значения в диапазоне от 0 до N-1 включительно, то есть мы
               // сможем обратить к первым N элементам.
    std::cin >> my_array[i];  // считываем данные с консоли и присываиваем их
                              // элементу массива с номером i
  }

  for (int i = 0; i < N; i++) {
    std::cout << my_array[i] << " ";  // поочередно выводим все элементы
  }
}
```
В комментариях в коде построчно объяснена логика программы. Теперь давайте попробуем справиться с проблемой, о которой мы переживали чуть выше. Как сделать элементы массива такими, чтобы предыдущий был меньше текущего на 2

```cpp
#include <iostream>

int main() {
  int N = 256;
  int my_array[N];  // создали массив
  my_array[0] = 1;  // приравняли к единице первый элемент массива

  for (int i = 1; i < N; i++) {
    my_array[i] = my_array[i - 1] + 2;
  }

  for (int i = 0; i < N; i++) {
    std::cout << my_array[i] << " ";  // поочередно выводим все элементы
  }
}
```
Давайте внимательно посмотрим на наш код. Мы приравняли первый элемент массива к единице, чтобы нам было, к чему прибавлять двойку. Далее мы проходимся циклам по всем элементам массива с индексами от 1 до N-1 включительно.

Почему с единицы, наверное, хотите спросить вы. Мы ответим, потому что: 
а)элемент с нулевым индексом мы уже приравняли к единице и больше не будем менять его значение
б) my_array[i] = my_array[i-1] + 2; Здесь мы обращаемся к элементу my_array[i-1], чтобы узнать значение предыдущего элемента перед текущим с индексом i. Если бы мы начали цикл не с 1, а с 0, то программа бы обратилась к элементу my_array[0-1], то есть к элементу с индексом -1, что является ошибкой.

>Всегда помните о том, что вы не должны выходить за границы массива.

Указатель
===

Переменная статического массива на самом деле это указатель на первый элемент массива.

Следствия :

1. Массивы нельзя присваивать. Так как просто указатель присвоить непонятно, что
делать с "потерявшейся памятью". Ещё есть вариант когда можно перекопировать все переменные, но это работает за `O(n)`, решили такое не делать, хотя в векторе такое есть. Это пришло из `C` исторически.

2. `sizeof(a)` гораздо больше чем `sizeof(pointer)``

3. Если вы сравните два массива с помощью `==`, он сравнит адреса массивов, поэтому он даст результат `true` только в том случае, если вы сравните массив с самим собой (или с указателем на элемент того же типа). В большинстве контекстов имена массивов превращаются в указатель на первый элемент массива. Вот почему многие новички думают, что массивы и указатели — это одно и то же. На самом деле это не так. Они разных типов.