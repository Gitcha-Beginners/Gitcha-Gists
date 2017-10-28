
# 1 window zsh 쉘 사용하는 방법

### 1-1 window bash 쉘 활성화 하기
    
[윈도우 bash 쉘 활성화 하는 방법](http://blog.gaerae.com/2016/08/install-bash-windows-10.html)

### 1-2 window zsh 쉘 설치법

- window cmd 창 접속 
- cmd 창에서 @bash 명령어로 bash로 전환
```bash
bash
```
    
아래 명령어로 curl, zsh, Oh My Zsh를 설치한다.

```bash

sudo apt-get install curl

sudo apt-get install zsh

sudo curl -L https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh | bash

```

### 1-3 zsh 활성화 방법

vi ~/.zshrc




### 1-4 테마 적용 

- .zshrc 테마 설정 변경
```bash
$vi ~/.zshrc
```
안에 테마 설정을 다음과 변경한다.

ZSH_THEME="agnoster" 


### 1-4 폰트 및 CMDER 설치 

폰트가 깨지므로 아래의 링크에서 폰트를 받는다.
[폰트설치링크](https://github.com/powerline/fonts)
해당 폰트 파일을 이 경로로 이동한다. 
C:\Windows\Fonts


우분트일 경우에는 다음과 같이 설치한다.
다음 저장소를 clone 하고 해당 프로젝트로 가서 install.sh 실행한다.
```bash
git clone https://github.com/powerline/fonts.git

cd fonts

./install.sh

```



Window 경우에는 shell 경우에는 zsh 을 테마를 적용하기 어려우므로 아래의
터미널 프로그램을 사용한다.

[cmder 설치 링크 ](http://cmder.net/)