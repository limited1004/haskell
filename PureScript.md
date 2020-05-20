### История


PureScript был первоначально разработан Филом Фриманом в 2013 году. Он начал работать над PureScript, поскольку различные попытки скомпилировать Haskell в JavaScript при сохранении его семантики (например, с использованием Fay, Haste или GHCJS) не сработали к его удовлетворению. 

С тех пор он был подхвачен сообществом и разработан на GitHub. Дополнительные основные инструменты, разработанные сообществом, включают в себя специальный инструмент сборки "Pulp", каталог документации "Pursuit" и менеджер пакетов "Spago".


# Особенности



PureScript отличается строгой оценкой, постоянными структурами данных и выводом типов. Система типов PureScript имеет много общих функций с аналогичными функциональными языками, такими как Haskell: алгебраические типы данных и сопоставление с образцом, типы с более высоким родом, классы типов и функциональные зависимости и полиморфизм более высокого ранга. Система типов PureScript добавляет поддержку полиморфизма строк и расширяемых записей. Однако в PureScript отсутствует поддержка некоторых более продвинутых функций Haskell, таких как GADT и семейства типов.

Компилятор PureScript пытается создать читабельный код JavaScript, где это возможно. Через простой интерфейс FFI он также позволяет повторно использовать существующий код JavaScript.
PureScript поддерживает пошаговую компиляцию, а в дистрибутив компилятора включена поддержка создания плагинов редактора исходного кода для итеративной разработки . Плагины редактора существуют для многих популярных текстовых редакторов, включая Vim, Emacs, Sublime Text, Atom и Visual Studio Code.

PureScript поддерживает разработку по типу с помощью функции типизированных дырок , в которой программа может быть построена с отсутствующими подвыражениями. Впоследствии компилятор попытается определить типы пропущенных подвыражений и сообщить об этих типах пользователю. Эта особенность вдохновила на аналогичную работу в компиляторе GHC Haskell. 



**Table of Contents**

[TOCM]

[TOC]

#Начало работы

Поскольку Purescript - это собственный язык, нам понадобятся некоторые новые инструменты. Вы можете следовать инструкциям на сайте Purescript, но вот основные моменты.

Установите Node.js и NPM, менеджер пакетов Node.js.
-      Запустите npm install -g purescript
-      Запустите npm install -g целлюлозный бауэр
-      Создайте каталог вашего проекта и запустите pulp init.
-      Затем вы можете создать и протестировать код с помощью сборки пульпы и теста пульпы.
-      Вы также можете использовать PSCI в качестве консоли, аналогично GHCI.

Во-первых, нам нужен NPM. Purescript - это его собственный язык, но мы хотим скомпилировать его в Javascript, который мы можем использовать в браузере, поэтому нам нужен Node.js. Тогда мы будем глобально устанавливать библиотеки Purescript. Мы также установим целлюлозу и беседку. Pulp будет нашим инструментом для сборки, таким как Cabal.

Bower - это хранилище пакетов, подобное Hackage. Чтобы добавить дополнительные библиотеки в нашу программу, вы должны использовать команду bower. Например, нам нужны целые числа purescript для нашего решения позже в этой статье. Чтобы получить это, запустите команду:

bower install - сохранить purescript-integer
##Простой пример

Как только вы настроитесь, пришло время побаловаться с языком. В то время как Purescript компилируется в Javascript, сам язык на самом деле больше похож на Haskell! Мы рассмотрим это в сравнении. Предположим, мы хотим найти все пифагорейские тройки, сумма которых меньше 100. Вот как мы можем написать это решение на Хаскелле:
```haskell
sourceList :: [Int]
sourceList = [1..100]

allTriples :: [(Int, Int, Int)]
allTriples =
  [(a, b, c) | a <- sourceList, b <- sourceList, c <- sourceList]

isPythagorean :: (Int, Int, Int) -> Bool
isPythagorean (a, b, c) = a ^ 2 + b ^ 2 == c ^ 2

isSmallEnough :: (Int, Int, Int) -> Bool
isSmallEnough (a, b, c) = a + b + c < 100

finalAnswer :: [(Int, Int, Int)]
finalAnswer = filter 
  (\t -> isPythagorean t && isSmallEnough t)
      allTriples
```

Давайте сделаем модуль в Purescript, который позволит нам решить эту же проблему. Начнем с написания модуля Pythagoras.purs. Вот код, который мы напишем, чтобы он соответствовал описанному выше Haskell. Мы рассмотрим детали по частям ниже.

```purescript
module Pythagoras where

import Data.List (List, range, filter)
import Data.Int (pow)
import Prelude

sourceList :: List Int
sourceList = range 1 100

data Triple = Triple
  { a :: Int
  , b :: Int
  , c :: Int
  }

allTriples :: List Triple
allTriples = do
  a <- sourceList
  b <- sourceList
  c <- sourceList
  pure $ Triple {a: a, b: b, c: c}

isPythagorean :: Triple -> Boolean
isPythagorean (Triple triple) =
  (pow triple.a 2) + (pow triple.b 2) == (pow triple.c 2)

isSmallEnough :: Triple -> Boolean
isSmallEnough (Triple triple) =
  (triple.a) + (triple.b) + (triple.c) < 100

finalAnswer :: List Triple
finalAnswer = filter
  (\triple -> isPythagorean triple && isSmallEnough triple) 
  allTriples
```
#Purescript Типы данных
##### PureScript имеет 3 встроенных типа, которые соответствуют примитивным типам JavaScript:
1. Числа (Eg. 1.0, 2.0, 3.14, 5.4398576)
2. Строки (Eg. “Hello World”, “Foo Bar”)
3. Булевые (Eg. true, false)

###### PureScript также имеет другие встроенные типы, такие как:
1.     Int (Eg. 141, 171, 0, -3, -120)
2.     Char (Eg. ‘a’, ‘b’, ‘y’, ’z’ Note. Character literals are wrapped in single quotes)
3.     Arrays (All elements of the Array should be of same data type)
4.     Records (Records are similar to JavaScript Objects, they are Key-Value pairs)
5.     Functions (Functions are similar to functions of any other languages, which can be invoked by its name and passing its parameters)

# Определение типа с использованием PSCI

PSCI - это интерактивный инструмент PureScript, который может выполнить команду в терминале

```bash
$ pulp psci
```

Мы можем найти тип любой объявленной переменной, используя: type или: t, за которыми следует имя переменной в интерактивном терминале.

![](https://github.com/limited1004/haskell/blob/master/img/img.png)

Здесь создается переменная `intVal` и ей присваивается значение 5.

Чтобы найти тип переменной, мы используем команду `: type`, за которой следует имя переменной.

PSCI возвращает тип intVal как Int.

Аналогичным образом были объявлены переменные для других типов данных, таких как Char, String и был найден тип переменных:.

recordVal - это запись PureScript. Это как объекты JavaScript. когда: type был применен к recordVal, мы находим тип данных каждого ключа внутри записей. Эта концепция важна, так как мы будем применять ее в дальнейших концепциях и при создании наших собственных типов.

Как было сказано ранее, Array - это встроенный тип данных. Его можно определить как совокупность элементов одного вида.

Некоторые примеры массива:

Массив целых чисел:
```
[-2,-1,0,1,2,3,4,5,6,7]
```