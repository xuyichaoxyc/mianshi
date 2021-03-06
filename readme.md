# 面试总结

##  算法题

- 常用算法

  - 快慢指针
  - dfs bfs
  - 二分，注意边界
  - dp
  - 位运算，异或为主

- 最大公约数

  ```c++
  #辗转相除法
  int gcd(int a, int b) {
      return b == 0 ? a : gcd(b, a % b);
  }
  ```

  

- three sum

  ```c++
  class Solution {
  public:
      vector<vector<int>> threeSum(vector<int>& nums) {
          int target;
          vector<vector<int>> res;
          sort(nums.begin(), nums.end());
          for (int i = 0; i < nums.size(); i++) {
              if (i > 0 && nums[i] == nums[i - 1]) {
                  continue;
              }
              if ((target = nums[i]) > 0) {
                  break;
              }
              int l = i + 1;
              int r = nums.size() - 1;
              while (l < r) {
                  if (nums[l] + nums[r] + target > 0) {
                      r--;
                  } else if (nums[l] + nums[r] + target < 0) {
                      l++;
                  } else {
                      target.push_back({target, nums[l], nums[r]});
                      l++, r--;
                      while (nums[l] == nums[l - 1]) {
                          l++;
                      }
                      while (nums[r] == nums[r + 1]) {
                          r--;
                      }
                  }
              }
          }
          return res;
      }
  };
  ```

  

- 链表有环,求入口点

- 原地翻转数组

- 反转链表

- 随机散列

- 堆排，桶排,快排，归并

- 快排

  ```c++
  class Solution {
  public:
      int partition(vector<int>& nums, int left, int right) {
          int l = left + 1;
          int r = right;
          while (l <= r) {
              while (l <= r && nums[l] < nums[left]) {
                  l++;
              }
              while (l <= r && nums[r] > nums[left]) {
                  r--;
              }
              if (l <= r) {
                  swap(nums[l++], nums[r--]);
              }
          }
          swap(nums[r], nums[left]);
          return r;
      }
      void quickSort(vector<int>& nums, int l, int r) {
          if (l >= r) {
              return;
          }
          int p = partition(nums, l, r);
          quickSort(nums, l, p - 1);
          quickSort(nums, p + 1, r);
      }
      vector<int> sortArray(vector<int>& nums) {
          quickSort(nums, 0, nums.size() - 1);
          return nums;
      }
  };
  ```
  
- 堆排

  ```c++
  class Solution {
  public:
      void adjust(vector<int>& nums, int parent, int size) {
          while (2 * parent + 1 < size) {
              int l = parent * 2 + 1;
              int r = parent * 2 + 2;
              int child = (r < size && nums[r] > nums[l]) ? r : l;
              if (nums[parent] > nums[child]) {
                  return;
              } else {
                  swap(nums[parent], nums[child]);
                  parent = child;
              }
          }
      }
      vector<int> sortArray(vector<int>& nums) {
          for (int i = nums.size() / 2; i >= 0; i--) {
              adjust(nums, i, nums.size());
          }
          for (int i = nums.size() - 1; i >= 0; i--) {
              swap(nums[0], nums[i]);
              adjust(nums, 0, i);
          }
          return nums;
      }
  };
  ```

- 归并排序

  ```c++
  class Solution {
  public:
      void merge(vector<int>& nums, int left, int right) {
          if (left >= right) {
              return;
          }
          int mid = left + (right - left) / 2;
          merge(nums, left, mid);
          merge(nums, mid + 1, right);
          vector<int> tmp;
          int l = left, r = mid + 1;
          while (l <= mid && r <= right) {
              if (nums[l] < nums[r]) {
                  tmp.push_back(nums[l++]);
              } else {
                  tmp.push_back(nums[r++]);
              }
          }
          while (l <= mid) {
              tmp.push_back(nums[l++]);
          }
          while (r <= right) {
              tmp.push_back(nums[r++]);
          }
          for (int i = 0; i < right - left + 1; i++) {
              nums[left + i] = tmp[i];
          }
      }
      
      vector<int> sortArray(vector<int>& nums) {
          merge(nums, 0, nums.size() - 1);
          return nums;
      }
  };
  ```

  

- 不借助临时变量交换a, b

  ```c
  b = a - b;
  a = a - b;
  b = a + b
  ```

  ```c
  a = a ^ b;
  b = a ^ b;
  a = a ^ b;
  #利用 a ^ b ^ b = a;
  #注意 a != b
  ```

  

- 逆序对个数 　归并排序

- 数据流中的中位数　一个大根堆一个小根堆

- BST倒数第k小节点 leetcode230　

- 海量数据处理，大数据日志找最多 https://blog.csdn.net/weixin_34161029/article/details/92441368

- 查找二叉树两个节点的公共父节点

- 两个链表的第一个公共节点

- 括号匹配

- top k　partition 或者大根堆小根堆

- 非递归前中后

- 判断两个链表是否相交

- 最长上升子序列

- 随机洗牌算法

- 树的水平遍历用递归

  ```c
  #include <cstdlib>
  #include <iostream>
  using namespace std;
   //取start到end之间的随机值
  int rand(int range_start, int range_end)
  {
  	srand((unsigned int)time(NULL));
  	return rand()%(range_end - range_start) + range_start;
  }
  
  void swap(int* a, int* b)
  {
  	int* temp;
  	*temp = *a;
  	*a = *b;
  	*b = *temp;
  }
  void shuffle(int a[], int len)
  {
  	int shuffle_key;
  	for(int i = 0; i< len; i++)
  	{
  		shuffle_key = rand(0, len);
  		swap(a[i], a[shuffle_key]);
  	}
  }
   
  int main(int argc, char *argv[])
  {
  	int a[8]={3, 5, 7, 2, 12, 1, 5, 6};
  	shuffle(a, 8);
  	for(int i = 0; i < 8; i++)
  	{
  		printf("%d", a[i]);
  	}
      system("PAUSE");
      return EXIT_SUCCESS;
  }
  ```

  

## **linux**

- 进程线程区别，进程间通信,各自特点

- 分页

  - elf文件需要装载到内存中执行，进程的地址空间被划分成若干固定大小的页，内存空间分成若干大小的块，页和块的大小相等，实现了离散分配，虚拟内存使程序认为他有连续的地址．

