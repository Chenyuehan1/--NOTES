# 网络是怎样连接的 Notes



## 1.1浏览器生成消息

### URL解析

URL: Uniform Resourse Locator, 统一资源定位符

HTTP：Hypertext Transfer Protocol，超文本传送协议

根据访问目标的不同，URL 的写法也会不同，浏览器生成请求的内容也会不一样。

URL的各种格式

<img src="https://raw.githubusercontent.com/Chenyuehan1/HowNetworksWork-NOTES/main/img/image-20220311195605471.png" alt="image-20220311195605471" style="zoom:80%;" />

浏览器的第一步工作就是对URL进行解析，Web浏览器解析URL的过程如下图

<img src="https://raw.githubusercontent.com/Chenyuehan1/HowNetworksWork-NOTES/main/img/image-20220311195834062.png" alt="image-20220311195834062" style="zoom:80%;" />

Web服务器文件树示例

<img src="https://raw.githubusercontent.com/Chenyuehan1/HowNetworksWork-NOTES/main/img/image-20220311195959060.png" alt="image-20220311195959060" style="zoom:80%;" />

省略文件名的情况：

1. http://www.lab.glasscom.com/dir/
   我们可以这样理解， 以“/” 结尾代表 /dir/ 后面本来应该有的文件名被省略了。 根据 URL 的规则，文件名可以像前面这样省略，此时服务器会默认访问 我们会在服务器上事先设置好的文件名省略时要访问的默认文件名。 这个设置根据服务器不同而不同， 大多数情况下是 index.html 或者 default.htm 之类的文件名。 因此， 像前面这样省略文件名时， 服务器就会访问 /dir/index.html或者 /dir/default.htm。
   还有一些 URL 是像下面这样只有 Web 服务器的域名的， 这也是一种省略了文件名的形式。
2. http://www.lab.glasscom.com/
   这个 URL 也是以“/” 结尾的， 也就是说它表示访问一个名叫“/” 的目录 。 而且， 由于省略了文件名， 所以结果就是访问 /index.html 或者/default.htm 这样的文件了。
3. http://www.lab.glasscom.com
   这次连结尾的“/” 都省略了。 像这样连目录名都省略时， 真不知道到底在请求哪个文件了， 实在有些过分。 不过， 这种写法也是允许的。 当没有路径名时， 就代表访问根目录下事先设置的默认文件 ， 也就是 /index.html 或者 /default.htm 这些文件， 这样就不会发生混乱了。
4. http://www.lab.glasscom.com/whatisthis
   前面这个例子中， 由于末尾没有“/”， 所以 whatisthis 应该理 解为文件名才对。 但实际上， 很多人并没有正确理解省略文件名的规则， 经常会把目录末尾的“/” 也给省略了。 因此， 或许我们不应该总是将 whatisthis 作为文件名来处理。 一般来说， 这种情况会按照下面的惯例进行处理： 如果Web 服务器上存在名为 whatisthis 的文件， 则将 whatisthis 作为文件名来处理； 如果存在名为 whatisthis 的目录， 则将 whatisthis 作为目录名来处理。 

### HTTP协议的基本思路

**HTTP协议的基本思路如图1.4所示。浏览器解析URL后，自动生成请求消息。请求消息可以看成请求方法+请求对象（URI, Uniform Resourse Identifier)+请求附加信息（+数据）。浏览器将请求消息委托给协议栈（因为浏览器本身不能发送请求消息），让协议栈发送给Web服务器，服务器解析请求消息获得请求数据返回请求消息，请求消息可以看成状态码+相应短语+附加信息（+数据）。协议栈接受响应消息传递给浏览器，浏览器会读取所需数据并显示在屏幕上。**

<img src="https://raw.githubusercontent.com/Chenyuehan1/HowNetworksWork-NOTES/main/img/image-20220311201100781.png" alt="image-20220311201100781" style="zoom:80%;" />

HTTP请求消息与相应消息的格式如图1.5所示。

