# 9장 - 여러 파일과 변수 영역

## 여러 파일
With multiple files our code will look more organized and easier to navigate. Create a new file called `example.lua`. Make sure it's in the same folder as a new and empty `main.lua`.
여러 파일을 사용하면 코드가 더 체계적이고 탐색하기 쉬워 보일 것입니다. `example.lua`라는 새 파일을 만듭니다. 새 파일과 빈 `main.lua`와 같은 폴더에 있는지 확인합니다.

Inside this file, create a variable. I will put `--! file:` at the top of the codeblock to make clear in what file you have to put the code. This is only for this tutorial, it has no use (it's only commentary after all) and you don't need to copy it. If a codeblock doesn't have this in future tutorials, it's either in main.lua, or the previous mentioned file.
이 파일 안에 변수를 만듭니다. 코드블록 상단에 `--! file:`을 넣어 어떤 파일에 코드를 넣어야 하는지 명확하게 설명하겠습니다. 이 튜토리얼에만 해당되며, 사용할 필요가 없으며(결국 주석으로만 표시됩니다) 복사할 필요도 없습니다. 향후 튜토리얼에 코드블록에 이 기능이 없는 경우 main.lua 또는 이전에 언급한 파일 중 하나입니다.


```lua
--! file: example.lua
test = 20
```

Now in `main.lua`, put `print(test)`. When you run the game, you'll see that `test` equals `nil`. This is because we have to load the file first. We do this with `require`, by passing the filename as string as first argument.
이제 `main.lua`에서, `print(test)`를 넣습니다. 게임을 실행하면, `test`가 `nil`과 같을 것입니다. 이는 파일을 먼저 불러와야 하기 때문입니다. `require` 함수에 파일 이름을 첫 번째 인자로 전달하여 이러한 작업을 할 수 있습니다.

```lua
--! file: main.lua
--Leave out the .lua
-- No need for love.load or whatever
require("example")
print(test)
```

We don't add the `".lua"` in the filename, because Lua does this for us.
`".lua"`를 파일 이름 뒤에 추가하지 않았는데, 왜냐하면 Lua가 알아서 해주기 때문입니다.

You can also put the file in a subdirectory, but in that case make sure to include the whole path.
파일을 하위 디렉터리에 넣을 수도 있지만, 전체 경로를 포함해야 합니다.

```lua
--With require we use . instead of /
require("path.to.example")
```

Now when you print `test`, after we loaded `example.lua`, you should see it says 20.
이제 `example.lua`를 불러오고 난 뒤에, `test`의 값을 출력하면, 20이라고 표시될 것입니다.

`test` in this case is what we call a *global variable* (or a *global* for short). It's a variable that we can use everywhere in our project. The opposite of a global variable, is a *local variable* (or *local* for short). You create a local variable by writing `local` in front of the variable name.
`test`는 우리가 *전역 변수*(또는 줄여서 *전역*)이라고 부르는 변수입니다. 이 변수는 프로젝트의 모든 곳에서 사용할 수 있는 변수입니다. 전역 변수의 반대말은 *지역*(또는 줄여서 *지역*)입니다. 변수 이름 앞에 `local`을 써서 지역 변수를 작성합니다.

```lua
--! file: example.lua
local test = 20
```
Run the game to see test is now `nil` again. This is because of its *scope*.
게임을 실행하여 test가 `nil`이 되었는지 확인하세요. 이는 *변수 영역* 때문입니다.
___

## 변수 영역

Local variables are limited to their *scope*. In the case of `test`, the scope is the file `example.lua`. This means that `test` can be used everywhere inside that file, but not in other files.
지역 변수는 *변수 영역*으로 제한됩니다. `test`의 경우, 변수 영역은 `example.lua` 파일입니다. 이는 `test`를 해당 파일의 모든 곳에서 사용할 수 있지만, 다른 파일에서는 사용할 수 없습니다.

If we were to create a local variable inside a *block*, like a function, if-statement or for-loop, then that would be the variable's scope.
함수, if문, for문 같은 *블록* 내에 지역 변수를 작성한다면 블록이 지역 변수의 변수 영역이 될 것입니다.

```lua
--! file: example.lua
if true then
	local test = 20
end

print(test)
--Output: nil
```

`test` is `nil`, because we print it outside of its scope.
`test`는 `nil`인데, 왜냐하면 변수 영역의 밖에서 출력했기 때문입니다.

Parameters of functions are like local variables. Only existing inside the function.
함수의 매개 변수는 지역 변수와 비슷합니다. 오직 함수 안에서만 존재합니다.

To really understand how scope works, take a look at the following code:
변수 영역이 어떻게 작동하는지 제대로 이해하려면, 다음의 코드를 살펴보세요.

```lua
--! file: main.lua
test = 10
require("example")
print(test)
--Output: 10
```

```lua
--! file: example.lua
local test = 20

function some_function (test)
	if true then
		local test = 40
		print(test)
		--Output: 40
	end
	print(test)
	--Output: 30
end

some_function(30)

print(test)
--Output: 20
```

If you run the game, it should print: 40, 30, 20, 10. Let's take a look at this code step by step.
게임을 실행하면, 40, 30, 20, 10의 순서대로 출력됩니다. 이 코드를 단계별로 살펴보겠습니다.

First we create the variable `test` in `main.lua`, but before we print it we require `example.lua`.
첫 번째로, 우리는 `main.lua`에서 `test`라는 변수를 작성했습니다. 하지만 이를 출력하기 전에 `example.lua`를 불러옵니다.

In `example.lua` we create a local variable `test`, which does not affect the global variable in `main.lua`. Meaning that the value we give to this local variable we create is not given to the global variable.
`example.lua`에서 우리는 `main.lua`의 전역 변수에 영향을 미치지 않는 지역 변수 `test`를 작성합니다. 즉, 우리가 작성한 이 지역 변수에 부여하는 값은 전역 변수에 영향을 미치지 않습니다.

We create a function called `some_function(test)` and then call that function.
`some_function(test)`라는 함수를 작성하고 그 함수를 호출합니다.

Inside that function the parameter `test` does not affect the local variable we created earlier.
해당 함수의 매개 변수 `test`는 이전에 작성한 지역 변수에 영향을 미치지 않습니다.

Inside the if-statement we create another local variable called `test`, which does not affect the parameter `test`.
if 문에서 우리는 또 다른 지역 변수인 `test`를 작성했는데, 이 또한 매개 변수인 `test`에 영향을 미치지 않습니다.

The first print is inside the if-statement, and it's 40. After the if-statement we print `test` again, and now it's 30, which is what we passed as argument. The parameter `test` was not affected by the `test` inside the if-statement. Inside the if-statement the local variable took priority over the parameter.
if 문 안에서 첫번째 출력이 있고, 이는 40입니다. if 문 뒤에 `test`를 다시 출력하는데 이 값은 30이고, 우리가 전달한 인자입니다. 매개 변수 `test`는 if 문 안의 `test`에 영향을 미치지 않습니다. if 문 안의 지역 변수는 매개 변수보다 우선 순위가 높습니다.

Outside of the function we also print `test`. This time it's 20. The `test` created at the start of `example.lua` was not affected by the `test` inside the function.
함수의 밖에서 우리는 `test`도 출력합니다. 이제는 20입니다. `example.lua`의 시작 부분에 생성된 `test`는 함수 내부의 `test`에 영향을 받지 않았습니다.

And finally we print `test` in `main.lua`, and it's 10. The global variable was not affected by the local variables inside `example.lua`.
마지막으로 `main.lua`에서 우리는 `test`를 출력하면 10이 됩니다. 전역 변수는 `example.lua` 내부의 지역 변수에 영향을 받지 않았습니다.

I made a visualization of the scope of each `test` to make it even more clear:
각 `test`의 유효 범위를 더욱 명확하게 하기 위해 시각화 했습니다.

![](/images/book/9/scope.png)

When creating a local variable, you don't have to assign a value right away.
지역 변수를 만들 때는, 값을 바로 할당할 필요가 없습니다.

```lua
local test
test = 20
```

## 값을 반환하기

If you add a return statement at the top scope of a file (so not in any function) it will be returned when you use `require` to load the file.
파일의 최상위 구문에서 반환문을 추가하면 (아무 함수에 속하지 않으므로) 이는 당신이 `require`를 사용하여 파일을 로드할 때 반환됩니다.

예를 들어:

```lua
--! file: example.lua
local test = 99
return test
```
```lua
--! file: main.lua
local hello = require "example"
print(hello)
--Ouput: 99
```

## 언제, 그리고 왜 지역 변수를 사용하나요?

The best practice is to always use local variables, and there are multiple reasons for it. First of all Lua is faster with accessing locals than globals. Now this is a very small difference, perhaps not bigger than 0.000001 seconds, but when you use a lot of globals it quickly adds up.  
가장 좋은 방법은 항상 지역 변수를 사용하는 것이며, 그 이유는 여러 가지가 있습니다. 우선 Lua는 전역 변수보다 지역 변수에 더 빠르게 접근할 수 있습니다. 이는 0.000001초보다 크지 않은 매우 작은 차이이지만, 전역 변수를 많이 사용하면 빠르게 증가합니다.

Another reason is that with globals you're more likely to make mistakes. You might accidentally use the same variable twice at different locations, changing the variable to something at location 1 where it won't make sense to have that value at location 2. If you're going to use a variable only in a certain scope then make it local.  
또 다른 이유는 전역 변수에서 실수를 할 가능성이 더 높기 때문입니다. 실수로 같은 변수를 다른 위치에서 두 번 사용하여 변수를 위치 1에서 해당 값을 가지는 것이 말이 안 되는 값으로 변경할 수 있습니다. 특정 범위에서만 변수를 사용하려면 지역 변수로 만드세요.

In the previous chapter we made a function that creates rectangles. In this function we could have made the variable `rect` local, since we only use it in that function. We still use that rectangle outside the function, but we access it from the table `listOfRectangles` to which we add it.  
이전 장에서 우리는 직사각형을 생성하는 함수를 만들었습니다. 이 함수에서는 변수 `rect`을 지역 변수로 만들 수 있었는데, 이는 해당 함수에서만 사용하기 때문입니다. 우리는 여전히 함수 외부에서 그 직사각형을 사용하지만, 이를 추가하는 테이블 `listOfRectangles`에서 접근합니다.

We don't make `listOfRectangles` local because we use it in multiple functions.  
우리는 `listOfRectangles`를 여러 함수에서 사용하기 때문에 지역 변수로 만들지 않습니다.  

```lua
function love.load ()
	listOfRectangles = {}
end

function createRect ()
	local rect = {}
	rect.x = 100
	rect.y = 100
	rect.width = 70
	rect.height = 90
	rect.speed = 100

	-- 새로운 직사각형을 리스트에 추가합니다.
	table.insert(listOfRectangles, rect)
end
```

Though we could still make it local by creating the variable outside of the `love.load` function.
`love.load` 함수 외부에 변수를 생성하여 여전히 지역 변수를 만들 수 있습니다.

```lua
-- 여기에 선언하면 우리는 이 파일의 모든 곳에서 접근할 수 있습니다.
local listOfRectangles = {}

function love.load ()
	-- 비어 있어서 이제 쓸모가 없습니다
end
```

So are there moments when it is okay to use globals? People have mixed opinions on this. Some people will tell you never to use globals. I'll tell you that it's fine, especially as a beginner, to use global variables when you need them in multiple files. Similarly to how `love` is a global variable. Just keep in mind that locals are faster.  
그렇다면 전역 변수를 사용해도 괜찮은 순간이 있을까요? 이에 대해 사람들은 엇갈린 의견을 가지고 있습니다. 어떤 사람들은 절대 전역 변수를 사용하지 말라고 말할 것입니다. 특히 초보자라면 여러 파일에 전역 변수가 필요할 때 전역 변수를 사용해도 괜찮다고 말씀드리겠습니다. `love`가 전역 변수인 것과 비슷합니다. 지역 변수가 더 빠르다는 점만 명심하세요.

Note that throughout this tutorial I use a lot of globals, but this is to make the code smaller and easier to explain.  
이 자습서에서 저는 많은 전역 변수를 사용하지만, 이는 코드를 더 작고 설명하기 쉽게 만들기 위한 것입니다.

___

## 요약
With `require` we can load other lua-files. When you create a variable you can use it in all files. Unless you create a local variable, which is limited to its scope. Local variables do not affect variables with the same name outside of their scope. Always try use local variables over global variables, as they are faster.  
`require`를 사용하면 다른 `.lua` 파일을 불러올 수 있습니다. 변수를 만들 때 지역 변수를 만들지 않는 한 모든 파일에서 변수를 사용할 수 있으나, 유효 범위에 제약됩니다. 지역 변수는 동일한 이름을 가진 변수가 범위를 벗어나도 영향을 미치지 않습니다. 항상 지역 변수를 사용하는 것이, 전역 변수보다 더 빠릅니다.
