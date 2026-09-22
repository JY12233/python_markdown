# 简介

1. 来源

   在 2000 年由 Pete Shinners 开发，专门用于开发和设计 2D 电子游戏。

2. 特点

   - 高可移植性。
   - 开源、免费。
   - 支持多种操作系统。
   - 专门用于多媒体应用的开发，包含对图像、声音、视频、事件、碰撞等的支持。

3. 底层

   在 SDL （Simple DirectMedia Layer）基础上编写。SDL 是一套用 C 语言实现的跨平台多媒体开发库，被广泛使用于游戏、模拟器、播放器等的开发。

# 常用子模块

| 子模块               | 说明                                                       |
| -------------------- | ---------------------------------------------------------- |
| `pygame.cdrom`       | 访问光驱                                                   |
| `pygame.cursors`     | 加载光标图像，包括标准指针                                 |
| `pygame.display`     | 控制显示窗口或屏幕                                         |
| `pygame.locals`      | 集中定义并导出 Pygame 中常用的常量（如事件类型、按键码等） |
| `pygame.draw`        | 绘制简单的数学形状                                         |
| `pygame.event`       | 管理事件                                                   |
| `pygame.font`        | 创建和渲染 TrueType 字体                                   |
| `pygame.image`       | 加载和存储图片                                             |
| `pygame.key`         | 读取键盘按键                                               |
| `pygame.mouse`       | 鼠标                                                       |
| `pygame.surface`     | 管理图像                                                   |
| `pygame.rect`        | 管理矩形区域                                               |
| `pygame.sprite`      | 管理移动图像（精灵序列图）                                 |
| `pygame.time`        | 管理时间和帧信息                                           |
| `pygame.math`        | 管理向量                                                   |
| `pygame.transform`   | 缩放、旋转和翻转图像                                       |
| `pygame.mixer_music` | 管理音乐                                                   |
| `pygame.joystick`    | 管理操纵杆设备（游戏手柄等）                               |
| `pygame.overlay`     | 访问高级视频叠加                                           |
| `pygame.mixer`       | 管理声音                                                   |

# 第一个pygame程序

```python
import pygame, sys

# 初始化
pygame.init()

# 创建pygame窗口
screen = pygame.display.set_mode((500, 200), 0, 32)

# 创建字体对象
font = pygame.font.SysFont(None, 60)

# 创建文本图像
mingri = font.render("Hello Pygame World", True, (255, 255, 255))

# 创建主循环，不断刷新页面
while True:
    screen.fill((25, 102, 173)) # 清屏
    screen.blit(mingri, (50, 80)) # 绘制
    for event in pygame.event.get(): # 事件获取
        if event.type == pygame.QUIT: # 判断退出事件
            pygame.quit() # 退出游戏
            sys.exit() 3 退出程序
    pygame.display.update() # 刷新
```