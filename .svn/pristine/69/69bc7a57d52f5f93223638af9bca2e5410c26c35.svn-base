/*
@author hsingh23, cfilla2, phann
@reviewed ddkang2, ajyoo2, yjhwang2

 
* Patrick! V8 Made By Harsh Singh CS 125 Fun game I made where you have to get
 * boxes with your mouse and avoid the player from touching you. It insults you
 * when you lose, and posts random quotes when you win. Open Source - Modify in
 * anyway you like (just give me credit too). If you want to donate to me - buy
 * a kit kat and leave it at the ACM in Siebal Center for Harsh Singh Note:
 * x.txt has insults. win.txt has random win quotes.
 */

import java.awt.event.KeyEvent;
import java.io.*; // Need this for reading the file
import java.util.*; // Need this for math.random and ArrayLists
import java.awt.*;

// TODO: work on timer and improve alternate version of game [when you press
// down key]
// TODO: ObjectOrient adding sprites? No trailer? Better trailer? Alternate
// Sprite images? Better implementation of sprite images? PUT TIMER. MAKE RED-KILL BOXES. MAKE ? (fast, slow, next level, random level+-5 BOXES. MAKE FAST BOXES. MAKE SLOW BOXES 
//live is the game - so if you die - you dont love automatically {+ life box) SCores, after interval of score - life ++
/*
 * Add your ideas here: (name - email - idea - will work on it yes/no) [Harsh
 * Singh - hsingh23@ill - As player gets to level 10 play as sprite (boss).
 * Mouse moves randomly away from user for 1 min. Level 11 is last level - 3
 * sprite - must last for 30 seconds : yes] [Harsh Singh - hsingh23 - As time
 * goes on message player saying they've been playing too much. Level 8.
 * Premature game over at Level 9 - wait 5 seconds to keep playing - yes] [Harsh
 * Singh - hsingh23 - Vengence - level 10 win gives a cut scene where mouse
 * kills sprite - no] [Harsh Singh - hsingh23 - tron like feature? snake trail
 * kills - make short trail? - no] [Harsh Singh - hsingh23 - Alternate sprite
 * images - yes] [Harsh Singh - hsingh23 - Better Transitions - yes]
 */

public class RainingNumbers {
	private static int x, y, mx, my, direction, speed, checkpoint, ctr, width,
			height, cx, cy, level, x2, y2, direction2, zh, zw, rainNumber, rx,
			ry, matrixY, matrixS, score, actionTime;
	private static long totalTimeLevel, startTime, elapsedtime;
	private static boolean avoid = true;
	private static boolean first, isRaining, matrixIsON, o9, shiftmix;
	private static String rainString;
	// private static String neo = "neo";
	private static Random gen = new Random();
	private static Image seizure = Zen.getCachedImage("matrix.jpg");
	private static Image over9000 = Zen.getCachedImage("9000.gif");
	private static Image moon = Zen.getCachedImage("moon.jpeg");
	private static Image win = Zen.getCachedImage("win.jpg");
	private static Image[] matrix = new Image[5];

	public static ArrayList<String> insults = new ArrayList<String>();

	// public static ArrayList<String> win = new ArrayList<String>();

	public static void rainNumber() {
		rainNumber = gen.nextInt(99999);
		rx = gen.nextInt((int) Zen.getZenWidth() - 150) + 50;
		// rx = 300;
		rainString = "" + rainNumber;
		ry = 0;

	}

	public static void drawNumber() {
		Zen.setFont("Times-22");
		Zen.setColor(0, 0, 0);
		Zen.fillRect(rx, ry - 22, 80, 25);
		Zen.setColor(255, 0, 0);
		Zen.drawText(rainString, rx, ry);
		ry++;
		if ((rx > 100 || rx < Zen.getZenWidth() - 200) && level > 10) {
			rx += gen.nextInt(20) - 10;
		}
	}

