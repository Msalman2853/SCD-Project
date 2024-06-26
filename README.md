import java.awt.Color;
import java.awt.EventQueue;
import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JPanel;
import javax.swing.JTextField;
import javax.swing.JRadioButton;
import javax.swing.ButtonGroup;
import javax.swing.border.EmptyBorder;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class Calculator extends JFrame {

    private static final long serialVersionUID = 1L;
    private JPanel contentPane;
    private JTextField display;
    private double num1, num2, result;
    private char operator;
    private boolean isOn;
    private final Color backgroundColor = new Color(255, 204, 102);
    private final Color buttonColor = new Color(0, 102, 102);
    private final Color operatorColor = new Color(255, 102, 102);
    private final Color specialButtonColor = new Color(0, 204, 153);
    public static void main(String[] args) {
        EventQueue.invokeLater(new Runnable() {
            public void run() {
                try {
                    Calculator frame = new Calculator();
                    frame.setVisible(true);
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        });
    }

   
    public Calculator() {
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setBounds(100, 100, 400, 500);
        contentPane = new JPanel();
        contentPane.setBackground(backgroundColor);
        contentPane.setBorder(new EmptyBorder(5, 5, 5, 5));
        setContentPane(contentPane);
        contentPane.setLayout(null);

        display = new JTextField();
        display.setEditable(false);
        display.setBounds(10, 10, 360, 50);
        contentPane.add(display);
        display.setColumns(10);

        JRadioButton rdbtnOn = new JRadioButton("on");
        rdbtnOn.setBounds(10, 70, 50, 30);
        rdbtnOn.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                display.setText("");
                display.setEnabled(true);
                isOn = true;
            }
        });
        contentPane.add(rdbtnOn);

        JRadioButton rdbtnOff = new JRadioButton("off");
        rdbtnOff.setBounds(70, 70, 50, 30);
        rdbtnOff.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                display.setText("");
                display.setEnabled(false);
                isOn = false;
            }
        });
        contentPane.add(rdbtnOff);

        ButtonGroup bg = new ButtonGroup();
        bg.add(rdbtnOn);
        bg.add(rdbtnOff);

        String[] buttonLabels = {
            "C", "DEL", "/", 
            "7", "8", "9", "*",
            "4", "5", "6", "-",
            "1", "2", "3", "+",
            "0", ".", "=", "%"
        };

        int x = 10, y = 110;
        for (String text : buttonLabels) {
            JButton button = new JButton(text);
            button.setBounds(x, y, 80, 50);
            button.setBackground(buttonColor);
            if (text.equals("/") || text.equals("*") || text.equals("-") || text.equals("+") || text.equals("%") || text.equals("=")) {
                button.setBackground(operatorColor);
            } else if (text.equals("C") || text.equals("DEL")) {
                button.setBackground(specialButtonColor);
            }
            button.addActionListener(new ButtonClickListener());
            contentPane.add(button);
            x += 90;
            if (x > 280) {
                x = 10;
                y += 60;
            }
        }
    }

    private class ButtonClickListener implements ActionListener {
        public void actionPerformed(ActionEvent e) {
            String command = e.getActionCommand();

            if (!isOn) {
                return;
            }

            if (command.equals("C")) {
                display.setText("");
                return;
            }

            if (command.equals("DEL")) {
                String currentText = display.getText();
                if (currentText.length() > 0) {
                    display.setText(currentText.substring(0, currentText.length() - 1));
                }
                return;
            }

            if (command.equals("=")) {
                num2 = Double.parseDouble(display.getText());
                switch (operator) {
                    case '+': result = num1 + num2; break;
                    case '-': result = num1 - num2; break;
                    case '*': result = num1 * num2; break;
                    case '/': result = num1 / num2; break;
                    case '%': result = num1 * (num2 / 100); break;
                }
                display.setText(String.valueOf(result));
                return;
            }

            if (command.equals("+") || command.equals("-") || command.equals("*") || command.equals("/") || command.equals("%")) {
                num1 = Double.parseDouble(display.getText());
                operator = command.charAt(0);
                display.setText("");
                return;
            }

            display.setText(display.getText() + command);
        }
    }
}
