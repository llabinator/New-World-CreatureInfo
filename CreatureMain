import java.awt.AWTException;
import java.awt.Color;
import java.awt.Dimension;
import java.awt.Rectangle;
import java.awt.Robot;
import java.awt.Toolkit;
import java.awt.image.BufferedImage;
import java.io.File;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.ArrayList;
import java.util.concurrent.TimeUnit;

import javax.imageio.ImageIO;
import javax.swing.DefaultListModel;
import javax.swing.JFrame;
import javax.swing.JList;
import javax.swing.JScrollPane;

import org.json.JSONArray;
import org.json.JSONObject;

import net.sourceforge.tess4j.ITesseract;
import net.sourceforge.tess4j.Tesseract;
import net.sourceforge.tess4j.TesseractException;

public class MainMob {
		
		public static void main(String[] args) {
			
		Dimension screenSize = Toolkit.getDefaultToolkit().getScreenSize();
		Robot robot = null;
		try {
			robot = new Robot();
		} catch (AWTException e3) {
			System.out.println("Robot Object Failed...");
			e3.printStackTrace();
		}
		
		boolean loop = true;
		boolean first = true;
		boolean add = true;
		//char[] replaceCharacters = { '?', '!', '#', '@', '$', '%', '^', '&', '*', '_', '-', '\\', '/', '\'', '\"', '{' };

	 	JFrame frame = new JFrame(); 	
	 	DefaultListModel<String> model = new DefaultListModel<>();
	 	JList<String> list = new JList<>( model );
	 	JScrollPane scroll = new JScrollPane(list);
	 	frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
	 	frame.setSize(new Dimension(380, 450));
	 	frame.setPreferredSize(new Dimension(380, 450));
	 	frame.setVisible(true);
	 	frame.setAlwaysOnTop(true);
	 	frame.add(scroll);
	 	frame.pack();
	 	
	 	
	 	// Get Creature Data from JSON
	 	JSONArray creatureData = new JSONArray();
	 	
	 	Path fileName = Paths.get("updatedcreatures.json");
	    String content  = "";
	    try {
			content = new String(Files.readAllBytes(fileName));
		} catch (IOException e3) {
			System.out.println("Reading creature data failed...");
			e3.printStackTrace();
		}
	    creatureData = new JSONArray(content);
	    
	    // Initialize Tesseract
    	ITesseract centerInstance = new Tesseract();
    	BufferedImage center;
    	
    	// MAIN LOOP BODY
    	while(loop) {
    		try {
	    		first = true;
		    	center = robot.createScreenCapture(new Rectangle((int) (screenSize.getWidth() / 2 - screenSize.getWidth() / 6), (int) (screenSize.getHeight() / 2 - screenSize.getHeight() / 4), (int) screenSize.getWidth() / 3, (int) screenSize.getHeight() / 2 ));
		    	try {
					ImageIO.write(center, "png", new File("screenShotCenter.png"));
				} catch (IOException e2) {
					System.out.println("IOException: Writing screenshot failed...");
					e2.printStackTrace();
				}
		    	boolean found = false;
				for(int i = 0; i < center.getWidth() && !found; i+=50) {
					for(int j = 0; j < center.getHeight() && !found; j+= 10) {
						Color color = new Color(center.getRGB(i, j), true);
						if(color.getRed() > 110 && color.getGreen() < 35 && color.getBlue() < 35) {
							while((color.getRed() > 110 && color.getGreen() < 35 && color.getBlue() < 35)) {
								try {
									color = new Color(center.getRGB(i -= 4, j));
								} catch(Exception e) {}
							}
							i -= 50;
							try {
								//System.out.println("Rect: " + i + ", " + (j-50) + ", " + (i+400) + ", " + j );
									try {
										center = center.getSubimage(i-50, j-50, (int) (screenSize.getWidth() * 0.15625), 50);
									} catch(Exception e) {
										//IGNORE ERROR, THIS HAPPENS OFTEN WHEN SEARCHING FOR MOB NAMETAGS
										//e.printStackTrace();
									}
								
								for(int k = 0; k < center.getWidth(); k++) {
									for(int l = 0; l < center.getHeight(); l++) {
										
										color = new Color(center.getRGB(k, l));
										if(!((color.getRed() >= 145 && color.getRed() <= 180) && (color.getGreen() >= 145 && color.getGreen() <= 180) && (color.getBlue() >= 145 && color.getBlue() <= 180))) {
											try {
												center.setRGB(k, l, 0);
											} catch(Exception e) {
												System.out.println("Contrasting screenshot failed...");
												e.printStackTrace();
											}
										}
									}
								}
							} catch(Exception e) {
								System.out.println("Error with subimage main:");
								e.printStackTrace();
							}
							try {
								ImageIO.write(center, "png", new File("screenshotContrast.png"));
							} catch (IOException e) {
								System.out.println("IOException: Writing contrast screenshot failed...");
								e.printStackTrace();
							}
							found = true;
						}
					}
				}
		    	
		    	if(found) {
					
					ArrayList<String> foundCreatureData = new ArrayList<>();
					String centerResult = "";
					try {
						centerResult = centerInstance.doOCR(center);
					} catch (TesseractException e1) {
						System.out.println("OCR Reading Failed...");
						e1.printStackTrace();
					}
					if(centerResult.length() > 3 && centerResult.length() < 50) {
						System.out.println("Center Result : " + centerResult);
						//for(char  ch : replaceCharacters)
							//centerResult = centerResult.replace(ch, Character.MIN_VALUE);
						
						//centerResult = centerResult.replaceAll(".*[a-zA-Z]+.*", "");
						//System.out.println("Center Result Cleaned : " + centerResult);
						
						for(int i = 0; i < creatureData.length(); i++) {
							if (centerResult.contains(((JSONObject) creatureData.get(i)).getString("name"))) {
								if(first) {
									first = false;
									model.clear();
								}
								add = true;
				            	System.out.println("======================================================================");
				            	System.out.println("Found Creature: " + creatureData.get(i));
				            	System.out.println("======================================================================");
				            	String data = ((JSONObject) creatureData.get(i)).getString("name") /*+ "   \n" + ((JSONObject) creatureData.get(i)).getString("slug")*/;
				            	data += "   Level: " + ((JSONObject) creatureData.get(i)).getString("level") + "\n   Health: " + ((JSONObject) creatureData.get(i)).getString("health") + "\n   XP: " + ((JSONObject) creatureData.get(i)).getString("xp");
				            	for(String s : foundCreatureData) {
				            		if(s.equals(data))
				            			add = false;
				            	}
				            	if(add) {
					            	foundCreatureData.add(data);
					            	model.insertElementAt(data, 0);
				            	}
				            	try {
									TimeUnit.MILLISECONDS.sleep(1);
								} catch (InterruptedException e) {
									System.out.println("Sleep interrupted...");
									e.printStackTrace();
								}
				            }
						}
						
						list.revalidate();

						
						try {
							if(foundCreatureData.size() > 0)
								TimeUnit.MILLISECONDS.sleep(4800);
							
							TimeUnit.MILLISECONDS.sleep(200);
						} catch (InterruptedException e) {
							System.out.println("Sleep interrupted...");
							e.printStackTrace();
						}
					}
		    	}
    		} catch(Exception e) {
    			System.out.println("Main body triggered Exception...");
    			e.printStackTrace();
    		}
    	}
	}
    	
}
