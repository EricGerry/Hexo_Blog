---
title: Book_Manager
date: 2019-05-11 21:54:10
tags:
    - java
    - 项目
categories:
    - java
    - 项目
---
![ ](./Book-Manager/BookManager.jpg)

## 代码实现（基于封装多态）

### Book

```java
package Package_BookManager.Book;
public class book {
    private String name;
    private String id;
    private String author;
    private int price;
    private String type;
    private boolean isBorrowed;

    public book(String name, String id, String author, int price, String type, Boolean isBorrowed) {
        this.name = name;
        this.id = id;
        this.author = author;
        this.price = price;
        this.type = type;
        this.isBorrowed = isBorrowed;
    }

    public String getName() {
        return name;
    }

    public String getId() {
        return id;
    }

    public boolean isBorrowed() {
        return isBorrowed;
    }

    public void setBorrowed(boolean borrowed) {
        isBorrowed = borrowed;
    }

    @Override
    public String toString() {
        return "book{" +
                "name='" + name + '\'' +
                ", id='" + id + '\'' +
                ", author='" + author + '\'' +
                ", price=" + price +
                ", type='" + type + '\'' +
                ", isBorrowed=" + isBorrowed +
                '}';
    }
}
```

```java
package Package_BookManager.Book;

public class bookList {
    private book[] books=new book[100];
    private int size;

    public bookList() {
        books[0]=new book("C语言","001","mou1",100,"计算机",false);
                books[1]=new book("水浒","002","施耐庵",58,"古典名著",true);
        books[2]=new book("西游记","003","吴承恩",103,"古典名著",false);
        books[3]=new book("三国","004","罗贯中",124,"古典名著",false);
        size=4;
    }

    public int getSize() {
        return size;
    }

    public void setSize(int size) {
        this.size = size;
    }

    public book getBooks(int index) {
        return books[index];
    }

    public void setBooks(book books,int index) {
        this.books[index] = books;
    }
}
```

### Operation

```java
package Package_BookManager.Operation;

import Package_BookManager.Book.bookList;

public interface Ioperation {
    void work(bookList bookList);
}
```

```java
package Package_BookManager.Operation;

import Package_BookManager.Book.book;
import Package_BookManager.Book.bookList;

import java.util.Scanner;

public class AddOperation implements Ioperation {
    @Override
    public void work(bookList bookList) {
        System.out.println("新增一本书籍");
        Scanner scanner = new Scanner(System.in);
        System.out.println("请输入书名:");
        String name = scanner.next();
        System.out.println("请输入序号:");
        String id = scanner.next();
        System.out.println("请输入作者: ");
        String author = scanner.next();
        System.out.println("请输入价格:");
        int price = scanner.nextInt();
        System.out.println("请输入类别: ");
        String type = scanner.next();
        book book = new book(name, id,
                author, price, type, false);
        bookList.setBooks(book, bookList.getSize());
        bookList.setSize(bookList.getSize() + 1);
    }
}
```

```java
package Package_BookManager.Operation;

import Package_BookManager.Book.book;
import Package_BookManager.Book.bookList;

import java.util.Scanner;

public class BorrowOperation implements Ioperation {
    @Override
    public void work(bookList bookList) {
        System.out.println("借阅书籍");
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入要借阅的书号：");
        String id = sc.next();
        for (int i = 0; i < bookList.getSize(); i++) {
            book book = bookList.getBooks(i);
            if (!book.getId().equals(id)) {
                continue;
            }
            if (book.isBorrowed()) {
                System.out.println("这本书已经被借走！");
                break;
            }
            book.setBorrowed(true);
        }

    }
}
```

```java
package Package_BookManager.Operation;

import Package_BookManager.Book.book;
import Package_BookManager.Book.bookList;

import java.util.Scanner;

public class DelOperation implements Ioperation {
    @Override
    public void work(bookList bookList) {
        System.out.println("删除书籍");
        Scanner sc = new Scanner(System.in);
        System.out.print("请输入要删除的书号：");
        String id = sc.next();
        int i = 0;
        for (; i < bookList.getSize(); i++) {
            book book = bookList.getBooks(i);
            if (book.getId().equals(id)) {
                break;
            }
        }
        if (i > bookList.getSize()) {
            System.out.println("未找到要删除的书籍");
            return;
        }
        book lastbook = bookList.getBooks(bookList.getSize() - 1);
        bookList.setBooks(lastbook, i);
        bookList.setSize(bookList.getSize() - 1);
        System.out.println("删除成功");

    }
}
```

```java
package Package_BookManager.Operation;

import Package_BookManager.Book.bookList;

public class ExitOperation implements Ioperation {
    @Override
    public void work(bookList bookList) {
        System.out.println("GoodBye!");
        System.exit(0);
    }
}
```

