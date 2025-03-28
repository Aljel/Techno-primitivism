Общее:
	Был создан в 1987
	Является чисто функциональным языком программирования 
	В функциональных языках все переменные неизменяемые. 
	Реализует ленивые вычисления 

Зачем?
	Использование только неизменяемых переменных определенно осложняет написание кода
	Пример:
		Имеется множество точек, необходимо найти ближайшую точку к выбранной
		На хаскелле решение в одну строчку:
		``` closest target = minimumBy(compare `on` (distance target))```
		На питоне в строк 20..
	Общая суть хаскелля- делать код читабельнее и легче
	Ну и исходя из реализации на хаскелле, можно применить идеи и на питоне 

Синтаксис: 

```haskell
func x y = <expr> -- определение функции 
add x y = x + y -- простейшая функция
addtwice x y = add x y + add x y -- во чо можно делать 
add x y == x `add` y -- две одинаковые записи
lst = [1,2,3] -- список как в питоне. Но вообще можно и по-другому
lst = 1 : 2 : 3 : nil -- реальный вид создания списка. nil - пустой список, а двоеточие - оператор присоединения 

f = if <condition> then <true_expr> else <false_expr> -- оператор ветвления
--по сути это не if, а тернарный оператор 

--сопоставление с образцом и по сути вид рекурсии. Если поменять строки местами, то в случае нуля будет использоваться функция, стоящая на первом месте:
factorial 0 = 1 
factorial n = n * factorial(n-1) 

--В другом языке программирования пришлось бы использовать циклы, здесь же их нет и все делается рекурсией:
sum []     = 0
sum (x:xs) = x + sum xs

--функция с разными значениями пишется так: 
sign x | x > 0 = 1 
       | x < 0 = -1
       | otherwise = 0

--let по сути объявляет локальные имена внутри функции
absDiff a b = let 
	abs x | x < 0 = -x
          | otherwise = x
in 
	abs a - abs b

--то же самое, но понятнее 
absDiff a b = abs a - abs b
	where 
		abs x | x < 0 = -x
	          | otherwise = x
```

Система типов и каррирование: 

```haskell
--определяем функцую типа integer, которая берет и возвращает integer
add :: Integer -> Integer -> Integer 
add x y = x + y 

--здесь нам выдаст ошибку, так как не факт, что тип a имеет сложение 
add :: a -> a -> a
add x y = x + y

--но мы может поставить ограничение. В КЛАССЕ Num определено сложение
add :: Num a => a -> a -> a
add x y = x + y

-- каррирование 
add :: Num a => a -> a -> a 
add x y = x + y
add :: Num a => a -> a 
add42 x = add 42
```

Пример 2: Решето Эратосфена

```haskell
primes = sieve [2..]
	where sieve (p : xs) = p : sieve (filter (\x -> x `mod` p /= 0) xs)
-- /= это то же самое, что !=. x `mod` p \= 0 то же самое, что x % p != 0
main = do
	print $ take 10 primes -- Output: [2,3,5,7,11,13,17,19,23,29]
-- take - возьми первые 10 из primes
-- $ - синтаксический сахар против скобок
-- print $ x == print(x) 
```

Пример 3: Бинарное дерево, там выбран элемент target. Найти путь от вершины до target

```haskell
--описание дерева
data Tree a = Empty
	| Node a (Tree a) (Tree a) 
-- empty и node - конструкторы. Три аргумента node - объекты типа a 
-- в самой node есть значение, три аргумента - дети 
isLeaf :: Tree a -> Bool
ifLeaf (Node _  Empty Empty) = True
	| otherwise = False --наверное

findPathToLeaf :: Eq a => a -> Tree a -> Maybe[a] --Maybe - опциональный тип
findPathToLeaf _ Empty = Nothing
findPathToLeaf target (Node value left right) 
	| value == target && isLeaf (Node value left right) = Just[value]
	| otherwise = case findPathToLeaf target left of
		Just path -> Just (value : path)
		Nothing -> case findPathToLeaf target right of
			Just path -> Just (value : path)
			Nothing -> Nothing
```


Производительность
	В общем все не так плохо, он вполне быстрый, но C и C++ явно быстрее 
	Математики в страхе под столом кстати (давай давай посчитай сложность рекурсивного копирования переменных и тд)
	Но параллельный код на хаскелле обыгрывает плюсы

