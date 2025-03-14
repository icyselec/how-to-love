# 4장 - LÖVE

## LÖVE가 뭐죠?
뢰브(LÖVE, 예전에는 Love2D라고 불렸습니다.)는 이전에 프레임워크라고 불렀던 그것입니다. 간단히 말해, 게임을 쉽게 프로그래밍할 수 있게 해주는 도구입니다.

LÖVE는 *C++*과 *OpenGL*로 짜여져 있고, 둘 다 진짜 겁나 어렵습니다. LÖVE의 소스 코드는 매우 복잡하지만 그 존재의 이유는 바로, 게임을 쉽게 만들기 위해서 입니다.

예를 들어, `love.graphics.ellipse`로 타원을 그릴 수 있는데, 그 이면에는 다음과 같은 소스 코드가 있습니다.

```cpp
void Graphics::ellipse(DrawMode mode, float x, float y, float a, float b, int points)
{
	float two_pi = static_cast<float>(LOVE_M_PI * 2);
	if (points <= 0) points = 1;
	float angle_shift = (two_pi / points);
	float phi = .0f;

	float *coords = new float[2 * (points + 1)];
	for (int i = 0; i < points; ++i, phi += angle_shift)
	{
		coords[2*i+0] = x + a * cosf(phi);
		coords[2*i+1] = y + b * sinf(phi);
	}

	coords[2*points+0] = coords[0];
	coords[2*points+1] = coords[1];

	polygon(mode, coords, (points + 1) * 2);

	delete[] coords;
}
```

위의 코드를 전혀 이해하지 못할 수도 있고(저도 거의 이해하지 못합니다), 그래서 우리가 LÖVE를 사용하는 겁니다. 게임을 프로그래밍할 때 어려운 부분을 처리해주고, 재미있는 부분은 우리에게 남겨줍니다.

___

## 루아

루아(Lua)는 LÖVE가 사용하는 프로그래밍 언어입니다. 루아는 그 자체로 프로그래밍 언어이며, LÖVE를 위해서, LÖVE에 의해서 만들어지지 않았습니다. LÖVE의 창시자들은 단순히 루아를 그들의 프레임워크를 위한 언어로 선택했습니다.

그럼, 우리가 작성하는 코드 중에 LÖVE는 어떤 부분이고 루아는 무엇일까요? 아주 간단합니다. `love`로 시작하는 모든 것이 LÖVE 프레임워크의 일부이고, 나머지는 전부 루아입니다.

다음의 함수들은 LÖVE 프레임워크의 일부 기능입니다:

```lua
love.graphics.circle("fill", 10, 10, 100, 25)
love.graphics.rectangle("line", 200, 30, 120, 100)
```

그리고 이건 그냥 루아입니다:

```lua
function test (a, b)
	return a + b
end
print(test(10, 20))
--Output: 30
```

아직 이해가 가지 않을 수도 있지만 괜찮습니다. 지금 가장 중요한 것은 아니니까요.

___

## LÖVE는 어떻게 작동하는 거죠?

___


*이 시점부터는 LÖVE가 필요합니다. 설치되어 있지 않다면, [1장](1)으로 되돌아가세요.*

___

LÖVE는 3개의 함수를 호출합니다. 먼저 `love.load`를 호출합니다. 여기서는 변수를 만듭니다.

그런 다음 `love.update`와 `love.draw`를 반복하여 호출합니다.

그러므로, 처음에는 `love.load`를 호출하고, `love.update`와 `love.draw`를 반복해서 호출합니다.


눈에 보이지 않는 부분에서는 LÖVE가 이 함수들을 호출하고, 우리는 이것을 작성하고 코드로 채웁니다. 이를 *콜백 함수*라고 부릅니다.

LÖVE는 `love.graphics`, `love.audio`, `love.filesysteme`등의 *모듈*로 만들어집니다. 모듈이 15개 정도 되는데, 각 모듈은 하나의 일을 잘합니다. 컴퓨터로 그림을 그리는 모든 것은 `love.graphics`로 합니다. 그리고 컴퓨터로 음악 재생은 `love.audio`로 합니다.

지금은 `love.graphics` 모듈에 초점을 맞추겠습니다.

LÖVE는 모든 함수에 대한 설명이 있는 [위키](https://www.love2d.org/wiki/Main_Page)를 가지고 있습니다. 한번 사각형(rectangle)을 그려보고 싶다고 생각해보세요. 일단 위키에서 [`love.graphics`](https://www.love2d.org/wiki/love.graphics)로 이동합시다, 그리고 "rectangle"에 대해 검색해봅시다. 그곳에서는 [`love.graphics.rectangle`](https://www.love2d.org/wiki/love.graphics.rectangle)을 찾을 수 있었습니다.

[![](/images/book/4/rectangle.png "love2d.org/wiki/love.graphics.rectangle")](https://www.love2d.org/wiki/love.graphics.rectangle)

이 페이지에서는 함수가 하는 일과 어떤 인수가 필요한지를 말해줍니다. 첫 번째 인수는 `mode`이고, `DrawMode` 형이여야 합니다. [`DrawMode`](https://www.love2d.org/wiki/DrawMode)를 클릭하여 더 자세한 내용을 확인할 수 있습니다.

[![](/images/book/4/drawmode.png "love2d.org/wiki/DrawMode")](https://www.love2d.org/wiki/DrawMode)

`DrawMode`는 문자열이고 `'fill'`이거나 `'line'` 둘 중에 하나겠군요. 이는 도형의 속이 비워질지, 아니면 속이 채워질지를 제어합니다.

나머지 인수들은 전부 숫자입니다.

따라서, 속이 채워진 사각형을 그리려면 다음과 같이 하면 됩니다.
```lua
function love.draw ()
	love.graphics.rectangle('fill', 100, 200, 50, 80)
end
```

이제 게임을 실행하면, 속이 채워진 사각형이 표시됩니다.

![](/images/book/4/example_rectangle.png)

LÖVE가 제공하는 기능은 우리가 흔히 [API](https://ko.wikipedia.org/wiki/API)라고 부르는 것입니다. 위키백과 문서를 읽고 API가 정확히 무엇인지 알 수 있지만, 지금 같은 상황에서는 단순히 LÖVE가 제공하는 기능을 의미합니다.

___

## 요약

LÖVE는 게임을 쉽게 만들 수 있는 도구이다. LÖVE는 루아라는 프로그래밍 언어를 사용한다. `love`로 시작하는 모든 것은 LÖVE 프레임워크의 일부이다. LÖVE 위키는 LÖVE 사용법에 대해 알아야 할 모든 것을 알려준다.
