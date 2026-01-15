// Java Music Player

// A simple Java Swing music player that plays WAV audio files.

// Features
// Open WAV files
// Play & Stop buttons
//Clean Swing interface

##  Source Code

```java
import javax.sound.sampled.*;
import javax.swing.*;
import java.awt.*;
import java.io.File;

public class MusicPlayer extends JFrame {

    private Clip clip;
    private JLabel songLabel;
    private JButton playButton;
    private JButton stopButton;

    public MusicPlayer() {
        setTitle("Java Music Player");
        setSize(350, 200);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(new BorderLayout());
        setLocationRelativeTo(null);

        songLabel = new JLabel("No song selected", SwingConstants.CENTER);

        JButton openButton = new JButton("Open");
        playButton = new JButton("Play");
        stopButton = new JButton("Stop");

        playButton.setEnabled(false);
        stopButton.setEnabled(false);

        JPanel buttonPanel = new JPanel();
        buttonPanel.add(openButton);
        buttonPanel.add(playButton);
        buttonPanel.add(stopButton);

        add(songLabel, BorderLayout.NORTH);
        add(buttonPanel, BorderLayout.CENTER);

        openButton.addActionListener(e -> openFile());
        playButton.addActionListener(e -> play());
        stopButton.addActionListener(e -> stop());
    }

    private void openFile() {
        JFileChooser chooser = new JFileChooser();
        int result = chooser.showOpenDialog(this);

        if (result == JFileChooser.APPROVE_OPTION) {
            try {
                if (clip != null) {
                    clip.stop();
                    clip.close();
                }

                File file = chooser.getSelectedFile();
                AudioInputStream audioStream =
                        AudioSystem.getAudioInputStream(file);

                clip = AudioSystem.getClip();
                clip.open(audioStream);

                songLabel.setText(file.getName());
                playButton.setEnabled(true);
                stopButton.setEnabled(true);

            } catch (Exception ex) {
                JOptionPane.showMessageDialog(
                        this,
                        "Only WAV files are supported",
                        "Error",
                        JOptionPane.ERROR_MESSAGE
                );
            }
        }
    }

    private void play() {
        if (clip != null) {
            clip.setFramePosition(0);
            clip.start();
        }
    }

    private void stop() {
        if (clip != null && clip.isRunning()) {
            clip.stop();
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            new MusicPlayer().setVisible(true);
        });
    }
}
```
