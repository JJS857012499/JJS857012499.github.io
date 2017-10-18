---
title: Thread的join方法应用
date: 2017-10-18 16:51:58
category: java基础
tags: [Thread, join]
---
如题：现在有T1、T2、T3三个线程，你怎样保证T2在T1执行完后执行，T3在T2执行完后执行
解决思路：应用Thread的join方法，看方法注释，大意就是会等待线程die掉，
T3先执行，在T3的run中，调用t2.join，让t2执行完成后再执行t3，在T2的run中，调用t1.join，让t1执行完成后再让T2执行
```java

package com.zhidian.test;

/**
 * Created by 江俊升 on 2017/10/17.
 */
public class TestJoin {

    public static void main(String[] args){
        Thread t1 = new Thread(){
            @Override
            public void run() {
                super.run();
                System.out.println("t1执行...");
            }
        };
        Thread t2 = new Thread(){
            @Override
            public void run() {
                super.run();
                try {
                    t1.join(); //文档： Waits for this thread to die
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println("t2执行...");
            }
        };
        Thread t3 = new Thread(){
            @Override
            public void run() {
                super.run();
                try {
                    t2.join();
                    t2.wait();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println("t3执行");
            }
        };
        t1.start();
        //这里模拟卡0.1s
        try {
            Thread.currentThread().sleep(100);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        t2.start();
        t3.start();
    }
}

```

