# 7장 - 테이블과 for문

## 테이블
Tables are basically lists in which we can store values.
테이블은 기본적으로 값을 저장할 수 있는 리스트입니다.

You create a table with curly brackets ({ }):
테이블은 *중괄호*({})를 이용해서 만들 수 있습니다.

> 번역자: 원문은 curly brackets으로, 직역하면 꼬부랑 괄호 정도가 됩니다. 한국어에는 중괄호가 있기 때문에 이를 대체 번역으로 사용했습니다.

```lua
function love.load ()
	fruits = {}
end
```

We just created a table called fruits. Now we can store values inside the table. There are multiple ways to do this.
우리는 방금 과일이라는 테이블을 만들었습니다. 이제 우리는 테이블 안에 가치를 저장할 수 있습니다. 이것을 할 수 있는 여러 가지 방법이 있습니다.
우리는 `fruits`라고 불리는 테이블을 생성했습니다. 이제 우리는 테이블 안에 값들을 저장할 수 있습니다. 테이블을 만드는 것은 여러가지 방법이 있습니다.

One way is to put the values inside the curly brackets.
한 가지 방법은 값들을 중괄호에 넣는 것입니다.

```lua
function love.load ()
	-- 각 값들은 콤마로 구분됩니다, 그냥 매개변수와 인자를 전달하는 것과 비슷합니다.
	fruits = {"apple", "banana"}
end
```

We can also use the function `table.insert`. The first argument is the table, the second argument is the value we want to store inside that table.
`table.insert` 함수를 사용할 수도 있습니다. 첫번째 인자는 테이블이고, 두번째 인자는 테이블에 넣고자 하는 값입니다.

```lua
function love.load ()
	--각 값들은 콤마로 구분됩니다, 그냥 매개변수와 인자를 전달하는 거랑 비슷합니다.
	fruits = {"apple", "banana"}
	table.insert(fruits, "pear")
end
```

So now after love.load our table will contain `"apple"`, `"banana"` and `"pear"`. To prove that, let's put the values on screen. For that we're going to use `love.graphics.print(text, x, y)`.
그럼, `love.load`의 작동이 끝난 이후에는 테이블에 `"apple"`, `"banana"`, `"pear"`가 담길 것입니다. 그럼 사실인지 확인해보기 위해, 값들을 화면에 출력해봅시다. 우리는 `love.graphics.print`를 사용할 겁니다.

```lua
function love.draw ()
	--Arguments: (text, x-position, y-position)
	love.graphics.print("Test", 100, 100)
end
```

When you run the game, you should see the text "test" written. We can access the values of our table by writing the tables name, followed by brackets (\[ \]) (So not curly but square brackets!). Inside these brackets, we write the position of the value we want.
게임을 실행하면, `"Test"`라는 문자가 적혀 있는 것을 봐야 합니다. 우리는 테이블 이름과 괄호 (\[ \]) (꼬부랑 괄호 말고 네모난 괄호!)를 입력하여 테이블의 값에 접근할 수 있습니다. 이 괄호 안에, 우리가 원하는 값의 위치를 적습니다.

![](/images/book/7/table.png)

Like I said, tables are a list of values. We first added `"apple"` and `"banana"`, so those are on the first and second position in the list. Next we added `"pear"`, so that's on the third position in the list. On position 4 there is no value (yet), since we only added 3 values.
아까 말씀드린 대로, 테이블은 값의 목록입니다. `"apple"`과 `"banana"`를 먼저 추가했기 때문에, 이것들은 리스트에서 각각 1번째와 2번째 위치에 있습니다. 우리는 그 다음에 `"pear"`를 추가했기 때문에 3번째에 있습니다. 우리는 값을 오직 3개만 추가했기 때문에, 4번째에는 값이 (아직은) 없습니다.

So if we want to print `"apple"`, we have to get the first value of the list.
그래서 `"apple"`을 출력하려면, 리스트의 1번째 값을 얻어야 합니다.

```lua
function love.draw ()
	love.graphics.print(fruits[1], 100, 100)
end
```

