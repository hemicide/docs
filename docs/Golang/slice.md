# Slice

Создайте пакет для базовых математических вычислений со слайсами. В итоге проект будет содержать два пакета main и тот, который разработали вы. Расположение папок и структура проекта остаются на ваше усмотрение.
Пакет должен реализовывать следующие математические операции:

`func SumSlice(s []int) int` возвращает сумму слайса.
`func MapSlice(s []int, op func (int) int)` применяет функцию, переданную во втором аргументе к слайсу.
`func FoldSlice(s []int, op func (int, int) int, init int) int` сворачивает слайс.

То есть применяют операцию по цепочке сначала к `init` и `s[]`, потом результат и полученное значение — к следующему и так далее. Итоговый результат возвращается функцией, которая не должна изменять слайс.
Импортируйте ваш проект в свою программу и протестируйте:

```go
package mathslice

type Slice []Element

type Element int

// SumSlice — возвращает сумму элементов
func SumSlice(slice Slice) (res Element) {
    for _, s := range slice {
        res += s
    }
    return
}

// MapSlice — применяет функцию op к каждому элементу
func MapSlice(slice Slice, op func(Element) Element) {
    for i, s := range slice {
        slice[i] = op(s)
    }
}

// FolвSlice — сворачивает слайс.
func FoldSlice(slice Slice, op func(Element, Element) Element, init Element) (res Element) {
    res = op(init, slice[0])
    for i := 1; i < len(slice); i++ {
        res = op(res, slice[i])
    }
    return
}
```

```go
package main

import (
    "exercise/mathslice"
    "fmt"
)

func main() {

    s := mathslice.Slice{1, 2, 3}
    fmt.Println(s)
    fmt.Println("Сумма слайса: ", mathslice.SumSlice(s))

    mathslice.MapSlice(s, func(i mathslice.Element) mathslice.Element {
        return i * 2
    })

    fmt.Println("Слайс, умноженный на два: ", s)

    fmt.Println("Сумма слайса: ", mathslice.SumSlice(s))

    fmt.Println("Свёртка слайса умножением ",
        mathslice.FoldSlice(s,
            func(x mathslice.Element, y mathslice.Element) mathslice.Element {
                return x * y
            },
            1))

    fmt.Println("Свёртка слайса сложением ",
        mathslice.FoldSlice(s,
            func(x mathslice.Element, y mathslice.Element) mathslice.Element {
                return x + y
            },
            0))

}
```
