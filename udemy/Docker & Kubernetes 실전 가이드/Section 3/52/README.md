# 52. 바인드 마운트 – 바로 가기(Shortcuts)

바인드 마운트 옵션에서 전체 경로를 복사하여 사용하고 싶지 않은 경우, 바로 가기를 사용할 수 있다.

`macOS / Linux: -v $(pwd):/app`
(+ "$(pwd):/app" 이게 맞는 것 같다.)

`Windows: -v "%cd%":/app`
