# 7장 - 테이블과 for문

## 테이블
테이블은 기본적으로 값을 저장할 수 있는 리스트입니다.

테이블은 *중괄호*({})를 이용해서 만들 수 있습니다.
> 번역자: 원문은 curly brackets으로, 직역하면 꼬부랑 괄호 정도가 됩니다. 한국어에는 중괄호가 있기 때문에 이를 대체 번역으로 사용했습니다.

```lua
function love.load ()
	fruits = {}
end
```

우리는 `fruits`라고 불리는 테이블을 생성했습니다. 이제 우리는 테이블 안에 값들을 저장할 수 있습니다. 테이블을 만드는 것은 여러가지 방법이 있습니다.

한 가지 방법은 값들을 중괄호에 넣는 것입니다.

```lua
function love.load ()
	-- 각 값들은 콤마로 구분되며, 그냥 매개변수와 인자를 전달하는 것과 비슷합니다.
	fruits = {"apple", "banana"}
end
```

`table.insert` 함수를 사용할 수도 있습니다. 첫번째 인자는 테이블이고, 두번째 인자는 테이블에 넣고자 하는 값입니다.

```lua
function love.load ()
	-- 각 값들은 콤마로 구분되며, 그냥 매개변수와 인자를 전달하는 것과 비슷합니다.
	fruits = {"apple", "banana"}
	table.insert(fruits, "pear")
end
```

그럼, `love.load`의 작동이 끝난 이후에는 테이블에 `"apple"`, `"banana"`, `"pear"`가 담길 것입니다. 이것이 사실인지 확인해보기 위해, 값들을 화면에 출력해봅시다. 우리는 `love.graphics.print`를 사용할 겁니다.

```lua
function love.draw ()
	--Arguments: (text, x-position, y-position)
	love.graphics.print("Test", 100, 100)
end
```

게임을 실행하면, `"Test"`라는 문자가 적혀 있는 것을 봐야 합니다. 우리는 테이블 이름과 괄호 (\[ \]) (꼬부랑 괄호 말고 네모난 괄호!)를 입력하여 테이블의 값에 접근할 수 있습니다. 이 괄호 안에, 우리가 원하는 값의 위치를 적습니다.

![](/images/book/7/table.png)

아까 말씀드린 대로, 테이블은 값의 목록입니다. `"apple"`과 `"banana"`를 먼저 추가했기 때문에, 이것들은 리스트에서 각각 1번째와 2번째 위치에 있습니다. 우리는 그 다음에 `"pear"`를 추가했기 때문에 3번째에 있습니다. 우리는 값을 오직 3개만 추가했기 때문에, 4번째에는 값이 (아직은) 없습니다.

그래서 `"apple"`을 출력하려면, 리스트의 1번째 값을 얻어야 합니다.

```lua
function love.draw ()
	love.graphics.print(fruits[1], 100, 100)
end
```

그리고 이제 화면에서는 `"apple"`이 표시 될 것입니다. `[1]`을 `[2]`로 바꾸면 `"banana"`를, `[3]`으로 바꾸면 `"pear"`를 보시게 될 것입니다.

이제 우리는 3가지 과일을 모두 그려보고 싶습니다. `love.graphics.print`를 3번, 각각 다른 테이블의 요소로 사용할 수 있었습니다...

```lua
function love.draw ()
	love.graphics.print(fruits[1], 100, 100)
	love.graphics.print(fruits[2], 100, 200)
	love.graphics.print(fruits[3], 100, 300)
end
```

하지만 생각해보세요... 테이블에 값이 100개가 있다면요? 운 좋게도, 여기에는 해결책이 있습니다. for문!

___

## for문

for문은 코드의 일부분을 일정 횟수 동안 반복시키는 방법입니다.

for문은 다음과 같이 하여 작성할 수 있습니다:

```lua
function love.load ()
	fruits = {"apple", "banana"}
	table.insert(fruits, "pear")

	for i = 1, 10 do
		print("hello", i)
	end
end
```

만약 게임을 실행하면 hello 1부터 10까지 출력됩니다.

for문을 작성하려면, 먼저 `for` 예약어를 써야합니다. 그 다음에는 변수를 쓰고 숫자값을 줍니다. 여기서 for-반복문이 시작됩니다. 변수 이름은 무엇이든지 가능하지만 보통 `i`를 사용합니다. 이 변수는 for문 안에서만 사용될 수 있습니다. (함수의 매개변수와 비슷합니다.) 그 다음에는 세어야 할 숫자를 입력합니다. 예를 들어, `for i = 6, 18 do end`는 6에서 시작하여 18을 초과하기 전까지 계속 반복합니다.

세번째 값도 있는데, 생략 가능한 숫자값입니다. 이는 변수가 얼마씩 증가해야 하는가 입니다. `for i = 6, 18, 4 do end`는 6, 10, 14, 18로 진행합니다. 또, -1을 사용하면 for문이 거꾸로 갈 수도 있습니다.

우리의 테이블은 1부터 시작하여 3개의 값을 가지고 있으니, 다음과 같이 작성할 수 있습니다.

```lua
function love.load ()
	fruits = {"apple", "banana"}
	table.insert(fruits, "pear")

	for i = 1, 3 do
		print(fruits[i])
	end
end
```

게임을 실행하면 3개의 모든 과일이 출력되는 것을 볼 수 있습니다. 첫번째 반복에서는 `fruits[1]`를 출력하고, 두번째 반복에서는 `fruits[2]`를, 그리고 마지막 반복에서는 `fruits[3]`를 출력합니다.

___

## 테이블 편집하기