- 分段

  - 分成代码段，数据段，堆栈段，每个内存段都有一个权限界别，内核最高，用户最低，虚拟地址偏移量加上段基地址可以定位某个字节的位置，如果用户想要访问一个

- 共享内存实现原理

  - 在Linux中，每个进程都有属于自己的进程控制块（PCB）和地址空间（Addr Space），并且都有一个与之对应的页表，负责将进程的虚拟地址与物理地址进行映射，通过内存管理单元（MMU）进行管理。两个不同的虚拟地址通过页表映射到物理空间的同一区域，它们所指向的这块区域即共享内存。

  - 共享内存的通信原理示意图：

  - 对于上图我的理解是：当两个进程通过页表将虚拟地址映射到物理地址时，在物理地址中有一块共同的内存区，即共享内存，这块内存可以被两个进程同时看到。这样当一个进程进行写操作，另一个进程读操作就可以实现进程间通信。但是，我们要确保一个进程在写的时候不能被读，因此我们使用信号量来实现同步与互斥。

  - 对于一个共享内存，实现采用的是引用计数的原理，当进程脱离共享存储区后，计数器减一，挂架成功时，计数器加一，只有当计数器变为零时，才能被删除。当进程终止时，它所附加的共享存储区都会自动脱离。

- 管道实现原理n

- 用户态　内核态

- linux调试多线程，多进程

- TLB是一种页表cache，是特殊的寄存器，减少一次访问内存

- 高并发，**epoll**工作模式，epoll和select比较，epoll遇到的问题

  - 

  - select, poll返回整个用户注册的事件集合，而且把句柄和事件注册到一起，所以每次索引文件就绪的时间复杂度是O(n)，而epoll把句柄和事件分开，注册到内核红黑树上，时间复杂读O(lgN)，将就绪事件放在链表上，通过回调唤醒，回调在epoll_ctl注册到句柄，用户太只需要拷贝就绪链表就可以，复杂度为O(1)
  - epoll适合注册很多句柄但是发生事件不多，如果注册句柄很少还频繁发生，会反复触发回调函数，回调的开销可能大于遍历的开销

  - 水平工作机制（LT）
    - 对于读，只要缓冲区不为空，返回就绪
    - 对于写，只要缓冲区不满，返回就绪
  - 边沿工作机制（ET）
    - 有不可读变为可读，缓冲区由空变为不空，缓冲区有新数据到达
    - 由不可写变为可写

- epoll的问题

  - ET应该是非阻塞的，如果是阻塞的读或写会因为没有后续事件而阻塞
  - EPOLLONESHOT，只触发一次，一个线程读取完Socket数据之后开始处理数据，这时候这个socket又来了数据，会唤醒另一个线程，epolloneshot保证唤醒一次
  - LT可以不立即处理，ET必须立即处理事件

- 自旋锁和读写锁,互斥锁，信号量，大内核索,屏障，cas

- fork vfork　fork父子进程执行顺序不一定，vfork先执行子进程，子进程在父进程的地址空间运行，不可以写，父子进程一样的虚拟内存和物理内存不需要拷贝页表，父进程阻塞等待子进程执行结束之后再执行．fork之后父子进程在没有写之前，共用一样的物理内存，但是不一样的虚拟内存，fork会复制父进程的页表．

- threadlocal, __thread

- 零拷贝　cpu不执行拷贝数据从一块到另一块的操作.

  - 一次网络io数据:
    1. 用户调用read，系统调用陷入到内核态（一次上下文切换）
    2. 向磁盘请求数据
    3. 通过DMA将磁盘数据读取到内核缓冲区
    4. read系统调用返回到用户态，数据从内核缓冲区拷贝到用户缓冲区（二次上下文切换）
    5. write系统调用，将数据写入到内核socket缓冲区（三次上下文切换）
    6. write系统调用返回（四次上下文切换）
    7. DMA将内核socket缓冲区拷贝到协议栈(四次数据拷贝)
  - 零拷贝过程：
    1. 第二次和第三次上下文切换和数据拷贝是多余的
    2. 用sendfile进行优化
    3. sendfile系统调用，进入内核态（一次上下文切换）
    4. 通过DMA将数据从磁盘读到内核缓冲区（１）
    5. 从内核缓冲区拷贝到socket缓冲区（２）
    6. sendfile返回，从内核态进入用户态（二次上下文切换）
    7. DMA将socket缓冲区数据拷贝到网络协议栈（３次数据拷贝）
  - 还可以用mmap省去用户和内核的拷贝，用户和内核共用设备内存

- 虚拟内存能否重叠：可以重叠，虚拟内存重叠，物理内存不一定重叠，虚拟内存在磁盘，页表在内存，TLB快表是寄存器，MMU是硬件，页表把虚拟内存转换为物理地址，当物理地址不在内存发生缺页

- unordered map   map

- copy on write写时拷贝，当fork之后，exec之前父子进程不同的进程地址空间但是共用一样的物理空间，exec之后代码段不一样，如果写但是不exec则数据段堆栈再分配空间并拷贝父进程

- **线程池负载 ****项目必问

- 判断内存泄漏，避免内存泄漏 重载operator new计数

- 为什么要有字节对齐

  1. CPU访问数据效率问题．还可以有效节省内存空间．假设处理器总是从内存中获取8个字节，地址必须是8的倍数。如果我们可以保证任何double将被对齐，使其地址是8的倍数，那么可以用一个内存操作读取或写入该值。否则，我们可能需要执行两次内存访问，因为对象可能被分成两个8字节的内存块。
  2. 另一方面，用于实现多媒体操作的一些SSE指令对于未对齐的数据将不能正确工作。

- 怎么控制字节对齐

  - #pragma pack(n)
  - 不能用memcmp比较，因为对齐填充的是随机值

- **时间轮，小根堆计时器**

- 虚拟内存和物理内存都是以页为单位,4k字节

- malloc, new，operator new, placement new brk sbrk mmap

- ldd 查看程序运行所需的共享库

- cut分割字符, grep, sed, awk

- 如何避免内存泄漏  RAII等，如何检测内存泄漏　重载operator new

- git

  ```git
  git add .
  git commit -m " "
  git checkout 更换分支
  git status -s状态
  git merge 合并分支
  git fetch　拉远程主机最新分支
  git pull = git fetch + git merge
  ```