	public static void getInsultsArray() throws Exception {
		File file = new File("x.txt");
		Scanner sc = new Scanner(file);
		while (sc.hasNext()) {
			insults.add((String) sc.nextLine());
		}
		/*
		 * File file2 = new File("win.txt"); Scanner sc2 = new Scanner(file2);
		 * while (sc2.hasNext()) { win.add((String) sc2.nextLine()); }
		 */
	}

	public static void loadInstructions() {
		// TODO: Make this look better. Make correct instructions
		Zen.setFont("Times-20");
		Zen.setColor(0, 24, 178);
		Zen.drawText("PATRICK! V16 By: Harsh Singh & Philip Hann", 80, 20);
		Zen.drawText("The Game of OLD SCHOOL TOUGHNESS", 90, 40);
		Zen.drawText("Goal: AVOID THE DUDE. GET WHITE BOXES. Win Levels", 10,
				80);
		Zen.drawText("Get box by hovering mouse over box", 20, 100);
		Zen
				.drawText("R - Resets game. Down changes Game Type (buggy)",
						20, 120);
		Zen.drawText("P - Pause. I - Instructions ", 180, 140);
		Zen.setFont("Times-80");
		Zen.setColor(225, 24, 18);
		Zen.drawText("Click to start ", 60, 310);
		Zen.waitForClick();
		Zen.setColor(0, 0, 0);
		Zen.fillRect(0, 0, 640, 480);
	}

	public static void initialize() {
		x = 100;
		y = 100;
		matrixY = 0;
		matrixIsON = false;
		o9 = false;
		if (level >= 2 && level < 9000) {
			matrixIsON = true;
		}
		if (level >= 9000) {
			o9 = true;
			matrixIsON = false;
		}
		x2 = y2 = -100;
		if (level > 3) {
			x2 = gen.nextInt(300) + 100;
			y2 = gen.nextInt(300) + 200;
		}
		direction = gen.nextInt(5) + 2;
		direction2 = gen.nextInt(5) + 2;
		ctr = 0;
		width = 50;
		height = 50;
		nextCheckpoint();
		checkpoint = 5;
		zh = Zen.getZenHeight();
		zw = Zen.getZenWidth();
		isRaining = false;
		rainString = "";
		startTime = System.currentTimeMillis();
		elapsedtime = 0;
		actionTime = gen.nextInt(10000) + 1000;

	}

	public static void moveSprite1() {
		// improve algorithm
		mx = Zen.getMouseX();
		my = Zen.getMouseY();
		if (mx > x) {
			x = x + direction2;
		} else if (mx < x) {
			x = x - direction2;
		}
		if (my > y) {
			y = y + direction;
		} else if (my < y) {
			y = y - direction;
		}
	}

	public static void checkRain() {
		String user = Zen.getEditText();
		Zen.setEditText("");

		for (int i = 0; i < user.length(); i++) {
			char c = user.charAt(i);
			if (c == rainString.charAt(0))
				rainString = rainString.substring(1);
		}
	}

	public static void moveSprite2() {
		// improve algorithm
		mx = Zen.getMouseX();
		my = Zen.getMouseY();
		if (mx > x2) {
			x2 = x2 + direction;
		} else if (mx < x2) {
			x2 = x2 - direction;
		}
		if (my > y2) {
			y2 = y2 + direction2;
		} else if (my < y2) {
			y2 = y2 - direction2;
		}
	}

	public static int cutString(String s) {
		int d = 48;
		if (s.length() > 48)
			while (s.charAt(d) != ' ') {
				d--;
			}
		else
			return s.length();
		return d;
	}

	public static void checkBounds() {
		if (direction < 0) {
			if (x >= Zen.getZenWidth())
				youLose();
			if (y >= Zen.getZenHeight())
				youLose();
			if (x <= 0)
				youLose();
			if (y <= 0)
				youLose();
		}
	}

