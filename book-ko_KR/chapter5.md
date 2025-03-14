# 5장 - 직사각형 움직이기

Now we can start with what I like to call "The fun part". We're going to make stuff move!
이제 제가 좋아하는 "꿀잼 파트"부터 시작해보겠습니다. 뭔가를 움직여봅시다!

Let's start with the 3 main callbacks.
3가지의 주요 콜백 함수부터 시작하겠습니다.
> 번역자: 아래 코드를 기억해두는 것이 좋습니다. 왜냐하면 이는 Love2D의 내부 작동 순서와도 연관이 있기 때문입니다.

```lua
function love.load ()

end


function love.update ()

end

function love.draw ()

end
```

Next, we draw a rectangle.
다음으로, 우리는 직사각형을 그려볼겁니다.

```lua
function love.draw ()
	love.graphics.rectangle("line", 100, 50, 200, 150)
end
```

![](/images/book/5/rectangle.png)

The second and third argument of this function are the x and y position.
함수의 두번째와 세번째 인자는 x 좌표와 y 좌표입니다.

x means "horizontal position on the screen". 0 is the left side of the screen.
x는 "화면에서 수평으로의 위치"를 말합니다. 0은 화면의 왼쪽입니다.

y means "vertical position on the screen". 0 is the top side of the screen.
y는 "화면에서 수직으로의 위치"를 말합니다. 0은 화면의 위쪽입니다.

![](/images/book/5/coordinates.png)

Now we want to make the rectangle move. It's time to start thinking like a programmer. What exactly needs to happen in order for the rectangle to move to the right? The x-position needs to go up. 100, 101, 102, 103, 104, etc. But we can't change 100 to 101. 100 is simply 100. We need to have something that can change in any number we want it to be. That's right, a **variable**!
우리는 직사각형을 움직이게 하고 싶습니다. 이제부터 프로그래머처럼 생각해야할 시간입니다. 직사각형이 오른쪽으로 움직이게 하려면 정확히 어떻게 해야합니까? 그건 바로, x 좌표가 올라가야 합니다. 100, 101, 102, 103 등... 하지만 100은 그 자체로 100이지 변할 수 없는 값입니다. 우리에게 필요한 건 원하는 값으로 변할 수 있게 하는 무언가입니다. 혹시 **변수**를 떠올리셨다면 맞습니다!


In love.load, create a new variable called `x`, and replace the 100 in `love.graphics.rectangle` with `x`.
`love.load`에서, `x`라는 이름으로 변수 하나를 작성하고 `love.graphics.rectangle`에 있는 100을 `x`로 바꿉니다.

```lua
function love.load ()
	x = 100	
end

function love.draw ()
	love.graphics.rectangle("line", x, 50, 200, 150)
end
```

So now the x-position of our rectangle is the value of `x`.
이제, 직사각형의 x 좌표는 `x`의 값이 됩니다.

Note that the variable name `x` is just a name. We could've named it `icecream` or `unicorn` or whatever. Functions don't care about variable names, it only cares about its value.
`x`라는 변수 이름은 그냥 이름일 뿐입니다. `kimchi`나 `horange` 같은 이름으로 지을 수도 있습니다. 함수는 변수의 이름을 신경쓰지 않고, 값에만 신경을 씁니다.

Now we want to make the rectangle move. We do this in love.update. Every update we want to increase `x` by 5. In other words, `x` needs to be value of `x` + 5. And that's exactly how we write it.
이제 우리는 직사각형을 움직이게 하고 싶습니다. 다음을 `love.update`에서 수행합니다. 매 업데이트마다 `x`의 값이 5만큼씩 증가하기를 원합니다. 즉, `x`는 `x` + 5의 값이여야 합니다. 이는 우리가 사용할 방법입니다.

```lua
function love.update ()
	x = x + 5
end
```

So now when `x` equals 100, it will change `x` into 100 + 5. Then next update `x` is 105 and `x` will change into 105 + 5, etc.
그래서 `x`가 100일 때, 위 함수는 `x`를 100 + 5로 바꿉니다. 그리고 다음 업데이트에 `x`가 105면, `x`는 105 + 5가 됩니다...