- 线程共有和私有数据

  - 公有数据
    - 堆，进程地址空间，代码段，进程文件描述符，全局变量，静态变量
  - 私有数据
    - 线程栈．线程私有数据，寄存器，程序计数器，线程, __thread(thread_local c++11关键字)线程局部存储每个线程有一份独立实体相互不干扰
  
- 文件系统

  - 虚拟文件系统，作为中间层，用户和虚拟文件系统打交到，统一的ioapi
  - 超级块　一个超级块对应一个文件系统，保存文件系统的大小类型状态
  - 索引节点inode,保存了文件的元信息，存有指向文件的指针，一个文件只有一个Inode,inode号是唯一的
  - 目录项：存在于内存的，描述文件逻辑属性，为了提高查找性能，文件和文件夹都属于目录项，目录也是文件也有Inode,目录项组成目录树
  - 文件对象　进程已经打开的文件
  
- 查看进程

  - ps -ef

- shell

  ```shell
  awk -F ':' '{print $3}' demo.log | sort | uniq -c | sort -r
  sort -nr -k2 #sort -n按出现次数排序，-r倒序排序，-k2按第２列排序
  ```

- 死锁

  https://blog.csdn.net/guaiguaihenguai/article/details/80303835#commentBox

## 网络

- TCP四次挥手，为什么四次挥手

- time_wait

- SO_LINGER

- TCP发大包出现情况，UDP会么　缓冲区小于包大小，粘包，　UDP不会粘包会丢失数据

- 拥塞窗口：发送窗口取拥塞窗口和接受窗口的最小值

- udp还是tcp选择：
  - tcp
    - 数据要求完整不能丢包，不许发生错误
    - 有限连接
  - udp
    - 大量连接，tcp会性能下降
    - 实时性，允许丢包, 游戏 直播
  
- 网络编程字节序问题出现的原因

- web安全攻击

- HTTP和HTTPS不同　HTTP + SSL

  - HTTP请求是明文传输，HTTPS传输的是密文

  - 对称加密　非对称加密

    - 非对称加密是加密和解密不用一把密钥

  - HTTPS过程

    1. 客户端向服务端发送HTTPS请求，连接到443端口
    2. 服务端保留两把密钥，公钥可以发送，自己保留私钥
    3. 服务端将公钥发给客户端
    4. 客户端收到公钥进行检查
       - 验证服务端发送的数字证书的合法性
    5. 客户端发起HTTPS第二个HTTP请求，将加密之后的客户端密钥发送给服务端
6. 服务端用私钥进行解密，获取客户端密钥，用客户端密钥对数据进行加密
    7. 加密后的密文发送给客户端
    8. 客户端用密钥解密，得到数据

- HTTP

  - 请求行，请求头，请求体
    - 请求行　请求方法　url 　http协议／版本
  - 响应行，响应头，响应体
    - 响应行　http协议/版本　响应码　描述

- arp协议, icmp协议

- 客户端发fin一直失败怎么办
  
- 发fin失败会重发，如果一直失败会断开连接，如果服务端还向客户端写数据，会发RST报文，服务端出发sigpipe信号，需要处理，不然服务端会挂掉
  
- 大量TIME_WAIT原因，后果，解决

  - TIME_WAIT作用：
    1. 保证tcp可靠关闭，如果客户端最后的ack报文没有服务端收到，服务端需要重传FIN，如果客户端已经关闭了就接受不到
    2. 上一次网络传输的数据不会被带到下一次
  - 原因:
    - 服务端主动关闭，比如关机
  - 后果:
    - 高并发短链接，导致文件描述符被占用过多，端口被占用
    - 高并发让服务器短时间内占用大量端口
    - 短链接代表业务处理和io时间短于2msl
  - 解决：
    - SO_REUSEADDR，通知内核，可重用端口
    - 避免服务端主动关闭连接，因为timewait产生在主动关闭端
    - ulimit -n 查看最大文件描述符，　改/etc/security/limits.conf配置文件
    - SO_LINGER,优雅关闭，正常close如果send缓冲区还有数据，则会由内核接管然后保证数据发送到对端，SO_LINGER如果设置超时时间为0，则立即放弃缓冲区的数据，直接发送RST报文

- tcp四种定时器

  - 超时重传定时器（丢失的报文）
  - 坚持定时器(针对零窗口)
    - 零窗口：发送方的发送速度大于接收方的接收速度，接收方缓冲塞满，告诉发送方不要再发送，窗口大小设为０
  - 保活定时器(keepalive)
  - 时间等待计时器（timewait）

- TCP为什么是三次握手不是两次或者四次

  - 我将一次握手理解为序列号的从一端到一端的发送，server在收到client 的seq号后，既需要发送对client序列号的ack即（seq+1），还需要发送自己的seq

  - 为什么不是两次：

    socket是双向管道，因此两次握手只能保证client端正确获取server的序列号即建立从client到server的单向可靠通信，当client发送消息在网络中丢弃或者重传，server会再建立连接

  - 为什么不是四次：

    四次即server发送ack和seq拆成两次发送，这样会造成效率的浪费

- TCP序号的作用：

  - 保证数据传输的顺序,接收方能按顺序接收到数据包
  
- 怎么检测客户端关闭:

  - read到0
  - epoll会出发epollin事件，read返回0说明客户端关闭
  - epollerr说明服务端出现问题，才会被触发

## 数据库

- 数据库查询优化方法

- MyIsam和InnoDB区别

  - https://blog.csdn.net/qq_35642036/article/details/82820178
  - InnoDB支持事务，MyIsam不支持事务
  - InnoDB是主键索引是聚簇索引，数据文件和主键索引绑到一起，主键索引可以直接找到数据文件，辅助索引需要查两次表，先查到主键再通过主键查到数据，因此主键不应该过大．主键索引叶子节点保存的是数据文件，辅助索引叶子节点保存的是主键的值
  - MyIsam是非聚集索引，索引和数据文件是分离的，叶子节点保存的是数据文件的指针．
  - MyIsam全文索引效率更高
  - InnoDB支持表,行级锁，默认是行级锁，MyIsam只支持表级锁,InnoDB的行锁是实现在索引上的，而不是锁在物理行上的，如果没有命中索引，无法使用行锁，会退化成表锁．
  - InnoDB表必须有主键，没有指定的话会自己找或生产一个主键，MyIsam可以没有主键

