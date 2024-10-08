import javax.swing.*;
import javax.swing.table.DefaultTableModel;
import java.awt.*;
import java.awt.event.*;
import java.util.ArrayList;

class Contact {
    String name;
    String phoneNumber;
    String email;

    Contact(String name, String phoneNumber, String email) {
        this.name = name;
        this.phoneNumber = phoneNumber;
        this.email = email;
    }
}

public class JJ extends JFrame {
    ArrayList<Contact> contacts = new ArrayList<>();
    JTextField nameField, phoneField, emailField, searchField;
    JTable contactTable;
    DefaultTableModel tableModel;

    public JJ() {
        setTitle("Enhanced Phonebook Application");
        setSize(600, 500);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        // Set the background color of the entire frame
        getContentPane().setBackground(new Color(50, 50, 50)); // Classy grey background

        // Use BorderLayout for structure
        setLayout(new BorderLayout());

        // Top panel for contact input
        JPanel inputPanel = new JPanel(new GridLayout(4, 2, 10, 10));
        inputPanel.setBorder(BorderFactory.createEmptyBorder(10, 10, 10, 10));
        inputPanel.setBackground(new Color(50, 50, 50)); // Apply grey background to the panel as well
        inputPanel.setForeground(Color.WHITE); // Set text color to white for better readability

        // Input fields and labels
        JLabel nameLabel = new JLabel("Name:");
        nameLabel.setForeground(Color.WHITE);
        inputPanel.add(nameLabel);
        nameField = new JTextField();
        inputPanel.add(nameField);

        JLabel phoneLabel = new JLabel("Phone Number:");
        phoneLabel.setForeground(Color.WHITE);
        inputPanel.add(phoneLabel);
        phoneField = new JTextField();
        inputPanel.add(phoneField);

        JLabel emailLabel = new JLabel("Email:");
        emailLabel.setForeground(Color.WHITE);
        inputPanel.add(emailLabel);
        emailField = new JTextField();
        inputPanel.add(emailField);

        // Insert button with blue color
        JButton insertBtn = new JButton("Insert Contact");
        styleButton(insertBtn);
        insertBtn.addActionListener(e -> insertContact());
        inputPanel.add(insertBtn);

        add(inputPanel, BorderLayout.NORTH);

        // Table for displaying contacts
        tableModel = new DefaultTableModel(new String[]{"Name", "Phone Number", "Email"}, 0);
        contactTable = new JTable(tableModel);
        JScrollPane scrollPane = new JScrollPane(contactTable);
        add(scrollPane, BorderLayout.CENTER);

        // Bottom panel for search and delete
        JPanel actionPanel = new JPanel();
        actionPanel.setLayout(new FlowLayout());
        actionPanel.setBackground(new Color(50, 50, 50)); // Grey background for action panel

        // Search field and button
        searchField = new JTextField(15);
        actionPanel.add(new JLabel("Search:"));
        JButton searchBtn = new JButton("Search");
        styleButton(searchBtn);
        searchBtn.addActionListener(e -> searchContact());
        actionPanel.add(searchField);
        actionPanel.add(searchBtn);

        // Delete button with blue color
        JButton deleteBtn = new JButton("Delete Contact");
        styleButton(deleteBtn);
        deleteBtn.addActionListener(e -> deleteContact());
        actionPanel.add(deleteBtn);

        // Display all button with blue color
        JButton displayAllBtn = new JButton("Display All Contacts");
        styleButton(displayAllBtn);
        displayAllBtn.addActionListener(e -> displayAllContacts());
        actionPanel.add(displayAllBtn);

        add(actionPanel, BorderLayout.SOUTH);

        setVisible(true);
    }

    // Helper method to style buttons (make them blue with white text)
    private void styleButton(JButton button) {
        button.setBackground(new Color(70, 130, 180)); // Steel blue color
        button.setForeground(Color.WHITE); // White text
        button.setFocusPainted(false); // Remove focus border
        button.setFont(new Font("Arial", Font.BOLD, 14)); // Slightly larger and bold font
    }

    // Insert contact method
    public void insertContact() {
        String name = nameField.getText();
        String phoneNumber = phoneField.getText();
        String email = emailField.getText();

        if (!name.isEmpty() && !phoneNumber.isEmpty()) {
            Contact newContact = new Contact(name, phoneNumber, email);
            contacts.add(newContact);
            tableModel.addRow(new Object[]{name, phoneNumber, email});
            clearFields();
            JOptionPane.showMessageDialog(this, "Contact added: " + name);
        } else {
            JOptionPane.showMessageDialog(this, "Please enter both name and phone number.");
        }
    }

    // Search contact method
    public void searchContact() {
        String query = searchField.getText();
        for (Contact contact : contacts) {
            if (contact.name.equalsIgnoreCase(query) || contact.phoneNumber.equals(query)) {
                JOptionPane.showMessageDialog(this, "Found: " + contact.name + " - " + contact.phoneNumber + " - " + contact.email);
                return;
            }
        }
        JOptionPane.showMessageDialog(this, "Contact not found.");
    }

    // Delete contact method
    public void deleteContact() {
        String query = searchField.getText();
        for (int i = 0; i < contacts.size(); i++) {
            Contact contact = contacts.get(i);
            if (contact.name.equalsIgnoreCase(query) || contact.phoneNumber.equals(query)) {
                contacts.remove(i);
                tableModel.removeRow(i);
                JOptionPane.showMessageDialog(this, "Contact deleted: " + contact.name);
                return;
            }
        }
        JOptionPane.showMessageDialog(this, "Contact not found.");
    }

    // Display all contacts method
    public void displayAllContacts() {
        tableModel.setRowCount(0);
        for (Contact contact : contacts) {
            tableModel.addRow(new Object[]{contact.name, contact.phoneNumber, contact.email});
        }
    }

    // Clear input fields
    public void clearFields() {
        nameField.setText("");
        phoneField.setText("");
        emailField.setText("");
    }

    public static void main(String[] args) {
        new JJ1();
    }
}