无论是请求消息还是响应消息，基本格式都相同：请求行+消息头+空行+消息体。区别只在于请求行内容不同。请求消息的格式为：`<method><space><URI><space><HTTP Version>`，响应消息的格式为`<HTTP Version><space><状态码><space><相应短语>`。

<img src="https://raw.githubusercontent.com/Chenyuehan1/HowNetworksWork-NOTES/main/img/image-20220311205355746.png" alt="image-20220311205355746" style="zoom:80%;" />

方法如下

<img src="https://raw.githubusercontent.com/Chenyuehan1/HowNetworksWork-NOTES/main/img/image-20220311205923830.png" alt="image-20220311205923830" style="zoom:80%;" />

HTTP消息头中的主要头字段如下

<img src="https://raw.githubusercontent.com/Chenyuehan1/HowNetworksWork-NOTES/main/img/image-20220311212043218.png" alt="image-20220311212043218" style="zoom: 50%;" />

HTTP相应消息状态码：

状态码和响应短语表示的内容一致， 但它们的用途不同。 状态码是一个数字， 它主要用来向程序告知执行的结果（表1.3）； 相对地， 响应短语则是一段文字， 用来向人们告知执行的结果。  

<img src="https://raw.githubusercontent.com/Chenyuehan1/HowNetworksWork-NOTES/main/img/image-20220311212942176.png" alt="image-20220311212942176" style="zoom:80%;" />

HTTP请求示例

由于每条请求消息中只能写 1 个 URI， 所以每次只能获取 1 个文件，如果需要获取多个文件， 必须对每个文件单独发送 1 条请求。  比如 1 个网页中包含 3 张图片， 那么获取网页加上获取图片， 一共需要向 Web 服务器发送 4 条请求。

<img src="https://raw.githubusercontent.com/Chenyuehan1/HowNetworksWork-NOTES/main/img/image-20220311213204698.png" alt="image-20220311213204698" style="zoom:80%;" />

<img src="../../../AppData/Roaming/Typora/typora-user-images/image-20220311213219308.png" alt="image-20220311213219308" style="zoom:80%;" />

<img src="https://raw.githubusercontent.com/Chenyuehan1/HowNetworksWork-NOTES/main/img/image-20220311213229449.png" alt="image-20220311213229449" style="zoom:80%;" />

## 1.2 向 DNS 服务器查询 Web 服务器的 IP 地址  

浏览器本身不具有发送功能，它根据URL的协议的种类生成对应的请求消息后，要把请求消息委托给操作系统来发送。但是给操作系统发送的前提是知道目的地（比如：Web服务器）的ip地址。通过DNS（Domain Name System）解析器能将服务器域名解析为ip地址，这个操作被称之为**域名解析**。解析器本质上是一个程序。域名解析的代码很简单，就是调用一下Socket库中的`gethostbyname()`函数，输入参数为服务器域名，输出为域名对应的ip地址。（实际上， 实现解析器的功能需要多个程序相互配合，可能还会从gethostbyname 程序中调用其他的程序）域名解析的内部原理也容易理解。

### TCP/IP 网络简述

TCP/IP 的结构如图 1.8 所示， 就是由一些小的子网， 通过路由器 A 连接起来组成一个大的网络。 这里的子网可以理解为用集线器 B 连接起来的几台计算机 C， 我们将它看作一个单位， 称为子网。  

<img src="https://raw.githubusercontent.com/Chenyuehan1/HowNetworksWork-NOTES/main/img/image-20220311224007044.png" alt="image-20220311224007044" style="zoom:80%;" />

### IP地址

IP地址由网络号和主机号组成。网络号是分配给整个子网的，主机号是分配给子网中的网络设备的。

实际的 IP 地址是一串32 比特的数字， 按照 8 比特（ 1 字节） 为一组分成 4 组， 分别用十进制表示然后再用圆点隔开。   

域名与IP并用的理由：相较IP地址，域名更有意义，容易被人类记忆。域名虽然也可以代替IP地址确定通信对象。但是域名字节长度比IP的4字节要长，增加了路由器的负担，降低了通信的效率。于是人类用域名记忆通信对象，路由器用IP地址确定通信对象。为了建立起两者的转换关系，引入了DNS解析器。