	public static void youLose() {
		Zen.setFont("Times-36");
		int shade = 255;
		while (shade > 0) {
			Zen.setColor(shade, shade, shade);
			Zen.fillRect(0, 0, zw, zh);
			Zen.sleep(1);
			shade = shade - 1;
			Zen.setColor(255, 0, 0);
			Zen.drawText("Aww Gee - Was it Really That Hard? I mean REALLY?",
					10, 200);
			Zen.flipBuffer();
		}
		Zen.setColor(0, 24, 178);
		Zen.setFont("Times-25");
		Zen.drawImage(moon, 0, 0, Zen.getZenWidth(), Zen.getZenHeight());
		int z = gen.nextInt(insults.size());
		String s = insults.get(z);
		int d = cutString(s);
		Zen.drawText(s.substring(0, d), 10, 300);
		Zen.drawText(s.substring(d, s.length()), 10, 340);
		showLevel();
		Zen.flipBuffer();
		Zen.sleep(5000);
		Zen.setFont("Times-25");
		Zen.drawText("Click for Next Level.", 50, 240);
		Zen.flipBuffer();
		Zen.waitForClick();
		Zen.setColor(0, 0, 0);
		Zen.fillRect(0, 0, 640, 480);
		level = 0;
		speed = 1;
		initialize();
		showLevel();
		Zen.flipBuffer();

	}

	public static void showLevel() {
		Zen.setColor(0, 0, 0);
		Zen.fillRect(0, 0, 250, 25);
		Zen.setFont("Times-12");
		Zen.setColor(250, 0, 0);
		Zen.drawText("L:" + level + " Chkpts:" + checkpoint + " ctr:" + ctr
				+ " Speed:" + speed, 12, 15);
	}

	public static void youWin() {
		// Display Win message. initialize; level ++; checkpoint += level; speed
		// -= level;
		// format message better
		Zen.setFont("Times-40");
		int shade = 255;
		while (shade > 0) {
			Zen.setColor(0, shade, shade);
			Zen.fillRect(0, 0, zw, zh);
			Zen.sleep(1);
			shade = shade - 1;
			Zen.setColor(255, 0, 0);
			Zen.drawText("You WIN.", 50, 200);
			Zen.flipBuffer();
		}
		Zen.setColor(0, 24, 178);
		Zen.setFont("Times-25");
		int z = gen.nextInt(insults.size());
		String s = insults.get(z);
		int d = cutString(s);
		Zen.setColor(260, 260, 260);
		Zen.fillRect(0, 0, Zen.getZenWidth(), Zen.getZenHeight());
		Zen.setColor(0, 24, 178);
		double ty = 0;
		double tx = 0;
		double velocityX = (double) gen.nextInt(150);
		double velocityY = 0;
		while (ty < Zen.getZenHeight() - 300) {

			Zen.setColor(260, 260, 260);
			Zen.fillRect(0, 0, Zen.getZenWidth(), Zen.getZenHeight());
			// Zen.drawImage(win, tx+(int)(Math.sin((double)ty)*50), ty, 300,
			// 300);
			Zen.drawImage(win, (int) tx, (int) ty, 300, 300);
			velocityY = velocityY + 1;
			if (ty + velocityY > 400)
				velocityY = -Math.abs(0.9 * velocityY);
			if (tx >= Zen.getZenWidth() - 300 || tx < 0)
				velocityX *= -1;
			tx = tx + velocityX;
			ty = ty + velocityY;
			Zen.sleep(10);
			Zen.flipBuffer();

		}
		Zen.setColor(260, 260, 260);
		Zen.fillRect(0, 0, Zen.getZenWidth(), Zen.getZenHeight());
		if (tx > Zen.getZenWidth() - 300)
			tx = Zen.getZenWidth() - 300;
		if (tx <= 0)
			tx = 0;
		Zen.drawImage(win, (int) tx, (int) ty, 300, 300);
		Zen.setColor(0, 24, 178);
		Zen.drawText(s.substring(0, d), 10, 300);
		Zen.drawText(s.substring(d, s.length()), 10, 340);
		showLevel();

		if (level > 6) {
			Zen.flipBuffer();
			Zen.sleep(3000);
		}

		Zen.setFont("Times-25");
		Zen.drawText("Click for Next Level.", 50, 240);

		Zen.flipBuffer();
		Zen.waitForClick();

		initialize();
		level++;
		checkpoint += level;
		speed++;
		showLevel();
		Zen.flipBuffer();
		printMsg("Times are getting tough - take your asprin now!", 4);
		printMsg("Press Tab to skip lame part", 3);
		printMsg("I hope you don't have to pee",
				"Things are about to get intense.", 12);
		printMsg("You should take a screenshot of this right now!", 10);
		printMsg("You got skill - but how long can you hold your breath?", 6);
		printMsg("Welcome to the Real World ~ Neo", 42);

		printMsg("BWAHAHAHHAHAHHAA - IT\'S OVER 9000!!!!!!!!!!!!", 9001);

	}

