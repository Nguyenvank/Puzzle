# Puzzle
package Puzzle.java;

import java.awt.BorderLayout;
import java.awt.Color;
import java.awt.Dimension;
import java.awt.Font;
import java.awt.FontMetrics;
import java.awt.Graphics;
import java.awt.Graphics2D;
import java.awt.RenderingHints;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;
import java.awt.image.BufferedImage;
import java.io.File;
import java.io.IOException;
import java.util.Random;
import javax.imageio.ImageIO;
import javax.swing.ImageIcon;
import javax.swing.JFrame;
import javax.swing.JOptionPane;
import javax.swing.JPanel;
import sun.swing.SwingUtilities2;

/**
 *
 * @author Admin
 */
public class GameFifteen extends JPanel{
    private int size;
    private int nbtiles;
    private int dimension;
    private static final Color FOREGROUND_COLOR =new Color(239,83,80);
    private static final Random rd=new Random();
    private int[] tiles;
    private int tileSize;
    private int blankPos;
    private int margin;
    private int gridSize;
    private boolean over;
    public GameFifteen  (int size,int dim,int mar){
        this.size=size;
        this.dimension=dim;
        this.margin=mar;
        nbtiles=size*size-1;
        tiles=new int[size*size];
        gridSize=(dim-2*margin);
        tileSize=gridSize/size;
        setPreferredSize(new Dimension(dimension, dimension+margin));
        setBackground(Color.YELLOW);
        setForeground(FOREGROUND_COLOR);
        setFont(new Font("\t\t\t\t cu khoa gia vip    \t\t\\t   ",Font.BOLD, Font.CENTER_BASELINE));
        over=true;
        
        addMouseListener(new MouseAdapter() {
            @Override
            public void mousePressed(MouseEvent e) {
                 //To change body of generated methods, choose Tools | Templates.
                  if(over) newGame();
                  else{
                      int ex=e.getX()-margin;
                      int ey=e.getY()-margin;
                      if(ex < 0 ||ex > gridSize || ey <0 || ey > gridSize)  return;
                      int c1=ex/tileSize;
                      int c2= ey/tileSize;
                      int r1=blankPos %size;
                      int r2=blankPos/size;
                      int click =c2*size+c1;
                      int dir=0;
                      if(c1==r1  && Math.abs(c2-r2)>0)   dir =(c2-r2) >0 ? size: -size;
                      if(c2==r2&& Math.abs(c1-r1)>0)  dir=(c1-r1) >0? 1: -1;
                      if(dir!=0) {
                          do{
                              int newblankpos=blankPos+dir;
                              tiles[blankPos]=tiles[newblankpos];
                              blankPos=newblankpos;
                          }while(blankPos!=click);
                          tiles[blankPos]=0;
                      }
                      over=isSolved();
                  }
                  repaint();
          }  
        });
           
        newGame();    
        
        } 

    GameFifteen() {
        throw new UnsupportedOperationException("Not supported yet."); //To change body of generated methods, choose Tools | Templates.
    }
    public ImageIcon anh(String s){
        ImageIcon sid =new ImageIcon(s);
        return sid;
    }
    
    
    public void newGame(){
        
    
          do{
              reset();
              shuffle();
              
          }while(isSolvable()==0);
                  over=false;
      }
            
    private void reset(){
        for(int i=0;i<tiles.length;i++){
            tiles[i]=(i+1)% tiles.length;
        }
        blankPos=tiles.length-1;
    }
    // khoi tao trang thai dau tien cho bang
    private void shuffle(){
        int n=nbtiles;
        while (n>1){
            int r=rd.nextInt(n--);
            int temp=tiles[r];
            tiles[r]=tiles[n];
            tiles[n]=temp;
            
        }
    }
    //  dien dien ket thuc game
      private int  isSolvable(){
        int countI=0;
        for(int i=0;i<nbtiles;i++){
            for(int j=0;j<i;j++){
                if(tiles[j]>tiles[i])
                    countI++;
            }
        }
        return countI%2;
        
    }
      
     private boolean isSolved(){
         if (tiles[tiles.length-1]!=0) return false;
         for(int i=nbtiles-1;i>0;i++){
             if(tiles[i]!=i+1) return false;
         }
         return true;
           
         
     }
     public void drawGrid(Graphics2D g){
         int i;
         for( i=0;i<tiles.length;i++){
             int r=i/size;
             int c=i%size;
             
             int x=margin+ c*tileSize;
             int y=margin+r*tileSize;
             
         
         if(tiles[i]==0){
             if(over){
                 g.setColor(FOREGROUND_COLOR);
                  drawCenteredString(g, "", x, y);
             }
             continue;
         }
         g.setColor(getForeground());
         g.fillRoundRect(x, y, tileSize, tileSize, 25, 25);
         g.drawRoundRect(x, y, tileSize, tileSize, 25 , 25);
         g.setColor(Color.red);
             drawCenteredString(g,String.valueOf(tiles[i]), x, y);
     }
   
}
     public void drawStartMessage(Graphics2D g){
         if (over ){
             g.setFont(getFont().deriveFont(Font.BOLD, 18));
             g.setColor(FOREGROUND_COLOR);
             String s="click to start game";
             g.drawString(s, 26, TOP_ALIGNMENT);
             
             
         }
     }

    private void drawCenteredString(Graphics2D g, String s,int x, int y) {
        
    
        FontMetrics fm=g.getFontMetrics();
        Font asc= fm.getFont();
        Font desc=fm.getFont();
        
        
        
        
        
 
    }
    protected void paintComponent(Graphics g){
        super.paintComponent(g);
        Graphics2D g2D=(Graphics2D) g;
        g2D.setRenderingHint(RenderingHints.KEY_ANTIALIASING, RenderingHints.VALUE_ANTIALIAS_ON);
        drawGrid(g2D);
        drawStartMessage(g2D);
    }

    private Object setFont() {
        throw new UnsupportedOperationException("Not supported yet."); //To change body of generated methods, choose Tools | Templates.
    }

public static void main(String [] args)
{
   
       
   
   JFrame frame=new JFrame();
   frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
   frame.setTitle("game");
   frame.setResizable(false);
   frame.add(new GameFifteen(3,550,30),BorderLayout.SOUTH);
   frame.pack();
   frame.setLocationRelativeTo(null);
   frame.setVisible(true);
     
}

   
