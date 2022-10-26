# autohotkey

```
#NoEnv  ; Recommended for performance and compatibility with future AutoHotkey releases.
; #Warn  ; Enable warnings to assist with detecting common errors.
SendMode Input  ; Recommended for new scripts due to its superior speed and reliability.
SetWorkingDir %A_ScriptDir%  ; Ensures a consistent starting directory.

#w::    ;;这里的 #->表示window键  w->表示字母w键                   
WinMinimize,A    ;;最小化当前窗口      
return     

!h:: Send {Left} 
return
!l:: Send {Right}
return
!j:: Send {Down}
return
!k:: Send {Up}
return 

; 上下左右滚轮
!u:: Send {WheelDown}
return 
!i:: Send {WheelUp}
return 
!y:: Send {Wheelleft}
return 
!o:: Send {WheelRight}
return 

; 鼠标左击
!-::  
SendEvent {Blind}{LButton down}
KeyWait RCtrl  
SendEvent {Blind}{LButton up}
return

; 鼠标右击
!=::  
SendEvent {Blind}{RButton down}
KeyWait AppsKey  
SendEvent {Blind}{RButton up}
return


; 删除 / 退格
!b:: Send {BackSpace}
return 
!m:: Send {NumpadAdd} 
return 
!n:: Send {NumpadSub}
return
!,:: Send {=}
return

; 其他快捷键
;!space:: {+}
;return

; 自用的按键映射
$CapsLock::
KeyWait, CapsLock
If (A_PriorKey="CapsLock")
SetCapsLockState, % GetKeyState("CapsLock","T") ? "Off":"On"
Return
#If, GetKeyState("CapsLock", "P")
k::Up
j::Down
h::Left
l::Right
1::Send #{1}
2::Send #{2}
3::Send #{3}
4::Send #{4}
5::Send #{5}
6::Send #{6}
7::Send #{7}
8::Send #{8}
9::Send #{9}
^k::Send +{Up}
^j::Send +{Down}
^h::Send +{Left}
^l::Send +{Right}
w::Send ^{Left}
e::Send ^{Right}
u::Send ^{z}
g::Send {Enter} 
z::Send ^{z}
c::Send ^{c}
v::Send ^{v}
s::Send ^{s}
x::Send ^{x}
#w::Send ^{w}
Enter::Send {Esc}
q::Send {Esc}
Space::Send {=}
,::Home
.::End
r::Send ^{/}
b::Send {BackSpace}
d::Send {Delete}
n::Send ^{Backspace}
m::Send ^{Delete}
```