	private static void printMsg(String s, String s2, int l) {
		if (level == l) {
			Zen.setFont("Times-25");
			Zen.drawText(s, 50, 240);
			Zen.drawText(s2, 50, 280);
			Zen.flipBuffer();
			Zen.waitForClick();
		}
	}

	public static void printMsg(String s, int l) {
		if (level == l) {
			Zen.setFont("Times-25");
			Zen.drawText(s, 50, 240);
			Zen.flipBuffer();
			Zen.waitForClick();
		}
	}

	public static void nextCheckpoint() {
		Zen.setColor(0, 0, 0);
		Zen.fillRect(cx, cy, width, height);
		cy = gen.nextInt(Zen.getZenHeight() - 50);
		cx = gen.nextInt(Zen.getZenWidth() - 50);
	}

	private static void drawCheckpoint() {
		Zen.setColor(255, 255, 255);
		Zen.fillRect(cx, cy, width, height);
		Zen.setFont("Times-40");
		Zen.setColor(255, 0, 0);
		Zen.drawText("" + (checkpoint - ctr), cx + 10, cy + 40);
		showLevel();
		if ((int) elapsedtime >= actionTime) {
			// do one of a random list of things
			Zen.setColor(255, 0, 0);
			// call a draw method that takes an int and draws for 1-5 seconds
			Zen.fillOval(gen.nextInt(Zen.getZenWidth() - width), gen
					.nextInt(Zen.getZenWidth() - height), width, height);
			elapsedtime = 0;

		}
		// random
	}

	public static void checkKeys() {
		/*
		 * if (Zen.isVirtualKeyPressed(KeyEvent.VK_UP)) { direction =
		 * gen.nextInt(5) + 2; direction2 = gen.nextInt(5) + 2; avoid = true; }
		 * if (Zen.isVirtualKeyPressed(KeyEvent.VK_DOWN)) { direction =
		 * -(gen.nextInt(5) + 2); direction2 = -(gen.nextInt(5) + 2); avoid =
		 * false; startTime = System.currentTimeMillis(); } if
		 * (Zen.isVirtualKeyPressed(KeyEvent.VK_LEFT)) { speed += 2; } if
		 * (Zen.isVirtualKeyPressed(KeyEvent.VK_RIGHT)) { speed -= 2; }
		 */
		if (Zen.isVirtualKeyPressed(KeyEvent.VK_P)) {
			Zen.waitForClick();
		}
		if (Zen.isVirtualKeyPressed(KeyEvent.VK_R)) {
			initialize();
		}
		if (Zen.isVirtualKeyPressed(KeyEvent.VK_I)) {
			loadInstructions();
		}
		if (Zen.isVirtualKeyPressed(KeyEvent.VK_F1)) {
			level = 41;
			speed = 10;
			// youWin();

		}
		if (Zen.isVirtualKeyPressed(KeyEvent.VK_UP)) {
/*			//Zen.setEditText("");
			String user = Zen.getEditText();
			//Zen.setEditText("");
			

			for (int i = 0; i < user.length(); i++) {
				char c = user.charAt(i);
				if (Character.isDigit(c)){
					level += (int)c;
					youWin();
					break;
				}
				
			}*/
			
			
			
			level +=4;
			youWin();
		}
		if (Zen.isVirtualKeyPressed(KeyEvent.VK_WINDOWS)) {
			level = 8999;
			speed = 25;
			youWin();
		}
		if (Zen.isVirtualKeyPressed(KeyEvent.VK_ESCAPE)) {
			shiftmix = !shiftmix;
		}
		if (Zen.isVirtualKeyPressed(KeyEvent.VK_F2)) {

			/*
			 * String user2 = Zen.getEditText(); Zen.setEditText("");
			 * 
			 * for (int i = 0; i < user2.length(); i++) { char c =
			 * user2.charAt(i); if (c == neo.charAt(0)) neo = neo.substring(1);
			 * } if (user2.length() == 0) level = 9000;
			 */
		}
		if (Zen.isVirtualKeyPressed(KeyEvent.VK_DELETE)) {
			matrixIsON = !matrixIsON;
		}
		if (Zen.isVirtualKeyPressed(KeyEvent.VK_TAB)) {
			level = 3;
			speed = 3;
			youWin();
		}

	}

