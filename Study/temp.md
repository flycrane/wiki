== C Language ==

1.windows下的zip文件到ubuntu下解压出来文件名字是乱码。

2.date设置时间的用法。

3.c标识符最大长度

4.return exit具体性质，用途。 

5.makefile中如何对头文件的时间戳也敏感。

6.signed char i = 0xff;%d = -1正确; %x = 0xffffffff为什么前面的24位也全部是1啊？;
	char 有符号正数是在127以上溢出的，<=127就是都是对的，大于等于128第八位符号位就被冲掉了溢出成负数了。
	答：这是因为printf类型提升了。

7.变量的定义与声明、初始化。
	两个文件中声明同名字的int型变量，不包含，加或不加extern。
	内存：全局初始化区，全局未初始化区。初始化的放在data段，未初始化的放在bss段（初始化为0的也可能放在bss段）
	/* ------------ cc -c file.c -------------------------------- */
		int i;		/* 全局变量非初始化赋值,默认为0，内存为0 */
		i = 100; 	/* warning: data definition has no type or storage class [enabled by default]
					 * 这里说:数据定义没有类型和存储类 
					 * 说明上面int i;只是声明.而且全局不能在函数外面赋值，只能在声明的时候初始化
					 */
	/* ---------------------------------------------------------- */
		int i;
		int main()
		{
			i = 100; /* 没有任何警告和错误 */
			return 0;
		}	
	/* ---------------------------------------------------------- */

	函数不进行外部声明，或不加extern进行声明（没有返回值的函数如何声明），可以链接库使用。
	无返回值的函数的声明。

8.多参数vt_list vsnprintf

9.位域、位段规则，与跨字节现象。

10.大小端是字节序，大小段的bcd8421码，是4bit序？？？？

11.git 分支和远程仓库

12.int数组定义 初始化，赋值。

13.#define CVMX_MT_CRC_POLYNOMIAL(val)         asm volatile ("dmtc2 %[rt],0x4200" : : [rt] "d" (val))

14.按位寻址，按字节寻址

15.线程 进程资源问题; C语言五大存储区域属性 与  汇编代码段、数据段的关系 
#------Cavium
16.整体构架、锁、原子操作、锁总线。

17.多核多线程

18.uboot,juson architect,flash,cpu,pci,fpa-buffer-pool,bootmem

19.CVMX_USE_1_TO_1_TLB_MAPPINGS,xkphys,kuseg,映射设置和物理地址、虚拟地址转换。

20.cvmx_fpa_alloc,cvmx_bootmem_alloc_name,关系和地址属性 (P181)
	The size of the L1 Dcache is also affected by the amount of Dcache set aside for cvmseg(10.6).

21.ELF File--->in-memory images(contain system memory allocated for the stack and heap),mapping or no mapping
	11.3.2 256的限制 如果映射了，就得注意shared段的大小，那么shared存在哪里和是否映射的影响。
	14.2.1 讲了原因,理解下
22.OCTEON有分类这一个原则，fast slow:
	normal packet processing(fast path),and exception processing(slow path).
23.L2 Cache分区和上锁 (Hardware Reference)

24.Note that the I/O space is selected if bit 48 of the physical address is “1”. Physical memory is selected if bit 48 of the physical address is "0"
25.167:10.4.1讲了unmapped segments分别是64位的xkphys，和32位的kseg0 kseg1,P197也有图可以参考
	cvmx_fpa_alloc() use pointer arguments and return values, not addresses	
	所谓MAPPING,就是物理地址按照虚拟地址进行映射，虚拟的是0，物理的也指向零。
	默认是1，就是默认MAPPING，地址转换就没有用了。
	如果不映射，SE-S通过skphys或者内嵌汇编指令获得内存地址。
26. load sets,cpumask
27.cvmx_bootmem_find_named_block(name)) 返回值的结构体

28.se system memory access P194

29.rwlock 和spinlock的区别，为什么以把named_block的list_head加上了读写锁，那么为什么里面还要用原子操作实现基数的增加和删除，
	c语言的malloc，无法用于申请FPA，那么boot_mem_alloc的内存和FPA缓冲的关系体现在哪里？
30.cvmx_fau_atomic_add64?
#End of Cavium
31.网络报文中字符串不用转大小端？ 单字节不需要，多字节需要。ip地址也是大端(字节序,共四个字节)
网络字节导入到char数组中ip是：
IP: 	0a 07 f9 20	网络字节序是大端，高位字节存储在前面低位地址，所以对应IP是 0a.07.f9.20
	在char数组中前面还是低位地址，这时候*(uint32_t *)addr时候，按照数组中地址顺序，前面还是低位地址，但是字节序是小端，存储的是低位的字节所以打印出来ip变成:20.f9.07.0a
