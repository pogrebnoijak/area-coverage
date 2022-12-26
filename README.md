# Area Coverage

## Описание

Проект, реализующий алгоритмы покрытия области.

### Пример

Разными цветами отмечены области, на которые декомпозирована карта и отдельно выделены препятствия. Пусть показан линией, последнее положение - красной точкой.

![boustrophedon_simple](./images/boustrophedon_simple.png)

## Установка

[//]: # (FIXME git checkout coverage)
```bash
git clone https://github.com/pogrebnoijak/area-coverage
cd area-coverage
git checkout coverage  
```

После этого появится `coverage.ipynb` - достаточно запустить его как обычный `jupyter notebook`.

## Запуск

### Входные данные

Есть класс `Map`, он принимает файлы формата [movingai](https://movingai.com/benchmarks/formats.html). Полученный инстанс впоследствии можно использовать для запуска алгоритмов.

Помимо этого данные могут иметь вид `List[List[int]]`, где стоят `1` в занятых клетках, а во всех остальных - `0`. В таком виде можно тоже передать карту для покрытия.

Например, так выглядит структура карты из примера выше.

```python
mapa = [
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 1, 1, 0, 0, 0, 0, 0, 0],
        [0, 0, 1, 1, 0, 0, 0, 1, 1, 1],
        [0, 0, 0, 0, 0, 0, 0, 1, 1, 0],
        [0, 0, 0, 0, 0, 0, 0, 1, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    ]
```

Так же есть генератор карт - `generate_cells`. Она принимает размеры карты и часть которая будет покрыта препятствиями. Так же есть `random_state` для повторяемости результата.

### Запуск

Пусть у нас есть карта `mapa`, полученная любым из выше описанных способов.

Тогда получить путь можно запустив `cover(mapa)`.

Эта функция имеет несколько дополнительных необязательных (но очень полезных) аргументов:

`decompositor_type` - тип декомпозиции, применяемый для карты. Реализованы `TrapezoidalMapDecomposition` и `BoustrophedonMapDecomposition`.

`area_type` - тип карты (каждая декомпозированая часть покрывается соответсвующим алгоритмом). Реализованы `ZigZagArea` и `SpanningTreesArea`.

`show` - надо ли рисовать карту после нахождения покрывающего пути.

`debug` - надо ли перерисовывать карту после каждого шага. (глобальная переменная `DEBUG_SLEEP` - время ожидания после каждого шага).

`start` - начальная позиция на карте. Если не задана выбирается автоматически.

`cover` возвращает путь - `List[Tuple[int, int]]` - последовательные позиции на карте.
 
## Алгоритмы

Декомпозиция: `Trapezoidal decomposition, Boustrophedon decomposition`

Покрытие: `zig-zag, Spanning trees`

## Картинки, примеры

Пример в начале покрыт `Boustrophedon decomposition + zig-zag`

### `Trapezoidal decomposition + zig-zag` выглядит так

![trapezoidal_simple](./images/trapezoidal_simple.png)

### Случайная карта `100 x 100`

![random_100x100](./images/random_100x100.png)

### [NewYork_0_1024](https://movingai.com/benchmarks/street/index.html) из `movingai`

(слишком мелко, для путей, но можно увидеть разные декомпозиции).

- Trapezoidal decomposition

![trapezoidal_NewYork_0_1024](./images/trapezoidal_NewYork_0_1024.png)

- Boustrophedon decomposition

![boustrophedon_NewYork_0_1024](./images/boustrophedon_NewYork_0_1024.png)

### Карта из примера с `debug=True` в `coverage`

![example](./images/example.gif)

## Использованные материалы

- A Survey on Coverage Path Planning for Robotics
- Competitive on-line coverage of grid environments by a mobile robot
- CPP и RPP: Xu, L.: 2011, Graph Planning for Environmental Coverage,
  Chapter 4; PhD thesis; Carnegie Mellon University.