And so now it should draw `"apple"`. If you replace the `[1]` with `[2]`, you should get `"banana"`, and with `[3]` you get `"pear"`.
그리고 이제 화면에서는 `"apple"`이 표시 될 것입니다. `[1]`을 `[2]`로 바꾸면 `"banana"`를, `[3]`으로 바꾸면 `"pear"`를 보시게 될 것입니다.

Now we want to draw all 3 fruits. We could use love.graphics.print 3 times, each with a different table entry...
이제 우리는 3가지 과일을 모두 그려보고 싶습니다. `love.graphics.print`를 3번, 각각 다른 테이블의 요소로 사용할 수 있었습니다...

```lua
function love.draw ()
	love.graphics.print(fruits[1], 100, 100)
	love.graphics.print(fruits[2], 100, 200)
	love.graphics.print(fruits[3], 100, 300)
end
```

...but imagine if we had 100 values in our table. Luckily, there's a solution for this: for-loops!
하지만 생각해보세요... 테이블에 값이 100개가 있다면요? 운 좋게도, 여기에는 해결책이 있습니다. for문!

___

## for문

A for-loop is a way to repeat a piece of code a certain amount of times.
for문은 코드의 일부분을 일정 횟수 동안 반복시키는 방법입니다.

You create a for-loop like this:
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

If you run the game you should see it prints hello 1, hello 2, hello 3, all the way to 10.
만약 게임을 실행하면 hello 1부터 10까지 출력됩니다.

To create a for-loop, first you write `for`. Next you write a variable and give it a numeric value. This is where the for-loop starts. The variable can be named anything, but it's common to use `i`. This variable can only be used inside the for-loop (just like with functions and parameters). Next you write the number to which it should count. So for example `for i = 6, 18 do` will start at 6 and keep looping till it's at 18.
for문을 작성하려면, 먼저 `for` 예약어를 써야합니다. 그 다음에는 변수를 쓰고 숫자값을 줍니다. 여기서 for-반복문이 시작됩니다. 변수 이름은 무엇이든지 가능하지만 보통 `i`를 사용합니다. 이 변수는 for문 안에서만 사용될 수 있습니다. (함수의 매개변수와 비슷합니다.) 그 다음에는 세어야 할 숫자를 입력합니다. 예를 들어, `for i = 6, 18 do end`는 6에서 시작하여 18을 초과하기 전까지 계속 반복합니다.

There is also a third, optional number. This is by how much the variable increases. `for i = 6, 18, 4 do` would go: 6, 10, 14, 18. With this you can also make for-loops go backwards with -1.
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

But what if we add or remove a value from a table? We would have to change the 3 into another number. For that we use `#fruits`. With the #-sign, we can get the length of a table. The length of a table refers to the number of things in that table. That length would be `3` in our case, since we have 3 entries: `apple`, `banana`, and `pear` in our `fruits` table.
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

Let's use this knowledge to draw all 3 fruits.
이 사실을 이용하여 3개의 과일을 모두 표시해보겠습니다.

```lua
function love.draw ()
	for i = 1, #fruits do
		love.graphics.print(fruits[i], 100, 100)
	end
end
```

If you run the game you should see it draws all 3 fruits, except they're all drawn on the same position. We can fix this by printing each number on a different height.
게임을 실행하면, 3개의 과일이 모두 표시되고 있는 것을 보실 수 있는데, 모두 같은 위치에 그려져 있다는 점만 빼면요. 이는 출력 함수에서 각기 다른 높이값을 주는 방식으로 해결할 수 있습니다.

```lua
function love.draw ()
	for i = 1, #fruits do
		love.graphics.print(fruits[i], 100, 100 + 50 * i)
	end
end
```

So now `"apple"` will be drawn on the y-position 100 + 50 * 1, which is 150. Then `"banana"` gets drawn on 200, and `"pear"` on 250.
이제 `"apple"`은 Y 좌표 100 + 50 * 1 즉, 150에서 표시될 것입니다. `"banana"`는 200에, `"pear"`는 250에서 그려질 것입니다.

![](/images/book/7/fruits.png)

