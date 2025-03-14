# 14장 - 게임: 적을 쏴라

지금까지 배운 모든 것을 사용하여 간단한 게임을 만들어 봅시다. 프로그래밍과 게임 만들기에 대해 원하는 대로 읽을 수 있지만 정말로 배우려면 직접 만들어야 합니다.

게임은 본질적으로 풀어야 할 여러 가지 문제들입니다. 숙련된 프로그래머에게 PONG을 만들어 달라고 요청하면 그는 *PONG을 만드는 방법*을 찾지 않습니다. 그들은 PONG을 별도의 문제로 나누고 각 문제를 해결하는 방법을 알 수 있습니다. 이 장에서는 게임을 여러 작업으로 나누어 해결하는 방법을 여러분께 보여드릴 것입니다.

우리가 만들 게임은 간단합니다. 적이 벽에 부딪히고 있습니다. 우리는 적을 쏴야 합니다. 맞출 때마다, 적이 조금씩 빠르게 갑니다. 놓치면 게임 오바, 싹다 다시 시작해야 합니다.

![](/images/book/14/demo.gif)

이 게임에서는 몇 가지 이미지를 사용할 것입니다. 자신의 이미지를 자유롭게 사용할 수 있지만, 저는 이 세 가지를 사용할 것입니다:

![](/images/book/14/panda.png)
![](/images/book/14/snake.png)
![](/images/book/14/bullet.png)

