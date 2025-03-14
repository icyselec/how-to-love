# 13장 - 충돌 감지
Let's say we're making a game where you can shoot down monsters. A monster should die when it is hit by a bullet. So what we need to check is: Is the monster colliding with a bullet?  
우리 한번 몬스터를 격추하는 게임을 만드는 것에 대해 이야기해 봅시다. 몬스터는 총알에 맞으면 죽어야 합니다. 그래서 우리가 확인해야 할 것은, 몬스터가 총알과 충돌하고 있는가? 입니다.

We're going to create a *collision check* function. We will be checking collision between rectangles. This is called AABB collision. So we need to know, when do two rectangles collide?  
우리는 *충돌 확인* 함수를 만들 것입니다. 직사각형 간의 충돌을 확인할 것입니다. 이것을 AABB 충돌이라고 합니다. 그렇다면 우리가 알아야 할 것은, 두 직사각형이 충돌하고 있는가? 입니다.

I created an image with three examples:  
저는 3가지 예를 들어 이미지를 만들었습니다:

![](/images/book/13/rectangles1.png)

It's time to turn on that programmer brain if you haven't already. What is going on in the third example that isn't happening in the first and second example?  
아직 준비가 안되었다면 프로그래머 두뇌를 켜야합니다. 세 번째 예제에서는 첫 번째와 두 번째 예시와 달리 무슨 일이 일어나고 있나요?

"They are colliding"  
서로 충돌하고 있습니다.

Yes, but you have to be more specific. We need information that the computer can use.  
네, 하지만 좀 더 구체적으로 말씀해 주셔야 합니다. 우리는 컴퓨터가 사용할 수 있는 정보가 필요합니다.

Take a look at the positions of the rectangles. In the first example, Red is not colliding with Blue, because Red is too far to the left. If Red was a bit further to the right, they would touch. How far exactly? Well, if **Red's right side** is further to the **right** than **Blue's left side**. This is something that is true for example 3.  
직사각형의 위치를 살펴보면 첫 번째 예제에서는, 빨강이 파랑과 충돌하지 않습니다. 왜냐하면, 빨강은 왼쪽 멀리 있기 때문입니다. 만약 빨강이 조금만 더 오른쪽으로 가면, 서로 맞닿을 것입니다. 정확히 얼마나 멀리요? 글쎄요, 만약 **빨강의 오른쪽 변**이 **파랑의 왼쪽 변**보다 **오른쪽**으로 더 멀리 있으면요. 이는 세 번째 예제에서는 해당됩니다.

But it's also true for example 2. We need more conditions to be sure there is collision. So example 2 shows we can't go too far to the right. How far exactly can we go? How much would Red have to move to the left for there to be collision? When **Red's left side** is further to the **left** than **Blue's right side**.  
하지만 두 번째 예제의 경우에도 해당합니다. 우리는 충돌이 있는지 확인하려면 더 많은 조건이 필요합니다. 두 번째 예제는 오른쪽으로 너무 멀리 갈 수 없음을 보여줍니다. 정확히 얼마나 멀리 갈 수 있나요? 충돌이 발생하려면 빨강이 왼쪽으로 얼마나 이동해야 하나요? **빨강의 왼쪽 변**이 **파랑의 오른쪽 변**보다 **왼쪽**으로 더 멀리 있을 때 입니다.

So we have two conditions, is that enough to ensure there is collision?  
그래서 두 가지 조건이 있는데, 충돌을 확인하기에 충분한가요?

Well no, look at the following image:  
아니요, 다음 이미지를 보세요:

![](/images/book/13/rectangles2.png)

This situation agrees with our conditions. Red's right side is further to the right than Blue's left side. And Red's left side is further to the left than Blue's right side. Yet, there is no collision. That's because Red is too high. It needs to move down. How far? Till **Red's bottom side** is further to the **bottom** than **Blue's top side**.  
이 상황은 우리의 조건과 일치합니다. 빨강의 오른쪽 변은 파랑의 왼쪽 변보다 오른쪽으로 더 멀리 있습니다. 그리고 빨강의 왼쪽 은 파랑의 오른쪽 변보다 왼쪽에 더 있습니다. 하지만 충돌은 없습니다. 그 이유는 빨강이 너무 높이 있기 때문이고 아래로 이동해야 합니다. 얼마나 멀리요? **빨강의 아래쪽 변**이 **파랑의 위쪽 변**보다 **아래쪽**으로 더 멀리 있을 때 까지요.

