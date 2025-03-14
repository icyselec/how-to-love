# 10장 - 라이브러리

A library is code that everyone can use to add certain functionality to their project.  
라이브러리는 어떤 사용자든 프로젝트에 특정 기능을 추가하는 데 사용할 수 있는 코드입니다.

Let's try out a library. We're going to use *tick* by *rxi*. You can find the library on [GitHub](https://github.com/rxi/tick).  
라이브러리를 사용해 보겠습니다. 우리는 *rxi*가 만든 *tick*을 사용할 것입니다. 라이브러리는 [GitHub](https://github.com/rxi/tick)에서 찾을 수 있습니다.

Click on `tick.lua` and then on Raw, and [copy the code](https://raw.githubusercontent.com/rxi/tick/master/tick.lua).
`tick.lua`를 클릭한 다음 Raw에서 [copy the code](https://raw.githubusercontent.com/rxi/tick/master/tick.lua)를 클릭합니다.

Go to your text editor, create a new file called `tick.lua` and paste the code.  
편집기로 이동하여 `tick.lua`라는 새 파일을 만들고 코드를 붙여넣습니다.

Now we have to follow the instructions on the GitHub page. First we have to load it with `require`.
이제 GitHub 페이지의 지침을 따라야 합니다. 먼저 `require`로 파일을 불러와야 합니다.

```lua
function love.load ()
	tick = require "tick"
end
```

Notice how `require` doesn't have parantheses (). This is because when you only pass 1 argument, you don't have to use them. Now I recommend you still do use them for any other function, but with `require` it's common to leave them out. But in the end, it doesn't even matter.  
`require`에는 괄호()가 없다는 점에 유의하세요. 이는 하나의 인자만 전달할 때는 괄호를 사용할 필요가 없기 때문입니다. 저는 이제 다른 함수에는 여전히 괄호를 사용할 것을 권장하지만, `require`에서는 괄호를 빼는 것이 일반적입니다. 하지만 결국에는 상관없습니다.

Next we have to put `tick.update(dt)` in our updater.  
다음으로 업데이트 코드에 `tick.update(dt)`를 추가해야 합니다.

```lua
function love.update(dt)
	tick.update(dt)
end
```

And now we're ready to start using the library. Let's make it so that a rectangle will be drawn after 2 seconds.  
이제 라이브러리 사용을 시작할 준비가 되었습니다. 2초 후에 직사각형이 그려지도록 만들어 보겠습니다.

```lua
function love.load()
	tick = require "tick"

	-- 불리언 변수를 생성
	drawRectangle = false

	-- 첫 번째 인자는 함수
	-- 두 번째 인자는 함수를 호출하기까지 걸리는 시간
	tick.delay(function () drawRectangle = true end, 2)
end

function love.draw()
	-- drawRectangle이 참이면 직사각형을 그립니다.
	if drawRectangle then
		love.graphics.rectangle("fill", 100, 100, 300, 200)
	end
end
```

Did we just pass a function as an argument? Sure, why not? A function is a type of variable after all.  
방금 우리는 함수를 인수로 넘겼지 않나요? 물론이죠, 안될 거 뭐 있나요? 함수는 결국 변수의 일종입니다.

When you run the game  you can see that with this library we can put a delay on calling functions. And like that there are tons of libraries with all kinds of functionality.  
게임을 실행하면 이 라이브러리를 사용하여 호출 기능을 지연시킬 수 있다는 것을 알 수 있습니다. 그리고 모든 종류의 기능을 갖춘 수 많은 라이브러리가 있습니다.

Don't feel guilty for using a library. Why reinvent the wheel? That is, unless you are interested in learning it. I personally use about 10 libraries in my projects. They provide functionality that I don't understand how to make myself, and I'm simply not interested in learning it.  
라이브러리를 사용하는 것에 대해 죄책감을 느끼지 마세요. 왜 바퀴를 재발명하세요? 즉, 바퀴를 배우는 데 관심이 없는 한 말이죠. 저는 개인적으로 프로젝트에서 약 10개의 라이브러리를 사용합니다. 라이브러리는 제가 직접 만드는 방법을 이해할 수 없는 기능을 제공하며, 단순히 배우는 데 관심이 없습니다.

Libraries aren't magic. It's all Lua code that you and I could've written as well (in case we have the knowledge of course). We'll create a library in a future chapter so that we have a better understanding of how they work.  
라이브러리는 마법이 아닙니다. 여러분과 제가 (물론 지식이 있다면) 작성할 수 있었던 모든 것이 Lua 코드입니다. 앞으로의 장에서 라이브러리를 만들어서 라이브러리가 어떻게 작동하는지 더 잘 이해할 수 있도록 하겠습니다.

___

## 표준 라이브러리

Lua has built-in libraries. These are called *Standard Libraries*. They are the functions that are built into Lua. `print`, for example, is part of the standard library. And so is `table.insert` and `table.remove`.  
Lua에는 내장 라이브러리가 있습니다. 이를 *표준 라이브러리*라고 합니다. Lua에 내장된 함수입니다. 예를 들어, `print`는 표준 라이브러리의 일부입니다. `table.insert`와 `table.remove`도 마찬가지입니다.

One important standard library we haven't looked at yet is the *math library*. It provides math functions, which can be very useful when making a game.  
아직 살펴보지 않은 중요한 표준 라이브러리 중 하나는 *수학 라이브러리*입니다. 이 라이브러리는 게임을 만들 때 매우 유용하게 사용할 수 있는 수학 함수를 제공합니다.

For example, `math.random` gives us a random number. Let's use it to place a rectangle on a random position whenever you press the spacebar.
예를 들어 `math.random`은 무작위 숫자를 제공합니다. 스페이스 바를 누를 때마다 사각형을 무작위 위치에 배치하는 데 사용해 보겠습니다.

```lua
function love.load()
	x = 30
	y = 50
end


function love.draw()
	love.graphics.rectangle("line", x, y, 100, 100)
end


function love.keypressed(key)
	-- 만약, 스페이스 바를 눌렀을 때..
	if key == "space" then
		-- x와 y는 100에서 500 사이의 무작위 숫자가 된다
		x = math.random(100, 500)
		y = math.random(100, 500)
	end
end
```
> 번역자: How to Love의 최신 판에서는 어떻게 되었을지 모르겠지만, `love.math.random`을 사용하는 것이 더 권장됩니다.

Now that we understand what libraries are, we can start using a class library.
이제 라이브러리가 무엇인지 이해했으니 객체 라이브러리를 사용해봅시다.

___

## 요약
Libraries are code that gives us functionality. Anyone can make a library. Lua also has built-in libraries which we call the Standard Libraries.  
라이브러리는 우리에게 기능을 제공하는 코드입니다. 누구나 라이브러리를 만들 수 있습니다. Lua에는 표준 라이브러리라고 부르는 내장 라이브러리도 있습니다.