Run the game. The rectangle should now be moving.
게임을 실행합니다. 직사각형이 움직일 겁니다.

![](/images/book/5/rectangle_move.gif)

___

## 델타 시간

We got a moving rectangle, but there's one small problem. If you were to run the game on a different computer, the rectangle might move with a different speed. This is because not all computers update at the same rate, and that can cause problems.
원하는 것을 결과를 얻었지만, 작은 문제가 하나 있습니다. 다른 컴퓨터에서 실행하면, 직사각형이 다른 속도로 움직일 수도 있습니다. 왜냐하면 모든 컴퓨터는 같은 갱신 주기를 가지고 있지 않으며, 이는 문제를 일으킵니다.
> 번역자: 번역자의 컴퓨터는 75 프레임으로 작동하며, 60 프레임일 때보다 움직임이 25% 더 빨라보입니다.

For example, let's say that Computer A runs with 100 fps (frames per second), and Computer B runs with 200 fps.
예를 들어, A 컴퓨터에서는 100 프레임(초당 프레임)으로 돌아가고, B 컴퓨터에서는 200 프레임으로 돌아갑니다.

100 x 5 = 500

200 x 5 = 1000

So in 1 second, x has increased with 500 on computer A, while on computer B x has increased with 1000.
그래서 1초에 x가 A 컴퓨터에서는 500이 증가했지만, B 컴퓨터에서는 x가 1000이나 증가했습니다.

Luckily, there's a solution for this: delta time.
다행히도, 여기에는 델타 시간이라는 해결책이 하나 있습니다.

When LÖVE calls love.update, it passes an argument. Add the parameter dt (short for delta time) in the love.update, and let's print it.
LÖVE가 `love.update`를 호출할 때, 인자 하나를 넘깁니다. `dt`(Delta Time의 줄임말)라는 매개변수를 `love.update`에 추가하고, 이를 출력해봅시다.

```lua
function love.update (dt)
	print(dt)
	x = x + 5
end
```

Delta time is the time that has passed between the previous and the current update. So on computer A, which runs with 100 fps, delta time on average would be 1 / 100, which is 0.01.
델타 시간은 이전 게임 갱신 시점부터 현재 게임 갱신 시점까지 지난 시간을 나타냅니다. 그래서 A 컴퓨터에서 100 프레임으로 돌아가면, 델타 시간은 1 / 100이고 0.01과 같습니다.

On computer B, delta time would be 1 / 200, which is 0.005.
B 컴퓨터에서는 델타 시간은 1 / 200이고 0.005와 같습니다.

So in 1 second, computer A updates 100 times, and increases `x` by 5 x 0.01, and computer B updates 200 times and increases `x` by 5 x 0.005.
그래서 1초에 A 컴퓨터는 100번 게임이 갱신되고 `x`가 5 * 0.01씩 증가하고, B 컴퓨터는 200번 게임이 갱신되고 `x`가 5 * 0.005씩 증가합니다.

100 x 5 * 0.01 = 5

200 x 5 * 0.005 = 5

By using delta time our rectangle will move with the same speed on all computers.
델타 시간을 이용하면 직사각형이 모든 컴퓨터에서 같은 속도로 이동합니다.

```lua
function love.update (dt)
	x = x + 5 * dt
```

Now our rectangle moves 5 pixels per second, on all computers. Change 5 to 100 to make it go faster.
이제 직사각형은 모든 컴퓨터에서 5픽셀 만큼 움직입니다. 더 빠르게 이동하려면 값을 5에서 100으로 바꾸세요.

___

## 요약

We use a variable that we increase on each update to make the rectangle move. When increasing we multiply the added value by delta time. Delta time is the time between the previous and current update. Using delta time makes sure that our rectangle moves with the same speed on all computers.
직사각형을 이동시키기 위해 매 게임 갱신마다 증가하는 변수를 사용한다. 증가할 때, 기본값에 델타 시간을 곱한다. 델타 시간은 이전 게임 갱신과 현재 게임 갱신 사이의 시간이다. 델타 시간을 사용하면 모든 컴퓨터에서 직사각형이 동일한 속도로 이동한다.
