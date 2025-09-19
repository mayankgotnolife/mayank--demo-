import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import static java.lang.Math.*;

public class ScientificCalculator extends JFrame implements ActionListener {
    private JTextField display;
    private double num1 = 0, num2 = 0, result = 0;
    private char op;
    private boolean isNewInput = true;
    
    public ScientificCalculator() {
        setTitle("Scientific Calculator");
        setSize(400, 500);
        setDefaultCloseOperation(EXIT_ON_CLOSE);
        setLayout(new BorderLayout());
        
        // Display
        display = new JTextField("0");
        display.setFont(new Font("Arial", Font.BOLD, 20));
        display.setHorizontalAlignment(JTextField.RIGHT);
        display.setEditable(false);
        add(display, BorderLayout.NORTH);
        
        // Buttons panel
        JPanel panel = new JPanel(new GridLayout(6, 5, 2, 2));
        String[] buttons = {
            "sin", "cos", "tan", "log", "ln",
            "√", "x²", "x³", "1/x", "π",
            "C", "±", "%", "÷", "×",
            "7", "8", "9", "-", "(",
            "4", "5", "6", "+", ")",
            "1", "2", "3", "=", ".",
            "0", "0", "0", "=", "."
        };
        
        for(int i = 0; i < 35; i++) {
            if(i == 30 || i == 31) continue; // Skip duplicate 0
            JButton btn = new JButton(buttons[i]);
            btn.setFont(new Font("Arial", Font.BOLD, 12));
            btn.addActionListener(this);
            
            // Color coding
            if("+-×÷=".contains(buttons[i])) btn.setBackground(Color.ORANGE);
            else if("C±%".contains(buttons[i])) btn.setBackground(Color.LIGHT_GRAY);
            else if(buttons[i].matches("\\d")) btn.setBackground(Color.WHITE);
            else btn.setBackground(Color.CYAN);
            
            panel.add(btn);
        }
        
        // Make 0 span 3 columns
        panel.remove(panel.getComponent(25));
        JButton zeroBtn = new JButton("0");
        zeroBtn.setFont(new Font("Arial", Font.BOLD, 12));
        zeroBtn.addActionListener(this);
        zeroBtn.setBackground(Color.WHITE);
        panel.add(zeroBtn, 25);
        
        add(panel, BorderLayout.CENTER);
        setVisible(true);
    }
    
    public void actionPerformed(ActionEvent e) {
        String cmd = e.getActionCommand();
        
        try {
            if(cmd.matches("\\d") || cmd.equals(".")) {
                if(isNewInput) {
                    display.setText(cmd.equals(".") ? "0." : cmd);
                    isNewInput = false;
                } else {
                    if(cmd.equals(".") && display.getText().contains(".")) return;
                    display.setText(display.getText() + cmd);
                }
            }
            else if("+-×÷".contains(cmd)) {
                num1 = Double.parseDouble(display.getText());
                op = cmd.charAt(0);
                if(cmd.equals("×")) op = '*';
                if(cmd.equals("÷")) op = '/';
                isNewInput = true;
            }
            else if(cmd.equals("=")) {
                num2 = Double.parseDouble(display.getText());
                switch(op) {
                    case '+': result = num1 + num2; break;
                    case '-': result = num1 - num2; break;
                    case '*': result = num1 * num2; break;
                    case '/': result = num1 / num2; break;
                }
                display.setText(formatResult(result));
                isNewInput = true;
            }
            else if(cmd.equals("C")) {
                display.setText("0");
                num1 = num2 = result = 0;
                isNewInput = true;
            }
            else if(cmd.equals("±")) {
                double val = Double.parseDouble(display.getText());
                display.setText(formatResult(-val));
            }
            else if(cmd.equals("%")) {
                double val = Double.parseDouble(display.getText());
                display.setText(formatResult(val / 100));
            }
            else {
                double val = Double.parseDouble(display.getText());
                switch(cmd) {
                    case "sin": result = sin(toRadians(val)); break;
                    case "cos": result = cos(toRadians(val)); break;
                    case "tan": result = tan(toRadians(val)); break;
                    case "log": result = log10(val); break;
                    case "ln": result = log(val); break;
                    case "√": result = sqrt(val); break;
                    case "x²": result = pow(val, 2); break;
                    case "x³": result = pow(val, 3); break;
                    case "1/x": result = 1.0 / val; break;
                    case "π": result = PI; break;
                }
                display.setText(formatResult(result));
                isNewInput = true;
            }
        } catch(Exception ex) {
            display.setText("Error");
            isNewInput = true;
        }
    }
    
    private String formatResult(double val) {
        if(val == (long)val) return String.valueOf((long)val);
        return String.valueOf(val);
    }
    
    public static void main(String[] args) {
        new ScientificCalculator();
    }
}
