---
title: StackAndQueue
date: 2019-07-10 20:48:10
tags:
    - Stack
    - Queue
    - java
categories:
    - java
---

![ ](./StackAndQueue/1.jpg)

## 实现一个简单的栈

```java
public class MyStack {
    private int[] array=new int[100];
    private int size=0;
    //入栈
    public void push(int x){
        array[size] = x;
        size++;
    }
    //出栈
    public Integer pop(){
        if(size == 0){
            return null;
        }
        int ret = array[size - 1];
        size--;
        return ret;
    }
    //获取栈顶元素
    public Integer peek(){
        if(size == 0){
            return null;
        }
        return array[size - 1];
    }
    public int getSize(){
        return size;
    }
    public boolean isEmpty(){
        return size==0;
    }
}
```

## 用链表和数组实现队列

### 链表实现

```java
class Node {
    public Node next;
    public int val;

    public Node(int val) {
        this.val = val;
    }
    }

public class MyQueue {
    private Node head = null;
    private Node Tail = null;
    private int size = 0;

    //入队列
    public void offer(int x) {
        Node newNode = new Node(x);
        //处理头结点为空的情况，直接插入
        if (head == null) {
            head = newNode;
            Tail = newNode;
            size++;
            return;
        }
        //头结点不为空
        Tail.next = newNode;
        Tail = Tail.next;
        size++;
        return;
    }

    //出队列
    public Integer poll() {
        //头结点为空直接返回null
        if (head == null) {
            return null;
        }
        //返回头结点的val
        Integer ret = head.val;
        head = head.next;
        //处理队列里只有一个元素的情况
        if (head == null) {
            //修改Tail的指向
            Tail = null;
        }
        size--;
        return ret;
    }

    //取队首元素
    public Integer peek() {
               if (head == null) {
            return null;
        }
        return head.val;
    }

    //判定队列为空
    public boolean isEmpty() {
        return head == null;
    }

    public int getSize() {
        return size;
    }

    public static void main(String[] args) {
        MyQueue MyQueue = new MyQueue();
        MyQueue.offer(1);
        MyQueue.offer(2);
        MyQueue.offer(3);
        MyQueue.offer(4);
        while (!MyQueue.isEmpty()) {
            int curFront = MyQueue.peek();
            System.out.print(curFront+"\t");
            MyQueue.poll();
        }
    }
}
```

### 数组实现

```java
public class MyQueue2 {
    private int[] data = new int[100];
    // [head, tail)
    private int head = 0; // 队首元素的下标
    private int tail = 0; // 队尾元素的下标
    private int size = 0;

    // 1. 入队列, 如果插入成功, 返回 true, 否则返回 false
    //    如果队列满了, 再插入就会失败
    public boolean offer(int x) {
        if (size == data.length) {
                       return false;
        }
        // 新元素放到 tail 的位置上
        data[tail] = x;
        tail++;
        if (tail == data.length) {
            tail = 0;
        }
        size++;
        return true;
    }

    // 2. 出队列
    public Integer poll() {
        if (size == 0) {
            return null;
        }
        Integer ret = data[head];
        head++;
        if (head == data.length) {
            head = 0;
        }
        size--;
        return ret;
    }

    // 3. 取队首元素
    public Integer peek() {
        if (size == 0) {
            return null;
        }
        return data[head];
    }

    // 4. 判定为空
    public boolean isEmpty() {
        return size == 0;
    }

    // 5. 取长度
    public int size() {
        return size;
    }
    public static void main(String[] args) {
        MyQueue2 myQueue2 = new MyQueue2();
        myQueue2.offer(1);
        myQueue2.offer(2);
        myQueue2.offer(3);
        myQueue2.offer(4);
        while (!myQueue2.isEmpty()) {
            Integer cur = myQueue2.peek();
            System.out.print(cur+"\t");
            myQueue2.poll();
        }
    }
}
```

## 括号匹配问题

- 采用遇到左括号入栈
- 再从栈顶区出，与右括号匹配

```java
public boolean isValid(String s) {
        Stack<Character> stack = new Stack<Character>();
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (c == '[' || c == '(' || c == '<') {
                stack.push(c);
                continue;
            }
            if (stack.isEmpty()) {
                return false;
            }
            char top = stack.pop();
            if (top == '(' && c == ')') {
                continue;

            }
            if (top == '{' && c == '}') {
                continue;
            }
            if (top == '[' && c == ']') {
                continue;
            }
            return false;
        }
        if (stack.isEmpty()) {
            return true;
        }
         return false;
}
```

## 用队列实现栈

```java
import java.util.LinkedList;

public class MyStackBy2Queue {
    //A始终用来入栈，B始终用来出栈
    private LinkedList<Integer> A = new LinkedList<Integer>();
    private LinkedList<Integer> B = new LinkedList<Integer>();

    public void push(int x) {
        A.offer(x);
    }

    public int pop() {
        if (A.isEmpty()) {
            return 0;
        }
        while (A.size() > 1) {
            int cur = A.removeFirst();
            B.addLast(cur);
        }
        int ret = A.removeFirst();
        swap();
        return ret;

    }

    private void swap() {
        LinkedList<Integer> tmp = A;
        A = B;
        B = tmp;
    }
    public int peek(){
        if (A.isEmpty()) {
            return 0;
        }
        while (A.size() > 1) {
            int cur = A.removeFirst();
            B.addLast(cur);
        }
        // 最终要出栈的元素
        int ret = A.removeFirst();
               B.addLast(ret);
        // 交换 AB
        swap();
        return ret;
    }
    public boolean isEmpty(){
        return A.isEmpty();
    }
}
```

## 栈实现队列

```java
import java.util.Stack;

public class QueueBY2Stack {
    private Stack<Integer> A=new Stack<Integer>();
    private Stack<Integer> B=new Stack<Integer>();
    public void push(int x){
        while (!B.isEmpty()){
            int tmp=B.pop();
            A.push(tmp);
        }
        A.push(x);
    }
    public int pop(){
        if(A.isEmpty()&&B.isEmpty()){
            return 0;
        }
        while(!A.isEmpty()){
            int tmp=A.pop();
            B.push(tmp);
        }
        return B.pop();
    }
    public int peek(){
        if(A.isEmpty()&&B.isEmpty()){
            return 0;
        }
        while (!A.isEmpty()){
            int tmp=A.pop();
            B.push(tmp);
        }
        return B.peek();
    }
    public boolean isEmpty(){
         return A.isEmpty()&&B.isEmpty();
    }
}
```

## 最小栈

```java
import java.util.Stack;

class MinStack {
    private Stack<Integer> A=new Stack<Integer>();
    private Stack<Integer> B=new Stack<Integer>();

    /** initialize your data structure here. */
    public MinStack() {

    }

    public void push(int x) {
        A.push(x);
        if(B.isEmpty()){
            B.push(x);
            return;
        }
        int min=B.peek();
        if(x<min){
            min=x;
        }
        B.push(min);
    }

    public void pop() {
        if(A.isEmpty()){
            return;
        }
        A.pop();
        B.pop();
    }

    public int top() {
        return A.peek();
    }

    public int getMin() {
        return B.peek();
            }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
 ```