But if we move it too far down, there won't be collision anymore. How far can Red move down, and still collide with Blue? As long as **Red's top side** is further to the **top** than **Blue's bottom side**.  
하지만 너무 아래로 이동하면 더 이상 충돌이 발생하지 않습니다. 빨강이 아래로 이동하면서도 파랑과 충돌할 수 있는 거리는 얼마나 될까요? **빨강의 위쪽 변**이 **파란색의 아래쪽 변**보다 **위쪽**으로 더 멀리 있는 한이요.

Now we got four conditions. Are all four conditions true for these three examples?  
이제 네 가지 조건이 있습니다. 이 세 가지 예에 대해 네 가지 조건이 모두 사실인가요?

![](/images/book/13/rectangles3.png)

**Red's right side** is further to the **right** than **Blue's left side**.  
**빨강의 오른쪽 변**이 **파랑의 왼쪽 변**보다 **오른쪽**으로 더 멀리 있습니다.

**Red's left side** is further to the **left** than **Blue's right side**.  
**빨강의 왼쪽 변**이 **파랑의 오른쪽 변**보다 **왼쪽**으로 더 멀리 있습니다.

**Red's bottom side** is further to the **bottom** than **Blue's top side**.  
**빨강의 아래쪽 변**이 **파랑의 위쪽 변**보다 **아래쪽**으로 더 멀리 있습니다.

**Red's top side** is further to the **top** than **Blue's bottom side**.  
**빨강의 위쪽 변**이 **파랑의 아래쪽 변**보다 **위쪽**으로 더 멀리 있습니다.

Yes, they are! Now we need to turn this information into a function.  
네, 맞습니다! 이제 이 사실을 함수로 변환해야 합니다.

First let's create two rectangles.  
먼저 두 개의 직사각형을 만들어 보겠습니다.

```lua
function love.load ()
	-- 두 개의 직사각형을 생성함
	r1 = {
		x = 10,
		y = 100,
		width = 100,
		height = 100
	}

	r2 = {
		x = 250,
		y = 120,
		width = 150,
		height = 120
	}
end

function love.update (dt)
	-- 직사각형 중에 하나가 이동하도록 함
	r1.x = r1.x + 100 * dt
end

function love.draw ()
	love.graphics.rectangle('line', r1.x, r1.y, r1.width, r1.height)
	love.graphics.rectangle('line', r2.x, r2.y, r2.width, r2.height)
end
```

Now we create a new function called checkCollision(), with 2 rectangles as parameters.  
이제 두 개의 직사각형을 매개 변수로 하는 새로운 함수인 checkCollision()을 만듭니다.

```lua
function checkCollision (a, b)

end
```

First we need the sides of the rectangles. The left side is the x position, the right side is the x position + the width. Same with y and height.  
먼저 직사각형의 변이 필요합니다. 왼쪽은 X 위치, 오른쪽은 X 위치 + 너비입니다. Y와 높이도 마찬가지입니다.

```lua
function checkCollision (a, b)
	-- 지역 변수는 낙타 등 표기법 대신 밑줄 문자를 사용하는 것이 일반적입니다
	local a_left = a.x
	local a_right = a.x + a.width
	local a_top = a.y
	local a_bottom = a.y + a.height

	local b_left = b.x
	local b_right = b.x + b.width
	local b_top = b.y
	local b_bottom = b.y + b.height
end
```

Now that we have the four sides of each rectangle, we can use them to put our conditions in an if-statement.  
이제 각 사각형의 네 변을 가지게 되었으니, 이를 사용하여 조건을 if-문으로 표현할 수 있습니다.

