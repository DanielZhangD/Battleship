import javax.swing.*;
import javax.swing.border.*;
import java.util.*;
import java.awt.*;
import java.awt.event.*;
import java.io.BufferedReader;
import java.io.File;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;

public class BattleshipKGUI {
    
    //JButtonArray, other neccesary global variables
    private static JButton[][] cpu = new JButton[10][10];
    private static JButton[][] player = new JButton[10][10];
    private static char[][] playerBoard = new char[10][10];
    private static char[][] playerDisplay = new char[10][10];
    private static char[][] cpuBoard = new char[10][10];
    private static char[][] cpuDisplay = new char[10][10];
    private static int[][] ships = {{5, 4, 3, 3, 2}, {5, 4, 3, 3, 2}}; // 0 is player, 1 is CPU
    private static int[] lives = {17, 17}; // 0 is player, 1 is CPU
    private static int playerLoad;
    private static int cpuLoad;
    private static int goOrNo = 0;
    private static JFrame frame = new JFrame("BattleshipGUI");
    private static JLabel playerLabel = new JLabel("Please open Player.TXT to start!");
    private static JLabel cpuLabel = new JLabel("Please open Cpu.TXT to start!");
    private static String rovarspraket = "CBSDP";
    private static String[] shipNames = {"Carrier", "Battleship", "Submarine", "Destroyer", "Patrol boat"};
    private static String winner = "Player";
    private static double stats[][] = new double[2][2];
    
    public static int finalCheck(int[] lives) {//Checking if someone has won, and who won
  		if (lives[0] == 0) {
  			goOrNo = 100;
  			winner = "CPU";
  			return 0;
  		} else if (lives[1] == 0) {
  			goOrNo = 100;
  			winner = "Player";
  			return 1;
  		} else {
  			return -1;
  		}
  	}
    
    public static void restartValues() {//restarting the code
    	for (int i = 0; i < 10; i++) {
			for (int j = 0; j < 10; j++) {
				playerDisplay[i][j] = '*';
				cpu[i][j].setIcon(null);
				player[i][j].setIcon(null);
				cpuDisplay[i][j] = '*';
				player[i][j].setText("*");
				goOrNo = 0;
			}
		}
		lives = new int[] {17, 17};
		stats = new double[][] {{0, 0}, {0, 0}};
		ships = new int[][] {{5, 4, 3, 3, 2}, {5, 4, 3, 3, 2}};
		playerLabel.setText("Restarted! Load new files!");
		cpuLabel.setText("Load...");
		goOrNo = 0;
    }
    
    public static void shipSunk(int which) {//Checking which ship has sunk
  		for (int x = 0; x < 5; x++) {
  			if (ships[which][x] == 0) {
  				if (which == 1) {
  					playerLabel.setText(playerLabel.getText() + " You have sunk the CPU's " + shipNames[x] + "!");
  				} else {
  					cpuLabel.setText(cpuLabel.getText() + " The CPU has sunk your " + shipNames[x] + "!");
  				}
  				ships[which][x] = -100;
  			}
  		}
  	}
    
    public static void loadFile(char[][] board) throws IOException {//Loading the file in
    	JFileChooser fc = new JFileChooser();
		fc.setFileSelectionMode(JFileChooser.FILES_AND_DIRECTORIES);
		int returnVal = fc.showOpenDialog(null);
		File file = fc.getSelectedFile();
		BufferedReader inputStream = null;
		int row = 0;
		try {
			inputStream = new BufferedReader(new FileReader(file));
			String lineRead = inputStream.readLine();
			while (lineRead != null) {
				for (int col = 0; col < 20; col += 2){
					board[row][col / 2] = lineRead.charAt(col);
				}
				row++;
				lineRead = inputStream.readLine();
			}
		}
		catch (FileNotFoundException exception) {
			System.out.println("Error opening file");
		}
		finally {		
			if (inputStream != null)
				inputStream.close();
		}
	}
    
