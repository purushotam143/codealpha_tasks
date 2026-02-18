# codealpha_tasks
Java programming 
# 🎓 Student Grade Tracker (GUI Version)

## 📌 CodeAlpha Java Internship Task

This project is developed as part of the CodeAlpha Internship.

## 💡 Features
- Add student name
- Enter grades (comma separated)
- Calculate Average
- Find Highest score
- Find Lowest score
- Display summary report

## 🛠 Technologies Used
- Java
- Swing
- ArrayList
- OOP Concepts

## ▶ How to Run
Compile:
javac StudentGradeTrackerGUI.java

Run:
java StudentGradeTrackerGUI

## 👨‍💻 Author
Purushotam Sharma

import javax.swing.*;
import java.awt.*;
import java.util.ArrayList;

class Student {
    String name;
    ArrayList<Integer> grades;

    Student(String name) {
        this.name = name;
        grades = new ArrayList<>();
    }

    void addGrade(int grade) {
        grades.add(grade);
    }

    double getAverage() {
        if (grades.isEmpty()) return 0;
        int sum = 0;
        for (int g : grades) sum += g;
        return (double) sum / grades.size();
    }

    int getHighest() {
        if (grades.isEmpty()) return 0;
        int max = grades.get(0);
        for (int g : grades) if (g > max) max = g;
        return max;
    }

    int getLowest() {
        if (grades.isEmpty()) return 0;
        int min = grades.get(0);
        for (int g : grades) if (g < min) min = g;
        return min;
    }
}

public class StudentGradeTrackerGUI extends JFrame {

    private JTextField nameField;
    private JTextField gradesField;
    private JTextArea outputArea;
    private ArrayList<Student> students;

    public StudentGradeTrackerGUI() {

        students = new ArrayList<>();

        setTitle("🎓 Student Grade Tracker - CodeAlpha");
        setSize(650, 550);
        setLocationRelativeTo(null);
        setDefaultCloseOperation(EXIT_ON_CLOSE);

        setLayout(new BorderLayout(15, 15));

        // ===== Title =====
        JLabel title = new JLabel("Student Grade Tracker", JLabel.CENTER);
        title.setFont(new Font("Arial", Font.BOLD, 22));
        title.setBorder(BorderFactory.createEmptyBorder(10, 10, 10, 10));
        add(title, BorderLayout.NORTH);

        // ===== Main Panel =====
        JPanel mainPanel = new JPanel(new BorderLayout(10, 10));
        mainPanel.setBorder(BorderFactory.createEmptyBorder(10, 20, 10, 20));

        // ===== Input Panel =====
        JPanel inputPanel = new JPanel(new GridLayout(2, 2, 10, 10));

        inputPanel.add(new JLabel("Student Name:"));
        nameField = new JTextField();
        inputPanel.add(nameField);

        inputPanel.add(new JLabel("Grades (comma separated):"));
        gradesField = new JTextField();
        inputPanel.add(gradesField);

        mainPanel.add(inputPanel, BorderLayout.NORTH);

        // ===== Button Panel =====
        JPanel buttonPanel = new JPanel(new FlowLayout(FlowLayout.CENTER, 15, 10));

        JButton addBtn = new JButton("Add Student");
        JButton reportBtn = new JButton("Show Report");
        JButton clearBtn = new JButton("Clear");

        buttonPanel.add(addBtn);
        buttonPanel.add(reportBtn);
        buttonPanel.add(clearBtn);

        mainPanel.add(buttonPanel, BorderLayout.CENTER);

        // ===== Output Area =====
        outputArea = new JTextArea();
        outputArea.setEditable(false);
        outputArea.setFont(new Font("Monospaced", Font.PLAIN, 14));
        outputArea.setBorder(BorderFactory.createLineBorder(Color.GRAY));

        JScrollPane scrollPane = new JScrollPane(outputArea);
        scrollPane.setPreferredSize(new Dimension(500, 250));

        mainPanel.add(scrollPane, BorderLayout.SOUTH);

        add(mainPanel, BorderLayout.CENTER);

        // ===== Footer =====
        JLabel footer = new JLabel("Designed by Purushotam Sharma", JLabel.CENTER);
        footer.setFont(new Font("Arial", Font.ITALIC, 14));
        footer.setBorder(BorderFactory.createEmptyBorder(10, 10, 10, 10));
        footer.setForeground(Color.DARK_GRAY);
        add(footer, BorderLayout.SOUTH);

        // ===== Button Actions =====
        addBtn.addActionListener(e -> addStudent());
        reportBtn.addActionListener(e -> showReport());
        clearBtn.addActionListener(e -> clearFields());
    }

    private void addStudent() {

        String name = nameField.getText().trim();
        String gradeText = gradesField.getText().trim();

        if (name.isEmpty() || gradeText.isEmpty()) {
            JOptionPane.showMessageDialog(this, "Please fill all fields!");
            return;
        }

        Student student = new Student(name);

        try {
            String[] grades = gradeText.split(",");
            for (String g : grades) {
                student.addGrade(Integer.parseInt(g.trim()));
            }

            students.add(student);
            JOptionPane.showMessageDialog(this, "Student Added Successfully!");

            nameField.setText("");
            gradesField.setText("");

        } catch (NumberFormatException ex) {
            JOptionPane.showMessageDialog(this, "Enter valid numbers separated by commas!");
        }
    }

    private void showReport() {

        if (students.isEmpty()) {
            outputArea.setText("No student records available.\n");
            return;
        }

        outputArea.setText("===== STUDENT REPORT =====\n\n");

        for (Student s : students) {
            outputArea.append("Name     : " + s.name + "\n");
            outputArea.append("Average  : " + String.format("%.2f", s.getAverage()) + "\n");
            outputArea.append("Highest  : " + s.getHighest() + "\n");
            outputArea.append("Lowest   : " + s.getLowest() + "\n");
            outputArea.append("-----------------------------------\n");
        }
    }

    private void clearFields() {
        nameField.setText("");
        gradesField.setText("");
        outputArea.setText("");
    }

    public static void main(String[] args) {

        try {
            UIManager.setLookAndFeel(UIManager.getSystemLookAndFeelClassName());
        } catch (Exception ignored) {}

        SwingUtilities.invokeLater(() -> {
            new StudentGradeTrackerGUI().setVisible(true);
        });
    }
}