- 选择引擎

  - 支持事务选InnoDB
  - 大多数是读查询选MyIsam，有读有写选InnoDB
  - MyIsam难以恢复

- InnoDB推荐使用自增id作为主键

  - 自增id保证每次插入时B+索引是从右边扩展的，避免b+树的频繁合并和分裂，如果使用字符串主键和随机逐渐，使得数据随机插入，效率差

- INNODB四大特性

  - 插入缓冲，二次写，自适应哈希索引，预读

- binlog redolog

  - binlog用于归档，在Server层，存的是sql（不一定是sql），容量无限，追加写入
  - redolog用于崩溃恢复，在引擎层，是物理日志，容量有限，循环写入
    - ​    当数据库对数据做修改的时候，需要把数据页从磁盘读到buffer pool中，然后在buffer  pool中进行修改，那么这个时候buffer pool中的数据页就与磁盘上的数据页内容不一致，称buffer pool的数据页为dirty  page  脏数据，如果这个时候发生非正常的DB服务重启，那么这些数据还没在内存，并没有同步到磁盘文件中（注意，同步到磁盘文件是个随机IO），也就是会发生数据丢失，如果这个时候，能够在有一个文件，当buffer pool 中的data  page变更结束后，把相应修改记录记录到这个文件（注意，记录日志是顺序IO），那么当DB服务发生crash的情况，恢复DB的时候，也可以根据这个文件的记录内容，重新应用到磁盘文件，数据保持一致。
  - undo 用于回滚，存放修改的值

- 随机io

  - 顺序IO是指读取和写入操作基于逻辑块逐个连续访问来自相邻地址的数据.在顺序IO访问中，HDD所需的磁道搜索时间显着减少，因为读/写磁头可以以最小的移动访问下一个块。数据备份和日志记录等业务是顺序IO业务。
  - 随机IO是指读写操作时间连续，但访问地址不连续，随机分布在磁盘LUN的地址空间中。产生随机IO的业务有OLTP服务，SQL，即时消息服务等。                                

- 码，主键，候选键，关键字，主属性

  ```
  主码=主键=主关键字，关键字=候选码 候选关键字=候选码中除去主码的其他候选码
  码：唯一标识实体的属性或属性组合称为码
  
  候选码(关键字)：某一属性组的值能唯一标识一个元组而其子集不能（去掉任意一个属性都不能标识该元组），则称该属性组为候选码(补充元组:表中的一行即为一个元组)
  
  主属性:候选码包含的属性（一个或多个属性）
  
  主码(主键、主关键字):若一个关系有多个候选码，选择其中一个为主码
  ```

- 索引的实现

- 数据库范式

- linux指令

- 聚簇索引，非聚簇索引　

  - 聚簇索引，索引的顺序就是数据存放的顺序（物理顺序），只要索引是相邻的，那么对应的数据一定也是相邻地存放在磁盘上的。一张数据表只能有一个聚簇索引。（一个数据页中数据物理存储是有序的）
  
- 非聚簇索引通过叶子节点指针找到数据页中的数据，所以非聚簇索引是逻辑顺序。
  
- 主键索引，唯一索引，组合索引

  - 主键索引一定唯一，一定不为null，只能有一个主键索引
  - 唯一索引，索引值是唯一的，可以为NULL，可以有多个唯一索引
  - 组合索引，一个索引包含多个列，最左匹配原则，比如索引是a,b,c则实际是索引组合a, ab, abc

- 哪些字段加索引：

  - 主键，外键，where,group by, order by

  -   主键和外键建立索引是因为相对的这两个值比较能确定一些数据，所以比较适合建立索引； 

      where条件中的字段适合建立索引是因为要在查询过程中减少数据检索，需要使用索引； 

  - 不能盲目加索引，因为建立索引有开销，维护索引有开销

  - 链接：https://www.nowcoder.com/questionTerminal/91e1c256e2764c5a8217533b584bead5?from=14pdf

    1、表的主键、外键必须有索引；
     2、数据量超过300的表应该有索引；
     3、经常与其他表进行连接的表，在连接字段上应该建立索引；
     4、经常出现在Where子句中的字段，特别是大表的字段，应该建立索引；
     5、索引应该建在选择性高的字段上；
     6、索引应该建在小字段上，对于大的文本字段甚至超长字段，不要建索引；
     7、复合索引的建立需要进行仔细分析；尽量考虑用单字段索引代替

- 索引优劣:

  - 优点
    - 加快检索速度
    - 唯一索引可以保证数据库中每一行数据的唯一性
    - 加速表与表之间的连接
  - 缺点
    - 索引占物理空间
    - 当数据库增删改，索引也要动态维护，降低维护效率
    - 创建索引，维护索引需要花费时间

- 怎么添加索引　主键索引索引着数据，然后普通索引索引着主键ID值(这是在innodb中，但是如果是myisam中，主键索引和普通索引是没有区别的都是直接索引着数据)

  ```sql
  alter table `student` add primary key(`id`) #增加主键索引
  ALTER TABLE `table_name` ADD UNIQUE (`id`)#增加唯一索引
  ALTER TABLE `table_name` ADD INDEX index_name ( `id` )#增加普通索引
  ALTER TABLE `table_name` ADD INDEX index_name ( `column1`, `column2`, `column3` )#增加多列索引
  
  ```

- mysql半同步

  - mysql主从同步本身是一个异步的过程，主节点发送完binlog后立刻返回，有可能从服务器没有收到主服务器发来的binlog，从而导致主从数据库不一致
  
- mysql半同步在主库执行完binlog后，收到一个从库的ack才返回给客户端，确保事务提交后的binlog至少传输到一个从库
  
  - 网络异常或从库宕机，卡主库，直到超时或从库恢复
  
- mysql主从同步

  - 前提：主数据库开启二进制日志

  1. 主服务器上的任何修改通过自己的io线程保存到二进制日志里
  2. 从服务器也启动一个io线程，连接到主服务器请求读取二进制文件，写道本地的中继日志（realy log）里
  3. 从服务器开启一个sql线程定时检查中继日志，如果有更改在本服务器执行

  - 每个从服务器会记录二进制日志坐标
    - 文件名
    - 文件中已经从主服务器读取的位置
  - 如果一主多从，可以主的日志写给一个从，这个从再写到别的从服务器
  - 复制原理:
    - 三个线程，第一个线程，binlog转储线程，把主机Binlog转发给从服务器
    - 从属io线程　该线程连接到主服务器并请求把binlog的更新发送给从服务器，并复制到本地的中继日志
    - 从属sql线程　读取中继日志，并执行
    - 当从属服务器追上主服务器，从服务器睡眠

