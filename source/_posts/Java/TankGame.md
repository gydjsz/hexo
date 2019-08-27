---
title: 坦克大战
tags: java
---

## 一、前言：
整个坦克大战游戏做的比较匆忙的，里面也有很多的bug，代码也写得比较乱，整理博客的时候也不太好整理，本想优化一下下，可是由于整个项目结构的缘故，只能到这一步了，这次算是有了很多的经验，相信下次再做的时候，应该会好得多。
<!--more-->

## 二、主体介绍
游戏结构主要有游戏面板、设置面板、菜单栏、坦克、子弹以及提升属性的物品

### 玩法规则
1. 我方坦克将敌方坦克打完，就算获胜
2. 敌方坦克将我方坦克击败三次以及基地被摧毁，就算失败
3. 打掉草地以及击败敌方坦克均会掉落物品，捡到后能提升属性值

## 三、代码实现

<img src="https://raw.githubusercontent.com/gydjsz/TankGame/master/picture/1.jpg" width="50%" height="50%"/>

### 游戏面板
1. 游戏的开始面板将背景的图片，以及文字信息放了上去。这里由于出现了双缓冲技术，就在这里简单地说一下。

#### “双缓冲技术”的绘图过程如下
1. 在内存中创建与画布一致的缓冲区

2. 在缓冲区画图

3. 将缓冲区位图拷贝到当前画布上

4. 释放内存缓冲区

&emsp;双缓冲即在内存中创建一个与屏幕绘图区域一致的对象，先将图形绘制到内存中的这个对象上，再一次性将这个对象上的图形拷贝到屏幕上，这样能大大加快绘图的速度。

版本一（重写update方法）：
重量级中repaint首先调用update方法，update然后再调用paint方法。再轻量级组件中直接调用paint。

```java
private Image offScreenImage = null;
 
public void update(Graphics g) {
    if(offScreenImage == null)
        offScreenImage = this.createImage(500,500);//这是游戏窗口的宽度和高度
     
    Graphics gOff = offScreenImage.getGraphics();
    paint(gOff);
    g.drawImage(offScreenImage, 0, 0, null);
}   
```

由于JFrame是轻量级的，因而可以重写paint方法

```java
private Image offScreenImage = null;
public void paint(Graphics g) {     
	// 在重绘函数中实现双缓冲机制     
	offScreenImage = this.createImage(WIDTH, HEIGHT);     
	// 获得截取图片的画布     
	gImage = offScreenImage.getGraphics();      
	gImage.setColor(gImage.getColor());     
	gImage.fillRect(0, 0, WIDTH, HEIGHT); // 填充缓冲   
	super.paint(gImage);     //用gImage绘制图形, 代码根据具体情况
	g.drawImage(offScreenImage, 0, 0, null);    //将缓冲图案绘制在屏幕上 
} 
```

这中方式就比较容易理解，而且通用
```java
//初始化缓冲区
BufferedImage imag = new BufferedImage(Constent.width, Constent.height, BufferedImage.TYPE_INT_RGB);
Graphics g1 = imag.getGraphics();
public void paint(){
	g1.drawImage(x, y, width, height, null);
	g.drawImage(imag, x, y, width, height, null);
}
```

2. 里面有个设置鼠标指针样式一个代码，不过找的指针图片不太好看，就没加上，就先把方法留在里面了。

```java
Image coursor = GameUtil.getImage("TankImage/logo.jpg");  //设置指针图片
//设置鼠标指针样式
setCursor(Toolkit.getDefaultToolkit().createCustomCursor(coursor,new Point(0, 0), null));
```

3. 点击开始游戏和游戏设置的面板切换
这里使用了鼠标监听
```java
public void mousegCliked(MouseEvent e){
	e.getX();
	e.getY();  //获得鼠标的坐标
}
```
首先可以获得左上角和右下角的坐标位置，然后可以得到一个范围，只要是在这个范围内，都可以触发鼠标事件。

这里需要注意的是，要将事件监听添加到JFrame中。
如果是点击开始游戏，那么触发事件后就可以将初始面板设置为不可见setVisible(false)，然后将它从JFrame中移除，再将新的面板添加进来，并设置其为可见。
然后如果是点击游戏设置，那么就直接将该面板设置为可见


### 游戏设置窗口

<img src="https://raw.githubusercontent.com/gydjsz/TankGame/master/picture/2.jpg" width="50%" height="50%"/>

