## 前言

​		记录一下这套简陋的系统说明，把所遇到的问题和难点以及操作说明在这篇文档中说明清楚，当个回顾吧，万一以后那一天查看也能及时找到问题。这套系统是在本人大三时期完成的，从GitHub上借鉴了很多经验，也借用了一些界面，很感谢。但目前这套系统还存在很多bug，之后有时间会慢慢进行更新。

​		**我把这套QQ程序分为了两部分，第一部分也就是本文，主要讲述源码和原理以及各种工具的使用，最重要的聊天部分是只实现在本机进行通讯，也就是说两台机器就无法进行通讯。第二部分我将会说明如何实现不同机器之间的通讯，也就真正意义上的QQ聊天，觉得小弟写的还行的可以关注一下，第二部分参考《[Java TCP实现高仿版QQ聊天(二)](https://blog.csdn.net/qq_42376617/article/details/105765866)》**
​		
另外，最近做了使用JSP做了一套[资料共享平台](https://blog.csdn.net/qq_42376617/article/details/108309396)和[智能OA办公系统](https://blog.csdn.net/qq_42376617/article/details/108439167)，有需要的伙伴可以点击链接直达，都是可以提供源码的哦！

​	这套系统会发在本人[博客园](https://www.cnblogs.com/zwing/p/12765476.html)和[CSDN博客](https://blog.csdn.net/qq_42376617/article/details/105723697)做个记录。

## 环境配置说明

​	1、JDK用的是1.8版本

​	2、开发工具使用的是eclipse Version: 2019-12。

​	3、数据库用的是MySQL 8.0.17 Community Server	。

​	4、MySQL和Java连接用的是mysql-connector-java-8.0.16.jar。

​	5、代码的编码格式为GBK。

​	6、此外还用到了Photoshop、Navicat等工具。

​	因为之前没有了解到eclipse的swing插件画图，swing界面都是一个一个敲出来了，当然了，有一部分也是从网友那边借鉴过来的。想说的是用swing的时候用插件画图可以省去很多时间以及可能，推荐大家使用。

​	一般聊天的时候使用的是UDP协议，但在本系统中全部使用TCP协议，没有使用UDP。

## 代码结构

### 代码结构

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200424094035587.png#pic_center)


### 详细说明

#### com包

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200424094300894.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMzc2NjE3,size_16,color_FFFFFF,t_70#pic_center)

#### dao包

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200424094314455.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMzc2NjE3,size_16,color_FFFFFF,t_70#pic_center)


#### view包

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200424094327917.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMzc2NjE3,size_16,color_FFFFFF,t_70#pic_center)


## 数据库

利用Navicat导入项目中的SQL文件即可。

### 数据库表

#### users表

| Account | NickName | PassWord | Sex  | Birth | Signature | Headimage | LoginIP | LocalHost | RecentLoginTime | LoginState |
| ------- | -------- | -------- | ---- | ----- | --------- | --------- | ------- | --------- | --------------- | ---------- |
| 账号    | 昵称     | 密码     | 性别 | 生日  | 个性签名  | 头像索引  | 登录IP  | 本地IP    | 最近登录时间    | 登录状态   |

#### message表

| mid        | msg      | sender | getter | sendtime | type                         |
| ---------- | -------- | ------ | ------ | -------- | ---------------------------- |
| 自增长索引 | 信息正文 | 发送方 | 接收方 | 发送时间 | 消息类型(离线/窗口震动/常规) |

#### friendtable

| ID         | MySelf_Account | Friends_Account | Note       |
| ---------- | -------------- | --------------- | ---------- |
| 自增长索引 | 我的账号       | 朋友账号        | 朋友的备注 |



## 导入Eclipse

​	项目是一个JavaSE项目，按普通项目导入Eclipse即可。

​	导入后项目若出现红色感叹号，可以右键项目->Build Path->Configure Build Path->移除`JRE System Library`，然后再通过`Add Library`把`JRE System Library`添加回来即可。

​	若用的数据库版本不一样，可以换成相应版本号的jar包，然后右键jar包->Build Path->Add Build Path->之后若是项目出现红色感叹号，可以在项目的文件夹中找到.classpath文件，然后用记事本打开，删除这一行即可。

```
<classpathentry kind="lib" path="lib/mysql-connector-java-8.0.16.jar"/>
```

​	**注意：如果没有更换jar包或者没有更换jar包后没有出现感叹号，不需要上一步操作。**

## 修改配置

​	打开utils.MyTools.java文件设置QQServerPort、QQServerIP，也就是运行时的端口号和IP地址，一般端口号不需要进行更改，修改QQServerIP即可(本系统测试时就是127.0.0.1，也就是本地IP地址，一般都是这个，如果可以运行就不需要改)。

​	数据库配置文件放在项目根目录下面的db.properties文件中，这个是肯定需要改的，这个怎么操作自行百度。这些信息是通过utils.PropertiesUtils.java进行IO读取然后读取到 JDBCUtils中。

## 启动

​	第一步：启动服务端，view.ServerFrame.java，点击按钮“启动服务器”按钮。

​	第二步：启动客服端，view.LoginFrame.java，这是客户端唯一的入口。

## 窗体截图

### 服务端界面

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020042409340013.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMzc2NjE3,size_16,color_FFFFFF,t_70#pic_center)


### 登录界面

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200424093419549.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMzc2NjE3,size_16,color_FFFFFF,t_70#pic_center)


### 注册界面

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200424093437653.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMzc2NjE3,size_16,color_FFFFFF,t_70#pic_center)


​	

### 主界面

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200424093452104.png#pic_center)


### 修改个人信息

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200424093510976.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMzc2NjE3,size_16,color_FFFFFF,t_70#pic_center)


### 添加好友界面

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200424093525292.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMzc2NjE3,size_16,color_FFFFFF,t_70#pic_center)


### 聊天窗口

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200424093541631.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMzc2NjE3,size_16,color_FFFFFF,t_70#pic_center)


​	左侧是聊天界面，右侧是聊天记录，可以从数据库导入之前的聊天记录，并且聊天时的数据也会进入到聊天记录面板。

###  查看好友资料/删除好友弹窗 

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200424093721145.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200424093729873.png#pic_center)

### 离线消息

![在这里插入图片描述](https://img-blog.csdnimg.cn/202004240936239.png#pic_center)


​	如果是离线消息或者当对方给我发消息的时候我没有打开和对方的聊天窗口，那么实现弹窗提醒。

## 有待完善

​	发送文件

​	发送图片

## 部分代码说明

### com.Server.java

```java
@Override
    public void run() {
        try {
            //1.设置服务器套接字 ServerSocket(int port)创建绑定到指定端口的服务器套接字
            server = new ServerSocket(MyTools.QQServerPort);
            while(isRunning) {
                //2.阻塞式等待客户端连接  (返回值)Socket accept()侦听要连接到此套接字的客户端并接受它。
                client = server.accept();
                System.out.println("一个客户端已连接....");
                input = new ObjectInputStream(client.getInputStream());
                output = new ObjectOutputStream(client.getOutputStream());
                
                String text = (String)input.readObject();//将客服端发来的信息转换为String
                String[] temp = text.split(MyTools.FLAGEND);//对客户端发来的信息进行分割
                Flag flag = MyTools.stringToFlagEnum(temp[0]);//获得标志
                String congtent = temp[1];//客服端发送过来的正文
                dealWithMessage(flag,congtent);
            }
        } catch (IOException e) {
            close(output,input,client,server);//释放资源
        } catch(ClassNotFoundException e){
            e.printStackTrace();
        }
    }
    
    /** 
     * 按照flag，进行对应的函数操作
     * @param flag 标志
     * @param message 文本正文
     */
    public void dealWithMessage(Flag flag,String message) {
    	switch (flag) {
			case REGISTER:doUserRegister(message);break;//处理注册请求
			case LOGIN:doUserLogin(message);break;//处理登录请求
			case ADD_Query:doAddUsersQuery(message);break;//处理添加好友查询请求
			case ADD:doAddUsersAdd(message);break;//处理添加好友请求
			case DeleteUsers:doDeleteUsers(message);break;//处理删除好友请求
			case GET_FRIEND_INFO:doGetFriendInfo(message);break;//处理查看好友资料
//			case SettingFriendNote:doSettingFriendNote(message);break;//设置备注
			case GET_HeadImage:dogetUsersInfo(message);break;//处理获取头像请求
			case SHOWHISTORY:doShowHistory(message);break;//将数据库中的聊天记录设置在聊天记录面板
			case UpdateUsersInfo:doUpdateUsers(message);break;//修改用户资料
			case UpdateUsersPass:doUpdateUsersPass(message);break;//修改用户密码
			case GetNotOnlineMsg:dogetUnReadMsg(message);break;//获取离线消息
			default:break;
		}
	}
```

​	这部分代码是开启一个线程从com.LoginUser.java读取IO流发送过来的数据，因为com.LoginUser.java发送来的数据进行了字符串拼接，也就是每个方法对应的处理有一个枚举，然后在本方法中进行字符串分割，传入dealWithMessage(Flag flag,String message)方法中，然后进行switch判断，调用相应的方法。

### utils.MyTools.java

```java
public enum Flag{
		REGISTER,//注册
	    REGISTER_SUCCEED,//注册成功
	    REGISTER_FAILED, //注册失败
	    ALREADY_REGISTER,//已经注册
	    
		LOGIN,//登录
	    LOGIN_SUCCEED,//登录成功
	    LOGIN_FAILED,//登录失败
	    ALREADY_LOGIN,//已登录
	    UNLOAD_LOGIN,//退出登录
	  	....
		....
        ....
	};
```

​	这是定义枚举，也就是相应的操作数据定义一个枚举，方便判断IO流中的数据是需要进行什么操作。相当于一个标志了，在发送IO流数据的前面加上这些字段，可以让IO流接收方判断字段信息是准备进行什么操作。

### utils.PropertiesUtils.java

```java
public class PropertiesUtils {
	
	private static Properties pro;
	
	static{
		pro = new Properties();
		try {
			//把配置文件的信息加载到 pro对象中
			pro.load(new FileInputStream("db.properties"));
		} catch (FileNotFoundException e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
	
	//根据属性名来获取属性值 
	public static String getPropertiesValue(String key){
		return pro.getProperty(key);
	}
}
```

​	读取db.properties文件中数据库的配置文件，把数据库信息放置在文件中的好处是当改数据库信息的时候不需要对代码进行改动。

### view.ChatFrame.java

```java
public void showMessage(JTextPane jtp, Message msg, boolean fromSelf) {
        //设置显示格式
        SimpleAttributeSet attrset = new SimpleAttributeSet();
        StyleConstants.setFontFamily(attrset, "仿宋");
        StyleConstants.setFontSize(attrset,14);
        Document docs = jtp.getDocument();
        String info = null;
        try {
            if(fromSelf){//发出去的消息内容，发送的内容从本类中获取
            	info = msg.getSendTime()+"  ";//发送时间：绿色
            	StyleConstants.setForeground(attrset, Color.blue);
				StyleConstants.setFontSize(attrset,16);
                StyleConstants.setBold(attrset, false);
                StyleConstants.setItalic(attrset, false);
            	docs.insertString(docs.getLength(), info, attrset);
            	
                info = "我:\n";//自己账号：紫色
                StyleConstants.setForeground(attrset, Color.blue);
				StyleConstants.setFontSize(attrset,16);
                StyleConstants.setBold(attrset, false);
                StyleConstants.setItalic(attrset, false);
                docs.insertString(docs.getLength(), info, attrset); 
                
                info = " "+msg.getContent()+"\n";//发送内容：黑色
                StyleConstants.setFontSize(attrset,20);
                StyleConstants.setForeground(attrset, Color.black);
                StyleConstants.setBold(attrset, true);
                StyleConstants.setItalic(attrset, true);
                docs.insertString(docs.getLength(), info, attrset);
                
            }else{//接收到的消息内容，接收的内容是ClientToServerThread类调用该方法
                info = msg.getSendTime()+"  ";//发送时间：绿色
                StyleConstants.setForeground(attrset, Color.red);
				StyleConstants.setFontSize(attrset,16);
                StyleConstants.setBold(attrset, false);
                StyleConstants.setItalic(attrset, false);
                docs.insertString(docs.getLength(), info, attrset);
                
                info = msg.getSenderName()+"("+msg.getSenderId()+"):\n";//对方账号：红色
                StyleConstants.setForeground(attrset, Color.red);
				StyleConstants.setFontSize(attrset,16);
                StyleConstants.setBold(attrset, false);
                StyleConstants.setItalic(attrset, false);
                docs.insertString(docs.getLength(), info, attrset); 
                
                info = " "+msg.getContent()+"\n";//发送内容：蓝色
                StyleConstants.setFontSize(attrset,20);
                StyleConstants.setForeground(attrset, Color.black);
                StyleConstants.setBold(attrset, true);
                StyleConstants.setItalic(attrset, true);
                docs.insertString(docs.getLength(), info, attrset);
            }
        } catch (BadLocationException e) {
            e.printStackTrace();
        }
    }
```

​	传入的参数为（JTextPane jtp，Message msg，boolean fromSelf），jtp表示的是在哪个JTextPane中展示，msg表示聊天内容，fromSelf（true表示发送方，false表示接收方），然后下面的代码是对接收方和发送方的格式进行设置，代码中表示发送方为蓝色，接收发为红色。便于判断信息是发送还是接收。

### com.ManageFriendListFrame.java

```java
public class ManageFriendListFrame {
    private static Hashtable<String, MainFrame> friendListFrames = new Hashtable<>();

    public static void addFriendListFrame(String frameName,MainFrame fl){
        friendListFrames.put(frameName,fl);
    }

    public static MainFrame getFriendListFrame(String frameName){
        return friendListFrames.get(frameName);
    }

    public static MainFrame removeFriendListFrame(String frameName){
        return friendListFrames.remove(frameName);
    }
}
```

​	这个类是管理用户的聊天界面的，也就是说一个用户只能打开一个主界面。主要是用Hashtable来管理，后来网上查了一下，Hashtable的父类已经过时，现在使用HashMap比较好。

​	Hashtable和HashMap的区别主要在于：

​	**1、继承的父类不同**

​	HashMap继承自AbstractMap类。但二者都实现了Map接口。
​	Hashtable继承自Dictionary类，Dictionary类是一个已经被废弃的类（见其源码中的注释）。父类都被废弃，自然而然也没人用它的子类Hashtable了。

​	**2、HashMap线程不安全,HashTable线程安全**

​	**........**

## TCP协议

### com.LoginUser.java

​	客户端调用本类实现TCP发送，由于篇幅有限，就只截取部分代码示例。构造方法初始化了Socket，ObjectOutputStream和ObjectInputStream，然后在sendLoginInfoToServer方法中把需要发送的方法组成字符串，在通过对象输出流output.writeObject(text)输出给Server.java；

​	再利用对象输入流接收结果并转换为实体类对象，Message msg = (Message) input.readObject()，就可以直接使用get方法获取到服务端发送过来的值。

```java
public class LoginUser {

    private Socket client;
    private ObjectOutputStream output;
    private ObjectInputStream input;

    public LoginUser(){
        try {
            client = new Socket(MyTools.QQServerIP, MyTools.QQServerPort);
            output = new ObjectOutputStream(client.getOutputStream());
            input = new ObjectInputStream(client.getInputStream());
        } catch (IOException e) {
            System.out.println("连接服务器失败！");
            e.printStackTrace();
        }
    }
     /**
     * 将通过校验的登录信息发送到服务器
     * 并将得到的消息包返回(包含当前用户的所有好友)
     */
    public Message sendLoginInfoToServer(JFrame f,Users users){
    	//把要发送的信息组成字符串
    	String text = Flag.LOGIN+MyTools.FLAGEND
    			+users.getAccount()+MyTools.SPLIT1
    			+users.getPassword()+MyTools.SPLIT1
    			+users.getLoginState()+MyTools.SPLIT1
    			+users.getRecentLoginTime()+MyTools.SPLIT1
    			+users.getLoginIP()+MyTools.SPLIT1
    			+users.getLocalHost();
    	//检测用户输入信息是否合理
        Users u = checkLoginInfo(f,users.getAccount(),users.getPassword());
        
        if(u != null){
            try {
                output.writeObject(text);//发送到服务器
                Message msg = (Message) input.readObject();//接收返回结果
                if(msg.getType() == Flag.LOGIN_SUCCEED){//登录成功
                    ClientToServerThread th = new ClientToServerThread(client);
                    th.start();//创建与服务器通信线程
                    ManageThread.addThread(users.getAccount(),th);//为登录者开启一个线程
                    return msg;
                } else if(msg.getType() == Flag.LOGIN_FAILED){
                    JOptionPane.showMessageDialog(f, "账号或密码输入错误，请重新输入！");
                } else if(msg.getType() == Flag.ALREADY_LOGIN){
                    JOptionPane.showMessageDialog(f, "该用户已登录，请勿重复操作！");
                }
            } catch (IOException | ClassNotFoundException e) {
                e.printStackTrace();
            }
        }
        return null;
    }
```

### com.Server.java

​	实现Runnable接口，定义ServerSocket，ObjectInputStream，ObjectOutputStream。开启一个线程接收LoginUser.java发送过来的数据，派出客服代表与之联系client = server.accept()；然后把输入流转换为字符串，再把字符串分割为字符串数组，看服务端发送的是什么请求，之后调用dealWithMessage()方法进行处理。

​	在相对应的方法中把数据库操作返回的结果通过输出流发送给LoginUser.java。

​	这边完成了TCP之间的沟通和交流。

```java
public class Server implements Runnable{

    private ServerSocket server;
    private Socket client;
    private ObjectInputStream input;
    private ObjectOutputStream output;
    private volatile boolean isRunning;
    
    Message msg = new Message();
    UserDao ud = new UserDao();
    MsgDao md = new MsgDao();

    public Server(){
        isRunning = true;
        new Thread(this).start();
    }
    
    @Override
    public void run() {
        try {
            //1.设置服务器套接字 ServerSocket(int port)创建绑定到指定端口的服务器套接字
            server = new ServerSocket(MyTools.QQServerPort);
            while(isRunning) {
            //2.阻塞式等待客户端连接  (返回值)Socket accept()侦听要连接到此套接字的客户端并接受它。
                client = server.accept();
                input = new ObjectInputStream(client.getInputStream());
                output = new ObjectOutputStream(client.getOutputStream());
                
                String text = (String)input.readObject();//将客服端发来的信息转换为String
                String[] temp = text.split(MyTools.FLAGEND);//对客户端发来的信息进行分割
                Flag flag = MyTools.stringToFlagEnum(temp[0]);//获得标志
                String congtent = temp[1];//客服端发送过来的正文
                dealWithMessage(flag,congtent);
            }
        } catch (IOException e) {
            close(output,input,client,server);//释放资源
        } catch(ClassNotFoundException e){
            e.printStackTrace();
        }
    }
    
    public void dealWithMessage(Flag flag,String message) {
    	switch (flag) {
			case REGISTER:doUserRegister(message);break;//处理注册请求
			case LOGIN:doUserLogin(message);break;//处理登录请求
			case ADD_Query:doAddUsersQuery(message);break;//处理添加好友查询请求
			case ADD:doAddUsersAdd(message);break;//处理添加好友请求
			case DeleteUsers:doDeleteUsers(message);break;//处理删除好友请求
			case GET_FRIEND_INFO:doGetFriendInfo(message);break;//处理查看好友资料
			case GET_HeadImage:dogetUsersInfo(message);break;//处理获取头像请求
			case SHOWHISTORY:doShowHistory(message);break;//将数据库的聊天记录放在聊天记录面板
			case UpdateUsersInfo:doUpdateUsers(message);break;//修改用户资料
			case UpdateUsersPass:doUpdateUsersPass(message);break;//修改用户密码
			case GetNotOnlineMsg:dogetUnReadMsg(message);break;//获取离线消息
			default:break;
		}
	
```


**代码整理不易，需要源码的可以联系企鹅号863772270，请作者喝瓶水即可**