```lua
function checkCollision(a, b)
	-- 지역 변수는 낙타 등 표기법 대신 밑줄 문자를 사용하는 것이 일반적입니다
	local a_left = a.x
	local a_right = a.x + a.width
	local a_top = a.y
	local a_bottom = a.y + a.height

	local b_left = b.x
	local b_right = b.x + b.width
	local b_top = b.y
	local b_bottom = b.y + b.height

	-- 만약 빨강의 오른쪽 변이 파랑의 왼쪽 변보다 오른쪽으로 더 멀리 있고,
	if  a_right > b_left
	-- 그리고 빨강의 왼쪽 변이 파랑의 오른쪽 변보다 왼쪽으로 더 멀리 있고,
	and a_left < b_right
	-- 그리고 빨강의 아래쪽 변이 파랑의 위쪽 변보다 아래쪽으로 더 멀리 있고,
	and a_bottom > b_top
	-- 그리고 빨강의 위쪽 변이 파랑의 아래쪽 변보다 위쪽으로 더 멀리 있으면...
	and a_top < b_bottom then
		-- 충돌이 발생했습니다!
		return true
	else
		-- 어느 하나라도 조건에 맞지 않으면 false를 반환합니다.
		return false
	end
end
```

Notice that the if-condition itself is a boolean value. `checkCollision` returns `true` when the if-condition is `true` and vise-versa. Therefore `checkCollision` can be simplified into the following form:  
조건문에 들어가는 값 자체가 부울 값임을 유의하세요. `checkCollision`은 조건이 `true`일 때 `true`를 반환하고 그 반대의 경우도 마찬가지입니다. 따라서 `checkCollision`은 다음과 같은 형태로 단순화할 수 있습니다:

```lua
function checkCollision (a, b)
	-- 지역 변수는 낙타 등 표기법 대신 밑줄 문자를 사용하는 것이 일반적입니다
	local a_left = a.x
	local a_right = a.x + a.width
	local a_top = a.y
	local a_bottom = a.y + a.height

	local b_left = b.x
	local b_right = b.x + b.width
	local b_top = b.y
	local b_bottom = b.y + b.height

	-- 조건문 없이 부울 값을 바로 반환합니다
	return  a_right > b_left
		and a_left < b_right
		and a_bottom > b_top
		and a_top < b_bottom
end
```

Okay, we have our function. Let's try it out! We draw the rectangles filled or lined based on  
좋아요, 저희 함수가 있습니다. 한번 사용해 봅시다! 다음을 기준으로 채워진 직사각형이나 선을 그립니다

```lua
function love.draw ()
	-- mode라고 불리는 지역 변수를 생성합니다
	local mode

	if checkCollision(r1, r2) then
		-- 충돌이 있으면, 직사각형을 면으로 그립니다
		mode = 'fill'
	else
		-- 그렇지 않으면, 직사각형을 선으로 그립니다
		mode = 'line'
	end

	-- 변수를 첫 번째 인자로 합니다
    love.graphics.rectangle(mode, r1.x, r1.y, r1.width, r1.height)
    love.graphics.rectangle(mode, r2.x, r2.y, r2.width, r2.height)
end
```

It works! Now you know how to detect collision between two rectangles.  
작동합니다! 이제 두 직사각형 사이의 충돌을 감지하는 방법을 알게 되었습니다.

___

## 요약

Collision between two rectangles can be checked with four conditions.  
두 직사각형 사이의 충돌은 네 가지 조건으로 확인할 수 있습니다.

직사각형 A, B가 있을 때:

A's right side is further to the right than B's left side.  
A의 오른쪽 변이 B의 왼쪽 변보다 오른쪽으로 더 멀리 있음.

A's left side is further to the left than B's right side.  
A의 왼쪽 변이 B의 오른쪽 변보다 왼쪽으로 더 멀리 있음.

A's bottom side is further to the bottom than B's top side.  
A의 아래쪽 변이 B의 위쪽 변보다 아래쪽으로 더 멀리 있음.

A's top side is further to the top than B's bottom side.  
A의 위쪽 변이 B의 아래쪽 변보다 위쪽으로 더 멀리 있음.