<img src="https://raw.githubusercontent.com/Chenyuehan1/HowNetworksWork-NOTES/main/img/image-20220311213717773.png" alt="image-20220311213717773" style="zoom:80%;" />

<img src="https://raw.githubusercontent.com/Chenyuehan1/HowNetworksWork-NOTES/main/img/image-20220311213724311.png" alt="image-20220311213724311" style="zoom:80%;" />

### DNS

DNS： Domain Name System，域名服务系统。将服务器名称和 IP 地址进行关联是 DNS 最常见的用法，但 DNS 的功能并不仅限于此，它还可以将邮件地址和邮件服务器进行关联，以及为各种信息关联相应的名称。  

解析器实际上是一段程序， 它包含在操作系统的 Socket 库中。调用解析器后， 解析器会向 DNS 服务器发送查询消息， 然后 DNS 服务器会返回响应消息。 响应消息中包含查询到的 IP 地址， 解析器会取出 IP地址， 并将其写入浏览器指定的内存地址中。   

<img src="https://raw.githubusercontent.com/Chenyuehan1/HowNetworksWork-NOTES/main/img/image-20220311224817773.png" alt="image-20220311224817773" style="zoom:80%;" />

内部原理：

几个注意的点：

- 解析器本质上是一段程序，所以可以把Socket理解成解析器。调用解析器就是调用`gethostbyname()`函数。

- `gethostbyname()`函数负责生成了DNS服务器的查询消息和接受DNS服务器返回的响应消息。
- 如果要向DNS服务器发送询问请求，也要事先知道DNS服务器的IP地址。只不过这个 IP 地址是作为 TCP/IP 的一个设置项目事先设
  置好的，不需要再去查询了。Window7系统DNS服务器IP地址如图1.13所示。

<img src="https://raw.githubusercontent.com/Chenyuehan1/HowNetworksWork-NOTES/main/img/image-20220311225202369.png" alt="image-20220311225202369" style="zoom:80%;" />

<img src="../../../AppData/Roaming/Typora/typora-user-images/image-20220311230118617.png" alt="image-20220311230118617" style="zoom:80%;" />

DNS解析器生成的查询消息包含包含以下 3 种信息。

- 域名
  服务器、 邮件服务器（ 邮件地址中 @ 后面的部分） 的名称
- Class
  在最早设计 DNS 方案时， DNS 在互联网以外的其他网络中的应用  也被考虑到了， 而 Class 就是用来识别网络的信息。 不过， 如今除了互联网并没有其他的网络了， 因此 Class 的值永远是代表互联网的 IN。

- 记录类型
  表示域名对应何种类型的记录。 例如， 当类型为 A 时， 表示域名对应的是 IP 地址； 当类型为 MX 时， 表示域名对应的是邮件服务器。 对于不同的记录类型， 服务器向客户端返回的信息也会不同。

DNS 服务器上事先保存有前面这 3 种信息对应的记录数据， 如图 1.14所示。DNS 服务器就是根据这些记录查找符合查询请求的内容并对客户端作出响应的。

<img src="../../../AppData/Roaming/Typora/typora-user-images/image-20220311230619073.png" alt="image-20220311230619073" style="zoom:80%;" />

注：

- A 是 Address 的缩写。  
- 不仅是 Web 服务器，像邮件服务器、数据库服务器等，无论任何服务器，只要注册了 A 类型的记录，都可以作为服务器的域名来使用。准确来说，A 类型的记录表示与 IP 地址所对应的域名，因此与其说是某个服务器的域名，不如说是被分配了某个 IP 地址的某台具体设备的域名。  
- MX： Mail eXchange，邮件交换。  

域名的层级结构

 **互联网中存在着不计其数的服务器，将这些服务器的信息全部保存在一台DNS服务器中是不可能的，因此一定会出现在DNS服务器中找不到要查询的信息的情况。信息分布保存在多台DNS服务器中，这些DNS服务器相互接力配合，从而查找出要查询的信息。**

