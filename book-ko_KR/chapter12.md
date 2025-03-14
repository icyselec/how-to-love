# 12장 - 이미지

Creating and using images is a very easy task in LÖVE. First we need an image. I'm going to use this image:  
LÖVE에서 이미지를 다루는 것은 매우 쉬운 작업입니다. 먼저 이미지 파일이 필요합니다. 이 그림을 사용하겠습니다:

![](/images/book/12/sheep.png)

Of course, you can use any image you like, as long as it's of the type *.png*. Make sure the image is in the same folder as your `main.lua`.  
물론 *.png* 유형의 이미지라면 원하는 것을 사용할 수 있습니다. 이미지 파일이 `main.lua`와 동일한 폴더에 있는지 확인하세요.

First we need to load the image, and store it in a variable. For this we will use `love.graphics.newImage(path)`. Pass the name of the image as a string as first argument. So if you have a  
먼저 이미지를 불러오고 변수에 저장해야 합니다. 이를 위해 `love.graphics.newImage(path)`를 사용합니다. 첫 번째 인자로 그림 파일 이름을 문자열로 전달합니다. 따라서,

```lua
function love.load ()
	myImage = love.graphics.newImage("sheep.png")
end
```
You can also put your image in a subdirectory, but in that case make sure to include the whole path.  
이미지를 하위 디렉토리에 넣을 수도 있지만, 이 경우 전체 경로를 반드시 포함해야 합니다.

```lua
myImage = love.graphics.newImage("path/to/sheep.png")
```

Now our image is stored inside `myImage`. We can use `love.graphics.draw` to draw our image.  
이제 우리의 이미지는 `myImage` 안에 저장됩니다. `love.graphics.draw`를 사용하여 이미지를 그릴 수 있습니다.

```lua
function love.draw ()
	love.graphics.draw(myImage, 100, 100)
end
```

And that is how you draw an image.  
이것이 바로 이미지를 그리는 방법입니다.

___

## .draw()의 매개 변수들

Let's take a look at all the arguments of `love.graphics.draw()`. All arguments besides the image are optional.  
`love.graphics.draw()`의 모든 인자를 살펴보겠습니다. 이미지 외의 모든 인자는 선택적으로 전달할 수 있습니다.

**image**

The image you want to draw.  
당신이 그리고 싶은 이미지.

**x**, **y**

The horizontal and vertical position of where you want to draw the image.  
이미지를 그릴 X와 Y 좌표.

**r**

The **r**otation (or angle). All angles in LÖVE are radians. I'll explain more about radians in another chapter.  
회전을 뜻하는 rotation의 첫 번째 알파벳. LÖVE의 모든 각도는 라디안입니다. 라디안에 대해서는 다른 장에서 더 자세히 설명하겠습니다.

**sx**, **sy**

The **x**-**s**cale and **y**-**s**cale. If you want to make your image twice as big you do  
이미지의 가로 및 세로 비율. 이미지를 두 배로 늘리고 싶다면 다음과 같이 하세요

`love.graphics.draw(myImage, 100, 100, 0, 2, 2)`

 You can also use this to mirror an image with  
 또한, 다음과 같이 하여 이미지의 방향을 뒤집을 수 있습니다.

`love.graphics.draw(myImage, 100, 100, 0, -1, 1)`

**ox**, **oy**

The **x**-**o**rigin and **y**-**o**rigin of the image.  
이미지의 X 원점과 Y 원점.

By default, all the scaling and rotating is based on the top-left of the image.  
기본적으로, 모든 크기 조정과 회전은 이미지의 좌측 상단을 기준으로 합니다.

![](/images/book/12/origin1.png)

This is based on the *origin* of the image. If we want to scale the image from the center, we'll have to put the origin in the center of the image.  
이것은 이미지의 *원점*을 기준으로 합니다. 이미지를 중앙에서 확대하려면 이미지의 중앙에 원점을 두어야 합니다.

`love.graphics.draw(myImage, 100, 100, 0, 2, 2, 39, 50)`

![](/images/book/12/origin2.png)

**kx**, **ky**

These are for shearing (which doesn't have a **k** at all so I'm not sure what to make of it).  
이것들은 전단용입니다 (**k**가 전혀 없어서 어떻게 해야 할지 잘 모르겠어요).

With it you can skew images.  
이미지를 비스듬하게 만듭니다.
> 번역자: *전단변환행렬*을 참조하세요. 기울임체를 생각하면 이해하기 쉽습니다.

![](/images/book/12/shear.png)

`love.graphics.print`, which we used before to draw text, has these same arguments.  
우리가 이전에 문장을 출력할 때 사용했던 `love.graphics.print`도 같은 인자를 가지고 있습니다.

x, y, r, sx, sy, ox, oy, kx, ky 

Again, all these arguments except **image** can be left out. We call these *optional arguments*.  
다시 말하지만, **image**를 제외한 모든 인자는 생략할 수 있습니다. 우리는 이를 *선택적 인자*라고 부릅니다.

