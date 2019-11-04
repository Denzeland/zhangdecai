---
title: python实现树莓派中转扫码枪
date: 2019-11-01 16:09:43
tags:
- python
---

### python实现树莓派中转扫码枪

最近在做一个项目涉及到利用树莓派来作为媒介， 读入扫码枪的数据并处理数据，处理完后再输出数据到和电脑的问题，想了一下如果通过网络通信的话，涉及到寻找ip比较复杂，所以选择了通过串口来和电脑通信，最后实现了用python操作串口收发数据及整个项目的需求，下面记录下扫码枪数据的读取和串口操作的过程。

### 使用串口前树莓派的配置

要使用树莓派的串口来为我们自由使用收发数据，需要改两个配置，为什么要改这两个配置，{% link 详细信息可以参考 https://blog.csdn.net/Tang_Chuanlin/article/details/92847343 %}
：

1. 将树莓派3的硬件串口与mini串口默认映射对换

执行下列命令修改配置文件：

`sudo nano /boot/config.txt`

在配置文件最后一行添加

`dtoverlay=pi3-miniuart-bt`

2. 禁用串口的控制台功能

执行下面两条命令：
`sudo systemctl stop serial-getty@ttyAMA0.service`
`sudo systemctl disable serial-getty@ttyAMA0.service`
再修改配置文件：
`sudo nano /boot/cmdline.txt`

删除语句 console=serial0,115200，最后重启树莓派，让修改生效。

### python接收扫码枪的数据

扫码枪本质也是通过模拟建模来输入数据的，一般输入的字符间隔时间都在80毫秒以内， 根据这个原理， 使用keboard库来处理扫码枪扫描的数据。

首先安装keyboard包：

`pip install keyboard`

下面贴出代码：

{% codeblock lang:python %}
import keyboard, threading, time, serial
from datetime import datetime

lock = threading.Lock()

ser = serial.Serial('/dev/ttyAMA0', 115200)

code = ''  # 扫码枪扫出的内容
next_code = ''
last_time = None
last_code = ''
next_time = None

# 处理按键KEY_DOWN事件，拼出code
def handle_press(event):
    lock.acquire()
    global code, next_code, last_time, last_code, next_time
    next_time = datetime.now().timestamp()
    next_code = event.name
    is_modifier = keyboard.is_modifier(next_code)
    if last_time == None and last_code == '':
        if is_modifier:
            code = ''
        else:
            code = next_code
    if last_time != None and last_code != '' and (next_time - last_time) <= 0.08 and is_modifier != True:
        code += next_code
    if last_time != None and last_code != '' and (next_time - last_time) > 0.08:
        if is_modifier:
            code = ''
        else:
            code = next_code
    last_code = next_code
    last_time = next_time
    lock.release()

if __name__ == '__main__':
    keyboard.on_press(handle_press, suppress=False)
    while True:
        time.sleep(0.08)
        # 在这里可以对拿到的数据做处理，这里是简单的把数据通过串口发送出去
        ser.write(code)
        code = ''
{% endcodeblock %}

### 电脑端通过串口接收数据并处理

电脑连接上USB转TTL线，然后可以运行下面的代码来接收数据：

{% codeblock lang:python %}
import serial, time

ser = serial.Serial('COM3', 115200)

try:
    while True:
        # size的值可能每次循环的时候会变化
        size = ser.inWaiting()               # 获得缓冲区字符
        if size != 0:
            # 如果确定要读取数据的规则，read函数参数可以是一个定值
            response = ser.read(size=13)        # 读取内容并显示
            # 这里可以进行具体的操作，比如模拟键盘输出     
            ser.flushInput()                 # 清空接收缓存区
            # time.sleep(0.1)                  # 软件延时
except KeyboardInterrupt:
    ser.close()
{% endcodeblock %}