이 이미지들은 누구나 게임에서 사용할 수 있는 많은 무료 에셋을 만드는 [Kenney](http://kenney.nl/assets)에 의해 만들어졌습니다. 한 번 둘러보세요!

세 가지 주요 콜백부터 시작하여 클래스를 흉내낼 때 사용하는 라이브러리인 *classic*을 불러 오겠습니다.
> 번역자: 5장에서 이미 언급된 내용이지만, 아래 3개의 콜백 함수는 기억하는 것이 좋습니다.

```lua
function love.load ()
	Object = require "classic"
end

function love.update (dt)

end

function love.draw ()

end
```

플레이어부터 시작해봅시다. `player.lua`라는 새 파일을 만듭니다.

모든 객체에 대해 기본 클래스를 만들 수 있지만, 너무 짧은 간단한 게임이기 때문에 기본 클래스 없이도 할 수 있습니다. 이 장의 마지막에서 여러분이 스스로 기본 클래스를 추가할 수 있도록 개선하는 코드의 예시를 보여 드리겠습니다.

___

## 작업: 움직이는 플레이어 만들기

플레이어 클래스를 만듭니다:

```lua
--! file: player.lua
Player = Object:extend()

function Player:new ()

end
```

저는 팬더 이미지를 플레이어에게 부여할 것입니다.

```lua
function Player:new ()
	self.image = love.graphics.newImage("panda.png")
end


function Player:draw ()
	love.graphics.draw(self.image)
end
```

다음은, 방향 키로 플레이어가 움직일 수 있게 만들어 봅시다.

```lua
function Player:new ()
	self.image = love.graphics.newImage("panda.png")
	self.x = 300
	self.y = 20
	self.speed = 500
	self.width = self.image:getWidth() 
end

function Player:update (dt)
	if love.keyboard.isDown 'left' then
		self.x = self.x - self.speed * dt
	elseif love.keyboard.isDown 'right' then
		self.x = self.x + self.speed * dt
	end
end

function Player:draw ()
	love.graphics.draw(self.image, self.x, self.y)
end
```

그리고 이제 플레이어를 이동시킬 수 있을 겁니다. `main.lua`로 돌아와서 플레이어 클래스를 불러 오겠습니다.

```lua
--! file: main.lua
function love.load ()
    Object = require 'classic'
    require 'player'

    player = Player()
end

function love.update (dt)
	player:update(dt)
end

function love.draw ()
	player:draw()
end
```

보시다시피, 우리는 플레이어를 움직일 수 있습니다. 하지만 플레이어가 창 밖으로 나갈 수 있는데 조건문으로 이것을 고쳐봅시다.

```lua
--! file: player.lua

function Player:update (dt)
	if love.keyboard.isDown 'left' then
		self.x = self.x - self.speed * dt
	elseif love.keyboard.isDown 'right' then
		self.x = self.x + self.speed * dt
	end

	-- 창의 너비를 얻어옵니다.
	local window_width = love.graphics.getWidth()

	-- 만약 x가 왼쪽으로 너무 멀리 가면..
	if self.x < 0 then
		-- x를 0으로 합니다
		self.x = 0

	-- 그렇지 않고, x가 오른쪽으로 너무 멀리 가면..
	elseif self.x > window_width then
		-- x를 창의 너비로 설정합니다.
		self.x = window_width
	end
end
```

이런, 우리 플레이어는 여전히 오른쪽으로 너무 멀리 이동할 수 있어요. 오른쪽 벽에 부딪히는지 확인할 때 너비를 포함해야 합니다.

```lua
-- 만약 x가 왼쪽으로 너무 멀리 가면..
if self.x < 0 then
	-- x를 0으로 합니다
	self.x = 0

-- 그렇지 않고, x가 오른쪽으로 너무 멀리 가면..
elseif self.x + self.width > window_width then
	-- 창의 너비에 플레이어의 너비를 뺀 값을 x로 합니다
	self.x = window_width - self.width
end
```

이제 해결되었습니다. 우리 플레이어는 더 이상 창 밖으로 나갈 수 없습니다.

___

## 작업: 움직이는 적 만들기

이제 적 클래스를 만들어 보겠습니다. `enemy.lua`라는 새 파일을 만들고 다음과 같이 입력합니다:

```lua
--! file: enemy.lua
Enemy = Object:extend()

function Enemy:new ()

end
```

적에게 뱀 이미지를 부여하고 스스로 움직이게 할 것입니다.

```lua
function Enemy:new ()
	self.image = love.graphics.newImage("snake.png")
	self.x = 325
	self.y = 450
    self.speed = 100
end

function Enemy:update (dt)
	self.x = self.x + self.speed * dt 
end

function Enemy:draw ()
	love.graphics.draw(self.image, self.x, self.y)
end
```

적을 벽에서 튕겨나가게 해야 하지만, 먼저 게임에 불러 오겠습니다.

```lua
--! file: main.lua
function love.load ()
    Object = require 'classic'
    require 'player'
    require 'enemy'

    player = Player()
    enemy = Enemy()
end

function love.update (dt)
	player:update(dt)
	enemy:update(dt)
end

function love.draw ()
	player:draw()
	enemy:draw()
end
```

이제 적이 움직이는 것을 볼 수 있고, 창 밖으로 나가는 것을 볼 수 있습니다. 플레이어처럼 창 밖으로 움직이지 않도록 합시다.

```lua
function Enemy:new ()
	self.image = love.graphics.newImage("snake.png")
	self.x = 325
	self.y = 450
    self.speed = 100
    self.width = self.image:getWidth()
    self.height = self.image:getHeight()
end

function Enemy:update (dt)
	self.x = self.x + self.speed * dt

	local window_width = love.graphics.getWidth()

	if self.x < 0 then
		self.x = 0
	elseif self.x + self.width > window_width then
		self.x = window_width - self.width
	end
end
```

적이 벽에 멈추지만 우리는 그것을 튕기고 싶습니다. 어떻게 그렇게 할 수 있을까요? 오른쪽 벽에 부딪힌 다음 무엇을 할 수 있을까요? 다른 방향으로 이동해야 합니다. 어떻게 하면 다른 방향으로 이동할 수 있을까요? `speed`의 값을 변경하여요. 그리고 `speed`의 값은 어떻게 되어야 할까요? 100이 아니라 -100이어야 합니다.

그렇다면 `self.speed = -100`을 해야 할까요? 아니요. 앞서 말했듯이 적이 맞을 때 속도를 높이고, 이렇게 하면 튕길 때 속도를 재설정할 수 있기 때문입니다. 대신 `speed`의 값을 반전시켜야 합니다. 따라서 `speed`는 `-speed`가 됩니다. 즉, 속도를 120으로 늘리면 -120이 됩니다.

그리고 왼쪽 벽에 부딪히면 어떻게 될까요? 그 시점에서 속도는 음수이므로 양수로 바꿔야 합니다. 어떻게 하면 좋을까요? 음, [음수와 음수를 곱하면 양수가 됩니다](https://ko.khanacademy.org/math/algebra-basics/basic-alg-foundations/alg-basics-negative-numbers/v/why-a-negative-times-a-negative-is-a-positive). 따라서 그 지점에서 음수인 `speed`가 `-speed`가 되면 양수로 변합니다.
> 번역자: -speed는 speed에 -1을 곱하는 것과 같습니다. 즉, -(-speed)는 양수입니다.

```lua
function Enemy:update (dt)
	self.x = self.x + self.speed * dt

	local window_width = love.graphics.getWidth()

	if self.x < 0 then
		self.x = 0
		self.speed = -self.speed
	elseif self.x + self.width > window_width then
		self.x = window_width - self.width
		self.speed = -self.speed
	end
end
```

우리는 플레이어와 움직이는 적을 만들었습니다. 이제 남은 것은 총알 뿐입니다.
___

## 작업: 총알을 발사할 수 있게 하기

`bullet.lua`라는 새 파일을 만들고 다음 코드를 작성합니다:

```lua
--! file: bullet.lua

Bullet = Object:extend()

function Bullet:new ()
	self.image = love.graphics.newImage("bullet.png")
end

function Bullet:draw ()
	love.graphics.draw(self.image)
end
```

총알은 수평이 아닌 수직으로 움직여야 합니다.

```lua
--We pass the x and y of the player.
function Bullet:new (x, y)
	self.image = love.graphics.newImage("bullet.png")
	self.x = x
	self.y = y
	self.speed = 700
	--We'll need these for collision checking
	self.width = self.image:getWidth()
	self.height = self.image:getHeight()
end

function Bullet:update (dt)
	self.y = self.y + self.speed * dt
end

function Bullet:draw ()
	love.graphics.draw(self.image, self.x, self.y)
end
```

이제 총알을 발사할 수 있어야 합니다. `main.lua`에서 파일을 불러오고 테이블을 만듭니다.

```lua
--! file: main.lua
function love.load ()
    Object = require 'classic'
    require 'player'
    require 'enemy'
    require 'bullet'

    player = Player()
    enemy = Enemy()
    listOfBullets = {}
end
```

이제 스페이스바 키를 누르면 총알이 생성되는 기능을 플레이어에게 제공합니다.

```lua
--! file: player.lua
function Player:keyPressed (key)
	-- 만약 스페이스바 키가 눌렸을 경우
	if key == "space" then
		-- Bullet의 새로운 인스턴스를 listOfBullets에 넣습니다
		table.insert(listOfBullets, Bullet(self.x, self.y))
	end
end
```

그리고 우리는 `love.keypressed` 콜백에서 이 함수를 호출해야 합니다.

```lua
--! file: main.lua
function love.keypressed (key)
	player:keyPressed(key)
end
```

이제 우리는 테이블을 순회하며 모든 총알에 대해 업데이트 및 그리기를 해야 합니다.

```
function love.load ()
    Object = require 'classic'
    require 'player'
    require 'enemy'
    require 'bullet'

    player = Player()
    enemy = Enemy()
    listOfBullets = {}
end

function love.update (dt)
	player:update(dt)
	enemy:update(dt)

	for i,v in ipairs(listOfBullets) do
		v:update(dt)
	end
end

function love.draw ()
	player:draw()
	enemy:draw()
	
	for i,v in ipairs(listOfBullets) do
		v:draw()
	end
end
```

멋지네요, 이제 우리 플레이어가 총알을 쏠 수 있습니다.
___

## 작업: 총알이 적의 속도에 영향을 미치도록 만들기

이제 뱀이 총알에 맞을 수 있도록 만들어야 합니다. 총알에 충돌 감지 기능을 제공합니다.

```lua
--! file: bullet.lua
function Bullet:checkCollision (obj)

end
```

아직도 어떻게 하는지 알고 있나요? 충돌을 보장하기 위해 참이어야 하는 4가지 조건을 알고 있나요?

참과 거짓을 반환하는 대신 적의 속도를 높입니다. 총알에 `dead`라는 속성을 부여하고, 이를 사용하여 총알을 목록에서 제거합니다.

```lua
function Bullet:checkCollision(obj)
    local self_left = self.x
    local self_right = self.x + self.width
    local self_top = self.y
    local self_bottom = self.y + self.height

    local obj_left = obj.x
    local obj_right = obj.x + obj.width
    local obj_top = obj.y
    local obj_bottom = obj.y + obj.height

    if  self_right > obj_left
    and self_left < obj_right
    and self_bottom > obj_top
    and self_top < obj_bottom then
        self.dead = true

        --Increase enemy speed
        obj.speed = obj.speed + 50
    end
end
```

이제 main.lua에서 checkCollision을 호출해야 합니다.

```lua
function love.update (dt)
	player:update(dt)
	enemy:update(dt)

	for i,v in ipairs(listOfBullets) do
		v:update(dt)

		--Each bullets checks if there is collision with the enemy
		v:checkCollision(enemy)
	end
end
```

그리고 다음으로 우리는 충돌한 총알을 파괴해야 합니다.

```lua
function love.update (dt)
	player:update(dt)
	enemy:update(dt)

	for i,v in ipairs(listOfBullets) do
		v:update(dt)
		v:checkCollision(enemy)

		-- 만약 총알에 dead 속성이 있고 이 값이 참이면..
		if v.dead then
			-- 리스트에서 제거함
			table.remove(listOfBullets, i)
		end
	end
end
```

적을 놓쳤을 때 마지막으로 해야 할 일은 게임을 재시작하는 것입니다. 총알이 화면 밖으로 나왔는지 확인해야 합니다.

```lua
--! file: bullet.lua
function Bullet:update(dt)
	self.y = self.y + self.speed * dt

	--If the bullet is out of the screen
	-- 만약 총알이 화면 밖으로 나갔으면
	if self.y > love.graphics.getHeight() then
		-- 게임을 재시작한다.
		love.event.quit('restart')
		return
	end
end
```
> 번역자: 게임을 올바르게 재시작하려면 `love.load` 대신 `love.event.quit('restart')`를 사용해야 합니다.

그리고 테스트해 보겠습니다. 적이 왼쪽으로 이동하는 동안 적을 치면 속도가 느려지는 것을 알 수 있습니다. 이는 적의 속도가 그 지점에서 음수이기 때문입니다. 따라서 숫자를 늘리면 적은 속도가 느려집니다. 이 문제를 해결하려면 적의 속도가 음수인지 여부를 확인해야 합니다.

```lua
function Bullet:checkCollision (obj)
    local self_left = self.x
    local self_right = self.x + self.width
    local self_top = self.y
    local self_bottom = self.y + self.height

    local obj_left = obj.x
    local obj_right = obj.x + obj.width
    local obj_top = obj.y
    local obj_bottom = obj.y + obj.height

    if  self_right > obj_left
    and self_left < obj_right
    and self_bottom > obj_top
    and self_top < obj_bottom then
        self.dead = true

        -- 적의 속도를 증가시킨다
        if obj.speed > 0 then
	        obj.speed = obj.speed + 50
	    else
	    	obj.speed = obj.speed - 50
	    end
    end
end
```

이제 게임이 끝났습니다. 아니면 그런 건가요? 직접 게임에 기능을 추가해 보세요. 아니면 완전히 새로운 게임을 만드세요. 계속 배우고 계속 게임을 만들면 상관없습니다!

___

## 요약

게임 만들기는 본질적으로 해결해야 할 많은 문제들의 연속이다.
