import java.awt.event.KeyEvent;
import java.awt.event.KeyListener;
import java.awt.image.BufferedImage;
import java.io.IOException;
import java.util.ArrayList;
import java.util.Random;
import javax.imageio.ImageIO;
import javax.swing.JFrame;
import javax.swing.JPanel;

public class EnlightenmentGame extends JPanel implements KeyListener {

    // Set the size of the window
    private final int WIDTH = 700;
    private final int HEIGHT = 500;

    // Create a list of falling objects
    private ArrayList<FallingObject> fallingObjects = new ArrayList<>();

    // Create a player object
    private Player player = new Player(WIDTH / 2, HEIGHT - 50);

    // Set the score
    private int score = 0;

    // Create a random generator
    private Random random = new Random();

    // Create a background image
    private BufferedImage backgroundImage;

    // Create a serene sound
    private Sound sereneSound;

    // Create a flag for game over
    private boolean gameOver = false;

    public EnlightenmentGame() {
        // Set the size of the window
        setSize(WIDTH, HEIGHT);

        // Add the first falling object
        fallingObjects.add(new FallingObject(random.nextInt(WIDTH), 0));

        // Set the background image
        try {
            backgroundImage = ImageIO.read(getClass().getResource("background.jpg"));
        } catch (IOException e) {
            e.printStackTrace();
        }

        // Set the serene sound
        sereneSound = new Sound("serene.wav");

        // Play the serene sound
        sereneSound.loop();

        // Add key listener
        addKeyListener(this);

        // Set the focus
        setFocusable(true);
    }

    public void paint(Graphics g) {
        // Draw the background
        g.drawImage(backgroundImage, 0, 0, this);

        // Draw the falling objects
        for (FallingObject obj : fallingObjects) {
            g.drawImage(obj.getImage(), obj.getX(), obj.getY(), this);
        }

        // Draw the player
        g.drawImage(player.getImage(), player.getX(), player.getY(), this);

        // Draw the score
        g.setColor(Color.WHITE);
        g.setFont(new Font("Arial", Font.PLAIN, 30));
        g.drawString("Score: " + score, 10, 30);

        // Check if game over
        if (gameOver) {
            g.setColor(Color.RED);
            g.setFont(new Font("Arial", Font.BOLD, 50));
            g.drawString("Game Over!", WIDTH / 2 - 150, HEIGHT / 2);
            sereneSound.stop();
        }
    }

    public void update() {
        // Move the falling objects
        for (FallingObject obj : fallingObjects) {
            obj.setY(
