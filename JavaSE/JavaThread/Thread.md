

## 多线程

##### 一、线程与进程的5个阶段

创建、就绪、运行、阻塞、终止

##### 二、实现多线程的方法（3种）

1. 扩展java.lang.Thread类
2. 实现java.lang.Runable接口
3. 实现Callable接口，并与 Future、线程池结合使用

##### 三、分类介绍

1. ###### 继承Thread类

   ```java
   package com.test.thread;
   
   public class Thread1 extends Thread {
   	private String name;
   	public Thread1(String name) {
   		super();
   		this.name = name;
   	}
   
   	@Override
   	public void run() {
   		// TODO Auto-generated method stub
   		for (int i = 0; i < 5; i++) {
   			System.out.println(name+"运行："+i);
   			try {
   				sleep((long) (Math.random()*10));
   			} catch (InterruptedException e) {
   				// TODO Auto-generated catch block
   				e.printStackTrace();
   			}
   		}
   	}
   }
   //测试类
   package com.test.thread;
   
   public class Test1 {
   
   	public static void main(String[] args) {
   		// TODO Auto-generated method stub
   		Thread1 thread1 = new Thread1("jordan");
   		Thread1 thread2 = new Thread1("kobe");
   		thread1.start();
   		thread2.start();
   	}
   
   }
   
   ```

   输出：

   jordan运行：0
   kobe运行：0
   kobe运行：1
   kobe运行：2
   jordan运行：1
   kobe运行：3
   jordan运行：2
   kobe运行：4
   jordan运行：3
   jordan运行：4

   > 注意：
   >
   > 1. start() 方法的调用后并不是立即执行多线程代码，而是使得该线程变为可运行态（Runnable），什么时候运行是由操作系统决定的
   > 2. 从程序运行的结果可以发现，多线程程序是乱序执行。因此，只有乱序执行的代码才有必要设计为多线程
   > 3. Thread.sleep() 方法调用目的是不让当前线程独自霸占该进程所获取的 CPU 资源，以留出一定时间给其他线程执行的机会
   > 4. 实际上所有的多线程代码执行顺序都是不确定的，每次执行的结果都是随机的
   > 5. start 方法重复调用的话，会出现 java.lang.IllegalThreadStateException 异常

   ###### 2.实现Runable接口

   ```Java
   package com.test.thread;
   
   public class Thread2 implements Runnable {
   	private String name;
   	
   	public Thread2(String name) {
   		super();
   		this.name = name;
   	}
   
   	@Override
   	public void run() {
   		// TODO Auto-generated method stub
   		for (int i = 0; i < 5; i++) {
   			System.out.println(name+"运行："+i);
   		}
   		try {
   			Thread.sleep((long) (Math.random()*10));
   		} catch (InterruptedException e) {
   			// TODO Auto-generated catch block
   			e.printStackTrace();
   		}
   	}
   }
   //测试类
   package com.test.thread;
   
   public class Test2 {
   
   	public static void main(String[] args) {
   		// TODO Auto-generated method stub
   		new Thread(new Thread2("jordan")).start();
   		new Thread(new Thread2("kobe")).start();
   	}
   
   }
   ```

   ##### 二者区别

   如果一个类继承 Thread，则不适合资源共享。但是如果实现了 Runable 接口的话，则很容易的实现资源共享

   ###### 总结：（实现Runable接口继承Thread的优点）

   1. 适合多个相同的程序代码的线程去处理同一个资源
   2. 可以避免Java单继承的缺点
   3. 增加程序的健壮性，代码可以被多个线程共享，代码和数据共享
   4. 线程池只能放入实现 Runable 或 callable 类线程，不能直接放入继承 Thread 的类

   > 注意：main 方法其实也是一个线程。在 java 中所以的线程都是同时启动的，至于什么时候，哪个先执行，完全看谁先得到 CPU 的资源

   ##### 四、线程状态转换

   1、新建状态（New）：新创建了一个线程对象。

   2、就绪状态（Runnable）：线程对象创建后，其他线程调用了该对象的 start() 方法。该状态的线程位于可运行线程池中，变得可运行，等待获取 CPU 的使用权。

   3、运行状态（Running）：就绪状态的线程获取了 CPU，执行程序代码。

   4、阻塞状态（Blocked）：阻塞状态是线程因为某种原因放弃 CPU 使用权，暂时停止运行。直到线程进入就绪状态，才有机会转到运行状态。阻塞的情况分三种：

   （一）、等待阻塞：运行的线程执行 wait() 方法，JVM 会把该线程放入等待池中。(wait 会释放持有的锁)

   （二）、同步阻塞：运行的线程在获取对象的同步锁时，若该同步锁被别的线程占用，则 JVM 会把该线程放入锁池中。

   （三）、其他阻塞：运行的线程执行 sleep() 或 join() 方法，或者发出了 I/O 请求时，JVM 会把该线程置为阻塞状态。当 sleep() 状态超时、join() 等待线程终止或者超时、或者 I/O 处理完毕时，线程重新转入就绪状态。（注意, sleep 是不会释放持有的锁）

   5、死亡状态（Dead）：线程执行完了或者因异常退出了 run() 方法，该线程结束生命周期。

   ##### 五、线程调度

   1. 调整线程优先级：Java 线程有优先级，优先级高的线程会获得较多的运行机会

      Java 线程的优先级用整数表示，取值范围是 1~10，Thread 类有以下三个静态常量：

      > ```Java
   > static int MAX_PRIORITY
      >           //线程可以具有的最高优先级，取值为10。
   > static int MIN_PRIORITY
      >           //线程可以具有的最低优先级，取值为1。
   > static int NORM_PRIORITY
      >           //分配给线程的默认优先级，取值为5。
   > ```
   
   Thread 类的 setPriority() 和 getPriority() 方法分别用来设置和获取线程的优先级
   
   每个线程都有默认的优先级。主线程的默认优先级为 Thread.NORM_PRIORITY
   
   线程的优先级有继承关系，比如 A 线程中创建了 B 线程，那么 B 将和 A 具有相同的优先级
   
   JVM 提供了 10 个线程优先级，但与常见的操作系统都不能很好的映射。如果希望程序能移植到各个操作系统中，应该仅仅使用 Thread 类有以下三个静态常量作为优先级，这样能保证同样的优先级采用了同样的调度方式
   
2. 线程睡眠（sleep()）:Thread()的方法，使线程转到阻塞状态，结束后，线程转入就绪（Runable）状态
   
3. 线程等待（wait()）:Object()的方法，使线程等待，直到其他线程调用此对象的notify(all)()方法时方可唤醒此线程
   
4. 线程让步（yield()）:Thread()的方法，暂停正在执行的线程，将执行让给有优先级相同或者更高的线程执行
   
5. 线程加入（join()）:Thread()的方法，在当前线程调用其他线程的join()方法，当前线程转入阻塞状态，直到被调用的线程运行完毕，当前线程由阻塞状态转为就绪状态
   
6. 线程唤醒（notify()）:Object()的方法，
   
注意：Thread 中 suspend() 和 resume() 两个方法在 JDK1.5 中已经废除，不再介绍。因为有死锁倾向
   

   

   

   

   

   

   

   

   

   

   

   

   

   

   

   

   

   

   

   

   

   

   

   

   

   

   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   