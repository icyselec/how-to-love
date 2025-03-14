# 6장 - 조건문
___
*We use the code from the previous chapter*
*이번 장에서는 이전 장에서 작성한 코드를 사용합니다.*
___
With if-statements, we can allow pieces of code to only be executed when a condition is met.
조건문을 사용하면 조건을 만족할때만 코드를 실행하게 할 수 있습니다.

You create an if-statement like this:
다음과 같이 하여 조건문을 만들 수 있습니다:
```lua
if condition then
	-- code
end
```

A condition, or statement, is something that's either true or false.
조건식의 결과는 `true` 또는 `false` 값이 될 수 있습니다.

For example: `5 > 9`
예를 들어, `5 > 9`에서

The `>` means, higher than. So the statement is that 5 is higher than 9, which is false.
`>`가 의미하는 것은 '높을 때' 입니다. 그래서 이 조건식은 5가 9보다 높을 때이고, 그럴 일은 없으므로 이 조건식은 `false` 즉, 거짓입니다!

Put an if-statement around the code of x increasing.
조건문을 x가 증가하는 코드 주변에 붙혀봅시다.

```lua
function love.update (dt)
	if 5 > 9 then
		x = x + 100 * dt
	end
end
```

When you run the game you'll notice that our rectangle isn't moving. This is because the statement is false. If we were to change the statement to `5 < 9` (5 is lower than 9), then the statement is true, and the code inside the if-statement will execute.
게임을 실행하면 직사각형이 움직이지 않는 것을 알 수 있습니다. 이는 조건식의 결과가 거짓이기 때문입니다. 만약 조건식을 `5 < 9`(5가 9보다 낮을 때)로 바꾼다면 조건식의 결과는 참이 되고 조건문 안에 있는 코드가 실행됩니다.

With this, we can for example make `x` go up to 600, and then make it stop moving, with `x < 600`.
이를 사용하면, `x`가 600까지 올라가고 `x < 600`에 의해 더이상 움직이지 않게 할 수 있습니다.

```lua
function love.update(dt)
	if x < 600 then
		x = x + 100 * dt
	end
end
```

![](/images/book/6/rectangle_stop.gif)

If we want to check if a value is equal to another value, we need to use 2 equal-signs (==).
어떤 값이 다른 값과 동일한지 확인하려면, 두개의 등호를(==) 사용하여 확인할 수 있습니다.

예를 들어, `4 == 7`와 같이 합니다.

1 equal-sign is for assigning, 2 equal-signs is for comparing.
한 개의 등호는 대입에 사용되고, 두개의 등호는 비교에 사용됩니다.

```lua
x = 10 --x에 10을 대입합니다.
x == 10 --x와 10이 동일한지 비교합니다.
```

We can also use `>=` and `<=` to check if values are higher or equal to each other or if the values are lower and equal to each other.
또한 `>=`와 `<=`를 사용하여 값과 값이 서로 높거나 같을 때, 낮거나 같을 때를 비교할 수 있습니다.

```lua
10 <= 10 --참, 10은 10과 같음
15 >= 4 --참, 15는 4보다 큼
```

So the code above is a shorthand for
그러므로 위의 코드는 다음 코드의 축약 표현입니다.
```lua
10 == 10 or 10 < 10
15 == 4 or 15 > 4
```

## 불리언

A variable can also be `true` or `false`. This type of variable is what we call a boolean.
변수는 `true` 또는 `false`가 될 수 있습니다. 이 변수의 자료형을 불리언(또는 부울)이라고 부릅니다.

Let's make a new variable called `move`, with the value `true`, and check if `move` is `true` in our if-statement.
`move`라는 이름으로 변수를 만들어 `true` 값을 대입하고, 조건문에서 `move`가 참값인지 확인해봅시다.

```lua
function love.load()
	x = 100
	move = true
end

function love.update (dt)
	-- 두개의 등호가 비교라는 사실을 꼭 기억하세요!
	if move == true then
		x = x + 100 * dt
	end
end
```

`move` is `true`, so our rectangle moves. But `move == true` is actually redundant. We're checking if it's true that the value of `move` is `true`. Simply using `move` as a statement is good enough.
`move`는 `true`이므로 직사각형은 이동합니다. 하지만 `move == true`는 사실 군더더기가 많은 표현입니다. 우리는 `move`의 값이 `true`인지 확인하고 있습니다. 간단히 `move`를 조건식으로 사용하는 것도 충분합니다.

```lua
if move then
	x = x + 100 * dt
end
```

If we want to check if `move` is `false`, we can put a `not` in front of it.
`move`가 `false`(거짓)인지 확인하려면 `not`을 앞에 붙히면 됩니다.

```lua
if not move then
	x = x + 100 * dt
end
```

If we want to check if a number is NOT equal to another number, we use a tilde (~).
값이 다른 값과 같지 않은지 확인하려면, 물결표(~)를 사용하면 됩니다.

```lua
if 4 ~= 5 then
	x = x + 100 * dt
end
```

We can also assign `true` or `false` to a variable with a statement.
또한, 조건식의 결과값을 변수에 넣을 수도 있습니다.

For example: `move = 6 > 3`.
예를 들어, `move = 6 > 3`과 같이 합니다.

