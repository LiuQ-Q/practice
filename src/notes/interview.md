# 面试准备

### 自我介绍

我叫刘琦,今年25岁,毕业于沈阳理工大学,机械自动化专业,因为个人对计算机非常感兴趣,就自学了web前端知识，决定长期从事此方面工作
我在去年找到了第一份前端的工作,公司前端主要用vue,期间我主要负责了两个项目：
第一个项目，需要我给一个APP追加一个答题的功能,当时考虑到答题系统的复用,就把它独立成了一个项目,主要功能包括转换数据，随机选题,收集答案,计算分数;
另一个项目,需要我给一套扫描系统重新做前端,功能完全一样只改变外观,主要功能包括,登录功能，管理员功能,用highcharts展示数据,文件上传下载,还有一些功能接口的调用,
虽然这家公司在去年年底以为资金问题倒闭了, 但这段工作让我收获很多，不单单是技术方面，还有自学能力，和同事的沟通能力，这都是我学到的，而且我认为很重要的。
虽然我非科班，经验一般，但是我觉得这不足以看出一个人未来的潜力，更不能看出一个人会对公司做出的贡献，对于工作,我会抱着认真负责的态度，我希望能接触并使用更多更深层次的前端技术,对于个人,我希望今后能在前端这方面有更深的研究,目标是将来能成为这方面的专家
以上是我的自我介绍，谢谢

### 工作中遇到的困难，怎么解决的
在扫描系统前端的项目中，文件下载，ie9不支持blob，改用创建a标签触发下载，返回的数据content-type无法触发下载，webpack转发，koa反向代理，改变content-type，触发下载，兼容ie9，成功