---
title: CSharpUI设计问题
date: 2022-12-12T12:12:57.000Z
updated: '2024-07-01 21:56:15'
categories:
  - C#
tags:
  - C#
  - Tools
---
C# 构筑软件的各种小问题，记录一下
<!-- more -->

## 配置问题
Q : “xxxxxx.Form是“命名空间”，但此处被当做“类型”来使用”的解决
A ：出现此问题是因为当前文件夹是以`Form`命名的，与要继承的`Form`类出现冲突 ，建议将`Form`文件夹
名字改成`FormPage`就完事了

##  多线程进度条

1. 编写线程所要执行的方法
2. 实例化Thread类，并传入一个指向线程所要执行方法的委托。（这时线程已经产生，但还没有运行）
3. 调用Thread实例的Start方法，标记该线程可以被CPU执行了，但具体执行时间由CPU决定

##### 方法一 多线程进度条
```cpp
//需要开线程的方法 无参
Thread thread = new Thread(new ThreadStart(method));//创建线程
thread.Start();                                     //启动线程

//需要开线程的方法 有参
Thread thread=new Thread(new ParameterizedThreadStart(method));//创建线程
thread.Start(3);                                               //启动线程
```
```csharp
//单开 Form 显示进度条，且显示后关闭
private void progressThread(int length){
    ProgressForm progress = new ProgressForm(); // 自己的Form
    progress.Show(); //Form显示
    //以下For 循环改成自己的进度条显示的逻辑
    for (int i = 0; i < 100; i++) 
            {
                progress.Addprogess();
                Thread.Sleep(50);
            }
    progress.Close(); // Form关闭
}
```
```csharp
class ProgressForm ： From{
    public void Addprogress(){
        this.progressBar.Value++;
    }
}
```

##### 方法二 委托同步更新进度条
```csharp
using System.Threading;  //记得导线程的包
using System.Threading.Tasks;
public partial class MainForm : Form {
    public delegate void DelegateProcess();//创建委托
    
    private void button1_Click(object sender, EventArgs e){
        //按钮下方法
        private int n = 0;
        ProgressForm progress = new ProgressForm();//实例化ProgressForm
        progress.Show();//显示ProgressForm窗口实例
        Thread thread = new Thread(new ParameterizedThreadStart(progressThread));//创建线程

        thread.Start();//启动线程
    }

    public void thread(object length) //对象必须是object,不能 是具体类型
    {
        int temp  = 1;
        for (int i = 0; i < 3000; i++){
            if(i/30 > temp){
                this.Invoke(new DelegateProcess(progress.Addprogress));
                temp++;
            }
            this.Invoke(new addProgress(DataNumberAdd));//同步执行委托
            Thread.Sleep(50);//使线程挂起50毫秒，减缓进度条的变化
        }
        
    }
}
```

### Assembly.GetManifestResourceStream总是返回null。
A ：右键点击该文件，在属性中选择生成操作为嵌入的资源「埋め込まれたリソース」即可

### 关于Form.Shwo()方法
此方法非阻塞，但又没说线程
思考：每个show方法都单独开了子线程，只不过这个方法是非阻塞的，
 相反，DialogForm就是阻塞的，但是查了，又说Form和线程没关系，那么到底Form底层是创建了
 线程吗？

### 关于load 和show

1. load比show触发的更早；
2. load在第一次显示窗体前发生，只发生一次；
3. show在第一次显示窗体时发生，也只发生一次，之后无论窗体最小化、最大化、隐藏、还原都不会引发该事件

### 为啥要用委托，它和直接调用有什么区别
多线程调用跨线程的方法 必须 使用委托，不然窗口假死
```csharp
//委托使用
// void 是需要委托方法 的返回值
//Parameter para 是需要委托方法 的参数
//1. 定义
public delegate void delegateName(Parameter para,Parameter para1);

//2. 方法实现
private void func(Parameter para){ //void 与上对应 ，参数与参数 对应 无参就不写
    ...
}

//3. 委托调用 
this.invoke(new delegateName(func),new object[]{para,para1}) //无参 new object就不需要写

// 作用
//1. 相当于用方法作为另一方法参数(类似于C++的函数指针)
//2. 在两个不能直接调用的方法中作为桥梁,如:在多线程中的跨线程的方法调用就得用委托
//3. 当不知道方法具体实现什么时使用委托,如:事件中使用委托
//4. 解耦

```

### 主线程等待子线程执行完继续
常用的就1 2 了，其他作为了解

1. t1.join()
2. while(t1.isAlive())
3. while(Thread.activeCount() > 1)

### 进度条实时更新数据一种方法
利用计时器功能，每秒查次db更新数据
```csharp
//控件设置一个timer1 添加tick方法(从控件处手动生成)
//控件设置一个timer2 添加tick方法(从控件处手动生成)

public class Test : From{
    System.Diagnostics.StopWatch sw;

    public void Test_Load(object sender, EventArgs e){
        //指定时间间隔触发 单位毫秒 
        timer2.Interval = 10000; //10s一次
        sw = new System.Diagnostics.StopWatch();
        sw.start();
        timer1.start();
        timer2.start();
        
        
    }

    //此计时器统计时间
    private void timer1_Tick(object sender, EventArgs e){
        TimeSpan ts = sw.Elapsed;
        label_time.text = label_time.Text = String.Format("{0}時{1}分{2}秒", ts.Hours, ts.Minutes, ts.Seconds);
    }

    private void timer2_Tick(object sender, EventArgs e){
        String data = xxxservice.getData();

        ... //处理数据更新到画面

        if(flag){
            //进度条已结束
            //再获取最后一条数据更新到画面
            String data = xxxservice.getData();

            timer2.Stop();
            sw.Stop();
            TimeSpan ts = sw.Elapsed;
            label_time.Text = String.Format("{0}時{1}分{2}秒", ts.Hours, ts.Minutes, ts.Seconds);
        }
    }
}

```

### Form关闭前后的处理添加
点击就行
![image.png](/images/f2237b61254fac829dd9315c7343bad6.png)
