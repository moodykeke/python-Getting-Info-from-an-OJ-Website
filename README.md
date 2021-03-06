# python-Getting the Question Library of an OJ website
python抓取信息学奥赛数据，保存于excel中
## 一、踩点

url里传递数据“pid=”后面的数字就是题目的编号，从1000开始。

我们需要的数据是一道题目的“编号”、“题目名称”、“题目描述”、“输入”、“输出”、“输入样例”、“输出样例”、“提示”（部分题目中有这一项）。这个OJ中“来源”写的都是“NO”，对我们意义不大，就不抓他了。

这里想到需要注意的部分：

1. 可能会有图片。

2. 部分题目有```提示```这一块内容。

F12查看我们需要的数据的部分，写的不是很整齐，包含关系有点混乱。


## 二、抓取数据

这里用```requests.get```抓取网站。

复制题目的```url```，先随便写个合理的```pid```，到时候外层套个循环就能抓所有题目了。按照规范构造个```headers```模拟浏览器。


## 三、数据整理

我用的是lxml库处理抓下来的网页数据。

用xpath找我们想要的部分

但是后面的数据有点难下手，小标题包含在```h3```标签中，内容文字在```p```标签里，且他俩并列而非包含，但是像```pid=1000```的例子中看到的那样，后面输入样例的数据、输出样例的标题和数据跑到下一层包含中去了。

那么我们只能暂且不从逻辑关系上分析，把他们可能存在的标签位置全部抓取下来，再根据小标题文字内容进行整理。

比如，```题目描述```之后的内容都为题目描述的文字部分，直到出现```输入```。
  
用一个```tyb```作为标记，指示现在读取的是哪一块内容。

如果读到图片——也就是说内容不存在```text```属性，把图片保存起来，在文本位置加一段```图片+图片序号```作为标记，方便后期处理，把图片再加上去。

如果是文字的话，就把他加入到相应的字符串之中。

读到“来源”，说明前面的内容都已经处理完了，而我们又不需要记录来源，直接结束掉。

## 四、效果

抓取到的数据保存在excel中，图片保存在pic文件夹中。

## 五、保存数据

上一步已经成功按照内容类别，把需要的数据记录到各自的字符串中，保存数据就非常方便了，打印输出、保存到数据库都不是难事，我这里先保存在了excel中。
