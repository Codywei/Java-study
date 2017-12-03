# Java Web

标签（空格分隔）： Java Web

---

<h2>1.servlet生命周期及各个方法</h2>

参考链接：http://www.cnblogs.com/xuekyo/archive/2013/02/24/2924072.html</br>

<h2>2.servlet中如何自定义filter</h2>

参考链接：http://www.cnblogs.com/javawebsoa/archive/2013/07/31/3228858.html</br>


<h2>3.JSP原理以及和servlet的区别</h2>

参考链接：http://blog.csdn.net/hanxuemin12345/article/details/23831645</br>
http://blog.csdn.net/gwblue/article/details/10363967</br>

<h2>4.JSP的动态include和静态include</h2>

(1)动态include用jsp:include动作实现，如<jsp:include page="abc.jsp" flush="true" />
，它总是会检查所含文件中的变化，适合用于包含动态页面，并且可以带参数。会先解析所要包含的页面，解析后和主页面合并一起显示，即先编译后包含。</br>
(2)静态include用include伪码实现，不会检查所含文件的变化，适用于包含静态页面，如<%@ include file="qq.htm" %>，不会提前解析所要包含的页面，先把要显示的页面包含进来，然后统一编译，即先包含后编译。</br>

<h2>5.MVC概念</h2>

参考链接：http://www.cnblogs.com/scwyh/articles/1436802.html</br>

<h2>6.Springmvc与Struts区别</h2>
  
参考链接：http://blog.csdn.net/chenleixing/article/details/44570681</br>


<h2>7.Hibernate/Ibatis两者的区别</h2>

参考链接:http://blog.csdn.net/firejuly/article/details/8190229</br>

<h2>8.Hibernate一级和二级缓存</h2>

参考链接：http://blog.csdn.net/windrui/article/details/23165845</br>

<h2>9.简述Hibernate常见优化策略</h2>

参考链接：http://blog.csdn.net/shimiso/article/details/8819114</br>

<h2>10.Springbean的加载过程(推荐看Spring的源码)</h2>

参考链接：http://geeekr.com/read-spring-source-1-how-to-load-bean</br>

<h2>11.Springbean的实例化(推荐看Spring的源码)</h2>

参考链接：http://geeekr.com/read-spring-source-two-beans-initialization/</br>

<h2>12.Spring如何实现AOP和IOC(推荐看Spring的源码)</h2>

参考链接：http://www.360doc.com/content/15/0116/21/12385684_441408260.shtml</br>

<h2>13.Springbean注入方式</h2>

参考链接：http://blessht.iteye.com/blog/1162131</br>

<h2>14.springmvc用过哪些注解</h2>

参考链接：http://aijuans.iteye.com/blog/2160141</br>



<h2>15.Restful好处</h2>
(1)客户-服务器：客户-服务器约束背后的原则是分离关注点。通过分离用户接口和数据存储这两个关注点，改善了用户接口跨多个平台的可移植性；同时通过简化服务器组件，改善了系统的可伸缩性。</br>
(2)无状态：通信在本质上是无状态的，改善了可见性、可靠性、可伸缩性。</br>
(3)缓存：改善了网络效率减少一系列交互的平均延迟时间，来提高效率、可伸缩性和用户可觉察的性能。</br>
(4)统一接口：REST架构风格区别于其他基于网络的架构风格的核心特征是，它强调组件之间要有一个统一的接口。</br>

<h2>16.Tomcat，Apache，JBoss的区别</h2>
Apache:HTTP服务器(WEB服务器)，类似IIS，可以用于建立虚拟站点，编译处理静态页面，可以支持SSL技术，支持多个虚拟主机等功能。</br></br>
Tomcat:Servlet容器，用于解析jsp，Servlet的Servlet容器，是高效，轻量级的容器。缺点是不支持EJB，只能用于java应用。</br>
Jboss:应用服务器，运行EJB的J2EE应用服务器，遵循J2EE规范，能够提供更多平台的支持和更多集成功能，如数据库连接，JCA等，其对Servlet的支持是通过集成其他Servlet容器来实现的，如tomcat和jetty。</br>