하지만 테이블에서 값을 추가하거나 제거하면 어떻게 될까요? 우리는 3을 다른 숫자로 바꿔야 합니다. 이를 위해 우리는 `#fruits`를 사용합니다. 해시 기호(#)를 사용하면 테이블의 길이를 구할 수 있습니다. 테이블의 길이는 해당 테이블에 있는 항목의 수를 나타냅니다. `fruits` 테이블에는 `apple`, `banana`, `pear`의 세 가지 항목이 있으므로 이 길이는 `3`이 될 것입니다.

```lua
function love.load ()
	fruits = {"apple", "banana"}

	print(#fruits)
	--Output: 2

	table.insert(fruits, "pear")

	print(#fruits)
	--Output: 3

	for i = 1, #fruits do
		print(fruits[i])
	end
end
```

이 사실을 이용하여 3개의 과일을 모두 표시해보겠습니다.

```lua
function love.draw ()
	for i = 1, #fruits do
		love.graphics.print(fruits[i], 100, 100)
	end
end
```

게임을 실행하면, 3개의 과일이 모두 표시되고 있는 것을 보실 수 있는데, 모두 같은 위치에 그려져 있다는 점만 빼면요. 이는 출력 함수에서 각기 다른 높이값을 주는 방식으로 해결할 수 있습니다.

```lua
function love.draw ()
	for i = 1, #fruits do
		love.graphics.print(fruits[i], 100, 100 + 50 * i)
	end
end
```

이제 `"apple"`은 Y 좌표 100 + 50 * 1 즉, 150에서 표시될 것입니다. `"banana"`는 200에, `"pear"`는 250에서 그려질 것입니다.

![](/images/book/7/fruits.png)

과일을 하나 더 추가해도 우리는 아무것도 바꿀 필요가 없습니다. 자동으로 그려질 것입니다. `"pineapple"`을 추가해봅시다.

```lua
function love.load ()
	fruits = {"apple", "banana"}
	table.insert(fruits, "pear")
	table.insert(fruits, "pineapple")
end
```

우리는`table.remove`를 사용하여 테이블에서 값을 제거할 수도 있습니다. 첫번째 인자는 테이블인데 우리가 무언가를 제거하길 원하는 곳입니다, 두번째 인자는 제거하려는 값이 있는 위치입니다. 따라서 바나나를 테이블에서 제거하려면 다음과 같이 합니다.

```lua
function love.load ()
	fruits = {"apple", "banana"}
	table.insert(fruits, "pear")
	table.insert(fruits, "pineapple")
	table.remove(fruits, 2)
end
```

게임을 실행하면 바나나가 더이상 표시되지 않고, 배와 파인애플이 위로 올라온 것을 보실 수 있습니다.

![](/images/book/7/shift.png)

`table.remove`로 테이블에서 값을 제거할 때, 그 이후에 있는 모든 항목들이 위로 이동할겁니다. 따라서 3번째 위치에 있던 것이 2번째 위치가 됩니다. 그리고 4번째 위치에 있던 것이 3번째 위치가 됩니다.

테이블 내부의 값을 직접 추가하거나 변경할 수도 있습니다. 예를 들어, 우리는 `"apple"`에서 `"tomato"`로 변경할 수 있습니다.

```lua
function love.load ()
	fruits = {"apple", "banana"}
	table.insert(fruits, "pear")
	table.insert(fruits, "pineapple")
	table.remove(fruits, 2)
	-- 1번째 위치에 있던 값이 "tomato"가 됩니다.
	fruits[1] = "tomato"
end
```

___

## ipairs

for문 이야기로 돌아옵시다. 여기에는 테이블을 순회하는 더 쉬운 방법이 있습니다. 우리는 `ipairs` 반복을 사용할 수 있습니다.

```lua
function love.load ()
	fruits = {"apple", "banana"}
	table.insert(fruits, "pear")
	table.insert(fruits, "pineapple")
	table.remove(fruits, 2)
	fruits[1] = "tomato"

	for i, v in ipairs(fruits) do
		print(i, v)
	end
	--Output:
	--1, "tomato"
	--2, "pear"
	--3, "pineapple"
end
```

이것은 for 반복문 또는 *반복기*라고도 부르는 것으로, 테이블의 모든 값을 순회합니다. 변수 `i`는 테이블의 위치를 알려주고, `v`는 그 위치에 있는 값입니다. 이는 기본적으로 `fruits[i]`의 축약 표현입니다. 예를 들어, 첫 번째 반복에서 변수 `i`의 값은 `1`이고 `v`는 `"apple"`입니다. 두 번째 반복에서 `i`와 `v`의 값은 각각 `2`와 `"pear"`가 될 것입니다.

하지만 이게 어떻게 작동할까요? 왜 `ipairs` 함수는 이렇게 작동하는 것을 가능하게 할까요? 그건 다음 시간에 알아봅시다. 지금은 `ipairs`가 다음의 축약 표현이라는 것만 알면 됩니다.

```lua
for i = 1, #fruits do
	v = fruits[i]
end
```

`ipairs`를 사용하여 테이블을 그려봅시다.

```lua
function love.draw ()
	-- i와 v는 변수이기 때문에 원하는 값을 지정할 수 있습니다
	for i, frt in ipairs(fruits) do
		love.graphics.print(frt, 100, 100 + 50 * i)
	end
end
```

___

## 요약
테이블은 값을 저장할 수 있는 목록이다. 테이블을 만들 때 `table.insert` 함수 또는 다음과 같은 대입문 `table_name[1] = "some_value"`으로 이 값을 저장한다. 테이블의 이름 앞에 `#`을 붙혀 테이블의 길이를 구할 수 있다. for문을 사용하면 코드 조각을 여러 번 반복할 수 있다. for문을 사용하여 테이블의 내용을 나열할 수도 있다.