ACSII:	ctwap@mycdma.cn 网络字节,说明大端时候字符串的前面字符是存储在低位地址，这和小端的存储方式一致,可以这么理解吗？

32.cpu 取指，寻址过程，和地址对齐的关系？

33.l2tp pptp
PPTP和L2TP都使用PPP协议对数据进行封装，然后添加附加包头用于数据在互联网络上的传输。尽管两个协议非常相似，但是仍存在以下几方面的不同：

1.PPTP要求互联网络为IP网络。L2TP只要求隧道媒介提供面向数据包的点对点的连接。L2TP可以在IP（使用UDP），桢中继永久虚拟电路（PVCs),X.25虚拟电路（VCs）或ATM VCs网络上使用。

2.PPTP只能在两端点间建立单一隧道。L2TP支持在两端点间使用多隧道。使用L2TP，用户可以针对不同的服务质量创建不同的隧道。

3.L2TP可以提供包头压缩。当压缩包头时，系统开销（overhead）占用4个字节，而PPTP协议下要占用6个字节。

4.L2TP可以提供隧道验证，而PPTP则不支持隧道验证。但是当L2TP或PPTP与IPSEC共同使用时，可以由IPSEC提供隧道验证，不需要在第2层协议上验证隧道

L2TP：第二层隧道协议 （L2TP: Layer 2 Tunneling Protocol）
来自：http://www.networkdictionary.com/chinese/protocols/

       第二层隧道协议（L2TP）是用来整合多协议拨号服务至现有的因特网服务提供商点。 PPP 定义了多协议跨越第二层点对点链接的一个封装机制。特别地，用户通过使用众多技术之一（如：拨号 POTS、ISDN、ADSL 等）获得第二层连接到网络访问服务器（NAS），然后在此连接上运行 PPP。在这样的配置中，第二层终端点和 PPP 会话终点处于相同的物理设备中（如：NAS）。

  L2TP 扩展了 PPP 模型，允许第二层和 PPP 终点处于不同的由包交换网络相互连接的设备来。通过 L2TP，用户在第二层连接到一个访问集中器（如：调制解调器池、ADSL DSLAM 等），然后这个集中器将单独得的 PPP 帧隧道到 NAS。这样，可以把 PPP 包的实际处理过程与 L2 连接的终点分离开来。

  对于这样的分离，其明显的一个好处是，L2 连接可以在一个（本地）电路集中器上终止，然后通过共享网络如帧中继电路或英特网扩展逻辑 PPP 会话，而不用在 NAS 上终止。从用户角度看，直接在 NAS 上终止 L2 连接与使用 L2TP 没有什么功能上的区别。L2TP 协议也用来解决“多连接联选组分离”问题。多链接 PPP，一般用来集中 ISDN B 通道，需要构成多链接捆绑的所有通道在一个单网络访问服务器（NAS）上组合。因为 L2TP 使得 PPP 会话可以出现在接收会话的物理点之外的位置，它用来使所有的通道出现在单个的 NAS 上，并允许多链接操作，即使是在物理呼叫分散在不同物理位置的 NAS 上的情况下。

  L2TP 使用以下两种信息类型，即控制信息和数据信息。控制信息用于隧道和呼叫的建立、维持和清除。数据信息用于封装隧道所携带的 PPP 帧。控制信息利用 L2TP 中的一个可靠控制通道来确保发送。当发生包丢失时，不转发数据信息。


34.补码，原码一位乘法，及其除法（练一下脑子，干别的东西突然就专注了）

35.指令的运算、分析到位级别

36.x86 rsp rbp，栈操作，栈字节对齐的实现。packed, align（1）等等,还有MIPS中一个a0寄存器上放两个参数，一个char，一个short中间是x。难道配合寄存器的局部操作指令使用?

37.位域定义和大小段的关系。

38.load/store指令详解

39.awk 'NR==FNR{a[$1]=$2}NR>FNR{if(a[$1]!=$3) print $1"\t" $3"\t"a[$1]}' CARD.report imp1.res
[sqm@sqm-juson:~/tmp]$ cat CARD.report 
table1          1000
table2          1234
[sqm@sqm-juson:~/tmp]$ cat imp1.res 
table1          tmp1    1000
table2          tmp2    1001
[sqm@sqm-juson:~/tmp]$ 
