# Chapter 1 - 프로그램 설치

___

*2장과 3장은 어떠한 설치 없이도 진행할 수 있습니다. 만약 소프트웨어를 설치하고 싶으시지 않다면 [repl.it](https://repl.it/languages/lua)로 대체할 수 있습니다. 그러나 아래의 **몇 가지 더** 단락을 반드시 읽어보세요.*

___

## LÖVE 프레임워크

[love2d.org](https://www.love2d.org/)에 접속합니다.

32비트 또는 64비트 **설치 프로그램**을 다운로드 받아야 합니다. 이는 시스템의 종류에 따라 달라질 수 있습니다. 잘 모르겠으면 64비트로 다운로드합니다.
> 번역자: 12.0부터는 32비트 윈도우에 대한 지원이 종료되었습니다. 한글판의 내용은 특정 버전을 기준으로 하지는 않으나 미래를 위해 64비트를 기본으로 했습니다.

![](/images/book/1/download_love.png)

설치 프로그램을 실행합니다. *Next*를 클릭합니다. *I agree*를 클릭합니다. 이제 프레임워크가 설치될 위치를 정할 수 있습니다. 설치 위치는 크게 중요하지 않지만 잠시 후 필요하므로 위치를 기억하세요. 이 폴더를 *설치 폴더*라고 합니다.

저자(Sheepolution)의 경우에는 `C:/Program Files/LOVE`였습니다.

*Next*를 클릭하고, *Install*를 클릭합니다.

프레임워크의 설치가 완료되면 *Finish*를 클릭하여 완료합니다.

___

## 편집기

이제 편집기를 설치해야 합니다. 이 자습서에서는 ZeroBrane Studio를 사용할 것입니다.

**Visual Studio Code**(비주얼 스튜디오 코드)를 사용하고자 하는 경우, [부록](vscode)을 참고하여 VSCode에서 Love2D를 실행하는 방법을 찾을 수 있습니다.

[studio.zerobrane.com](https://studio.zerobrane.com/)에 접속하여 "Download"를 클릭합니다.

![](/images/book/1/download_brane.png)

ZeroBrane Studio에 후원할 수 있는 옵션이 있습니다. 후원을 원하지 않으면 "Take me to the download page this time"을 누르세요.

설치 프로그램을 열고 원하는 폴더에 편집기를 설치합니다.

![](/images/book/1/install_brane.png)

설치가 완료되면 편집기를 엽니다.

이제 프로젝트 폴더를 만들어야 합니다. 파일 탐색기를 열어 원하는 곳에 원하는 이름으로 폴더를 만드세요. 편집기에서 `Select Project Folder` 아이콘을 누르시고 방금 만든 폴더를 선택하세요.

![](/images/book/1/project_brane.png)

편집기의 메뉴에서 `File` -> `New`를 누르거나, `Ctrl + N` 단축키를 이용하여 새로운 파일을 생성합니다.

파일 내에 다음 코드를 작성해봅시다:

```lua
function love.draw ()
	love.graphics.print("Hello World!", 100, 100)
end
```

메뉴에서 `File` -> `Save`를 누르거나, `Ctrl + S` 단축키를 이용하여 `main.lua`라는 이름으로 파일을 저장합니다.

메뉴에서 `Project` -> `Lua Interpreter`를 누르고 `LÖVE`를 선택합니다.

이제 `F6` 키를 누르면 "Hello World!"라는 문구와 함께 창이 열릴 것입니다. 축하드립니다, LÖVE를 배우실 준비가 되었습니다. 이제 제가 *게임 실행* 또는 *코드 실행*을 지시할 때마다, `F6` 키를 눌러 LÖVE를 실행하는 겁니다.

만약 아무 일도 일어나지 않고 *Can't find love2d executable in any of the following folders*라는 오류가 나타난다면 편집기가 인식할 수 없는 곳에 프레임워크를 설치한 것입니다. 메뉴에서 `Edit` -> `Preferrences` -> `Settings: User`로 들어갑니다. 그리고 다음 내용을 입력합니다.

```lua
path.love2d = 'C:/path/to/love.exe'
```

그리고 `'C:/path/to/love.exe'`를 *설치 폴더*의 경로로 바꿉니다. 반드시 슬래시(/)를 사용하세요.

___

## 몇 가지 더

혹시 예제 코드를 *Ctrl CV* 하셨나요? 제가 보여드리는 코드를 직접 입력하는 것을 권장합니다. 일거리가 더 늘어나는 것처럼 보일지는 몰라도, 배운 것을 기억하는데 도움이 될 것입니다.

직접 입력할 필요가 없는 단 한 가지는 주석입니다.

```lua
-- 이 줄은 주석이지, 코드가 아닙니다.
-- 다음 줄부터는 코드입니다.

print(123)

--Output: 123
```

뺄셈 기호(-) 두개로 시작하는 모든 줄은 주석입니다. 컴퓨터는 이를 무시하는데, 이는 오류 없이 원하는 모든 것을 입력할 수 있습니다. 저의 경우는 코드를 더 잘 설명하기 위해 주석을 사용합니다. 따라서 코드를 입력할때는 주석까지 따라 치지 않아도 됩니다.

`print`를 사용하면 출력 콘솔로 정보를 전송할 수 있습니다. 편집기 하단에 있는 박스입니다. **게임을 종료하면**, "123"이라고 출력되어야 합니다. 저는 예상되는 출력을 나타내기 위해 `--Output:`이라는 주석을 코드에 추가했습니다. `love.graphics.print`와 혼동하지 마세요.

`main.lua` 파일의 첫 줄에 다음 코드를 입력하면 출력 결과가 바로 나타납니다. 이것이 어떻게 작동하는지는 중요하진 않지만 원래는 출력 결과가 나타나기 전에 프로그램이 종료되기를 기다려야 했었습니다.

```lua
io.stdout:setvbuf("no")
```

___

## 다른 편집기들

* [Visual Studio Code](https://code.visualstudio.com/)
* [Sublime Text](https://love2d.org/wiki/Sublime_Text)
* [Atom](https://love2d.org/wiki/Atom)
