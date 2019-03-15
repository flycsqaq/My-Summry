# 1. 盒模型
盒子是指由width / padding / border / margin 构成的布局最小单位
1. block / inline 代表 盒子 / 文本。display: css渲染文本与盒子的顺序以及padding,margin.行内元素是可以设置padding，以及行内的margin(margin-left/right)，它的高度与margin-top(bottom)无关
2. 但其他元素只能感知inline的文本(或者是父元素只记录行内元素的文本)
3. box-sizing: content / padding / border

# 2. flex布局
设置一个元素为flex项目，那么他同样成为一个 flex 容器，它的孩子(直接子节点)也表现为flexible box 
1. 主要模型 flex-flow / direction + wrap
2. 排列方式 align-items / justify-content
3. 内部元素大小 flex / grow + shrink + basis

# 3. 选择器
1. 伪元素 after before
2. 伪类 firet/last-child first/last-of-type hover active
3. 属性 *= / ^= / $= /  / |= / ~= / =
4. 重要性、选择器、顺序
5. 组合器 , > + ~

# 4. float / position
1. left / right / none
2. relative / absolute / fixed / sticky

# 5. 动画
1. transform 过渡
2. animation 动画名称 + 效果时间 + 速度曲线 + 延时 + 播放次数 + 是否反向播放
3. animation-play-state paused

# 6. 技巧
1. 伪元素背景
2. vh / vw
3. rem + em 控制字体大小
4. 使用选择器:root来控制字体弹性
5. normalize