You can learn about LÖVE functions by reading the [API documentation](https://love2d.org/wiki/love.graphics.draw).  
[API 문서](https://love2d.org/wiki/love.graphics.draw)를 읽으면 LÖVE 함수들에 대해 배울 수 있습니다.

___

## 이미지 객체

The image that `love.graphics.newImage` returns, is actually an object. An [Image](https://love2d.org/wiki/Image) object. It has functions that we can use to edit our image, or get data about it.  
`love.graphics.newImage`가 반환하는 이미지는 실제로 객체입니다. [Image](https://love2d.org/wiki/Image) 객체입니다. 이미지를 편집하거나 이에 대한 데이터를 얻는 데 사용할 수 있는 기능이 있습니다.

For example, we can use `:getWidth()` and `:getHeight()` to get the width and height of the image. We can use this to put the origin in the center of our image.  
예를 들어 `:getWidth()`와 `:getHight()`를 사용하여 이미지의 너비와 높이를 구할 수 있습니다. 이를 사용하여 이미지의 중앙에 원점을 배치할 수 있습니다.

```lua
function love.load ()
    myImage = love.graphics.newImage("sheep.png")
    width = myImage:getWidth()
    height = myImage:getHeight()
end

function love.draw ()
	love.graphics.draw(myImage, 100, 100, 0, 2, 2, width/2, height/2)
end
```

___

## 색상

You can change in what color the image is drawn with `love.graphics.setColor(r, g, b)`. It sets the color for everything you draw, so not only images but rectangles, shapes and lines as well. It uses the [RGB-system](https://en.wikipedia.org/wiki/RGB_color_model). Although this officially ranges from 0 to 255, with LÖVE it ranges from 0 to 1. So instead of (255, 200, 40) you would use (1, 0.78, 0.15). If you only know the color using the 0-255 range, you can calculate the number you want with `number/255`. There is also the fourth argument `a` which stands for alpha and decides the transparency of everything you draw. Don't forget to set the color back to white if you don't want the color for any other draw calls. You can set the background color with `love.graphics.setBackgroundColor(r, g, b)`. Since we only want to call it once, we can call it in `love.load`.  
`love.graphics.setColor(r, g, b)`로 이미지가 그려질 색상을 변경할 수 있습니다. 이미지뿐만 아니라 직사각형, 도형, 선 등 화면에 그릴 수 있는 모든 것은 [RGB](https://ko.wikipedia.org/wiki/RGB)를 사용하여 색상을 설정할 수 있습니다. 공식적으로는 0에서 255까지 범위를 사용하지만, LÖVE에서는 0에서 1까지의 범위를 사용합니다. 따라서 (255, 200, 40) 대신 (1, 0.78, 0.15)을 사용합니다. 0에서 255 범위로만 색상을 알고 있으면 그 값을 255로 나누어서 원하는 값을 계산할 수 있습니다. 또한 alpha의 약자로 모든 그림의 투명도를 결정하는 네 번째 인자 `a`도 있습니다. 다른 그리기 호출이 영향을 받는 것을 원하지 않는다면 색상을 다시 흰색으로 설정하는 것을 잊지 마세요. 배경 색상은 `love.graphics.setbackgroundColor(r, g, b)`로 설정할 수 있습니다. 한 번만 호출하고 싶으므로, `love.load`에서 호출해야 합니다.

```lua
function love.load ()
    myImage = love.graphics.newImage("sheep.png")
    love.graphics.setBackgroundColor(1, 1, 1)
end

function love.draw ()
	-- 또는, love.graphics.setColor(1, 0.78, 0.15, 0.5)
	love.graphics.setColor(255/255, 200/255, 40/255, 127/255)
	love.graphics.draw(myImage, 100, 100)
	-- 알파값에 대한 인자를 전달하지 않으면 자동으로 1이 설정됩니다.
	love.graphics.setColor(1, 1, 1)
	love.graphics.draw(myImage, 200, 100)
end
``` 

![](/images/book/12/color.png)

___

## 요약

We load an image with `myImage = love.graphics.newImage("path/to/image.png")`, which returns an Image object that we can store in a variable. We can pass this variable to `love.graphics.draw(myImage)` to draw the image. This function has optional arguments for the position, angle and scale of the image. An Image object has functions that you can use to get data about it. We can use `love.graphics.setColor(r, g, b)` to change in what color the image and everything else is drawn.  
`myImage = love.graphics.newImage("path/to/image.png")`과 같이 이미지를 불러오면, 변수에 저장할 수 있는 이미지 객체가 반환됩니다. 이 변수를 `love.graphics.draw(myImage)`로 전달하여 이미지를 그릴 수 있습니다. 이 함수에는 이미지의 위치, 각도 및 스케일에 대한 선택적 인수가 있습니다. 이미지 객체에는 해당 객체에 대한 데이터를 얻는 데 사용할 수 있는 함수가 있습니다. `love.graphics.setColor(r, g, b)`를 사용하여 이미지 및 기타 모든 것을 어떤 색상으로 그릴지 변경할 수 있습니다.
