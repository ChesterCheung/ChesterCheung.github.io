---
layout: post
title:  "Java AI五子棋的实现"
categories: Java
tags:  Java 五子棋
author: Chester Cheung
---

* content
{:toc}

## Java五子棋开发



这次要做的项目是一个简单的五子棋项目，模仿QQ版的五子棋进行制作，整体分为几大功能的实现：开始游戏、悔棋、认输、人人大战、人机大战等模式，当任意一方的棋子达到连续的五颗棋子时，就需要判断输赢，并向用户告知比赛结果。

过程中要用到的API类有：JFrame，BorderLayout，JPanel，Dimension，FlowLayout，JButton，JRadioButton，ButtonGroup，JCheckBox，ActionListener，ActionEvent，MouseListener| MouseAdapter，MouseEvent，Graphics | Graphics2D，Color等。


具体的操作如下：


## 一、五子棋棋盘界面的开发



首先需要要做一个像画图板一样的界面，以便存放棋盘和功能功能板块，定义一个ChessMain类来继承JFrame，重写其中的抽象方法paint(Graphics g)，来绘制棋盘和棋子，

```php
	public class ChessMain extends JPanel implements Data{

		public static void main(String [] args) {
			ChessMain chess = new ChessMain();
			chess.initUI();	
		}
	
		public void initUI() {
			JFrame frame = new JFrame("五子棋");
			frame.setSize(720,630);
			frame.setDefaultCloseOperation(3);
			frame.setLocationRelativeTo(null);
			frame.setResizable(false);
			this.setPreferredSize(new Dimension(500,600));
			this.setBackground(Color.GRAY);
			frame.add(this, BorderLayout.CENTER);
			frame.setVisible(true);
		}
		public void paint (Graphics g) {
			super.paint(g);
			drawChessBorder(g);
		}

		public void drawChessBorder(Graphics g) {
			for (int i = 0;i < Row; i++)	//画棋盘的横线
				g.drawLine( X, Y + i * SIZE, X + (Colum -1) * SIZE, Y + i * SIZE );
			for (int i =0;i < Colum;i++)	//画棋盘的竖线
				g.drawLine(X + SIZE * i, Y, X + SIZE * i, Y + SIZE * (Row - 1));
		}
	}
```

如果在画横竖线时，直接输入长宽和线的数目，在后期想要修改格子大小时会很不方便，为了增强代码的灵活性，则要另外定义一个接口Data，把需要用到的数值定义为常量，在调用时直接调用常量就ok了，这样在修改棋盘或妻子大小时就会方便很多，ps.顺便别忘了让ChessMain类实现Data接口：

```php
	public interface Data {
		public static final int X = 50;	//左边框宽度
		public static final int Y = 50;	//上边框宽度
		public static final int SIZE = 35;		//格子的大小
		public static final int Chess_SIZE = 35;	//棋子的大小
		public static final int Row = 15;	//横线的数目
		public static final int Colum = 15;	//竖线的数目
		public static final int [][] arrays = new int [Row][Colum];	//数组用来存放下过棋子的位置信息
	}
```

![图示](https://img-blog.csdnimg.cn/20190317030530372.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM5MDE0NQ==,size_16,color_FFFFFF,t_70)

## 二、棋盘上绘制棋子



当鼠标在棋盘上点击以下时，需要在棋盘相应的位置画上一颗棋子，这个功能的实现就需要用到鼠标事件监听机制，用来监听鼠标上的动作信息。由于MouseListener中的抽象方法没有全部用到，所以不必把所有方法全部重写，为了简洁，让ChessListener鼠标事件监听方法继承MouseAdapter监听类，就只需要重写用到的方法，而不用一股脑儿都重写出来。

```php
	public class ChessListener extends MouseAdapter implements Data , ActionListener {

		private ChessMain chessmain;
		public  Graphics g;
	
		public  ChessListener(Graphics g) {
		this.g = g;
		}

		public void mouseClicked(MouseEvent e) {
			g = chessmain.getGraphics();
			int x = e.getX();
			int y = e.getY();
			//必须确保鼠标点击的点在棋盘范围内才会下棋，如果点击在了棋盘外，就不会画出棋子
			if (x >= X && y >= Y && x <= X + (Colum -1) * SIZE && y <= Y + (Row -1) * SIZE) {
				int  r = (y - Y + SIZE / 2) / SIZE;
				int  c = (x - X + SIZE / 2) / SIZE;
				x = c * SIZE + X - Chess_SIZE /2;
				y = r * SIZE + Y - Chess_SIZE /2;
				g.fillOval(x, y, Chess_SIZE, Chess_SIZE);
			}
		}
	}	
```

在下棋时，不仅要判断棋子的颜色，还要在下过一个颜色的棋子后，自动跳转到领完一种颜色的棋子上再画，因此需要定义一个Boolean类型的标志物flag来记录是黑子还是白子，给他默认定为true表示黑子。



现在还有一个问题就是，在已经下过棋子的位置继续下棋，后下的颜色会覆盖掉之前的棋子颜色，这个显然不符合现实情况，所以要再定义一个二维数组，用来记录棋盘上可以下棋的位置，初始化数组的值为0，如果落子为黑色，则将值改为1，如果落子为白色，则将值改为2，这样在下棋前，只需要判断一下该位置的数组值是否为0即可知道这个位置能不能下棋了。



## 三、判断五子棋输赢



现在已经能正确下棋子了，接下来要做的就是如何判断输赢，大体分为四个方向：可以水平方向、竖直方向、左上右下、左下右上，重新定义一个ChessWin类，里面沿着每个方向进行移动，依次判断相同颜色的棋子是否连续，如果连续，则返回连续棋子的个数，连续个数小于5时返回false，否则返回true，用于输出下棋胜利的结果。

```php
	public class ChessWin implements Data {

		public static boolean victory(int r, int c) {
			if (countH(r,c) >= 5 || countV(r,c) >= 5 || countX1(r,c) >= 5 || countX2(r,c) >=5) 
				return true ;
			else 
				return false;
		}

		//检查水平方向上的连续棋子个数
		public  static int  countH(int r,int c) {
			int num = 1;
			//往左
			for (int c1 = c-1 ;c1 > 0;c1--) {
				if (arrays[r][c] == arrays[r][c1])
					num++;
				else 
					break;
			}

			//往右
			for (int c1 = c+1;c1 < arrays[r].length;c1++) {
				if (arrays[r][c] == arrays[r][c1])
					num++;
				else 
					break;
			}
			return num;
		}	//其余三个方向上的代码在这里就省略不重复给出了
	}
```
