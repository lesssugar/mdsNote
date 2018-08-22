# IO

##项目内的文件路径基准
项目内相对路径是以项目为基准的
File f = new File("li.txt");
根目录下的li文件
##流的分类
output	将程序内存中的数据输出到磁盘中
input	将磁盘中的数据读取到程序的内存中
字节流	8bit	字符流	16bit
节点流	如果程序直接通过流对文件进行操作成为节点流
包括 FileInputStream FileOutPutStream FIleReader File Writer
如果讲一个节点流通过一个流包裹起来那么这个包裹的流就叫做操作流，可以对已有的节点流进行处理
抽象基类
InputStream	OutputStream	Reader	Writer
实现类
FileInputStream FileOutputStream	FileReader FileWriter
操作流
BufferedInputStream 	BufferedOutputStream	BufferedReader	BufferedWritter
转换流	字节流转换为字符流进行操作，read是编码过程，转换流做的是解码过程
InputStreamReader		OutputStreamWriter
##字节流
```java
/**
 * @Description: 文件的复制方法
 * @param: [被复制的文件路径, 复制到的文件路径, 传递数据的数组大小]
 * @return: void
 * @auther: lesssugar
 * @date: 2018/5/26 0026 22:00
 */
public void copyFile(String src, String dest,int arrLength) {
    //输入文件
    File fileInput = new File(src);
    //输出文件
    File fileOutput = new File(dest);
    //输入流
    FileInputStream fis = null;
    //输出流
    FileOutputStream fos = null;
    try {
        //初始化
        fis = new FileInputStream(fileInput);
        fos = new FileOutputStream(fileOutput);
        byte[] words = new byte[arrLength];
        int len;
        //通过数组进行字节流传递数据
        while ((len = fis.read(words)) != -1) {
            //写入到复制文件
            fos.write(words, 0, len);
        }
    } catch (FileNotFoundException e) {
        e.printStackTrace();
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        //关闭资源
        try {
            if (fis != null) {
                fis.close();
            }
            if (fos != null) {
                fos.close();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
##字符流
```java
字符流
/**
 * @Description: 文件的复制方法
 * @param: [被复制的文件路径, 复制到的文件路径, 传递数据的数组大小]
 * @return: void
 * @auther: lesssugar
 * @date: 2018/5/26 0026 22:00
 */