首先，DNS服务器中的所有信息都是按照域名以分层次的结构来保存的。DNS中的域名都是用句点来分隔的，比如www.lab.glasscom.com，这里的句点代表了不同层次之间的界限，就相当于公司里面的组织结构不用部、科之类的名称来划分，只是用句点来分隔而已。在域名中，越靠右的位置表示其层级越高，比如www.lab.glasscom.com这个域名如果按照公司里的组织结构来说，大概就是“com事业集团glasscom部lab科的www”这样。其中，相当于一个层级的部分称为域。因此，com域的下一层是glasscom域，再下一层是lab域，再下面才是www这个名字。这种具有层次结构的域名信息会注册到DNS服务器中，而每个域都是作为一个整体来处理的。换句话说就是，一个域的信息是作为一个整体存放在DNS服务器中的，不能将一个域拆开来存放在多台DNS服务器中。不过，DNS服务器和域之间的关系也并不总是一对一的，一台DNS服务器中也可以存放多个域的信息。

先假设一台DNS服务器中只存放一个域的信息，来阐述**下级域**的关系。比如，假设公司的域为example.co.jp，我们可以在这个域的下面创建两个子域，即sub1.example.co.jp和sub2.example.co.jp，然后就可以将这两个下级域分配给不同的事业集团来使用。

**互联网中有数万台 DNS 服务器，当我们要找我们要访问的Web服务器属于哪台DNS服务器管的时候，肯定不能一台一台挨个去找。  IP地址是以层级划分的，域的信息也是按层级存储在DNS服务器中的。**将负责管理下级域的 DNS 服务器的 IP 地址注册到它们的上级 DNS 服务器中，然后上级 DNS 服务器的 IP 地址再注册到更上一级的 DNS 服务器中，以此类推。也就是说，负责管理lab.glasscom.com 这个域的 DNS 服务器的 IP 地址需要注册到 glasscom.com 域的 DNS服务器中， 而 glasscom.com 域的 DNS 服务器的 IP 地址又需要注册到 com域的 DNS 服务器中。 这样，我们就可以通过上级 DNS 服务器查询出下级 DNS 服务器的 IP 地址，也就可以向下级 DNS 服务器发送查询请求了。最顶层的域为根域。根域不像 com、 jp 那样有自己的名字， 因此在一般书写域名时经常被省略，如果要明确表示根域， 应该像 www.lab.glasscom.com. 这样在域名的最后再加上一个句点， 而这个最后的句点就代表根域。 不过， 一般都不写最后那个句点， 因此根域的存在往往被忽略，但根域毕竟是真实存在的， 根域的 DNS 服务器中保管着com、 jp 等的 DNS 服务器的信息。由于上级 DNS 服务器保管着所有下级DNS 服务器的信息，所以我们可以从根域开始一路往下顺藤摸瓜找到任意一个域的 DNS 服务器。 

除此之外还需要完成另一项工作，那就是**将根域的 DNS 服务器信息保存在互联网中所有的 DNS 服务器**中。这样一来，任何 DNS 服务器就都可以找到并访问根域 DNS 服务器了。因此，客户端只要能够找到任意一台DNS 服务器，就可以通过它找到根域 DNS 服务器，然后再一路顺藤摸瓜找到位于下层的某台目标 DNS 服务器（ 图 1.15）。分配给根域 DNS 服务器的 IP 地址在全世界仅有 13 个 ，而且这些地址几乎不发生变化，因此将这些地址保存在所有的 DNS 服务器中也并不是一件难事。实际上，根域DNS 服务器的相关信息已经包含在DNS 服务器程序的配置文件中了，因此只要安装了 DNS 服务器程序，这些信息也就被自动配置好了。 

<img src="../../../AppData/Roaming/Typora/typora-user-images/image-20220312091110204.png" alt="image-20220312091110204" style="zoom:80%;" />

<img src="../../../AppData/Roaming/Typora/typora-user-images/image-20220312091316409.png" alt="image-20220312091316409" style="zoom:80%;" />