里面使用几个组件：
1. JLabel: 标签
2. JRadioButton: 单选按钮
3. JSLider: 滑块
4. JTextField: 文本框
5. JButton: 按钮
6. JComboBox: 下拉菜单

```
jSlider = new JSlider(10, 100, initValue);  //设置起始点值，终点值，默认值

jsl.setPaintTicks(true);       //设置滑块绘制刻度标记
jsl.setMajorTickSpacing(10);  //设置主刻度标记的间隔
jsl.setMinorTickSpacing(2);  //设置副刻度标记的间隔

//这是修改坦克数量显示的事件处理
jSlider.addChangeListener(new ChangeListener() {
	public void stateChanged(ChangeEvent e) {
		jTextField.setText(String.valueOf(jSlider.getValue()));
	}
});

jTextField.setEditable(true);  //设置文本不可更改

jDialog.setModalityType(ModalityType.APPLICATION_MODAL);  //设置该窗口打开后将其它窗口锁住
```

### 坦克大战的主面板

<img src="https://raw.githubusercontent.com/gydjsz/TankGame/master/picture/3.jpg" width="50%" height="50%"/>
<img src="https://raw.githubusercontent.com/gydjsz/TankGame/master/picture/5.jpg" width="50%" height="50%"/>

1. 这个是游戏的主体，包含地图的绘制、显示所有坦克的移动和子弹发射的轨迹、物品掉落、游戏信息面板，坦克死亡、基地破坏的判定;
2. 由于所有的地图、坦克、子弹都已经放在了各自的ArrayList容器之中，所以在paint方法之中就只需遍历容器中的每一个值，然后将里面的内容绘制在屏幕上。

```java
ArrayList<Tank> tank;
tank = new ArrayList<Tank>;
tank.add(tank1);  //将坦克添加到容器中
tank.add(tank2);

Tank t = tank.get(i);  //获得第i个坦克
tank.remove(i);        //移除第i个坦克
```

3. 设置字体的颜色和样式
setFont(new Font("楷体", Font.BOLD, 60));    //楷体，粗体，60号
setColor(Color.BLACK);     //白色

4. 记录游戏的时间
startTime = System.currentTimeMillis();
setEnd.endTime = System.currentTimeMillis();
setEnd.time = setEnd.endTime - setEnd.startTime;

### 菜单

<img src="https://raw.githubusercontent.com/gydjsz/TankGame/master/picture/4.jpg" width="50%" height="50%"/>

功能: 回到主面板、重新开始游戏、暂停游戏、恢复、退出游戏

1. 回到主面板
将游戏数据设置为结束的状态，然后将主面板设置为不可见，再移除坦克的键盘监听，新建一个主面板，再添加到JFrame中

2. 重新开始游戏
将游戏数据设置为结束的状态，然后初始化主面板

3. 暂停游戏
将所有的坦克和子弹，位置、方向不可改变，坦克不可键盘监听，这样线程还在运作，但是事实上所有的物体已经不能移动

4. 恢复
就反过来，将一切复原

5. 退出游戏
System.exit(0);

## 坦克设置
1. 坦克的属性有坐标、方向、速度、是否存活、生命值、攻击力等
这里速度可以直接设置坐标的改变量，然后也可以设置执行线程的时间，线程中有sleep()方法和坦克坐标移动的方法，避免由于线程执行太快而设置了线程暂停的时间，将时间缩短也可以提高速度

2. 坦克的子弹使用了线程，当发射子弹后，就执行线程，线程中每隔一段时间将子弹的坐标改变，以此达到子弹自动运动的效果

3. 在坦克的键盘监听之中，可以将按下的四个方向设置为boolean类型，如果直接按下方向键就进行移动，那么会有卡顿感，因为键盘监听是有延时的，当按下某一个方向的键时，就设置其为true，那么按下的过程中那个方向就一直为true，松开键时，将其设为false

4. 坦克的碰撞检测: 
碰撞检测的理论是：将两个物体看做矩形，矩形相交就判定为相碰。

```java
Rectangle r1 = new Rectangle(x1, y1, width, height);
Rectangle r2 = new Rectangle(x2, y2, width, height);
rl.intersects(r2);  //相交为true，否则为false
```

如果加上位移会相交，那么就刚好到碰不到物体的地方，否则可以移动

5. 坦克的移动使用的是随机数
```java
Random random = new Random();
int n = random.nextInt(4);   //随机数[0,3)
```

## 图片的插入
这里使用了GameUtil类，使用这个类比较容易添加图片

```java
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

使用方法
```java
Image image = GameUtil.getImage(路径);
```