public void copyFile(String src, String dest,int arrLength) {
    //输入文件
    File fileInput = new File(src);
    //输出文件
    File fileOutput = new File(dest);
    //输入流
    FileReader fr = null;
    //输出流
    FileWriter fw = null;
    try {
        //初始化
        fr = new FileReader(fileInput);
        fw = new FileWriter(fileOutput);
        char[] words = new char[arrLength];
        int len;
        //通过数组进行字节流传递数据
        while ((len = fr.read(words)) != -1) {
            for (char c: words) {
                System.out.print(c);
            }
            //写入到复制文件
            fw.write(words, 0, len);
        }
    } catch (FileNotFoundException e) {
        e.printStackTrace();
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        //关闭资源
        try {
            if (fr != null) {
                fr.close();
            }
            if (fw != null) {
                fw.close();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
##上面四个添加Buffer就是对应的处理流，用于包装节点流，增加数据的传输速度
BufferInputStream	BufferOutputStream
```java
/**
 * @Description: TODO
 * @param: [src, dest, arrLen]
 * @return: void
 * @auther: lesssugar
 * @date: 2018/5/27 0027 9:44
 */
public void copyFile(String src, String dest, int arrLen) {
    File file1 = new File(src);
    File file2 = new File(dest);
    BufferedInputStream bis = null;
    BufferedOutputStream bos = null;
    try {
        bis = new BufferedInputStream(new FileInputStream(file1));
        bos = new BufferedOutputStream(new FileOutputStream(file2));
        byte[] words = new byte[arrLen];
        int len;
        while ((len = bis.read(words)) != -1) {
            bos.write(words, 0, len);
bos.flush();
        }
    } catch (FileNotFoundException e) {
        e.printStackTrace();
    } catch (IOException e) {
        e.printStackTrace();
    } finally {

        try {
            if (bis != null) {
                bis.close();
            }
            if (bos != null) {
                bos.close();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}
```
##BufferReader和BufferWriter基本类似，区别在于字符流的包装可以一读读一行
```java
/**
 * @Description: TODO
 * @param: [src, dest, arrLen]
 * @return: void
 * @auther: lesssugar
 * @date: 2018/5/27 0027 9:44
 */
public void copyFile(String src, String dest, int arrLen) {
    File file1 = new File(src);
    File file2 = new File(dest);
    BufferedReader bis = null;
    BufferedWriter bos = null;
    try {
        bis = new BufferedReader(new FileReader(file1));
        bos = new BufferedWriter(new FileWriter(file2));
        char[] words = new char[arrLen];
        int len;
        while ((len = bis.read(words)) != -1) {
            bos.write(words, 0, len);
        }
    } catch (FileNotFoundException e) {
        e.printStackTrace();
    } catch (IOException e) {
        e.printStackTrace();
    } finally {

        try {
            if (bis != null) {
                bis.close();
            }
            if (bos != null) {
                bos.close();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}
```
##字节流的操作流每次读取一行，关键问题需要newLine()手动换行或者+"\n"也可以
```java
/**
 * @Description: TODO
 * @param: [src, dest, arrLen]
 * @return: void
 * @auther: lesssugar
 * @date: 2018/5/27 0027 9:44
 */
public void copyFile(String src, String dest, int arrLen) {
    File file1 = new File(src);
    File file2 = new File(dest);
    BufferedReader bis = null;
    BufferedWriter bos = null;
    try {
        bis = new BufferedReader(new FileReader(file1));
        bos = new BufferedWriter(new FileWriter(file2));
        String str = null;
        while ((str = bis.readLine()) != null){
            bos.write(str);
bos.newLine();
            bos.flush();
        }
    } catch (FileNotFoundException e) {
        e.printStackTrace();
    } catch (IOException e) {
        e.printStackTrace();
    } finally {

        try {
            if (bis != null) {
                bis.close();
            }
            if (bos != null) {
                bos.close();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}
```
##转换流，可以在读入的过程中进行解码并转换为字符流，并进行解码的过程，写成UTF-8即可，乱码的解决方案
```java
public void copyFile(String src, String dest,String charSet) {
    File file1 = new File(src);
    File file2 = new File(dest);
    BufferedReader br = null;
    BufferedWriter bw = null;
    try {
        br = new BufferedReader(new InputStreamReader(new FileInputStream(new File(src)),charSet));
        bw = new BufferedWriter(new OutputStreamWriter(new FileOutputStream(new File(dest)),charSet));
        String str;
        while ((str = br.readLine()) != null) {
            System.out.println(str);
            bw.write(str);
            bw.newLine();
            bw.flush();
        }
    } catch (FileNotFoundException e) {
        e.printStackTrace();
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        try {
            if (br != null) {
                br.close();
            }
            if (bw != null) {
                bw.close();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}
```
##对象流
##序列化

通过序列化机制可以将对象转化为与平台无关的二进制流，从而允许将其将其存储在硬盘或者进行网络的传输，类与类的属性都要实现Seralizable接口，凡是实现序列化接口的类都有一个表示序列化标识版本的序列号，表明该对象的位置，不显示声明可能对象修改后版本号编号就无法找到了。不能序列化static,transient的属性

可以对对象进行序列化用于网络传输
OcjectInputStream,ObjectOutputStream

```
/**
 * @version 1.0
 * @ClassName
 * @Description: 对象的io流的测试
 * @Auther: lesssugar
 * @Date: 2018/5/22 0022 13:54
 */
public class TestFile {
    @Test
    public void myFile() {
        //创建对象
        com.wyq.Person p1 = new com.wyq.Person("小明", 23);
        com.wyq.Person p2 = new com.wyq.Person("红米", 23);
        //创建对象的操作流
        ObjectOutputStream oos = null;
        ObjectInputStream ois = null;
        try {
            //对象的输出流
            oos = new ObjectOutputStream(new FileOutputStream(new File("1.txt")));
            oos.writeObject(p1);
            oos.flush();
            oos.writeObject(p2);
            oos.flush();
            ois = new ObjectInputStream(new FileInputStream(new File("1.txt")));
            //对象的读取流
            com.wyq.Person pX = (com.wyq.Person) ois.readObject();
            System.out.println("pX = " + pX);
            com.wyq.Person pR = (com.wyq.Person) ois.readObject();
            System.out.println("pR = " + pR);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } finally {
            try {
                if (oos != null){
                    oos.close();
                }
                if(ois != null){
                    ois.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

# 随机通道流

## RandomAccessFile

## 特点

- 可以充当输入流，也可以充当输出流
- 支持从文件的开头读取、写入
- 支持从任意位置的读取和写入

```
/**
 * @version 1.0
 * @ClassName
 * @Description: 随机读取流IO
 * @Auther: lesssugar
 * @Date: 2018/5/22 0022 13:54
 */
public class TestFile {
    @Test
    public void myFile() {
        RandomAccessFile rafr = null;
        RandomAccessFile rafw = null;
        try {
            rafr = new RandomAccessFile(new File("hello.txt"), "r");
            rafw = new RandomAccessFile(new File("hell1.txt"), "w");
            byte[] bytes = new byte[20];
            int len;
            while ((len = rafr.read(bytes)) != -1){
                rafw.write(bytes,0,len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            try {
                if(rafr != null){
                    rafr.close();
                }
                if (rafw != null){
                    rafw.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

}
```

```c++
/**
 * @version 1.0
 * @ClassName
 * @Description: 随机读取流IO插入字符串
 * @Auther: lesssugar
 * @Date: 2018/5/22 0022 13:54
 */
public class TestFile {
    @Test
    public void myFile() {
        RandomAccessFile rafw = null;
        try {
            rafw = new RandomAccessFile(new File("hello.txt"), "rw");
            byte[] bytes = new byte[20];
            rafw.seek(5);
            //将位置后面的字符串全部读取的字符串中
            int len;
            StringBuffer sb = new StringBuffer();
            while ((len = rafw.read(bytes)) != -1) {
                sb.append(new String(bytes,0,len));
            }
            //将指针调回
            rafw.seek(5);
            rafw.write("xy".getBytes());
            rafw.write(sb.toString().getBytes());

        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (rafw != null) {
                    rafw.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

}
```