```java
package Package_BookManager.Operation;

import Package_BookManager.Book.book;
import Package_BookManager.Book.bookList;

import java.util.Scanner;

public class FindOperation implements Ioperation {
    @Override
    public void work(bookList bookList) {
        System.out.println("查找书籍");
        System.out.print("请输入要查找的书名：");
        Scanner sc = new Scanner(System.in);
        String name = sc.next();
        int count = 0;
        for (int i = 0; i < bookList.getSize(); i++) {
            book book = bookList.getBooks(i);
            if (book.getName().equals(name)) {
                System.out.println(book);
                count++;
            }
        }
        if (count == 0) {
            System.out.println("未找到该书籍！");
        } else {
            System.out.println("共计找到" + count + "本" + name);
        }

    }
}
```

```java
package Package_BookManager.Operation;

import Package_BookManager.Book.bookList;

public class PrintAllOperation implements Ioperation {
    @Override
    public void work(bookList bookList) {
        System.out.println("打印所有书籍信息");
        for (int i=0;i<bookList.getSize();i++){
            System.out.println(bookList.getBooks(i));
        }
        System.out.println("共计"+bookList.getSize()+"本书");
    }
}
```

```java
package Package_BookManager.Operation;

import Package_BookManager.Book.book;
import Package_BookManager.Book.bookList;

import java.util.Scanner;

public class ReturnOperation implements Ioperation {
    @Override
    public void work(bookList bookList) {
        System.out.println("归还书籍");
        Scanner sc=new Scanner(System.in);
        System.out.print("请输入要归还书籍的编号：");
        String id=sc.next();
        for(int i=0;i<bookList.getSize();i++){
            book book=bookList.getBooks(i);
            if(!book.getId().equals(id)){
                continue;
            }
            if(!book.isBorrowed()){
                System.out.println("这本书已经被归还");
                break;
            }
            book.setBorrowed(false);

        }
    }
}
```

### User

```java
package Package_BookManager.User;

import Package_BookManager.Book.bookList;
import Package_BookManager.Operation.Ioperation;

abstract public class User {
    protected String name;
    protected Ioperation[] operations;

    public User(String name) {
        this.name = name;
    }

    abstract public int menu();

    public void doOperation(int choice, bookList bookList) {
        operations[choice].work(bookList);
            }
}
```

```java
package Package_BookManager.User;

import Package_BookManager.Operation.*;

import java.util.Scanner;

public class Admin extends User {
    public Admin(String name) {
        super(name);
        operations = new Ioperation[]{
                new ExitOperation(),
                new FindOperation(),
                new AddOperation(),
                new DelOperation(),
                new PrintAllOperation(),
        };
    }

    @Override
    public int menu() {
        System.out.println("============");
        System.out.println("hello " + name);
        System.out.println("1. 查找书籍");
        System.out.println("2. 增加书籍");
        System.out.println("3. 删除书籍");
        System.out.println("4. 打印所有信息");
        System.out.println("0. 退出");
        System.out.println("============");
        System.out.println("请输入您的选择: ");
        Scanner scanner = new Scanner(System.in);
        int choice = scanner.nextInt();
        return choice;
    }
}
```

```java
package Package_BookManager.User;

import Package_BookManager.Operation.*;

import java.util.Scanner;


public class NormalUser extends User {
      public NormalUser(String name) {
        super(name);
// 在这里构造 operation 数组
        // 我们让数组中 operation 对象的顺序和菜单中的序号相匹配
        operations = new Ioperation[]{
                new ExitOperation(),
                new FindOperation(),
                new BorrowOperation(),
                new ReturnOperation()
        };
    }

    @Override
    public int menu() {
        System.out.println("============");
        System.out.println("hello " + name);
        System.out.println("1. 查找图书");
        System.out.println("2. 借阅图书");
        System.out.println("3. 归还图书");
        System.out.println("0. 退出");
        System.out.println("============");
        System.out.println("请输入您的选择: ");
        Scanner scanner = new Scanner(System.in);
        int choice = scanner.nextInt();
        // close 本质上是在关闭 System.in
        // 由于后面还需要用到 System.in, 此处不能盲目关闭.
        // scanner.close();
        return choice;
    }
}
```

### Test

```java
package Package_BookManager;

import Package_BookManager.Book.bookList;
import Package_BookManager.User.Admin;
import Package_BookManager.User.NormalUser;
import Package_BookManager.User.User;

import java.util.Scanner;

public class Test {
    public static void main(String[] args) {
        bookList bookList = new bookList();
        User User = login();
        while (true) {
            int choice = User.menu();
            User.doOperation(choice, bookList);
        }

    }


    public static User login() {
        Scanner scanner = new Scanner(System.in);
        System.out.println("请输入您的姓名:");
        String name = scanner.next();
        System.out.println("请输入您的角色:(1 普通用户 2 管理员)");
        int role = scanner.nextInt();
        if (role == 1) {
            return new NormalUser(name);
        }
        return new Admin(name);
    }
}
```