	public static void genNewTime() {
		long yo = System.currentTimeMillis() + (long)(gen.nextInt(10000)+1000);
	}
	public static void checkTime(long inSec) {
		if( System.currentTimeMillis() >= inSec+startTime) {
			
		}
	}

	public static void main(String[] args) throws Exception {
		matrixS = 8;

		getInsultsArray();
		loadInstructions();
		initialize();
		for (int index = 0; index < matrix.length; index++) {
			matrix[index] = Zen.getCachedImage("" + (index % 4 + 1) + ".jpg");
		}
		first = true;
		// avoid = true;
		checkpoint = 5;
		while (true) { // what to do every level

			moveSprite1();
			Zen.flipBuffer();
			zh = Zen.getZenHeight();
			zw = Zen.getZenWidth();

			// check if you lost
			if ((mx > x - 5 && mx < x + 5) && (my > y - 5 && my < y + 5)) {
				youLose();
			}
			if ((mx > x2 - 5 && mx < x2 + 5) && (my > y2 - 5 && my < y2 + 5)) {
				youLose();
			}
			if (isRaining && ry > Zen.getZenHeight()) {
				youLose();
			}

			if ((x2 > x - 15 && x2 < x + 15) && (y2 > y - 15 && y2 < y + 15)
					&& !isRaining) {
				rainNumber();
				isRaining = true;

			}
			if (rainString.length() == 0) {
				isRaining = false;
			}
			//backgrounds
			if (matrixIsON) {
				for (int i = 0; i < matrix.length; i++)
					Zen.drawImage(matrix[i], 0, Zen.getZenHeight() * -i
							+ matrixY, Zen.getZenWidth(), Zen.getZenHeight());
				matrixY += Math.pow(1.3, level) + 10;
				if (matrixY >= Zen.getZenHeight() * 4)
					matrixY = 0;
			}
			if (shiftmix) {
				Zen.drawImage(seizure, gen.nextInt(10) - 10,
						gen.nextInt(10) - 10, Zen.getZenWidth() + 100, Zen
								.getZenHeight() + 100);
			}
			if (o9) {
				Zen.drawImage(over9000, 0, gen.nextInt(100) - 100, Zen
						.getZenWidth(), Zen.getZenHeight() + 100);
			}

			if (isRaining) {
				if (first) {
					// Zen.flipBuffer();
					// Zen.sleep(3000);
					Zen.setFont("Times-25");
					Zen.drawText("Write Dat Numbahahahahaha", 50, 240);
					Zen.flipBuffer();
					Zen.waitForClick();
					first = false;
				}
				checkRain();
				drawNumber();
				// Zen.flipBuffer();

			}
			if (mx >= cx && mx < cx + width && my >= cy && my <= cy + height
					&& avoid) {
				ctr++;
				nextCheckpoint();
			}
			if (ctr == checkpoint) {
				youWin();
			}
			if (level >= 4 && avoid) {
				// add sprite 2
				moveSprite2();
				Zen.drawImage("sprite2.gif", x2, y2);

			}
			checkKeys();
			drawCheckpoint();
			Zen.drawImage("sprite1.gif", x, y);
			Zen.sleep(25 - 2 * speed);
		}

	}
}