- mysql查找慢的原因，怎么优化

- 乐观锁，悲观锁

  - 乐观锁总是认为不会产生并发问题，每次去取数据不会认为有其他线程对数据进行修改，因此不会上锁，但是在更新时会判断其他线程在这之前有没有对数据进行修改，使用版本号或者CAS操作
  - 悲观锁，每次拿数据都认为别人会修改，所以每次拿数据都会上锁

- 脏读，不可重复读，幻读

  - 脏读：一个事务对数据库进行了修改，还未提交时，另一个事务对数据库读
  - 不可重复读：在一个事务内对一个数据多次读．在这个事务还未结束时，另一个事务对数据库进行了修改，两次读的数据可能是不一样的
  - 幻读：第一个事物对数据库表进行了处理，这时候第二个事务向数据库插入数据，第一个事务发现还有未修改的数据

- 隔离级别：

  - 读未提交:事务中的修改即使未提交，对其他事务也是可见的
  - 读已提交：一个事务只能读取已经提交的修改
  - 可重复读：保证在同一个事务中多次读取的数据是一样的
  - 可串行化：强制事务串行执行，不会出现一致性问题，需要加锁实现
  
- mvcc

  - 用于实现可提交读和可重复读，可串行化需要加锁不能用mvcc实现
  - 读读不互斥，读写再互斥
  - mvcc规定只能读取已经提交的快照
  - 系统版本号是一个递增的数字，每开始一个新的事务，系统版本号就会递增
  - 事物版本号，事务开始时的系统版本号
    - select
      1. 只查找版本早于当前事务版本号的行（行的系统版本号，小于或等于当前事务的系统版本号），可以保证事务读取的行，要么实在事务还是之前已经存在的要么是事务自身修改过的
      2. 行的删除版本号，要么未定义，要么大于当前事务版本号．事务读取到的行，在事务开始之前未删除
    - insert
      - 为插入的每一行数据吧保存当前的系统版本号作为行版本号
    - delete
      - 删除的每一行保存当前的系统版本号作为行的删除标志
    - update
      - 插入一行新数据，保存当前系统版本号作为新插入行的版本号

## 数据结构

- 哈希冲突
- 跳表
- 为什么redis用跳表不用红黑树
  - 范围查询跳表要好于红黑树
  - 插入删除相对简单
- 布隆过滤器:

  - k个哈希函数，得到k个值，去位图找，如果不都是１则一定不存在，如果是1不一定存在
- b+树，b+和b树和红黑树 **重点看**
- 红黑树
  - 每个节点红色或者黑色
  - 根节点是黑色
  - 红色节点的儿子是黑色
  - 从任意节点到叶子节点的路径包含相同的黑色节点
  - 从根节点到叶子节点的最远路径最长不会超过最近路径的二倍，高度差不会是二倍
- b+树
  - 为什么索引用b树不用平衡二叉树或者红黑树
    - 索引在文件上，是在磁盘上的，索引通常很大，无法一次将全部索引加载到内存中，每次从磁盘读取一个页到内存中，磁盘读取的速度较内存的读取速度差了很多
    - 逻辑结构相近的节点在物理结构上可能差很远，因此需要做多次的磁盘读取
    - io操作次数更多
  - 利用磁盘预读
    - 磁盘会预读，顺序向后多读取一定的长度放入内存，磁盘顺序读取效率较高，不需要寻道
    - 找盘面，找扇区,机械过程慢与电子信号
  - b+树叶子节点为双向链表
  - b+树索引不能找到键对应的具体值，只能找到数据行对应的页，然后把页读入到内存
  - 聚集索引叶节点，按表中主键的顺序构建b+树，保存的是整条行记录，将数据与索引存储到一块，找到索引也就找到了数据页
  - 非聚簇索引，叶节点不保存行记录的数据，仅包含索引中的所有键和查找行记录的主键，索引叶节点指向了数据的对应行
  - 回表：检索非聚簇索引获得主键，用主键在主索引中获得记录
  - 聚簇索引，表中存储的数据按照索引的顺序存储，对数据新增修改删除的影响比较大，非聚簇索引，检索效率低，对数据新增，删除修改的影响小
  - b+树比b树的优点，只有叶子节点存有信息，盘块能容纳的关键字更多，磁盘io次数更少，遍历叶子节点就可以遍历数据

## C++

- 虚函数表，虚继承

- inline:

   - 解决频繁调用的函数消耗内存栈空间，函数调用栈帧
   - 只是提示编译器
   - 将代码替换省去了函数调用开销，函数代码放入符号表
   - 以代码膨胀复制为代价
   - 不能virtual，不能for, 不能递归

- 指针，引用

- main函数执行之前：全局，静态变量完成初始化，arg argv参数传完毕，堆内存栈内存完成初始化，io初始化

  代码列子

  ```c++
  class Demo {
  public:
  	Demo()
      {
  		cout << "demo" << endl;
      }
  };
  int foo()
  {
      cout << "s" << endl;
      return 0;
  }
  
  int x = foo();
  Demo d;
  
  int main()
  {
      return 0;
  }
  ```
