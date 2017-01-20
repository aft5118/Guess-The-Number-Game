# Guess-The-Number-Game
In this game, my program generates a random number and the user has to make attempts to guess the random number. The background of the game changes red when you are getting warmer to the mystery number, and blue when you are getting colder (farther away). 

package guesstherightnumber;
import java.awt.FlowLayout;
import java.awt.event.ActionListener;
import java.awt.event.ActionEvent;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JTextField;
import javax.swing.JButton;
import javax.swing.JOptionPane;
import java.security.SecureRandom;
import java.awt.Color;
import javax.swing.Icon;
import javax.swing.ImageIcon;
import javax.swing.SwingConstants;
import java.awt.Font;
import static java.lang.Math.*;
import java.io.*;
import javax.swing.UIManager;


public class GuessTheNumberGame extends JFrame{
    private static final SecureRandom randomNumbers = new SecureRandom();
    private int number;
    private final JLabel title;
    private final JLabel prompt;
    private final JTextField guessField;
    private final JButton continueButton;
    private final JLabel message;
    private int lastGuess;
    private int counter;
    private final JLabel numberOfAttempts;
    private final JLabel highScoreLabel;
    private int highscore;
    private final JLabel highScoreMessage;
   
    
    public GuessTheNumberGame(){
        super("");
        setLayout(new FlowLayout());
        
        getContentPane().setBackground(Color.WHITE);
        
        number = randomNumbers.nextInt(1000);
        
        
        title = new JLabel("Guess The Number Game", SwingConstants.CENTER);
        title.setFont(new Font("Comic Sans MS", Font.BOLD, 24));
        add(title);
        
        message = new JLabel("");
        
        numberOfAttempts = new JLabel("");
        numberOfAttempts.setFont(new Font("Comic Sans MS", Font.BOLD, 14)); 
        
        highScoreLabel = new JLabel("");
        highScoreLabel.setFont(new Font("Comic Sans MS", Font.BOLD, 14)); 
        
        Icon numberImage = new ImageIcon(getClass().getResource("numberimage (1).png"));
        prompt = new JLabel("<html>I have a number between 1 and 1000. Can you guess my number? <br> Please enter your first guess. <html>", numberImage, SwingConstants.CENTER);
        prompt.setFont(new Font("Comic Sans MS", Font.BOLD, 14)); 
        add(prompt);
     
        guessField = new JTextField(4);
        add(guessField);
        
        
        continueButton = new JButton("Generate New Number");
        continueButton.setFont(new Font("Comic Sans MS", Font.PLAIN, 14));
        
        highScoreMessage = new JLabel("");
        Icon highScorePic = new ImageIcon(getClass().getResource("ahighscore.png"));
        

        highscore = 10000;
        
        
        guessField.addActionListener(
        new ActionListener()
        {
            @Override
            public void actionPerformed(ActionEvent event) {
            int guessNumber = 0;
            String guessString = "";
            message.setVisible(true);
            
            if (event.getSource() == guessField) {
               try {
                guessString = event.getActionCommand();
                guessNumber = Integer.parseInt(guessString);
               if (guessNumber > 1000 || guessNumber < 0) {
                    message.setText("Your guess is out of range. Please enter a number between 1-1000.");
               }
               else if(guessNumber > number) {
                    message.setText("Too high!");
                    if (counter == 0) {
                        getContentPane().setBackground(Color.RED);
                    }
                    if (counter > 0) {
                        int difference1 = 0;
                        int difference2 = 0;
                        difference1 = abs(number - guessNumber);
                        difference2 = abs(number - lastGuess);
                        if (difference1 < difference2) {
                            getContentPane().setBackground(Color.RED);
                        }
                        else {
                            getContentPane().setBackground(Color.BLUE);
                        }
                    }
                }
                else if (guessNumber < number) {
                     message.setText("Too low!");
                    if (counter == 0) {
                        getContentPane().setBackground(Color.RED);
                    }
                    if (counter > 0) {
                        int difference1 = 0;
                        int difference2 = 0;
                        difference1 = abs(number - guessNumber);
                        difference2 = abs(number - lastGuess);
                        if (difference1 < difference2) {
                            getContentPane().setBackground(Color.RED);
                        }
                        else {
                            getContentPane().setBackground(Color.BLUE);
                        }
                    }
                }
                else {
                    ++counter;
                    numberOfAttempts.setText("Number of attempts: " + counter);
                    message.setText("Correct!");
                    guessField.setEditable(false);
                    add(continueButton);
                     if (counter < highscore) {
                        highscore = counter;
                        highScoreMessage.setText("New High Score! High Score: " + highscore);
                        highScoreMessage.setFont(new Font("Comic Sans MS", Font.BOLD, 14));
                        UIManager UI = new UIManager();
                        UI.put("OptionPane.background", Color.GREEN);
                        UI.put("Panel.background", Color.GREEN);
                        JOptionPane.showMessageDialog(null,highScoreMessage, "", JOptionPane.PLAIN_MESSAGE, highScorePic);
                    }
                    highScoreLabel.setText("High Score: " + highscore);
                    continueButton.setVisible(true);
                    numberOfAttempts.setVisible(true);
                    highScoreLabel.setVisible(true);
                    continueButton.addActionListener(
                    new ActionListener()
                    {
                        @Override
                        public void actionPerformed(ActionEvent event) {
                            if(event.getSource() == continueButton) {
                                number = randomNumbers.nextInt(1000);
                                continueButton.setVisible(false);
                                numberOfAttempts.setVisible(false);
                                highScoreLabel.setVisible(false);
                                message.setVisible(false);
                                getContentPane().setBackground(Color.WHITE);
                                guessField.setEditable(true);
                                counter = 0;
                                
                            }
                        }
                    }       
                    );
                }
                
                } catch (Exception e) {
                  System.err.println("");
                  message.setText("Incorrect input. Please enter a valid guess.");
                  add(message);
                }
            lastGuess = guessNumber; 
            ++counter;
            }
        }
    }
    );
        message.setFont(new Font("Comic Sans MS", Font.BOLD, 14));
        add(message);
        add(numberOfAttempts);
        add(highScoreLabel);
    }
    
    public static void main(String[] args) {
        GuessTheNumberGame guessTheRightNumber = new GuessTheNumberGame();
        guessTheRightNumber.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        guessTheRightNumber.setSize(565, 250);
        guessTheRightNumber.setVisible(true);
    }
    
}
