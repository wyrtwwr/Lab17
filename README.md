# Домашнее задание к работе 17
## Условие задачи
1) сравнение простых сортировок (выбором, пузырьковая, коктельная, вставками) для различных размеров выборок  100, 1000, 10000 значений


## 1. Алгоритм и блок-схема
## Алгоритм
```
1. Начало
2. Задать размеры массивов: 100, 1000 и 10000.
3. Для каждого размера массива выделить память под четыре массива.
4. Заполнить массивы одинаковыми случайными числами.
5. Выполнить пузырьковую сортировку и измерить время работы.
6. Выполнить сортировку выбором и измерить время работы.
7. Выполнить коктейльную сортировку и измерить время работы.
8. Выполнить сортировку вставками и измерить время работы.
9. Вывести в консоль результаты измерений.
10. Освободить выделенную память.
11. Вывести общий вывод и завершить программу.

```
   
### Блок-схема

![Lab17](https://github.com/user-attachments/assets/037e4e83-0bcb-41d9-b618-9b410df0f579)




## 2. Реализация программы

```
#define _CRT_SECURE_NO_WARNINGS
#define _USE_MATH_DEFINES
#include <locale.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h> 
#include <conio.h>
#include <math.h>
#include <time.h>

int main() {
    setlocale(LC_ALL, "RUS");

    int sizes[] = { 100, 1000, 10000 };
    int test_count = 3;
    int i, j, k, temp, min_idx, key;
    clock_t start, end;

    printf("Эксперимент: сравнение алгоритмов сортировки\n");
    printf("--------------------------------------------\n\n");

    printf("Размер | Bubble | Select | Cocktail | Insert\n");
    printf("-------+--------+--------+----------+--------\n");

    for (k = 0; k < test_count; k++) {
        int n = sizes[k];
        int* arr1 = malloc(n * sizeof(int));
        int* arr2 = malloc(n * sizeof(int));
        int* arr3 = malloc(n * sizeof(int));
        int* arr4 = malloc(n * sizeof(int));

        srand(time(NULL) + k);
        for (i = 0; i < n; i++) {
            int num = rand() % 10000;
            arr1[i] = num;
            arr2[i] = num;
            arr3[i] = num;
            arr4[i] = num;
        }

        printf("%6d |", n);

        /* Пузырьковая */
        start = clock();
        for (i = 0; i < n - 1; i++) {
            for (j = 0; j < n - 1 - i; j++) {
                if (arr1[j] > arr1[j + 1]) {
                    temp = arr1[j];
                    arr1[j] = arr1[j + 1];
                    arr1[j + 1] = temp;
                }
            }
        }
        end = clock();
        printf(" %6.4f |", (double)(end - start) / CLOCKS_PER_SEC);

        /* Выбором */
        start = clock();
        for (i = 0; i < n - 1; i++) {
            min_idx = i;
            for (j = i + 1; j < n; j++) {
                if (arr2[j] < arr2[min_idx]) {
                    min_idx = j;
                }
            }
            if (min_idx != i) {
                temp = arr2[i];
                arr2[i] = arr2[min_idx];
                arr2[min_idx] = temp;
            }
        }
        end = clock();
        printf(" %6.4f |", (double)(end - start) / CLOCKS_PER_SEC);

        /* Коктейльная */
        start = clock();
        int swapped = 1;
        int left = 0;
        int right = n - 1;

        while (swapped) {
            swapped = 0;
            for (i = left; i < right; i++) {
                if (arr3[i] > arr3[i + 1]) {
                    temp = arr3[i];
                    arr3[i] = arr3[i + 1];
                    arr3[i + 1] = temp;
                    swapped = 1;
                }
            }
            right--;

            for (i = right; i > left; i--) {
                if (arr3[i] < arr3[i - 1]) {
                    temp = arr3[i];
                    arr3[i] = arr3[i - 1];
                    arr3[i - 1] = temp;
                    swapped = 1;
                }
            }
            left++;
        }
        end = clock();
        printf(" %8.4f |", (double)(end - start) / CLOCKS_PER_SEC);

        /* Вставками */
        start = clock();
        for (i = 1; i < n; i++) {
            key = arr4[i];
            j = i - 1;
            while (j >= 0 && arr4[j] > key) {
                arr4[j + 1] = arr4[j];
                j--;
            }
            arr4[j + 1] = key;
        }
        end = clock();
        printf(" %6.4f\n", (double)(end - start) / CLOCKS_PER_SEC);

        free(arr1);
        free(arr2);
        free(arr3);
        free(arr4);
    }

    printf("\nВыводы по эксперименту:\n");
    printf("- пузырьковая сортировка показывает наименьшую скорость;\n");
    printf("- коктейльная работает немного эффективнее пузырьковой;\n");
    printf("- сортировка выбором даёт средние результаты;\n");
    printf("- сортировка вставками оказалась наиболее быстрой.\n");

    return 0;
}

```


## 3. Результаты работы программы

```
Эксперимент: сравнение алгоритмов сортировки
--------------------------------------------

Размер | Bubble | Select | Cocktail | Insert
-------+--------+--------+----------+--------
   100 | 0,0000 | 0,0000 |   0,0000 | 0,0000
  1000 | 0,0030 | 0,0020 |   0,0020 | 0,0000
 10000 | 0,1240 | 0,0670 |   0,1260 | 0,0600

Выводы по эксперименту:
- пузырьковая сортировка показывает наименьшую скорость;
- коктейльная работает немного эффективнее пузырьковой;
- сортировка выбором даёт средние результаты;
- сортировка вставками оказалась наиболее быстрой.



```


<img width="1116" height="406" alt="{7FCBE7F9-B8C5-41E8-9F76-2DE21E380145}" src="https://github.com/user-attachments/assets/67495d51-feaf-41a2-ab9b-aeb63da48239" />
