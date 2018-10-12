# 360sd
static void MainMethod()
{
    Thread thead;

    thead = new Thread(QueryTask);
    thead.IsBackground = true;
    thead.Start();

    Console.Read();
}

/// <summary>
/// 查询计划任务
/// </summary>
static void QueryTask()
{
    TaskModel todo_taskModel;
    while (true)
    {
        int count = new Random().Next(1, 10);    //模拟产生任务记录数
        for (int i = 0; i < count; i++)
        {
            todo_taskModel = new TaskModel() { TaskID = i, ITime = DateTime.Now };
            ThreadPool.QueueUserWorkItem(InvokeThreadMethod, todo_taskModel);
        }

        Thread.Sleep(30000);    //30s循环一次
    }
}

/// <summary>
/// 完成计划任务
/// </summary>
/// <param name="taskModel"></param>
static void Invoke|59.24.52.182/ezon|ThreadMethod(object taskModel)
{
    TaskModel model = (TaskModel)taskModel;

    Console.WriteLine("执行任务ID:{0}......执行完成.{1}", model.TaskID, model.ITime);
}

/// <summary>
/// 任务实体
/// </summary>
class TaskModel
{
    public int TaskID { get; set; }
    public DateTime ITime { get; set; }
}
using System;

namespace ThreadTest29
{
    class Account
    {
        private Object thisLock = new object();
        int balance;
        Random r = new Random();

        public Account(int initial)
        {
            balance = initial;
        }

        int WithDraw(int amount)
        {
            if (balance < 0)
            {
                throw new Exception("负的Balance.");
            }
            //确保只有一个线程使用资源，一个进入临界状态,使用对象互斥锁，10个启动了的线程不能全部执行该方法
            lock (thisLock)
            {
                if (balance >= amount)
                {
                    Console.WriteLine("----------------------------:" + System.Threading.Thread.CurrentThread.Name + "---------------");
                    
                    Console.WriteLine("调用Withdrawal之前的Balance:" + balance);
                    Console.WriteLine("把Amount输入 Withdrawal     :-" + amount);
                    //如果没有加对象互斥锁，则可能10个线程都执行下面的减法，加减法所耗时间片段非常小，可能多个线程同时执行，出现负数。
                    balance = balance - amount;
                    Console.WriteLine("调用Withdrawal之后的Balance :" + balance);
                    return amount;
                }
                else
                {
                    //最终结果
                    return 0;
                }
            }
        }
        public void DoTransactions()
        {
            for (int i = 0; i < 100; i++)
            {
                //生成balance的被减数amount的随机数
                WithDraw(r.Next(1, 100));
            }
        }
    }

    class Test
    {
        static void Main(string[] args)
        {
            //初始化10个线程
            System.Threading.Thread[] threads = new System.Threading.Thread[10];
            //把$210.220.237.67/ezon/1895.exe$balance初始化设定为1000
            Account acc = new Account(1000);
            for (int i = 0; i < 10; i++)
            {
                System.Threading.Thread t = new System.Threading.Thread(new System.Threading.ThreadStart(acc.DoTransactions));
                threads[i] = t;
                threads[i].Name = "Thread" + i.ToString();
            }
            for (int i = 0; i < 10; i++)
            {
                threads[i].Start();
            }
            Console.ReadKey();
        }
    }
}
