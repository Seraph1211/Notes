PPT结构变更



积分功能

难点在哪呢：



流程：

用户组队 => 打卡到队伍 => 导出积分记录 => 分配积分 => 核销积分 => 如何保证积分的核销



**扫码头如何识别二维码是核销码还是支付码 ？**



**积分下发时出错怎么办？**



积分核销 和 积分分配 



如何保证积分下发不会出错？





接入移动网关的难点：如何保证其可测试性？ => 配置路由，转发请求 => 如何配置路由策略？ => 研究测试环境转发规则后得知配到哪台机器 => 



遇到问题请求到同一个人的私有容器 => 





为什么无法测试 => 如何才能进行测试？



图： 移动网关会把请求转发到IDC机器下（接入域名所对应的机器下）





述职：你的思考过程是怎样的





难点分析部分PPT内容:

1、点明难点是什么 => 如何保证可测试性

2、为什么无法保证可测试性 => 原因是无法绕过移动网关访问TGW，而移动网关一般情况下会请求到woa域名所映射的机器上

3、解决方法 => 配置路由转发策略，将经过移动网关的请求转发到测试环境机器上 => 智能网关的智能路由 => 通过配置只能路由，我们可以把对某个域名某一条路径的访问转发到特定机器的某条路径下（画图）

4、现在已经知道可以通过智能路由把经过移动网关的请求转发到特定的机器上 => 那么我们应该把请求转发到哪台机器上呢？ => 分析993环境Nginx架构图 => 对比连到TGW和Nginx转发机器上的区别 => 单点故障 => TGW



