If we were to add another fruit, we won't have to change anything. It will automatically be drawn. Let's add `"pineapple"`.
과일을 하나 더 추가해도 우리는 아무것도 바꿀 필요가 없습니다. 자동으로 그려질 것입니다. `"pineapple"`을 추가해봅시다.

```lua
function love.load ()
	fruits = {"apple", "banana"}
	table.insert(fruits, "pear")
	table.insert(fruits, "pineapple")
end
```

We can also remove values from our table. For that we use `table.remove`. The first argument is the table we want to remove something from, the second argument is the position we want to remove. So if we want to remove banana, we do the following:
우리는`table.remove`를 사용하여 테이블에서 값을 제거할 수도 있습니다. 첫번째 인자는 테이블인데 우리가 무언가를 제거하길 원하는 곳입니다, 두번째 인자는 제거하려는 값이 있는 위치입니다. 따라서 바나나를 테이블에서 제거하려면 다음과 같이 합니다.

```lua
function love.load ()
	fruits = {"apple", "banana"}
	table.insert(fruits, "pear")
	table.insert(fruits, "pineapple")
	table.remove(fruits, 2)
end
```

When you run the game you'll see that banana is no longer drawn, and that pear and pineapple have moved up.
게임을 실행하면 바나나가 더이상 표시되지 않고, 배와 파인애플이 위로 올라온 것을 보실 수 있습니다.

![](/images/book/7/shift.png)

When you remove a value from a table with `table.remove`, all the following items in the table will move up. So what was on position 3 is now on position 2 in the table. And what was on position 4 is now on position 3.
`table.remove`로 테이블에서 값을 제거할 때, 그 이후에 있는 모든 항목들이 위로 이동할겁니다. 따라서 3번째 위치에 있던 것이 2번째 위치가 됩니다. 그리고 4번째 위치에 있던 것이 3번째 위치가 됩니다.

You can also add or change the values inside the table directly. For example, we can change `"apple"` into `"tomato"`:
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

Back to the for-loops. There is actually another way, and an easier way to loop through the table. We can use an `ipairs` loop.
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

This for-loop loops, or what we also call *iterates*, through all the values in the table. The variables `i` tells us the position of the table, `v` is the value of that position in the table. It's basically a shorthand for `fruits[i]`. For example, in the first iteration the values for the variables  `i` would be `1` and `v` would be `"apple"`. In the second iteration, `i` and `v` would be `2` and `"pear"` respectively.
이것은 for 반복문 또는 *반복기*라고도 부르는 것으로, 테이블의 모든 값을 순회합니다. 변수 `i`는 테이블의 위치를 알려주고, `v`는 그 위치에 있는 값입니다. 이는 기본적으로 `fruits[i]`의 축약 표현입니다. 예를 들어, 첫 번째 반복에서 변수 `i`의 값은 `1`이고 `v`는 `"apple"`입니다. 두 번째 반복에서 `i`와 `v`의 값은 각각 `2`와 `"pear"`가 될 것입니다.

But how does it work? Why does the function `ipairs` allow for this? That is for another time. For now all you need to know is that `ipairs` is basically a shorthand for the following:
하지만 이게 어떻게 작동할까요? 왜 `ipairs` 함수는 이렇게 작동하는 것을 가능하게 할까요? 그건 다음 시간에 알아봅시다. 지금은 `ipairs`가 다음의 축약 표현이라는 것만 알면 됩니다.

```lua
for i = 1, #fruits do
	v = fruits[i]
end
```

Let's use `ipairs` for drawing our tables.
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
Tables are lists in which we can store values. We store these values when creating the table, with `table.insert`, or with `table_name[1] = "some_value"`. We can get the length of the table with `#table_name`. With for-loops we can repeat a piece of code a number of times. We can also use for-loops to iterate through tables.
테이블은 값을 저장할 수 있는 목록입니다. 테이블을 만들 때 `table.insert` 함수 또는 다음과 같은 대입문 `table_name[1] = "some_value"`으로 이 값을 저장합니다. 테이블의 이름 앞에 `#`을 붙혀 테이블의 길이를 구할 수 있습니다. for문을 사용하면 코드 조각을 여러 번 반복할 수 있습니다. for문을 사용하여 테이블의 내용을 열거할 수도 있습니다.