    public static void main(String[] args) {
		for (int i = 0; i < 10; i++) {//Initializing the JButtons
			for (int j = 0; j < 10; j++) {
				cpuDisplay[i][j] = '*';
				playerDisplay[i][j] = '*';
				cpu[i][j] = new JButton("*");
				cpu[i][j].setPreferredSize(new Dimension(50, 50));
				cpu[i][j].addActionListener(new ButtonPress());
				player[i][j] = new JButton("*");
				player[i][j].setPreferredSize(new Dimension(50, 50));
				cpu[i][j].setBackground(Color.WHITE);
				player[i][j].setBackground(Color.WHITE);
			}
		}
		//MenuBar 
		JMenuBar menuBar = new JMenuBar();
		JMenu menu = new JMenu("File");
		menuBar.add(menu);
		JMenuItem openPlayer = new JMenuItem("Open Player file");
		JMenuItem exit = new JMenuItem("Exit");
		JMenuItem restart = new JMenuItem("Restart");
		JMenuItem openCpu = new JMenuItem("Open CPU file");
		menu.add(openPlayer);
		menu.add(openCpu);
		menu.add(exit);
		menu.add(restart);
		openPlayer.setAccelerator(KeyStroke.getKeyStroke(KeyEvent.VK_P, ActionEvent.CTRL_MASK));
		openCpu.setAccelerator(KeyStroke.getKeyStroke(KeyEvent.VK_C, ActionEvent.CTRL_MASK));
		exit.setAccelerator(KeyStroke.getKeyStroke(KeyEvent.VK_E, ActionEvent.CTRL_MASK));
		restart.setAccelerator(KeyStroke.getKeyStroke(KeyEvent.VK_R, ActionEvent.CTRL_MASK));
		exit.addActionListener(new Exit());
		restart.addActionListener(new Restart());
		openPlayer.addActionListener(new openFile());
		openCpu.addActionListener(new openFile2());
		
		//Creating the grids, panels
		JLabel fill = new JLabel(" ");
		JLabel fill2 = new JLabel(" ");
		JLabel row1 = new JLabel(new ImageIcon("rows.png"));
		JLabel row2 = new JLabel(new ImageIcon("rows.png"));
		JLabel col1 = new JLabel(new ImageIcon("cols.png"));
		JLabel col2 = new JLabel(new ImageIcon("cols.png"));
		JPanel overall = new JPanel();
		JPanel panel1 = new JPanel(new FlowLayout());
		JPanel panel2 = new JPanel();
		JPanel cpuP = new JPanel(new GridLayout(10, 10));
		JPanel playerP = new JPanel (new GridLayout(10, 10));
		JPanel playerText = new JPanel();
		JPanel cpuText = new JPanel();
		
		//Formatting panels, etc
		overall.setLayout(new BoxLayout(overall, BoxLayout.PAGE_AXIS));
    	panel1.setBorder(BorderFactory.createLineBorder(Color.black));
    	panel2.setLayout(new BoxLayout(panel2, BoxLayout.Y_AXIS));
    	panel2.setBorder(BorderFactory.createLineBorder(Color.black));
    		
    		//Adding the bottom panel areas (borders)
		TitledBorder title1;
		title1 = BorderFactory.createTitledBorder("Player Messages");
		title1.setTitleColor(Color.blue);
		playerText.setBorder(title1);
		TitledBorder title2;
		title2 = BorderFactory.createTitledBorder("CPU Messages");
		title2.setTitleColor(Color.red);
		cpuText.setBorder(title2);
		
		//sizing, adding to frame, creating the GUI
		playerText.setSize(1150, 20);
		cpuText.setSize(1150, 20);
		cpuP.setSize(500, 500);
		playerP.setSize(500, 500);
		overall.setSize(1150, 800);
		panel1.setSize(1150,550);
		panel2.setPreferredSize(new Dimension(1150, 40));
		fill.setPreferredSize(new Dimension(50, 50));
		fill2.setPreferredSize(new Dimension(50, 50));
		
		//Adding panels to the GUI
		panel1.add(fill);
		panel1.add(col1);
		panel1.add(fill2);
		panel1.add(col2);
		panel1.add(row1);
		panel1.add(playerP);
		panel1.add(row2);
		panel1.add(cpuP);
		panel2.add(playerText);
		panel2.add(cpuText);
		playerText.add(playerLabel);
		cpuText.add(cpuLabel);
		playerLabel.setFont(new Font("Serif", Font.BOLD, 22));
		playerLabel.setForeground(Color.blue);
		cpuLabel.setFont(new Font("Serif", Font.BOLD, 22));
		cpuLabel.setForeground(Color.red);
		for (int i = 0; i < 10; i++) {
			for (int j = 0; j < 10; j++) {
				cpuP.add(cpu[i][j]);
				playerP.add(player[i][j]);
			}
		}
		overall.add(panel1);
		overall.add(panel2);
		
		//Frame business
	   	frame.setSize(1150, 800);
	   	frame.setResizable(false);
	   	frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
	   	frame.add(overall);
	   	frame.setJMenuBar(menuBar);
	   	frame.setVisible(true);
    }
    
    private static class ButtonPress implements ActionListener {//interaction
    
