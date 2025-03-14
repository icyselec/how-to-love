# 11장 - 클래스

클래스는 설계도와 같습니다. 하나의 설계도로 여러 개의 집을 만들 수 있습니다. 마찬가지로 하나의 클래스에서 여러 개의 객체를 만들 수 있습니다.

![](/images/book/11/blueprint.png)

우리는 클래스를 위해 [classic](https://github.com/rxi/classic) 라이브러리를 사용할 것입니다.

`classic.lua`를 클릭한 다음 Raw에서 [copy the code](https://raw.githubusercontent.com/rxi/classic/master/classic.lua)를 클릭합니다.

편집기로 이동하여 `classic.lua`라는 새 파일을 만들고 코드를 붙여넣습니다.

`require`로 이걸 불러옵니다.

```lua
function love.load ()
	Object = require "classic"
end
```

이제 클래스를 만들 준비가 되었습니다. `rectangle.lua`라는 새 파일을 만들고 다음 코드를 입력합니다:

```lua
--! file: rectangle.lua

-- `Object`를 첫 번째 인자로 전달합니다.
Rectangle = Object.extend(Object)

function Rectangle.new (self)
	self.test = math.random(1, 1000)
end
```

모든 것이 설명될 것입니다. 하지만 먼저 이 코드를 `main.lua`에 입력하세요.

```lua
--! file: main.lua
function love.load ()
	Object = require 'classic'
	-- 파일을 불러오는 것을 잊지 마세요
	require 'rectangle'

	r1 = Rectangle()
	r2 = Rectangle()
	print(r1.test, r2.test)
end
```

게임을 실행하면 두 개의 무작위 숫자가 출력되는 것을 볼 수 있습니다.

이제 이 코드를 단계별로 살펴보겠습니다. 먼저 `Rectangle = Object.extend(Object)`로 새로운 클래스를 만듭니다. 이렇게 하면 `Rectangle`이 클래스가 됩니다. 이것이 우리의 설계도가 될 것입니다. 일반적으로 클래스는 속성과 달리 대문자로 시작합니다. (그래서 UpperCamelCase 또는 PascalCase가 됩니다.)

`main.lua`에서 우리는 `r1 = Rectangle()`이라고 작성했습니다. `Rectangle`은 테이블이지만 함수인 것처럼 호출할 수 있습니다. 이것의 작동 방식은 다른 장의 내용이지만 `Rectangle()`을 호출하면 새로운 인스턴스가 생성됩니다. 즉, `Rectangle()`은 우리의 설계도를 가져와 클래스의 모든 특징을 가진 새로운 객체를 생성한다는 뜻입니다. 그리고 모든 새로운 인스턴스는 고유합니다.

`r1`이 고유하다는 것을 증명하기 위해 `r2`라는 또 다른 인스턴스를 만듭니다. 둘 다 `test`라는 속성을 가지고 있지만 값이 다릅니다.

`Rectangle()`을 호출하면 `Rectangle.new`가 실행됩니다. 이것을 *생성자*라고 부릅니다.

매개변수 `self`는 우리가 수정하고 있는 인스턴스입니다. 만약 `Rectangle.test = math.random(0, 1000)`를 입력한다면, 우리는 이 속성을 설계도로 만든 인스턴스가 아닌 설계도로 제공할 것입니다.

우리 클래스에 몇 가지 변화를 주겠습니다:

```lua
--! file: rectangle.lua
Rectangle = Object.extend(Object)

function Rectangle.new (self)
	self.x = 100
	self.y = 100
	self.width = 200
	self.height = 150
	self.speed = 100
end

function Rectangle.update (self, dt)
	self.x = self.x + self.speed * dt
end

function Rectangle.draw (self)
	love.graphics.rectangle('line', self.x, self.y, self.width, self.height)
end
```

마치 [8장](/learn/book/8)에서 만든 움직이는 직사각형 객체와 같습니다. 이번에는 객체에 이동과 그리기 코드를 넣습니다. 이제 `main.lua`만 호출하고 그리기만 하면 됩니다.

```lua
--! file: main.lua

function love.load ()
	Object = require 'classic'
	require 'rectangle'
	r1 = Rectangle()
	r2 = Rectangle()
end

function love.update (dt)
	r1.update(r1, dt)
end

function love.draw ()
	r1.draw(r1)
end
```

게임을 실행하면 움직이는 직사각형이 보일 것입니다.

그래서 우리는 `Rectangle`이라는 클래스를 만들었습니다. 우리는 이 클래스의 인스턴스인 `r1`을 생성했고, 여기에는 `update`와 `draw` 함수가 있습니다. 우리가 이 함수들을 호출할 때, 인스턴스인 `r1` 자기 자신을 첫 번째 인자로 전달했습니다. 이것이 함수의 매개 변수인 `self`가 가지는 의미입니다.

하지만 함수를 호출할 때마다 `r1`을 전달해야 하는 것은 다소 귀찮습니다. 다행히도 Lua는 이에 대한 축약 표현을 가지고 있습니다. 콜론(:)을 사용하면 함수 호출이 콜론의 왼쪽에 있는 객체가 첫 번째 인자가 되도록 자동으로 전달해줍니다.

```lua
--! file: main.lua
function love.update (dt)
	-- Lua의 컴파일러는 `r1.update(r1, dt)`와 같이 변환할 것입니다.
	r1:update(dt)
end

function love.draw ()
	-- Lua의 컴파일러는 `r1.draw(r1)`과 같이 변환할 것입니다.
	r1:draw()
end
```

그리고 함수를 정의할 때도 사용할 수 있습니다.

```lua
--! file: rectangle.lua

-- Lua의 컴파일러는 `Object.extend(Object)`와 같이 변환할 것입니다.
Rectangle = Object:extend()

-- Lua의 컴파일러는 `function Rectangle.new (self)`와 같이 변환할 것입니다.
function Rectangle:new ()
	self.x = 100
	self.y = 100
	self.width = 200
	self.height = 150
	self.speed = 100
end

-- Lua의 컴파일러는 `function Rectangle.update (self, dt)`와 같이 변환할 것입니다.
function Rectangle:update (dt)
	self.x = self.x + self.speed * dt
end

-- Lua의 컴파일러는 `function Rectangle.draw (self)`와 같이 변환할 것입니다.
function Rectangle:draw ()
	love.graphics.rectangle("line", self.x, self.y, self.width, self.height)
end
```

우리는 이것을 [*신택틱 슈거*](https://ko.wikipedia.org/wiki/%EC%8B%A0%ED%83%9D%ED%8B%B1_%EC%8A%88%EA%B1%B0)라고 부릅니다.
> 번역자: 문법적 설탕이 더 읽기 쉬울 것입니다.

`Rectangle:new()`에 몇 가지 매개 변수를 추가 해보겠습니다.

```lua
--! file: rectangle.lua
function Rectangle:new (x, y, width, height)
	self.x = x
	self.y = y
	self.width = width
	self.height = height
	self.speed = 100
end
```

이를 통해 `r1`과 `r2`가 각각 자신의 위치와 크기를 가지게 할 수 있습니다.

```lua
--! file: main.lua

function love.load ()
	Object = require 'classic'
	require 'rectangle'
	r1 = Rectangle(100, 100, 200, 50)
	r2 = Rectangle(350, 80, 25, 140)
end

function love.update (dt)
	r1:update(dt)
	r2:update(dt)
end

function love.draw ()
	r1:draw()
	r2:draw()
end
```

이제 움직이는 직사각형 두 개가 생겼습니다. 이것이 바로 클래스를 위대하게 만드는 이유입니다. `r1`과 `r2`는 고유하지만, 같습니다.

![](/images/book/11/moving_rectangles_classes.gif)

클래스를 위대하게 만드는 또 다른 요소는 *상속*입니다.

___

## 상속

상속을 통해 클래스를 확장할 수 있습니다. 즉, 원본 설계도를 편집하지 않고도 설계도의 복사본을 만들고 여기에 기능을 추가합니다.

![](/images/book/11/extension.png)

몬스터와 함께 게임을 한다고 가정해 보겠습니다. 각 몬스터는 고유한 공격력을 가지고 있으며, 움직임이 다릅니다. 하지만 몬스터는 피해를 입을 수도 있고 죽을 수도 있습니다. 이러한 겹치는 기능은 *슈퍼클래스* 또는 *베이스클래스*라고 부르는 것에 넣어야 합니다. 모든 몬스터가 가지고 있는 기능을 제공합니다. 그런 다음 각 몬스터의 클래스는 이 베이스 클래스를 확장하여 자신만의 기능을 추가할 수 있습니다.

또 다른 움직이는 모양인 원을 만들어 보겠습니다. 움직이는 직사각형과 원의 공통점은 무엇일까요? 둘 다 움직일 것입니다. 이제 두 도형 모두에 대한 기본 클래스를 만들어 보겠습니다.

`shape.lua`라는 새 파일을 만들고 다음 코드를 입력합니다:

```lua
--! file: shape.lua
Shape = Object:extend()

function Shape:new (x, y)
	self.x = x
	self.y = y
	self.speed = 100
end

function Shape:update (dt)
	self.x = self.x + self.speed * dt
end
```

이제 기본 클래스 `Shape`가 이 동작을 처리합니다. *기본 클래스*는 단지 용어일 뿐이라는 점을 지적하고 싶습니다. "X는 Y의 기본 클래스입니다.". 기본 클래스는 다른 클래스와 여전히 동일합니다. 우리가 사용하는 방식이 다를 뿐입니다.

어쨌든 이제 움직임을 처리하는 기본 클래스가 생겼으니 `Rectangle`을 `Shape`에서 확장되도록 만들고 업데이트 코드를 제거할 수 있습니다. 사용하기 전에 반드시 `Shape`를 불러오세요.

```lua
--! file: main.lua

function love.load ()
	Object = require 'classic'
	require 'shape'
	require 'rectangle'
	r1 = Rectangle()
	r2 = Rectangle()
end
```

```lua
--! file: rectangle.lua
Rectangle = Shape:extend()

function Rectangle:new (x, y, width, height)
	Rectangle.super.new(self, x, y)
	self.width = width
	self.height = height
end

function Rectangle:draw ()
	love.graphics.rectangle('line', self.x, self.y, self.width, self.height)
end
```

`Rectangle = Shape:extend()`로 `Rectangle`을 `Shape`에서 확장되도록 만들었습니다.

`Shape`에는 `:new()`라는 고유한 함수가 있습니다. `Rectangle:new()`를 만들면 원래 함수보다 우선합니다. 즉, `Rectangle()`을 호출하면 `Shape:new()`가 실행되지 않고 `Rectangle:new()`가 실행됩니다.

하지만 직사각형 클래스에는 `super` 속성이 있으며, 이는 `Rectangle` 클래스가 확장된 것입니다. `Rectangle.super`를 사용하면 기본 클래스의 함수에 접근할 수 있으며, 이를 사용하여 `Shape:new()`라고 부릅니다.
  
우리는 `self`를 첫 번째 인자로 넘겨야 하며, Lua가 그것을 콜론으로 다룰 수 없도록 해야 합니다. 왜냐하면 우리는 그 함수를 인스턴스라고 부르지 않기 때문입니다.

이제 원 클래스를 만들어야 합니다. `circle.lua`라는 새 파일을 만들고 다음 코드를 입력합니다.

```lua
--! file: circle.lua
Circle = Shape:extend()

function Circle:new (x, y, radius)
	Circle.super.new(self, x, y)
	-- 원은 너비나 높이가 아니라 반지름을 가지고 있습니다.
	self.radius = radius
end


function Circle:draw ()
	love.graphics.circle('line', self.x, self.y, self.radius)
end
```

그래서 우리는 `Circle`을 `Shape`에서 확장되도록 만듭니다. 우리는 `Circle.super.new(self, x, y)`를 가진 `Shape`의 `new()` 함수에 `x`와 `y`를 전달합니다.

우리는 `Circle` 클래스에 자체 드로잉 함수를 부여합니다. 이렇게 원을 그립니다. 원은 너비나 높이가 아니라 반지름을 가지고 있습니다.

이제 `main.lua`에서 `shape.lua`와 `circle.lua`를 불러오고 `r2`를 원으로 바꿉니다.

```lua
--! file: main.lua

function love.load ()
	Object = require 'classic'
	-- 파일을 불러오는 것을 잊지 마세요
	require 'shape'

	require 'rectangle'

	-- 파일을 불러오는 것을 잊지 마세요
	require 'circle'

	r1 = Rectangle(100, 100, 200, 50)

	-- r2를 Rectangle 대신에 Circle로 만듭니다.
	r2 = Circle(350, 80, 40)
end
```

이제 게임을 실행하면 움직이는 직사각형과 움직이는 원이 표시됩니다.

![](/images/book/11/moving_rectangle_circle.gif)

___

## 좀 더 해봅시다

이 모든 코드를 한 번 더 살펴봅시다.

먼저 classic 라이브러리를 `require 'classic'`으로 불러옵니다. 이 라이브러리를 불러오면 테이블이 반환되고, 이 테이블은 `Object` 안에 저장합니다. 클래스를 흉내내는 데 필요한 기본 사항이 포함되어 있습니다. Lua는 클래스가 없지만 `classic`을 사용하면 클래스를 매우 멋지게 모방할 수 있습니다.

다음으로 `shape.lua`를 불러옵니다. 해당 파일에서 `Shape`라는 새로운 클래스를 만듭니다. 이 클래스를 `Rectangle`과 `Circle`의 *기본 클래스*로 사용합니다. 이 클래스의 두 가지 공통점은 `x`와 `y` 속성을 가지고 있고 수평으로 움직인다는 것입니다. 이러한 유사점은 `Shape`에 넣는 것입니다.

다음으로 `Rectangle` 클래스를 만듭니다. 우리는 이 클래스를 기본 클래스 `Shape`에서 확장되도록 만듭니다. `:new()` 함수인 *생성자* 안에서 `Rectangle.super.new(self, x, y)`를 기본 클래스의 생성자라고 부릅니다. 첫 번째 인수로 `self`를 전달하여 `Shape`가 설계도 자체가 아닌 설계도의 인스턴스를 사용하도록 합니다. 우리는 직사각형에 `width`와 `height` 속성을 부여하고 그리기 함수를 작성합니다.

다음으로 원을 제외하고 위의 내용을 반복합니다. 따라서 `width`와 `height` 대신 `radius` 속성을 부여합니다.

이제 클래스가 준비되었으니 이러한 클래스의 *인스턴스*들을 만들기 시작할 수 있습니다. `r1 = Rectangle(100, 100, 200, 50)`을 사용하여 클래스 `Rectangle`의 인스턴스를 만듭니다. 이 인스턴스는 설계도 자체가 아니라 우리의 설계도로 만들어진 객체입니다. 이 인스턴스에 대한 변경 사항은 클래스에 영향을 미치지 않습니다. 우리는 이 인스턴스를 업데이트하고 그렸으며, 이를 위해 콜론(:)을 사용합니다. 이는 인스턴스를 첫 번째 인수로 전달해야 하기 때문이며, 콜론은 Lua가 이를 수행하도록 할 것입니다.

마지막으로 `r2`에 대해서도 동일하게 수행하지만, `Circle`로 만듭니다.

___

## 혼동되세요?

한 장이지만 많은 정보였고, 이 모든 것을 이해하는 데 어려움을 겪고 계신다는 것을 저는 이해합니다. 제 조언은 튜토리얼을 계속 따르세요. 프로그래밍을 처음 접하는 분들은 새로운 개념을 모두 익히기까지는 시간이 걸리겠지만, 결국에는 익숙해질 것입니다. 새로운 주제에 대해 이야기하면서 오래된 주제에 대한 설명을 계속 추가하겠습니다.

___

## 제대로 하기

이 튜토리얼에서는 뭔가를 더 쉽게 만들기 위해 전역 변수를 사용합니다. 하지만 더 적절한 방법은 지역 변수를 사용하는 것입니다.

우리는 이를 위해 두 가지 방식으로 클래스를 변경합니다.

1. 클래스를 파일의 지역 변수로 만듭니다.
2. 파일의 끝에서 지역 변수를 반환합니다.

그래서 우리가 `shape.lua`를 바꾸는 방법은 다음과 같습니다.

```lua
--! file: shape.lua
local Shape = Object:extend()

function Shape:new (x, y)
	self.x = x
	self.y = y
	self.speed = 100
end

function Shape:update (dt)
	self.x = self.x + self.speed * dt
end

return Shape
```

그런 다음 클래스의 다른 파일 안에 `Shape` 파일을 불러오고 그 값을 변수에 전달합니다. 우리가 classic을 사용하는 방식처럼 입니다.

```lua
--! file: rectangle.lua
-- 여기서는 변수의 이름을 무엇이든 지정할 수 있지만, 일관된 이름을 유지하는 것이 좋습니다.
local Shape = require 'shape.lua'

-- Rectangle도 지역 변수로 바꿔봅시다.
local Rectangle = Shape:extend()

function Rectangle:new (x, y, width, height)
	Rectangle.super.new(self, x, y)
	self.width = width
	self.height = height
end

function Rectangle:draw ()
	love.graphics.rectangle('line', self.x, self.y, self.width, self.height)
end

-- 그리고 반환하세요
return Rectangle
```

마지막으로, `main.lua`에서는 지역 변수도 만들 수 있습니다.

```lua
--! file: main.lua

function love.load ()
	Object = require 'classic'
	-- 우리는 더 이상 여기서 `Shape`를 불러올 필요가 없습니다.

	local Rectangle require 'rectangle'

	local Circle = require 'circle'

	r1 = Rectangle(100, 100, 200, 50)

	r2 = Circle(350, 80, 40)
end
```

`Object = require 'classic'`를 `shape.lua`로 이동하여 로컬 변수로 만들 수도 있습니다. `r1`과 `r2`도 로컬 변수로 만들 수 있지만 `love.load` 외부에서 만들어야 합니다.

```lua
--! file: main.lua

-- 이 변수들을 r1과 r2로 만들면 더 이상 전역 변수가 아닙니다.
local r1, r2

function love.load ()
	Object = require 'classic'

	local Rectangle require 'rectangle'

	local Circle = require 'circle'

	r1 = Rectangle(100, 100, 200, 50)

	r2 = Circle(350, 80, 40)
end
```

같은 결과지만 너무 많은 작업이 필요해 보이는데, 코드는 훨씬 더 깔끔할 것입니다. 점수를 저장하는 전역 변수 `score`가 있는 게임을 만든다고 상상해 보세요. 그런데 이 게임은 자체 점수 시스템을 가진 미니게임도 있는데, 실수로 전역 변수 `score`의 값을 덮어 씌웠습니다. 그리고 왜 점수 시스템이 제대로 작동하지 않는지 혼란스러워 하게 되죠. 어쨌든 전역 변수는 사용할 수 있지만, 주의해서 사용하세요.

___

## 요약

클래스는 설계도와 같다. 우리는 하나의 클래스에서 여러 개의 객체를 만들 수 있다. 클래스를 흉내내기 위해 classic 라이브러리를 사용한다. `ClassName = Object:extend()`로 클래스를 만든다. `instanceName = ClassName()`로 클래스의 인스턴스를 만든다. 이는 `ClassName:new()` 함수를 호출할 것이다. 이를 *생성자*라고 한다. 클래스의 모든 함수는 매개 변수 `self`로 시작해야 함수를 호출할 때 인스턴스를 첫 번째 인자로 전달할 수 있다. `instanceName.Function(instanceName)`와 같은 코드를 우리는 콜론(:)을 사용하여 Lua가 이 작업을 수행하도록 할 수 있다.

`ExtensionName = ClassName:extend()`로 클래스를 확장할 수 있다. 이렇게 하면 `ExtensionName`은 `ClassName`을 편집하지 않고도 속성을 추가할 수 있는 `ClassName`의 복사본이 된다. `ExtensionName`에 이미 `ClassName`이 가지고 있는 함수를 제공하더라도 `ExtensionName.super.functionName(self)`으로 원래 함수를 호출할 수 있다.

지역 변수를 사용하면 코드를 훨씬 더 깨끗하고 쉽게 유지보수할 수 있다.