```

- 如何防止内存泄漏

- 静态库动态库,静态库　动态库的区别

  - ```makefile
    all:sum print lib main clean
    main:./main.cc ./lib/libxxx.a
    	g++ main.cc -I include -L lib -lxxx -o main.prog
    sum:./src/sum.cc ./include/sum.h
    	g++ -I include ./src/sum.cc -c -o sum
    print:./src/print.cc ./include/print.h
    	g++ -I include ./src/print.cc -c -o print
    lib:./sum.o ./print.o
    	ar rcs ./lib/libxxx.a ./sum.o ./print.o
    clean:
    	rm -f ./*.o
    
```

- 静态库是链接到可执行文件，程序运行时不需要静态库，但是源文件需要重新编译，做成静态库可执行文件体积大，不需要依赖静态库
  
  - 动态库在目标文件编译之后不需要链接，而是程序运行时动态链接，程序运行时需要动态库存在，因此源文件不需要重新编译，做成动态库可执行文件体积小，但是依赖动态库．在调用函数时，根据函数映射表找到函数
  
- vector内存分配，分配一个２倍大小数组，把原数组元素copy过去，所以元素引用不可以，因为内存可能发生变化　1.引用不满足值语义。值语义(value sematics)指的是对象的拷贝与原对象无关，就像拷贝 int 一样。
   2.就算可以你如何初始化，你不可能里面存放引用为空的引用。

- 两个.h同时定义一个static变量会怎样　如果有一个源文件引用两个头文件编译失败，如果是两个源文件分别引用一个，然后编译生产一个elf文件，则没有问题，如果不定义为Static则链接会出错报重定义

- static函数，static函数不能在本代码文件以外的代码文件使用，普通函数默认是extern的可以被别的代码文件使用，其他文件可以定义相同名字的函数 https://www.cnblogs.com/nzbbody/p/3413169.html

- 虚表，虚继承，虚指针，虚基类表指针，虚基类表	

- 虚表　每个类有一个虚表，每个对象有一个虚指针,虚表在数据段，虚函数在代码段https://blog.csdn.net/primeprime/article/details/80776625			

- 虚继承，虚基类类表，指针 虚基类表存的是虚基类相对子类的偏移，虚函数表存的是虚函数指针，虚继承维持一份拷贝，虚继承只有虚基类指针没有虚表指针，通过虚基类表找到基类虚表https://blog.csdn.net/qq_36359022/article/details/81870219

- 重载重写多太　重载函数签名不同　多太：运行期，预编译期模板,函数重载

- 手写string类

   ```c++
   class String {
   private:
       char* data;
       int length;
       
   public:
       String(const char* src) {
           if (src == nullptr) {
               length = 0;
               data = new char[1];
               *data = '\0';
           } else {
               length = strlen(src);
               data = new char[length + 1];
               strcpy(data, src);
           }
       }
       const char* c_str() {
           return data;
       }
       int size() {
           return length;
       }
       String(const String& src) {
           char* d = src.c_str();
           length = src.size();
           data = new char[length + 1];
          	strcpy(data, d); 
       }
       String& operator= (const String& src) {
           if (this == &str) {
   			return *this;
           }
           delete[] data;
           
           char* d = src.c_str();
           length = src.size();
           data = new char[length + 1];
           strcpy(data, d);
           return *this;
       }
       char& operator[](int n) const {
           if (n >= length) {
               return data[length - 1];
           } else {
               return data[n]
           }
       }
       String operator+(const String& str) const {
           String newString;
           newString.size() = length + str.size();
           newString.c_str() = new char[newString.size() + 1];
           strcpy(newString.c_str(), data);
           strcat(newString.c_str(), str.c_str());
           return newString;
       }
       bool operator=(const String& str) {
           if (str.size() != length) {
               return false;
           }
           return strcmp(data, str.c_str()) ? false : true;
       }
   };
   ```

   

- 单例双检查

  ```c++
  class Singlenton {
  private:
      Singlenton()
      {
      }
  
      Singlenton(const Singlenton&)
      {
      }
  
      Singlenton& operator=(const Singlenton&)
      {
      }
  
      mutex m;
  
  public:
      static Singlenton* single;
  
      Singlenton* getSingenton()
      {
          if (single != nullptr) {
              m.lock();
              if (single != nullptr) {
                  single = new Singlenton;
              }
              m.unlock();
          }
          return single;
      }
  };
  
  Singlenton* Singlenton::single;
  ```

- 深拷贝　浅拷贝

- memcpy, memmove,strcpy,strcat　手写

  - strcpy以'\0'结尾，memcpy memmove以num结尾
  - memmove可以解决内存覆盖，memcpy不可以

  ```c++
  char* strcpy(char* dest, const char* src)
  {
      if (dest == NULL || src == NULL)
      {
  		return NULL;
      }
      char* tmp = dest;
      while ((*dest++ = *src++) != '\0');
      return dest;
  }
  ```

  ```c++
  char* strcat(char* dest, const char* src)
  {
      if (dest == nullptr || src == nullptr) {
          throw "error";
      }
      char* tmp = dest;
      while (*tmp != '\0') {
          tmp++;
      }
      while ((*tmp++ = *src++) != '\0');
      return dest;
  }
  ```

  ```c++
  void* memcpy(void* dest, const void* src, int len)
  {
      if (dest == nullptr || src == nullptr || len <= 0) {
          return nullptr;
      }
      char* d = (char*)dest;
      const char* s = (char*)src;
      if (d > s && d < s + len) {
          d = (char*)(dest + len - 1);
          s = (char*)(src + len - 1);
          while (count--) {
              *d-- = *s--;
          }
      } else {
          while (count--) {
              *d++ = *s++;
          }
      }
      return dest;
  }
  ```

  

- 析构函数不要抛出异常

  - 如果析构函数抛出异常，则异常点之后的程序不会执行，如果析构函数在异常点之后执行了某些必要的动作比如释放某些资源，则这些动作不会执行，会造成诸如资源泄漏的问题。
  - 通常如果发生异常会调用析构函数来释放资源，如果此时析构函数本身抛出异常，则前一个异常没有处理又来了新的异常
  
- 函数调用过程

   + 将参数从右向左入栈

   + 返回地址入栈，将当前代码区调用指令的下一条指令地址压入栈中，供函数返回时继续执行

   + 代码区跳转，执行调用函数

   + 栈帧调整

     - EBP(基址指针寄存器，内存里面存放一个指针)系统栈最上面的栈帧的底部，ESP栈顶寄存器内存里面存指针，指向系统栈最上面的栈帧的顶部

     - 保存当前栈帧状态值，以备后面恢复本栈帧使用(EBP入栈)
     - 将当前栈帧切换到新栈帧（将ESP装入EBP，更新栈帧底部）
     - 给新栈帧分配空间(把EBP减去所需空间大小，抬高栈顶)

- c++11特性

- 四种cast

   - const_cast 消除常量性
   - static_cast 
     - 没有运行时类型检查　无法保证安全性
     - 基本数据类型，或者non const -> const(const -> non const必须const cast)
     - 空指针转换为目标类型指针
     - 任何类型转为void
     - 子类指针转父类指针
   - dynamic_cast:只用于对象和引用，父类指针转换为子类指针,要求父类有虚函数
   - reinpreter_cast:指针引用函数指针算数类型成员指针，二进制拷贝．

- 智能指针

   - shared_ptr：共享，多个智能指针指向相同对象，最后一个引用被销毁才释放资源
   - unique_ptr:独占式，同一时刻只有一个智能指针指向该对象
   - weak_ptr:共享但不拥有某对象，当shared_ptr最后一个释放资源，weak_ptr失效
     - 可打破循环引用，即两个对象互值，造成还在使用的假象，造成内存泄漏

- 什么是循环引用，为什么weak_ptr可以解决循环引用

- define和typedef

   - define是预处理，只进行字符串替换，不进行正确性检查，只有在编译期展开编译器才会发现
   - typedef一般用来给变量类别起别名，define还可以用来宏开关，定义常量
   - typedef有自己的作用域，define没有

- 位域

   - ```c++
      typedef struct Test {
       
          char a : 1;
       
          char b : 1;
       
          char c : 1;
       
     }Test;
     ```

   - a b c各占一位　节省空间，位域不能跨字节存储，也不能跨类型

- 枚举

   - c++98和c++11不同

     - enum是全局的

     - ```c++
       enum A {
           yellow,
           red
       };
       
       enum B {
       	yellow
       };
       编译失败，yellow重复定义,可以用Namespace结局
       ```

     - 允许隐式转换为int，可能不安全

     - c++11强enum类型

     - ```c++
       enum class A {
           yello,
           white
       };
       ```

     - 强enum特点：强作用域,解决全局空间问题．不可以与Int隐式转换，可以强制转换

- constexpr:

   - 修饰变量，函数．构造函数，相当于告诉编译器将它修饰的当作在编译期就能得出的常量值去优化

   - ```c++
     constexpr int func() {
         return 10;
     }
     main(){
       int arr[func()];
     }
     //编译通过
     编译期就确定func返回值是10无需等到运行期计算
     ```

   - const只表示const修饰的在运行期不被更改，但是可能还是个变量，constexpt表示在编译期就得到值

- 右值引用：只能绑定右值的引用

   - const T&常量左值，既能绑定右值，又可以绑定左值

   - 已命名的右值引用是左值

   - 移动语义：移动构造和移动赋值将自己的指针指向这个资源，将别的指针指向Null将这个资源的所有权偷过来

     ```c++
     class Demo {
     public:
         int* data;
     public:
         Demo(Demo&& src) noexpect : data(src.data) {
             src.data = nullptr;
         }
     };
     ```

     

- decltype:

   - 推断出该变量的类型，但是不初始化，auto需要初始化,auto忽略顶层const,decltype保留顶层const,对引用，auto推断出原有类型，decltype推断出引用

- noexpect:
  
- 告诉编译器，函数不可以抛出任何异常，编译器会对代码进行优化，减小生成目标文件体积
  
- 空间配置器:

   - 一级空间配置器：大于128字节
     - 调用malloc和free，realloc分配内存，当内存不足可以自定义函数
     - 一级调用失败时，调用oom_alloc函数尝试清理，如果失败，抛出bad_alloc异常
   - 二级空间配置器：小于128字节
     - 16个元素的自由链表，每个节点挂载着大小为(8,16, .., 128)的内存块
     - 将客端申请的内存提升到8的倍数，如果对应的链表有内存块就分给客端
     - 如果对应的链表没有内存块，并且内存池有足够的内存，则取出20块内存，19块放到链表上，一块给客端
     - 如果内存池中不够20块但是够1块，就拿出来一块给客端然后把剩下的放到链表上
     - 如果一个内存块都不够了，则将剩下的内存挂载到相应的链表下，然后申请内存
     - 如果内存池内存没有，需要申请内存，申请20个所需要内存的内存块*2个大小的内存比如需要16b则申请 2 * 16 * 20b字节，20个节点放到内存池，19个内存挂载到链表，1个内存返回给客端
     - 如果内存池申请内存失败，说明Malloc内存失败内存不够，则从大于这个节点的节点分配出足够的内存，返回给客端
     - 如果内存分配失败并且大于这个节点的节点也没有内存了，调用oom_allocate清理内存，如果再失败说明没有内存可用了

- explict:

   - 构造函数只能是显示构造，默认是隐式构造的

- final:

   - ```c++
     class Base final {
     };
     Base类不会被继承
     ```

   - ```c++
     class Base {
     public:
         virtual void foo final {
         }
     };
     Base类的foo方法不会被重写
     ```

- delete[] 和 delete
  
  - delete删除数组会造成内存泄漏，只调用了一个对象的析构函数，只删除了一块内存
  
- new[]

   - new[10] 内存对象模型，会先有一个数组大小的记录，然后是10个object,如下所示
   - [10] [object] [object] [object] [object] [object] [object] [object] [object] [object] [object]
   - 因此delete[]的时候知道调用多少个析够函数

## 中间件

- protobuf原理

- gRpc原理

  - 基于Protobuf
  - 基于http2.0
  - rpc过程 stub封装了通信细节，本地代理

         　　1. 调用者以本地调用的方式发起调用
                 2. client stub收到调用，将被调用的方法名，参数序列化到消息体
                 3. client stub将消息体发送到服务端
                 4. server stub收到消息体，反序列化
                 5. server stub根据方法名和参数进行本地调用
                 6. 将本地调用执行结果返回给server stub
                 7. server stub将返回值序列化，发送给客户端
                 8. client stub解包反序列化，返回给client
                 9. client得到本次RPC调用结果

- Consul原理

  - 服务发现和配置共享
  - 基于raft算法

- redis集群，三种启动方式

  - 主从复制　主从读写分离

    全量同步

    ```
    1.从服务器发送SYNC
    2.主服务器收到SYNC之后，执行BGSAVE,将数据持久化二进制RDB文件，并将此后命令写入缓冲区
    3.从服务器收到快照文件后丢弃所有旧数据，载入收到的快照
    4.主服务器发送完快照文件后，将缓冲区的命令发送给从服务器
    5.从服务器执行命令
    6.从服务器完成读请求
    ```

    增量同步

    ```
    部分复制的条件，>2.8版本，runid 与master一致，掉线不重启，重启的话只能全两同步
    master有数据更新，同步到slave
    ```

  - 哨兵　高可用，任务是监控和故障转移

    - 主从的弊端，主节点挂掉后需要手动切换
    - 通过发送命令监控主节点和从节点
    - 当主节点挂机后，将slave升级为主节点，然后通知其他从节点更改配置
    - 多个哨兵，当一个哨兵观察到主机下线，称为主观下线
    - 后面的哨兵也检测到主机下线，数量到达一半以上，哨兵之间投票选出一个哨兵执行故障切换操作，然后通过发布订阅让各个哨兵监控主节点

  - 集群

    - 解决单机存储问题，需要将数据分片

    - 虚拟哈希槽

      - ```python
        CRC16(key) % 16384 计算在哪个槽节点
        ```

      - 把数据存在master上，master和对应的slave同步
      - 集群扩展时，把对应的节点数据移动到对应的槽节点

    - 一致性哈希

      - 一个0 -2的３２次方的闭合圆，拥有这么多个桶空间
      - 对节点（id）进行哈希，其值分布在圆上
      - 对要存储的数据key进行哈希，其值分布在圆上，顺时针方向第一个节点

- redis数据结构

- 一致性哈希，虚拟哈希槽

  - 一致性哈希　增加或者删除节点，数据迁移量降到最低．

- 数据倾斜

- 微服务，熔段，限流，api网关，go-micro

  - 服务注册发现配置中心consul
  - 微服务间通信，gRpc

  - api网关

    - 路由转发
    - 负载均衡
    - 对外暴露的api接口，用于路由转发

  - gomicro一个服务的流程

    - 在protobuf里写service
    - 实现service接口的所有方法
    - 注册到注册中心，把实现接口的方法的结构体传过去，这样别的服务调用这个这个微服务的方法实际执行的就是实现这个接口的方法的结构体的方法
    - gomicro是一个微服务rpc框架

  - 项目架构

    - api网关，

      - 路由转发

      - http请求拦截，token验证
      - 熔断限流
      - 初始化用户微服务，上传微服务，下载微服务

    - dbProxy

      - 文件表（文件哈系，文件名，文件存储路径）

      - 用户表（用户名，密码）
      - 用户Token表（用户名，登录token）
      - 用户文件表（用户名，文件哈希，文件信息）

    - 上传

      - 分块上传

        - 获得文件大小
        - 获得redis连接
        - 生成上传文件初始信息，文件哈希，文件大小，id，chunksize，一共多少chunk
        - 写入Redis缓存

        - 上传文件分块，将信息写入到redis缓存，用哈希，将分块文件写入磁盘，更新redis缓存状态
        - 上传合并，查看id是否等于chunk数量，合并用Cat,更新文件表，用户文件表

      - 秒传

        - 查看文件表里面是否有文件哈希
        - 如果有更新用户文件表

      - 异步队列

        - 文件上传的同时写入异步队列 包括文件哈希，文件地址，oss地址

    - 下载

    - rabbitMq

      - 异步上传到OSS
      - 发布订阅模式
        - 一个生产者发送消息，多个消费者获取消息，一个生产者一个交换机多个队列多个消费者
        - 每个消费者都有自己的队列
        - 生产者将消息发送到交换机

    - 异步转移

      - 从rabbitmq读取数据，包括文件哈希值，文件路径，oss路径
      - 上传至oss公有云
      - 和db微服务交互 mq

    - 用户

      - 生成token　md5(username + time + sault)
      - 用户注册登录
      - 和db微服务交互
  
- 网络库项目总结

  - 线程池

    - 构造函数创建了n个线程和n个消息队列
      - 每个线程创建的时候，会向对应id的消息队列注册可读事件，注册的文件描述符是eventfd，新建一个loop把这个loop传给消息队列
      - 当该线程对应的队列有数据来时，给eventfd发送一个字节数据，唤醒线程
      - 如果消息队列不空，从消息队列取出第一个元素，为消息（包括种类和socket句柄），将socket句柄注册到eventloop,回调函数是读取数据到用户缓存池
      - 阻塞在eventloop的epollwait上,每次当epollwait收到eventfd数据，执行上一步的函数，即取出fd，注册fd和回调函数

    - 线程id，池内线程index和消息队列id是一个

    - 消息队列二级指针，掌管线程池的多条消息队列，消息队列组成:主线程监听socket，将连接的socket通过消息队列push给线程池中的线程，通过eventfd唤醒线程

      - 消息：
        - 消息种类，io连接还是任务
        - socket句柄或者是任务回调函数

      - eventloop
        - epollfd
        - epollevents,存储从内核拷贝到用户态的就绪链表
        - hash表：<fd, io事件>
        - io事件：
          - 可读
          - 可写
        - 添加io事件函数，删除Io事件函数
        - 执行函数，阻塞在epollwait上，当epollwait唤醒，到hash表上找到这个fd对应的io事件，然后执行注册的回调函数
      - 队列
      - eventfd(通过eventfd唤醒阻塞在epollwait的工作线程)

##　 智力题

- 对一批编号为1-100，全部开关朝上（开）的灯进行以下操作：凡是1的倍数反方向拨一次开关；2的倍数反方向又拨一次开关；3的倍数反方向又拨一次开关……问：最后为关熄状态的灯的编号是哪些？
- 赛马问题　８个赛道，６４个马，选前四(先跑８个一组跑，去除掉每组后四个，每组第一取出，后四名的组全淘汰，第四名的组后三个淘汰，以此类推淘汰3+2+1个，还剩１０个)
- 有两根不均匀的香，烧完一根要1小时，怎么利用这两根香得到15分钟的时间？为什么这样是对的，能证明不
- 一个长方形(体)蛋糕中间有一个长方形(体)的空心位置，这个空心的长方形(体)是可见且贯穿的，怎么一刀分成平均的两份
- 鸡蛋掉落 leetcode887 二分法
- 有1000个一模一样的瓶子，其中有999瓶是普通的水，有一瓶是毒药。任何喝下毒药的生物都会在一个星期后死亡。现在，你只有10只小白鼠和一个星期的时间，如何检验出哪个瓶子里有毒药？（二分，把五百个瓶子的水放到A,另外五百水放到B，毒死了那个继续；不龙过滤器）

