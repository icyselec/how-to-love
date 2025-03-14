# 8장 - 객체
이전 장에서 우리는 테이블을 배열처럼 사용했지만, 다른 방법으로 값을 저장할 수 있습니다. 그건 바로 "문자열"입니다.

```lua
function love.load ()
	-- rect는 rectangle의 줄임말임.
	rect = {}
	rect["width"] = 100
end
```

이 경우 `"width"`은 우리가 *키(key)* 또는 *속성(property)*이라고 부르는 것입니다. 따라서 이제 테이블 `rectangle`은 100의 값을 가진 속성 `"width"`을 갖게 됩니다. 속성을 만들고 싶을 때마다 문자열을 사용할 필요는 없습니다. 점(.)은 `table_name["property_name"]`의 줄임말입니다.

```lua
function love.load ()
	rect = {}
	-- 아래 두 줄은 동일하다.
	rect["width"] = 100
	rect.width = 100
end
```

그럼 몇가지 속성들을 추가해봅시다.

```lua
function love.load ()
	rect = {}
	rect.x = 100
	rect.y = 100
	rect.width = 70
	rect.height = 90
end
```

이제 속성이 생겼으니 직사각형 그리기를 시작할 수 있습니다.

```lua
function love.draw ()
	love.graphics.rectangle("line", rect.x, rect.y, rect.width, rect.height)
end
```

그리고 이걸 움직이게 해봅시다!

```lua
function love.load ()
	rect = {}
	rect.x = 100
	rect.y = 100
	rect.width = 70
	rect.height = 90

	--Add a speed property
	rect.speed = 100
end

function love.update (dt)
	-- Increase the value of x. Don't forget to use delta time.
	rect.x = rect.x + rect.speed * dt
end
```

이제 다시 움직이는 직사각형이 생겼지만 테이블의 힘을 보여주기 위해 여러 개의 움직이는 직사각형을 만들고 싶습니다. 이를 위해 테이블을 목록으로 사용할 것입니다. 직사각형 목록을 만들겠습니다. `love.load` 내부의 코드를 새 함수로 이동하고 `love.load`에 새 테이블을 만듭니다.

```lua
function love.load ()
	-- Remember: camelCasing!
	listOfRectangles = {}
end

function createRect ()
	rect = {}
	rect.x = 100
	rect.y = 100
	rect.width = 70
	rect.height = 90
	rect.speed = 100

	-- 새로운 rectangle을 리스트에 넣어주세요
	table.insert(listOfRectangles, rect)
end
```

이제 `createRect`를 호출할 때마다 새로운 직사각형 객체가 리스트에 추가될 것입니다. 네 맞습니다, 테이블 안에 테이블이 있는 것입니다. 스페이스 키를 누를 때마다, `createRect`가 호출되도록 작성해봅시다. 콜백 함수인 `love.keypressed`로 이 일을 해낼 수 있습니다.

```lua
function love.keypressed (key)
	-- 두 개의 등호(==)가 비교에 사용된다는 것을 기억하세요!
	if key == "space" then
		createRect()
	end
end
```

키를 누를 때마다 LÖVE는 love.keypressed를 호출하고 누른 키를 인수로 전달합니다. 누른 키가 `"space"`인 경우, `createRect`를 호출합니다.

마지막으로 해야할 일은 update와 draw 함수를 변경하는 것입니다. 직사각형 목록에 대해서 순회하도록 작성해야 합니다.

```
function love.update (dt)
	for i, v in ipairs(listOfRectangles) do
		v.x = v.x + v.speed * dt
	end
end

function love.draw ()
	for i, v in ipairs(listOfRectangles) do
		love.graphics.rectangle("line", v.x, v.y, v.width, v.height)
	end
end
```

그리고 이제 게임을 실행하면, 움직이는 직사각형이 스페이스 키를 누를 때마다 나타날 것입니다.

![](/images/book/8/moving_rectangles.gif)

___

## 조금 더?

다소 짧은 시간 동안 많은 코드를 설명했습니다. 꽤 혼란스러울 수도 있으니 전체 코드를 다시 한 번 더 살펴봅시다.

우리는 `love.load`에서 `listOfRectangles`라는 테이블을 생성했습니다.

스페이스 키를 누를 때, LÖVE는 `love.keypressed`를 호출하고, 이 콜백 함수 안에서는 누른 키가 `"space"`인지 확인합니다. 스페이스 키를 누른게 맞다면 `createRect`를 호출합니다.

`createRect`에서 새로운 테이블을 생성합니다. 이 테이블에 `x`, `y`와 같은 속성을 부여하고, 테이블을 `listOfRectangles`에 저장합니다.

`love.update`와 `love.draw`에서 직사각형들을 순회하여 각 직사각형의 좌표를 갱신하고 화면에 표시합니다.

___

## 함수

객체에도 함수가 있을 수 있습니다. 객체에 함수를 다음과 같이 작성할 수 있습니다.

```lua
tableName.functionName = function ()

end

-- 좀 더 일반적인 방법으로
function tableName.functionName ()

end
```

___

## 요약
우리는 숫자키 말고도 문자열키를 사용하여 테이블에 값을 저장할 수 있다. 이러한 종류의 테이블을 우리는 객체라고 부른다. 객체가 있으면 많은 변수를 작성하지 않아도 된다.

