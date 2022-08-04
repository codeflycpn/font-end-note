# flex

## 概念

1. 容器和项目
2. 坐标轴
   1. 主轴
   2. 侧轴

## flex

### 一、容器的布局方式

flex-direction：设置主轴和侧轴

flex-wrap：项目是否换行，多行排列换行方向

justify-items：设置`主轴方向`上的排列方式：左、中、右、两端卡住、全等分、二分全等分

Align-content:设置`侧轴方向`

item-content：`多行排列`,设置交叉轴的对齐方式

### 二、尺寸、位置关系

order：设置主轴方向的排队号码，1，2，3，设置order值来进行排列

Flex-sk ring：收缩因子，当容器宽度不够时，需要进行收缩，然后按比例收缩

flex-grow：扩张因子，当容器宽度剩余时，进行比例分配

flex：1：剩余都给这个项目

flex-basis：取代主轴的宽高

Align-self:脱离统一样式的拘束，可以自定义位置