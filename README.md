import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.Random;

public class TecladoVirtualApp extends JFrame implements ActionListener {
    private JTextArea textArea;
    private JButton[] letterButtons;
    private String[] pangramas;
    private int correctCount;
    private int incorrectCount;
    private StringBuilder userTypedKeys;
    private String currentPangrama;

    public TecladoVirtualApp() {
        // Inicializar variables
        pangramas = cargarPangramasDesdeArchivo();
        correctCount = 0;
        incorrectCount = 0;
        userTypedKeys = new StringBuilder();
        currentPangrama = "";

        // Configurar la interfaz gráfica
        setTitle("Teclado Virtual App");
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(new BorderLayout());

        // Configurar JTextArea para mostrar lo que ha escrito el usuario
        textArea = new JTextArea();
        add(new JScrollPane(textArea), BorderLayout.CENTER);

        // Configurar el teclado virtual con JButton
        JPanel keyboardPanel = new JPanel(new GridLayout(4, 7));

        letterButtons = new JButton[26];
        for (int i = 0; i < 26; i++) {
            char letter = (char) ('A' + i);
            letterButtons[i] = new JButton(String.valueOf(letter));
            letterButtons[i].addActionListener(this);
            keyboardPanel.add(letterButtons[i]);
        }

        add(keyboardPanel, BorderLayout.SOUTH);

        pack();
        setLocationRelativeTo(null);

        // Inicializar la aplicación con un pangrama aleatorio
        mostrarPangramaAleatorio();

        setVisible(true);
    }

    // Método para cargar los pangramas desde un archivo de texto
    private String[] cargarPangramasDesdeArchivo() {
        // Implementa la lógica para cargar los pangramas desde un archivo
        // Retorna un array de strings con los pangramas
        // Ejemplo: return new String[]{"Pangrama 1", "Pangrama 2", ...};
        return null;
    }

    // Método para mostrar un pangrama aleatorio en la interfaz gráfica
    private void mostrarPangramaAleatorio() {
        if (pangramas != null && pangramas.length > 0) {
            Random random = new Random();
            int index = random.nextInt(pangramas.length);
            currentPangrama = pangramas[index];

            JOptionPane.showMessageDialog(this, "Pangrama:\n" + currentPangrama, "Pangrama", JOptionPane.INFORMATION_MESSAGE);
        }
    }

    // Manejar eventos de botones (teclas)
    @Override
    public void actionPerformed(ActionEvent e) {
        JButton source = (JButton) e.getSource();
        char pressedKey = source.getText().charAt(0);

        // Actualizar el JTextArea con la tecla presionada
        userTypedKeys.append(pressedKey);
        textArea.setText(userTypedKeys.toString());

        // Comparar con la letra actual del pangrama
        char currentPangramaLetter = currentPangrama.charAt(userTypedKeys.length() - 1);

        if (pressedKey == currentPangramaLetter) {
            correctCount++;
        } else {
            incorrectCount++;
            // Aquí puedes hacer algo para identificar las teclas que se le dificultan al usuario
            // Por ejemplo, resaltar el botón correspondiente en rojo, agregarlo a una lista, etc.
        }

        // Comprobar si se ha completado el pangrama
        if (userTypedKeys.length() == currentPangrama.length()) {
            // Reiniciar el seguimiento y mostrar un nuevo pangrama
            userTypedKeys.setLength(0);
            mostrarPangramaAleatorio();
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> new TecladoVirtualApp());
    }
}
