Задача 1. Установите golang.

```bash
Tsukanovs-Air:~ tsukanovdmitry$ go version
go version go1.19.1 darwin/amd64
Tsukanovs-Air:~ tsukanovdmitry$ 
```

Задача 2. Знакомство с gotour.
Изучил))

Задача 3. Написание кода.

Напишите программу для перевода метров в футы (1 фут = 0.3048 метр). Можно запросить исходные данные у пользователя, а можно статически задать в коде. Для взаимодействия с пользователем можно использовать функцию Scanf:

Почему то песочница не хотела принимать значения на вход... сделал статически

```go 
package main

import "fmt"

func main() {
	var input float64
	input = 5

	output := input * 3.28

	fmt.Println(output, "ft")
}
```
Напишите программу, которая найдет наименьший элемент в любом заданном списке, например:

```go
package main

import "fmt"

func main() {
	x := []int{48, 96, 86, 68, 57, 82, 63, 70, 37, 34, 83, 27, 19, 97, 9, 17}

	min := x[0]
	for _, y := range x {
		if y < min {
			min = y
		}
	}

	fmt.Println("minimum:", min)
}
```
Напишите программу, которая выводит числа от 1 до 100, которые делятся на 3. То есть (3, 6, 9, …).

```go
package main

import "fmt"

func main() {
	for i := 1; i < 100; i++ {
		if i%3 == 0 {
			fmt.Println(i)
		}
	}
}
```