If we check if move is true, and then change move to false inside the if-statement, it's not as if we jump out of the if-statement. All the code below will still be executed.
조건식이 참인지 확인한 다음, 곧바로 거짓값이 되도록 변경한다고 하더라도 여전히 조건문 안에 있습니다. 그 아래에 있는 모든 코드는 여전히 실행됩니다.
> 번역자: 조건식이 조건문을 통과하는 동안 계속 검사한다고 생각하지 말고, 단 한 번만 검사한다고 생각하세요.

```lua
if move then
	move = false
	print("This will still be executed!")
	x = x + 100 * dt
end
```

## 방향키 입력 받기
Let's make the rectangle move based on if we hold down the right arrowkey. For this we use the function `love.keyboard.isDown` [(wiki)](https://www.love2d.org/wiki/love.keyboard.isDown).
오른쪽 방향키를 눌렀을 때를 기준으로 직사각형이 움직이도록 하겠습니다. 이를 위해 우리는 `love.keyboard.isDown` 함수를 사용할 겁니다. [(위키)](https://www.love2d.org/wiki/love.keyboard.isDown)

Notice how the D of Down is uppercase. This is type of casing is what we call camelCasing. We start the first word in lowercase, and then every following word's first character we type in uppercase. This type of casing is also what we will be using for our variables throughout this tutorial.
Down의 D가 대문자라는 것을 주목하세요. 이는 낙타 표기법이라고 부르는 표기법의 한 유형입니다. 첫 번째 단어는 소문자로 시작하고, 그 다음 모든 단어의 첫 번째 문자를 대문자로 입력합니다. 이러한 표기법을 이 자습서에서는 변수에 사용할 것입니다.

We pass the string "right" as first argument to check if the right arrowkey is down.
오른쪽 방향키가 눌렸는지 확인하기 위해 문자열 "right"를 첫 번째 인수로 전달합니다.

```lua
if love.keyboard.isDown("right") then
	x = x + 100 * dt
end
```

So now only when we hold down the right arrowkey does our rectangle move.
따라서 이제 오른쪽 방향키를 눌러야 직사각형이 움직입니다.

![](/images/book/6/rectangle_right.gif)

We can also use `else` to tell our game what to do when the condition is `false`. Let's make our rectangle move to the left, when we don't press right.
`else`를 사용하여 조건식이 거짓일 때 어떻게 해야 하는지 게임에 알려줄 수도 있습니다. 오른쪽 방향키를 누르지 않을 때는 직사각형을 왼쪽으로 이동시키게 하겠습니다.

```lua
if love.keyboard.isDown("right") then
	x = x + 100 * dt
else
	x = x - 100 * dt --x의 값을 감소시킵니다.
end
```

We can also check if another statement is true, after the first is false, with `elseif`. Let's make it so that after checking if the right arrowkey is down, and it's not, we'll check if the left arrowkey is down.
`elseif`를 사용하여 어떤 조건식이 거짓인 경우, 그 다음 조건식이 참인지 확인할 수 있습니다. 오른쪽 방향키가 눌렸는지 확인하고, 그렇지 않으면, 왼쪽 방향키가 눌렸는지 확인하도록 해봅시다.

```lua
if love.keyboard.isDown("right") then
	x = x + 100 * dt
elseif love.keyboard.isDown("left") then
	x = x - 100 * dt
end
```
Try to make the rectangle move up and down as well.
직사각형을 위아래로 움직이도록 해보세요.

___

## and(논리곱)와 or(논리합)
With `and` we can check if multiple statements are true.
`and`를 사용하면 여러개의 수식이 참인지 확인할 수 있습니다.

```lua
if 5 < 9 and 14 > 7 then
	print("Both statements are true")
end
```

With `or`, the if-statement will execute if any of the statements is true.
`or`을 사용하면 어느 수식 중 하나라도 참일 때 전체 수식이 참이 됩니다.

```lua
if 20 < 9 or 14 > 7 or 5 == 10 then
	print("One of these statements is true")
end
```

___

## 한 가지 더
To be more precise, if-statements check if the statement is NOT `false` or `nil`.
좀 더 정확하게 말하면, 조건문은 해당 수식이 `false` 또는 `nil`이 아닌지를 확인합니다.
```lua
if true then print(1) end --false나 nil이 아닙니다, 실행됩니다.
if false then print(2) end --false입니다, 실행되지 않습니다.
if nil then print(3) end --nil입니다, 실행되지 않습니다.
if 5 then print(4) end --false나 nil이 아닙니다, 실행됩니다.
if "hello" then print(5) end --false나 nil이 아닙니다, 실행됩니다.
--Output: 1, 4, 5
```

___

## 요약문
With if-statements, we can allow pieces of code to only be executed when a condition is met. We can check if a number is higher, lower or equal to another number/value. A variable can be true or false. This type of variable is what we call a `boolean`. We can use `else` to tell our game what to execute when the statement was false, or `elseif` to do another check.
조건문을 사용하면 조건을 만족할 때만 코드를 실행하게 할 수 있다. 값이 다른 값보다 높은지, 낮은지, 같은지를 확인할 수 있다. 변수는 참이거나 거짓일 수 있다. 이때 변수의 자료형을 `boolean`이라고 한다. `else`를 사용하여 조건식이 거짓일 때 무엇을 실행할지 게임에 알려줄 수 있고, `elseif`를 사용하여 그런 상황에서 다시 한번 다른 조건식으로 확인할 수 있다.