注：当我们客户端事先配置好的最近的DNS服务器上没有我们要访问的域名的IP地址时，DNS服务器会将查询消息**转发给**根域DNS服务器。如果也没有查询到，那根域会**返回**管理下级域的服务器地址。

现实中上级域和下级域有可能共享同一台 DNS 服务器，也就是**一台DNS服务器与多个域相对应**。在这种情况下，访问上级DNS服务器时就可以向下跳过一级DNS服务器，直接返回再下一级DNS服务器的相关信息。此外，有时候并不需要从最上级的根域开始查找，因为**DNS服务器有一个缓存功能**，可以记住之前查询过的域名。如果要查询的域名和相关信息已经在缓存中，那么就可以直接返回响应，接下来的查询可以从缓存的位置开始向下进行。相比每次都从根域找起来说，缓存可以减少查询所需的时间。并且，当要查询的域名不存时，“不存在”这一响应结果也会被缓存。这样，当下次查询这个不存在的域名时，也可以快速响应。这个缓存机制中有一点需要注意，那就是信息被缓存后，原本的注册信息可能会发生改变，这时缓存中的信息就有可能是不正确的。因此，**DNS服务器中保存的信息都设置有一个有效期，当缓存中的信息超过有效期后，数据就会从缓存中删除。而且，在对查询进行响应时，DNS服务器也会告知客户端这一响应的结果是来自缓存中还是来自负责管理该域名的DNS服务器。** 

### 数据收发

浏览器不具有收发信息功能，而是将请求消息委托给协议栈执行收发操作，而真正收发数据的设备是网卡。通过DNS域名解析，我们DNS解析器已经将我们要访问的浏览器的IP地址存到了浏览器的内存里。协议栈执行收发操作之前，要和客户端建立数据流动的通道（便于形象理解），即建立套接字和服务器程序的套接字进行匹配。协议栈数据收发操作也是依靠操作系统中的Socket库。数据传输完后还要断开套接字的连接。

综上所述， 收发数据的操作分为若干个阶段， 可以大致总结为以下 4 个。

- 创建套接字（创建套接字阶段）
- 将管道连接到服务器端的套接字上（连接阶段）
- 收发数据（通信阶段）
- 断开管道并删除套接字（断开阶段）  

<img src="../../../AppData/Roaming/Typora/typora-user-images/image-20220312093056507.png" alt="image-20220312093056507" style="zoom:80%;" />

<img src="../../../AppData/Roaming/Typora/typora-user-images/image-20220312093519524.png" alt="image-20220312093519524" style="zoom:80%;" />

注：

- 创建套接字采用函数`socket()`，函数返回一个描述符，协议栈将描述符返回给程序。描述符是识别客户端内部不同的套接字的标识。
- 连接操作需要指定描述符、服务器端口号和服务器IP地址和自己端口号。描述符与IP地址都已经取得了。描述符是用来区别客户端内部不同套接字的，不用于告知网络连接的另一方。在客户端和浏览器都不知道对方套接字的描述符时，用端口号来确定对方的套接字。服务器上所使用的端口号是根据应用的种类事先规定好的， 仅此而已。比如 Web 是 80 号端口，电子邮件是 25 号端口 D。只要指定了事先规定好的端口号， 就可以连接到相应的服务器程序的套接字。其次，客户端在创建套接字时， 协议栈会为这个套接字随便分配一个端口号 。当协议栈执行连接操作时，会将这个随便分配的端口号通知给服务器。   

- 发送数据操作调用函数`write()`，接受数据操作调用函数`read()`。调用read 时需要指定用于存放接收到的响应消息的内存地址， 这一内存地址称为接收缓冲区。由于接收缓冲区是一块位于应用程序内部的内存空间，因此当消息被存放到接收缓冲区中时，就相当于已经转交给了应用程序。  
- 当浏览器收到数据之后，收发数据的过程就结束了。接下来，我们需要调用 Socket 库的 close 程序组件进入断开阶段。根据应用种类不同，客户端和服务器哪一方先执行 close 都有可能。

