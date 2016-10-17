# Lab4:死锁 #

- ## 死锁 ##
  1. 定义
  死锁就是两个或者多个进程，互相请求对方占有的资源
  2. 产生死锁的四个条件
   - 互斥条件：一个资源每次只能被一个进程使用
   - 请求与保持条件：一个进程因请求资源而阻塞时，对已获得的资源保持不放
   - 不剥夺条件:进程已获得的资源，在末使用完之前，不能强行剥夺
   - 循环等待条件:若干进程之间形成一种头尾相接的循环等待资源关系

- ## synchronized ##
 - 当用关键字synchronized修饰一个方法或者一个代码块的时候，能够保证在同一时刻最多只有一个线程执行该段代码。
 - 当一个线程访问object的一个用关键字synchronized同步的代码块或方法时，其他线程对object中所有其它用关键字synchronized同步的代码块或方法的访问将被阻塞

- ## 实验步骤 ## 
本实验在windows系统下完成
 1. 编写Deadlock.java文件
         class A{
	        synchronized void methodA(B b){
		        b.last();
	        }
	        synchronized void last(){
		        System.out.println("Inside A.last()");
	        }
         };

         class B{
	         synchronized void methodB(A a){
		         a.last();
	         }
	         synchronized void last(){
		         System.out.println("Inside B.last()");
	         }
         };

         //Runnable是一个线程，每次调度它执行的时候，它就运行run()中的语句 
         class Deadlock implements Runnable{
	         A a=new A();
	         B b=new B();
	         //构造函数 
	         Deadlock(){
		         Thread t=new Thread(this);
		         int count = 20000;
		         t.start();//线程t开始执行 
		         while(count-->0);//等待20000 
		         a.methodA(b); 
	         }
	         //Runnable执行时调用的方法 
	         public void run(){
		         b.methodB(a); 
	         }
	         public static void main(String args[]){
		         new Deadlock();
	         }
         }; 
      
  2. 启动命令行控制台，输入以下命令编译Deadlock.java文件，生成A.class、B.class、Deadlock.class三个类
  
     `javac Deadlock.java`

 3. 编写Deadlock.bat，并将其保存在与Deadlock.class相同的目录下
         cd /d %~dp0
         @echo off
         :start
         set /a var+=1
         echo %var% times
         java Deadlock
         if %var% leq 100 GOTO start
         pause
 
 4. 双击Deadlock.bat，使其运行，观察结果
  
- ## 实验结果及分析 ##
 1. 实验结果
 ![Deadlock](http://i.imgur.com/70a6Nv3.png)
   从图中可以看出，死锁停在第49次

 2. 实验分析  
   - 在A.class中，方法methodA调用了B.class的方法last，同时，方法methodA、last使用了关键字synchronized，所以当A.class的方法methodA（或者last）在执行的时候，方法last（或者methodA）是没有办法被同时调用执行的
   - 在B.class中，方法methodB调用了A.class的方法last，同时，方法methodB、last使用了关键字synchronized，所以当B.class的方法methodB（或者last）在执行的时候，方法last（或者methodB）是没有办法被同时调用执行的
   - 在Deadlock.class中，入口函数main创建了一个线程Deadlock，而Deadlock线程创建一个新线程t让它在后台执行，然后等待20000的时间，再执行A.class的methodA方法。线程t执行B.class的methodB方法。
   - Deadlock.bat让以上程序跑一百遍，由以上分析可知，当线程Deadlock执行A.class的methodA方法时，线程t是没有办法同时执行B.class的methodB方法的，如果线程Deadlock在等待时间内，线程t没有执行完成，导致线程Deadlock没有办法调用B.class的methodB方法，这就导致了死锁的产生
