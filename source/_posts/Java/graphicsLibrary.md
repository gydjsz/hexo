---
title: java图形库学习
---
需要继承JFrame类来画窗口 => public class Game extends JFrame {}
setTitle(String s);   //设置窗口标题
setLocation(int x, int y); //设置窗口位置
setSize(int width, int height);   //设置窗口宽和高
setVisible(true);   //设置窗口可见,默认为flase,这个方法放在setLocation()和setSize后面较好,我放在前面窗口为黑色,本来默认为白色的

<!--more-->
******

<font size=3>**paint方法画图**</font>   
定义后自动调用
```java
public class paint(Graphics g) {
	Color c = g.getColor();   //记录原来的颜色
	Font f = g.getFont();     //记录原来的字体
	g.setColor(Color.BLACK);  //设置画线的颜色
	g.drawLine(int x1, int y1, int x2, int y2); //两点画直线
	g.drawRect(int x, int y, int width, int height);  //左上角顶点加宽高画矩形
	g.fillRect(int x, int y, int width, int height);  //画填充矩形
	g.setFont(new Font("楷体", Font.BOLD, 40));   //设置字体为楷体,粗体,大小为40
	g.drawString(str, int x, int y);  //画出str字符串
	g.setColor(c);  //变回原来的颜色
	g.setFont(f);   //变回原来的字体
}
```
******

**GameUtil工具类导入图片**
```java
import java.awt.Image;
import java.awt.image.BufferedImage;
import java.io.IOException;
import java.net.URL;

import javax.imageio.ImageIO;


 
public class GameUtil {
    // 工具类最好将构造器私有化。
    private GameUtil() {
     
    } 
 
    public static Image getImage(String path) {
        BufferedImage bi = null;
        try {
            URL u = GameUtil.class.getClassLoader().getResource(path);
            bi = ImageIO.read(u);
        } catch (IOException e) {
            e.printStackTrace();
        }
        return bi;
    }
}
```
在Game类里面调用GameUtil
Image imag = GameUtil.getImage("images/picture.png");  //我建立的一个images包,用来存储图片,引号里面为图片的路径
g.drawImage(imag, x, y, width, height, null);   //imag图片,位置,宽高,观察者

```java
import java.awt.Color;
import java.awt.Font;
import java.awt.Graphics;
import java.awt.Image;


import javax.swing.JFrame;

public class MyGame extends JFrame{
	Image imag = GameUtil.getImage("images/text1.png");  //指定图片
	@Override
	public void paint(Graphics g) {
		Color c = g.getColor();
		Font f = g.getFont();
		g.setColor(Color.BLUE);             //设置线体颜色
		g.drawLine(100, 100, 650, 100);     //直线
		g.drawRect(50, 150, 200, 200);      //空心矩形
		g.fillRect(550, 150, 200, 200);      //实体矩形
		g.drawOval(300, 150, 200, 200);      //圆形
		g.setFont(new Font("楷体", Font.BOLD, 90));   //设置字体
		g.drawString("How are you？", 100, 100);      //写字
		g.drawImage(imag, 250, 400, 300, 300, null);   //插入图片
		g.setColor(c);     //线条颜色变为原来的样子
		g.setFont(f);      //字体变为原来的样子
	}
	public void launchJFrame() {
		this.setTitle("我的游戏");       //设置窗口标题
		this.setSize(800, 800);        //设置窗口大小
		this.setLocation(100, 100);    //设置窗口位置
		this.setVisible(true);         //设置窗口可见
		/*this.addWindowListener(new WindowAdapter() {    //叉掉窗口后，结束窗口所在的应用程序
			@Override
			public void windowClosing(WindowEvent e) {
				System.exit(0);
			}
		});	*/
		this.setDefaultCloseOperation(EXIT_ON_CLOSE);   //叉掉窗口后，结束窗口所在的应用程序
		
	}
	public static void main(String args[]) {
			MyGame game = new MyGame();
			game.launchJFrame();		
		}
}

```

<font size = 3>**设置图片的大小**</font>
public Image getScaledInstance(int width, int height, int hints) &emsp;//hints - 指示用于图像重新取样的算法类型的标志(这句话不知道是什么意思,照着下面的写就对了)
```java
Image img = GameUtil.getImage("images/text1.jpg");
img = img.getScaledInstance(width, height, Image.SCALE_DEFAULT);
```
如果是要获取图片的大小,直接使用getWidth()和getHeight()方法就可以了
```java
width = img.getWidth();
height = img.getheight();
```

<font size = 3>**双缓冲技术解决闪烁**</font>
原理大概是:先将所需要画的东西加载到缓冲区,然后将缓冲区中的内容全部画到屏幕上,这样就可以避免因为屏幕加载的东西太多导致屏幕疯狂闪烁
```java
public void paint(Graphics g){
	BufferedImage imag = new BufferedImage(width, height, BufferedImage.TYPE_INT_RGB);   //构建缓冲区
	Graphics g2 = imag.creatGraphics();   //新建一支画笔,使用这支画笔来将内容画到缓冲区中
	g2.drawRect(...);    //括号里面的参数就不写了,此处用来说明一些画图操作
	g2.drawImag(...);
	g2.fillOval(...);
	g.drawImage(imag, x, y, width, height, null);   //将内容画到屏幕上
}
```
