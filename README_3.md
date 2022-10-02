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
