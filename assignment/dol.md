# DOL实例分析&编程 #

- ## 任务 ##
  1. 修改example2，让3个square模块变成2个, tips:修改xml的iterator
  2. 修改example1，使其输出3次方数，tips:修改square.c


- ## 实验过程及结果 ##

 1. 进入dol文件夹并编译dol
         cd dol
         ant -f build_zip.xml all

     ![编译](http://i.imgur.com/MGrmeDR.jpg)
      
  2. 进入build/bin/mian路径下,运行example1
		 cd build/bin/main
         ant -f runexample.xml -Dnumber=1

       ![example1(2)](http://i.imgur.com/7Oiemep.png)

	   ![example1(2)dot](http://i.imgur.com/gGOpZ8M.png)

 3. 打开dol/examples/example1/src/square.c,将square_fire函数中第一个if语句的`i=i*i`修改为`i=i*i*i`,并保存
 
      ![i*i*i](http://i.imgur.com/VBaWcdB.jpg)
    
 4. 将dol/build/bin/main文件夹中的example1整个文件夹删除，如果example1文件夹右下角有锁的图标没办法删除，则先在终端中用以下命令解锁再删除它

   `sudo chown -R 用户名 文件名`

    ![解锁](http://i.imgur.com/wCeMDZx.jpg)

 5. 进入dol文件夹并编译dol
         cd
         cd dol
         ant -f build_zip.xml all

     ![回退编译](http://i.imgur.com/Dm5WDO6.jpg)

 6. 进入build/bin/mian路径下,再次运行example1
         cd build/bin/main
         ant -f runexample.xml -Dnumber=1

        ![example1(3)](http://i.imgur.com/CMkfH1G.png)
		
		![example1(3)dot](http://i.imgur.com/YYWVxj5.png)

 7. 运行example2
	`ant -f runexample.xml -Dnumber=2`

	![运行example2](http://i.imgur.com/jaup5nJ.jpg)

	![example2(3)](http://i.imgur.com/TQiiE0E.png)

	![example2(3)dot](http://i.imgur.com/3HswFD8.png)

 8. 鼠标点击进入dol/examples/example2，用gedit打开example2.xml,将变量N的值由3改为2,并保存
 
 	![square2](http://i.imgur.com/vpDtoOF.jpg)

 9. 将dol/build/bin/main文件夹中的example2整个文件夹删除，同样，如果example2文件夹右下角有锁的图标没办法删除，则先在终端中用以下命令解锁再删除它

   `sudo chown -R 用户名 文件名`

 10. 进入dol文件夹并编译dol
         cd
         cd dol
         ant -f build_zip.xml all

     ![回退编译](http://i.imgur.com/Dm5WDO6.jpg)

 11. 进入build/bin/mian路径下,再次运行example2
         cd build/bin/main
         ant -f runexample.xml -Dnumber=2

        ![example2(2)](http://i.imgur.com/XW9GJ4u.png)
		
		![example2(2)dot](http://i.imgur.com/aPb8eLB.png)

 8. 鼠标点击进入dol/examples/example1/src，将square.c、square.h文件分别重命名为cubic.c、cubic.h,用记事本打开cubic.c、cubic.h、example1.xml、map_1.xml、map_3.xml,将其中所有有关square的变量改为cubic,并保存
   - cubic.c:
   ![cubic.c](http://i.imgur.com/XbbKChx.jpg)

   - cubic.h:
   ![cubic.h](http://i.imgur.com/VGXBA8w.jpg)

   - example1.xml:
   ![example1](http://i.imgur.com/TFU5xa3.jpg)

   - map_1.xml:
   ![map_1](http://i.imgur.com/6c1dHZl.jpg)

   - map_3.xml:
   ![map_3](http://i.imgur.com/CoErQlB.jpg)
  
- ## 代码分析 ##
 1. square.c
    	 int square_fire(DOLProcess *p) {
    	 	float i;
    	 	if (p->local->index < p->local->len) {
        		DOL_read((void*)PORT_IN, &i, sizeof(float), p);
        		i = i*i;
        		DOL_write((void*)PORT_OUT, &i, sizeof(float), p);
        		p->local->index++;
    		}
    		if (p->local->index >= p->local->len) {
        		DOL_detach(p);
        		return -1;
    		}
    		return 0;
		 }
  在square_fire函数中，调用DOL_read函数在square端口“PORT_IN”读到值`i`，然后将`i*i`赋值给`i`，最后调用DOL_write函数将i值写到square的端口“PORT_OUT”，因此，要输出3次方数，将`i*i*i`赋值给`i`即可。
 1. example2.xml
         <variable value="3" name="N"/>
         <!-- instantiate resources -->
         <process name="generator">
         	<port type="output" name="10"/>
    		 <source type="c" location="generator.c"/>
  	   </process>
         <iterator variable="i" range="N">
             <process name="square">
         	    <append function="i"/>
      	       <port type="input" name="0"/>
      	       <port type="output" name="1"/>
      	       <source type="c" location="square.c"/>
    	     </process>
         </iterator>

   exampleXXX.xml定义了系统架构（模块连接方式），在example2.xml中定义了生产者模块（generator）、迭代产生3个平方模块（square）、消费者模块（customer），每个模块之间通过sw_channel连接，因此，要产生2个square模块，将迭代的次数N改为2即可。

- ## 实验感想 ##
     在操作系统课程中，我们的实验内容是修改pintos，所以这次实验我能很快适应dol的代码风格，加上对xml代码比较熟悉，所以对这次实验的代码理解没有遇到较大的问题，一旦理解了代码，修改代码完成实验任务也就没有什么困难了。
