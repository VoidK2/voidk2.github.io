---
layout:  post
title: 寒假写的几个小东西
subtitle: 啊呸
date: 2019-02-27
author: zhangzexin
header-img: img/post-bg-unix-linux.jpg
catalog: true
tags:
    - python
    - java
---

## Mysql Procedure
 -mysql的存储过程，生成三十万数据
```sql
-- 一天1000，持续365
-- 存储过程
delimiter $$
DROP PROCEDURE if exists myproc
CREATE PROCEDURE myproc()
BEGIN
DECLARE i INT;
DECLARE j INT DEFAULT 0;
DECLARE id_r1 INT;
DECLARE num_r1 INT;
DECLARE dt varchar(100);
DECLARE chars_str varchar(100) DEFAULT 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789';
DECLARE name_str varchar(255) DEFAULT '';
	SET i=0;
		loop1:WHILE i<=1000 DO
			SET name_str='';
			SET j=0;
			loop2:WHILE j < 10 DO
        		SET name_str = concat(name_str,substring(chars_str , FLOOR(1 + RAND()*62 ),1));
        		SET j = j +1;
    		END WHILE loop2;
			select RAND()*50000 into id_r1;
			select RAND()*50000 into num_r1;
			SET dt=DATE_FORMAT(NOW(),'%Y-%m-%d-%k-%i-%s');
			INSERT INTO table1(id,name) VALUES(id_r1,name_str);
			INSERT INTO table2(order1,name,num) VALUES(dt,name_str,num_r1);
			SET i=i+1;
		END WHILE loop1;
END $$
delimiter ;

-- 开启定时
show variables like '%event_sche%';
set global event_scheduler=1;
-- 创建定时事件
CREATE EVENT if not exists e_test
on schedule every 1 day ends current_timestamp()+interval 365 day
on completion perserve
do call myproc();


-- 一次30万 预计要执行
-- 存储过程
delimiter $$
DROP PROCEDURE if exists myproc
CREATE PROCEDURE myproc()
BEGIN
DECLARE i INT;
DECLARE j INT DEFAULT 0;
DECLARE id_r1 INT;
DECLARE num_r1 INT;
DECLARE dt varchar(100);
DECLARE chars_str varchar(100) DEFAULT 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789';
DECLARE name_str varchar(255) DEFAULT '';
	SET i=0;
		loop1:WHILE i<=300000 DO
			SET name_str='';
			SET j=0;
			loop2:WHILE j < 10 DO
        		SET name_str = concat(name_str,substring(chars_str , FLOOR(1 + RAND()*62 ),1));
        		SET j = j +1;
    		END WHILE loop2;
			select RAND()*50000 into id_r1;
			select RAND()*50000 into num_r1;
			SET dt=DATE_FORMAT(NOW(),'%Y-%m-%d-%k-%i-%s');
			INSERT INTO table1(id,name) VALUES(id_r1,name_str);
			INSERT INTO table2(order1,name,num) VALUES(dt,name_str,num_r1);
			SET i=i+1;
		END WHILE loop1;
END $$
delimiter ;

-- 执行
call myproc();

```
## Java位运算
- 长整型数据的存储与运算（集合）
```java
import java.util.ArrayList;
import java.util.Collection;
import java.util.List;

import static java.lang.Integer.max;
import static java.lang.Integer.min;

/**
 * For complexity analysis, let m be this.max and n be the number of elements in this set.
 */

public class nSet {
    // this class implements the set operations over sets of natural numbers.
    public int Max;        // maximal natural number in any set
    private int n_long;    // the number of long integers: 64*n_long > Max
    private long[] store;  // the store has n_long longs
    private int size;      // remember the size of the current set

    // Constructor: runtime = O(m), with a small constant less than 1
    public nSet(int n) {
        this.Max = n;
        if (n < 0) { this.Max = 1; }
        this.n_long = (n >> 6) + 1; // n_long = n/64+1, number of longs
        this.store = new long[n_long];
        for (int i = 0; i<this.n_long; i++) this.store[i] = 0;
        this.size = 0;   // the empty set: 
    }

    //Constant runtime
    public void add(int x) {
        // add x into the set
        if (x < 0 || x > this.Max) return;
        int i = (x>>6);     // i = x/64
        int j = x - (i<<6); // j = x % 64, i.e., 64i + j = x
        long y = this.store[i];
        if (((y>>>j) & 1) == 1) return;   // if x is present, do nothing
        this.store[i] |= ((long) 1<<j);  // "|" is the bitwise OR operation
        this.size++;
    }


    //Constant runtime
    public boolean find(int x) {
        // decide if x is in the set
        if (x < 0 || x > this.Max) return false;
        int i = (x>>6);     // i = x/64
        int j = x - (i<<6); // j = x % 64, i.e., 64i + j = x
        long y = this.store[i];
        return ((y>>>j) & 1) == 1; // "&" is the bitwise OR operation
    }


    //O(m) runtime, with a small constant less than 1
    public void clear () {
        for(int i = 0; i<this.n_long; i++) store[i]=0;
        this.size = 0;
    }


    // Constant runtime
    public int size () {
    	return this.size;
    }
    

    // O(m) linear time
    private void set_size () {
        int counter = 0;
        for(int i = 0; i<this.n_long; i++) {
            for (int j = 0; j < 64; j++) {
                if(((this.store[i]>>>j) & 1) == 1)
                    counter++;
            }
        }
        this.size = counter;
    }

    // Constant runtime
    public void print () {
        // print up to 30 numbers in the current nSet
    	System.out.print("{ ");
    	int count = 0;
        for(int i=0; i<this.n_long; i++) 
            for(int j=0; j<64 ; j++) 
                if (((this.store[i] >>> j) & 1) == 1) {                	
                    System.out.print(((i << 6) + j)+", ");    
                    if (++count >= 30) {
                    	System.out.println("... }");
                    	return;
                    }
                }
        System.out.println("}");
    }


    // O(m) runtime
    public nSet union (nSet X) {
        int maximum = max(this.Max, X.Max);
        nSet A = new nSet(maximum);

        for(int i=0 ;i < n_long; i++) {
            A.store[i] = this.store[i] | X.store[i];
        }
        
        A.set_size();
        return A;
    }


    // You need to complete the implementation of the following operations:
    // Constant runtime
    public boolean isEmpty () {
        // return true iff the current nSet is empty
        if (this.size==0) {return true;}
        else{return false;}
    }
    // Constant runtime
    public boolean delete (int x) {
        // return false if x isn't in the set;
        // delete the number x from the current set and return true.
        if (x < 0 || x > this.Max) return false;
        int i = (x>>6);
        int j = x - (i<<6);
        long y = this.store[i];
        boolean flag=((y>>>j) & 1) == 1;
        if(flag==true){
            this.store[i]=0;
            this.size--;
            return true;
        }else{
            return false;
        }

    }

    //O(m)
	public nSet intersect (nSet X) {
	   // return a new nSet which is the intersection of the current nSet and X
        nSet Ta=new nSet(1000);
        int[] tmp=new int[1001];
        for(int i=0;i<=1000;i++){tmp[i]=0;}
        for(int i=0;i<this.n_long; i++) 
        for(int j=0; j<64 ; j++) 
            if (((this.store[i] >>> j) & 1) == 1) {                 
                tmp[(i<<6)+j]+=1;
            }
        for(int i=0;i<X.n_long; i++) 
        for(int j=0; j<64 ; j++) 
            if (((X.store[i] >>> j) & 1) == 1) {                 
                tmp[(i<<6)+j]+=2;
            }
        for(int i=0;i<=1000;i++){
            if(tmp[i]==3){
                //System.out.print(i+",");
                int x = i;
                int o = (x>>6); 
                int k = x - (o<<6);
                long y = Ta.store[o];
                if (((y>>>k) & 1) == 1) continue;
                Ta.store[o] |= ((long) 1<<k); 
                Ta.size++;   
            }
        }
        return Ta;
	} 
	
    //O(m)
    public nSet subtract (nSet X) {
	   // return a new nSet which is the subtraction of the current nSet by X
        nSet Ta=new nSet(1000);
        int[] tmp=new int[1001];
        for(int i=0;i<=1000;i++){tmp[i]=0;}
        for(int i=0;i<this.n_long; i++) 
        for(int j=0; j<64 ; j++) 
            if (((this.store[i] >>> j) & 1) == 1) {                 
                tmp[(i<<6)+j]+=1;
            }
        for(int i=0;i<X.n_long; i++) 
        for(int j=0; j<64 ; j++) 
            if (((X.store[i] >>> j) & 1) == 1) {                 
                tmp[(i<<6)+j]-=1;
            }
        for(int i=0;i<=1000;i++){
            if(tmp[i]==1){
                int x = i;
                int o = (x>>6); 
                int k = x - (o<<6);
                long y = Ta.store[o];
                if (((y>>>k) & 1) == 1) continue;
                Ta.store[o] |= ((long) 1<<k); 
                Ta.size++;   
            }
        }
        return Ta;
	} 

    //O(m)
    public nSet complement() {
	   // return a new nSet which is the complement of the current nSet
        nSet Ta=new nSet(1000);
        int tmp=0;
        int[] tmp2=new int[1001];
        for(int i=0;i<=1000;i++){tmp2[i]=0;}
        for(int i=0;i<this.n_long; i++) 
        for(int j=0; j<64 ; j++) 
            if (((this.store[i] >>> j) & 1) == 1) {                 
                tmp2[(i<<6)+j]+=1;
                //System.out.print(((i<<6)+j)+",");
            }
        for(int i=0;i<=1000;i++){
            if(tmp2[i]==0){
                int x = i;
                int o = (x>>6); 
                int j = x - (o<<6);
                long y = Ta.store[o];
                if (((y>>>j) & 1) == 1) continue;
                Ta.store[o] |= ((long) 1<<j); 
                Ta.size++;   
            }
        }
        return Ta;
	}

	//O(m^2)
	public boolean equal(nSet X) {
	   // return true iff X and the current nSet contain the same set of numbers
        if(this.size!=X.size){return false;}
        int[] tmp=new int[1001];
        int cnt=0;
        for(int i=0;i<=1000;i++){tmp[i]=0;}
        for(int i=0; i<this.n_long; i++) 
            for(int j=0; j<64 ; j++) 
                if (((this.store[i] >>> j) & 1) == 1) {                 
                    tmp[(i<<6)+j]+=1;
                }
        for(int i=0; i<X.n_long; i++) 
            for(int j=0; j<64 ; j++) 
                if (((X.store[i] >>> j) & 1) == 1) {                 
                    tmp[(i<<6)+j]+=2;
                }
        for(int i=0;i<=1000;i++){
            if(tmp[i]==3){cnt++;}

        }
        if(this.size==cnt){return true;}
        else{return false;}
	}
	
	public boolean isSubset(nSet X) {
	   // return true iff X is a subset of the current nSet
        int[] tmp=new int[1001];
        int cnt=0;
        for(int i=0;i<=1000;i++){tmp[i]=0;}
        for(int i=0;i<this.n_long; i++) 
        for(int j=0; j<64 ; j++) 
            if (((this.store[i] >>> j) & 1) == 1) {                 
                tmp[(i<<6)+j]+=1;
            }
        for(int i=0;i<X.n_long; i++) 
        for(int j=0; j<64 ; j++) 
            if (((X.store[i] >>> j) & 1) == 1) {                 
                tmp[(i<<6)+j]+=1;
            }
        for(int i=0;i<=1000;i++){
            if(tmp[i]==1){cnt++;}
        }
        if(cnt==X.size){return true;}
        else{return false;}

	}

	// O(m) linear time
	public int[] toArray () {
	   // return an int array which contains all the numbers in the current nSet
	   int[] ans=new int[1000];
       int count=0;
       for(int i=0; i<this.n_long; i++) 
            for(int j=0; j<64 ; j++) 
                if (((this.store[i] >>> j) & 1) == 1) {                 
                    ans[count++]=(i << 6) + j;    
                }
        return ans;
    } 
	
	public void enumerate() {
	   // Enumerate all subsets of the current nSet and print them out.
	   // You may assume the current nSet contains less than 30 numbers.
        int[] enu=new int[1000];
        int cnt=0;
        for(int i=0; i<this.n_long; i++) 
            for(int j=0; j<64 ; j++) 
                if (((this.store[i] >>> j) & 1) == 1) {                 
                    enu[cnt++]=(i << 6) + j;    
                }
        int n=cnt;
        for(int i=0;i<(1<<n);i++){
                System.out.print("{");
                for(int j=0;j<n;j++){
                    if((i&(1<<j))!=0){
                        System.out.print(enu[j]+",");
                    }
                }
                System.out.print("} ");
            }
	}


    public static void main(String[] args) {
        // testing
        nSet A = new nSet(1000);

        for (int i = 1; i<A.Max; i += i) {
            A.add(i-1);
            A.add(i);
            A.add(i+1);
        }

        for (int i = 0; i<=A.Max; i++) {
            if (A.find(i)) System.out.println("found " + i + " in A");
        }
        
        
        // more testing code
        nSet B = new nSet(1000); // all natural numbers <= 1000, is a power of 2
        for(int i=2;i<B.Max;i*=2){
            B.add(i);
        }

        nSet C = new nSet(1000); // all odd natural numbers <= 1000  
        for(int i=1;i<C.Max;i+=2){
            C.add(i);
        }    

        nSet D = C.complement();
        
        nSet E = D.union(B);

        if (D.equal(E))
            System.out.println("D is equal to E");
        else
            System.out.println("D is not equal to E");

        nSet F = A.intersect(B);

        if (B.equal(F))
            System.out.println("B is equal to F");
        else
            System.out.println("B is not equal to F");

        nSet G = new nSet(1000); // all natural numbers <= 1000, and divisible by 8
        for(int i=8;i<C.Max;i+=8){
            G.add(i);
        }     
        nSet H = A.intersect(G);
        if (G.equal(H))
            System.out.println("G is equal to H");
        else
            System.out.println("G is not equal to H");

        nSet I = G.subtract(D);
        if (I.isEmpty())
            System.out.println("I is empty");
        else
            System.out.println("I is not empty");

        nSet J = H.intersect(E);
        
        nSet K = H.complement();
        
        // print out the sizes and members of A, B, C, D, E, F, G, H, I, J, K
        System.out.print(A.size() + " is the size of A = "); A.print();
        System.out.print(B.size() + " is the size of B = "); B.print();
        System.out.print(C.size() + " is the size of C = "); C.print();
        System.out.print(D.size() + " is the size of D = "); D.print();
        System.out.print(E.size() + " is the size of E = "); E.print();
        System.out.print(F.size() + " is the size of F = "); F.print();
        System.out.print(G.size() + " is the size of G = "); G.print();
        System.out.print(H.size() + " is the size of H = "); H.print();
        System.out.print(I.size() + " is the size of I = "); I.print();
        System.out.print(J.size() + " is the size of J = "); J.print();
        System.out.print(K.size() + " is the size of K = "); K.print();
        
        System.out.println("All subsets of H:");
        H.enumerate();
        //
    }
}

```
## 0x02 java文件输入格式化
- 格式化扫描端口
``` java
import java.io.*;
import java.net.*;
import java.util.regex.Pattern;
import java.util.regex.Matcher;
import org.apache.commons.net.telnet.TelnetClient;

public class t1{
	String fb;
	public static void main(String args[]){
		System.out.println("[begin]");
		t1 t11=new t1();
		String fb1=t11.readFile();	
		t11.cutAndScan(fb1);
		System.out.println("[end]");
	}
	public void cutAndScan(String fb1){
		TelnetClient telnet = new TelnetClient(); 
		Pattern pattern = Pattern.compile("[0-9]*");
		String[] sline=fb1.split("\n");
		String[] tmp;
		String[] port1;
		String ip=null,port=null;
        for (int i = 0; i < sline.length-1; i++){
			tmp=sline[i].split(":");
			ip=tmp[0];
			port=tmp[1];
			port1=port.split(",");
			for(int j=0;j<port1.length;j++){
				Matcher isNum = pattern.matcher(port1[j]);
				if(isNum.matches()){
				int portnum=Integer.valueOf(port1[j]);
				System.out.print("[success]"+ip+":"+port1[j]);
		try{
			telent.setConnectTimeout(30);//超时设置,单位毫秒，自己改
			telent.connect(ip,portnum);
			if(telnet!=null){
				System.out.printf("   True\n");
			}else{
				System.out.printf("   False\n");
				}
			//Socket server = new Socket();
			//InetSocketAddress address = new InetSocketAddress(ip,portnum);
			//server.connect(address,30);
			//server.close();
			
		}catch(Exception e){
			e.printStackTrace();
		}
	}else{System.out.println("[port error]"+ip+":"+port1[j]+" at line "+(i+1));}
	}
}	
	}
	
	public String readFile(){
		File file = new File("port_acl.data");
		if(!file.exists()){
			System.out.println("[file do not exists]");
		}
		try{
		InputStream is = new FileInputStream(file);
		byte[] bs=new byte[100000];
		int len=is.read(bs);
		is.close();
		fb=new String(bs);
		//System.out.println(fb);
		}catch(IOException e){
			e.printStackTrace();
		}
		return fb;
	}
}

```