		public void actionPerformed(ActionEvent e) {
			int random1 = 0;
			int random2 = 0;
			for (int i = 0; i < 10; i++) {
		    	for (int j = 0; j < 10; j++) {
		    		if (goOrNo == 2) {
			    		if (e.getSource() == cpu[i][j]) { //player[i][j] was clicked
			    			if (cpuDisplay[i][j] == '*') {//Player attack phase
			    				cpuDisplay[i][j] = 's';
			    				stats[1][0]++;
			    				if (cpuBoard[i][j] == '*') {
			    					cpu[i][j].setIcon(new ImageIcon("M.png"));
			    					playerLabel.setText("You have missed, Sir!");
			    				} else {
			    					cpu[i][j].setIcon(new ImageIcon("H.png"));
			    					lives[1]--;
			    					ships[1][rovarspraket.indexOf(cpuBoard[i][j])]--;
			    					playerLabel.setText("Direct hit, nice shot!");
			    					shipSunk(1);
			    					stats[1][1]++;
			    				}
			    				if (finalCheck(lives) == -1) {//cpu AI
			    					do { //cpu AI
			    						random1 = new Random().nextInt(10);
			    						random2 = new Random().nextInt(10);
			    					} while (playerDisplay[random1][random2] != '*');
			    					playerDisplay[random1][random2] = 's';
			    					stats[0][0]++;
			    					if (playerBoard[random1][random2] == '*') {
			    						player[random1][random2].setIcon(new ImageIcon("M.png"));
			    						cpuLabel.setText("The CPU attacked " + (char)(random1 + 65) + (random2) + " and missed!");
			    					} else {
			    						player[random1][random2].setIcon(new ImageIcon("H.png"));
			    						lives[0]--;
			    						ships[0][rovarspraket.indexOf(playerBoard[random1][random2])]--;
			    						cpuLabel.setText("The CPU attacked " + (char)(random1 + 65) + (random2) + " and hit your " + shipNames[rovarspraket.indexOf(playerBoard[random1][random2])] + "!");
			    						shipSunk(0);
			    						stats[0][1]++;
			    					}
			    				}
			    				if (finalCheck(lives) != -1) {//checks if game is over
			    					goOrNo = 100;
			    					JOptionPane.showMessageDialog(frame, "<html>The winner is " + winner + "!<br> Player Statistics:" + "<br>Shots Taken: " + stats[1][0] + "<br>Shots Hit: " + stats[1][1] + "<br>Shots missed: " + (stats[1][0] - stats[1][1]) + "<br>Accuracy: " + Math.round(stats[1][1] / stats[1][0] * 100) + "%</html>");
			    					JOptionPane.showMessageDialog(frame, "Cpu Statistics: \nShots Taken: "  + stats[0][0] + "\nShots Hit: " + stats[0][1] + "\nShots missed: " + (stats[0][0] - stats[0][1]) + "\nAccuracy: " + Math.round(stats[0][1] / stats[0][0] * 100) + "%");
		    					restartValues();	
				    			}
	    			    	}
	    			    }
    			    }
    			}
    		}
    	}
    }
    
    private static class Exit implements ActionListener {//exits game
    		public void actionPerformed(ActionEvent e) {
    			System.exit(0);
    		}
    }
    
    private static class Restart implements ActionListener {//restarts game
    		public void actionPerformed(ActionEvent e) {
    			restartValues();
    		}
    }
    
    private static class openFile implements ActionListener {//opens player file
    		public void actionPerformed(ActionEvent e) {
    			if (goOrNo < 2) {
	    			try {
	    				loadFile(playerBoard);
		    				playerLoad = 1;
						if (playerLoad == 1) {
							playerLabel.setText("File Loaded!");
						}
						goOrNo++;
	    			} catch (IOException e1) {
	    				playerLabel.setText("Load Failed...");
	    			}
	    			for (int i = 0; i < 10; i++) {
	    		   		for (int j = 0; j < 10; j++) {
	    		   			player[i][j].setText(String.valueOf(playerBoard[i][j]));
	    		   		}
	    		   	}
    			} else {
    				playerLabel.setText("You have already loaded files! If you wish to load new ones, restart!");
    			}
    		}
    }
    
    private static class openFile2 implements ActionListener {//opens cpu file
		public void actionPerformed(ActionEvent e) {
			if (goOrNo < 2) {
				try {
					loadFile(cpuBoard);
					cpuLoad = 1;
					if (cpuLoad == 1) {
						cpuLabel.setText("File Loaded!");
					}
					goOrNo++;
				} catch (IOException e1) {
					cpuLabel.setText("Load Failed...");
				}
			} else {
				cpuLabel.setText("You have already loaded files! If you wish to load new ones, restart!");
			}
		}
    }
}


