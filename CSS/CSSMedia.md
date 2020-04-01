### 媒体查询

#### CSS2.0 media属性

**screen     计算机屏幕（默认值）  
tty      电传打字机以及使用等宽字符网格的类似媒介
tv       电视类型设备（低分辨率、有限的屏幕翻滚能力）
projection   放映机
handheld    手持设备（小屏幕、有限的带宽）
print     打印预览模式 / 打印页
braille    盲人用点字法反馈设备
aural     语音合成器
all      适合所有设备**

CSS2.0用在style,link标签中

#### 媒体属性

**width | min-width | max-width
   height | min-height | max-height
   device-width | min-device-width | max-device-width
   device-height | min-device-height | max-device-height
   aspect-ratio | min-aspect-ratio | max-aspect-ratio
   device-aspect-ratio | min-device-aspect-ratio | max-device-aspect-ratio
   color | min-color | max-color 颜色，指定输出设备每个像素单元的比特值
   color-index | min-color-index | max-color-index
   monochrome | min-monochrome | max-monochrome
   resolution | min-resolution | max-resolution
   scan | grid**

color-index

颜色索引指定了输出设备中颜色查询表中的条目数量，如果没有使用颜色查询表，则值等于0。向所有使用至少256个索引颜色的设备应用样式表

宽高比aspect-ratio1/1

设备宽高比device-aspect-ratio1/1

网格grid网格设备还是位图设备