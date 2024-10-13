//importing of all the needed java Packages
import javax.swing.*;
import javax.swing.border.EmptyBorder;
import javax.swing.table.DefaultTableModel;// 
import java.awt.*;
import java.awt.event.*;
import java.util.LinkedList;

// Class to represent a contact with name, phone number, and email
class Contact {
    String name;
    String phoneNumber;
    String email;

    // Constructor to create a new contact
    Contact(String name, String phoneNumber, String email) {
        this.name = name;
        this.phoneNumber = phoneNumber;
        this.email = email;
    }
}

// Custom JButton class to create buttons with rounded edges
class RoundedButton extends JButton {
    private final int radius; // Radius for rounded corners

    // Constructor for the rounded button
    RoundedButton(String text, int radius) {
        super(text);
        this.radius = radius; // Set the radius
        setBorder(new EmptyBorder(10, 10, 10, 10)); // Optional padding
        setContentAreaFilled(false); // Remove default button background
    }

    // Override paintComponent to draw the button with rounded corners
    @Override
    protected void paintComponent(Graphics g) {
        if (getModel().isArmed()) { // If the button is pressed
            g.setColor(getBackground().darker()); // Darker color for pressed state
        } else {
            g.setColor(getBackground()); // Normal button color
        }
        // Draw the rounded rectangle
        g.fillRoundRect(0, 0, getWidth(), getHeight(), radius, radius);
        super.paintComponent(g); // Call the parent method
    }
}

// Main class for the phonebook application
public class Main extends JFrame {
    LinkedList<Contact> contactList = new LinkedList<>(); // List to store contacts
    JTextField nameField, phoneField, emailField; // Input fields for contact details
    JTable contactTable; // Table to display contacts
    DefaultTableModel tableModel; // Model for the contact table

    // Constructor to set up the main application frame
    public Main() {
        setTitle("Mobile Phonebook"); // Set the title
        setSize(400, 600); // Set the size of the window
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE); // Close on exit
        setLayout(new CardLayout()); // Use CardLayout for switching pages

        // Create Intro Page and add it to the frame
        JPanel introPanel = createIntroPage();
        add(introPanel, "Intro");

        // Create Main Content Panel and add it to the frame
        JPanel contentPanel = createMainPage();
        add(contentPanel, "Main");

        setVisible(true); // Make the frame visible
    }

    // Method to create the intro page with a welcome message and a button
    private JPanel createIntroPage() {
        JPanel introPanel = new JPanel(); // Create a new panel
        introPanel.setLayout(new BorderLayout()); // Set layout to BorderLayout
        introPanel.setBackground(new Color(211, 211, 211)); // Set background color to light gray

        // Create and set up the welcome label
        JLabel welcomeLabel = new JLabel("Welcome to PhoneBooker", SwingConstants.CENTER);
        welcomeLabel.setFont(new Font("Arial", Font.BOLD, 24)); // Set font for the label
        introPanel.add(welcomeLabel, BorderLayout.CENTER); // Add label to the center of the panel

        // Create a button to proceed to the main page
        RoundedButton proceedBtn = new RoundedButton("Proceed", 15);
        proceedBtn.setBackground(new Color(0, 123, 255)); // Set button color to sexy blue
        proceedBtn.setForeground(Color.WHITE); // Set button text color to white
        proceedBtn.addActionListener(e -> switchToMainPage()); // Add action listener for button click

        // Create a panel for the button and add it to the intro panel
        JPanel buttonPanel = new JPanel();
        buttonPanel.add(proceedBtn);
        introPanel.add(buttonPanel, BorderLayout.SOUTH); // Add button panel to the south of the intro panel

        return introPanel; // Return the completed intro panel
    }

    // Method to create the main content page
    private JPanel createMainPage() {
        JPanel contentPanel = new JPanel(); // Create a new panel for the main page
        contentPanel.setLayout(new BorderLayout()); // Set layout to BorderLayout
        contentPanel.setBackground(new Color(211, 211, 211)); // Set background color to light gray

        // Create Header Panel with the application title
        JPanel headerPanel = new JPanel();
        JLabel headerLabel = new JLabel("Phonebook App", SwingConstants.CENTER);
        headerLabel.setFont(new Font("Arial", Font.BOLD, 24)); // Set font for the header
        headerPanel.setBackground(new Color(51, 153, 255)); // Set header background color
        headerLabel.setForeground(Color.WHITE); // Set header text color to white
        headerPanel.add(headerLabel); // Add header label to the header panel
        contentPanel.add(headerPanel, BorderLayout.NORTH); // Add header panel to the north of the content panel

        // Input fields for adding contacts
        JPanel inputPanel = new JPanel(new GridLayout(3, 2, 10, 10)); // Create a grid layout
        inputPanel.setBorder(BorderFactory.createEmptyBorder(10, 10, 10, 10)); // Set border for input panel

        // Create input fields for contact details
        nameField = new JTextField();
        phoneField = new JTextField();
        emailField = new JTextField();

        // Add labels and fields to the input panel
        inputPanel.add(new JLabel("Name:"));
        inputPanel.add(nameField);
        inputPanel.add(new JLabel("Phone Number:"));
        inputPanel.add(phoneField);
        inputPanel.add(new JLabel("Email:"));
        inputPanel.add(emailField);

        contentPanel.add(inputPanel, BorderLayout.NORTH); // Add input panel to the north of the content panel

        // Create a table to display contacts
        tableModel = new DefaultTableModel(new String[]{"Name", "Phone", "Email"}, 0); // Set table model
        contactTable = new JTable(tableModel); // Create the table
        JScrollPane scrollPane = new JScrollPane(contactTable); // Add scroll pane for the table
        contentPanel.add(scrollPane, BorderLayout.CENTER); // Add scroll pane to the center of the content panel

        // Create Bottom Panel for action buttons
        JPanel actionPanel = new JPanel(new GridLayout(2, 3, 10, 10)); // Create grid layout for buttons
        actionPanel.setBorder(BorderFactory.createEmptyBorder(10, 10, 10, 10)); // Set border for action panel

        // Create action buttons using RoundedButton
        RoundedButton insertBtn = new RoundedButton("Add", 15);
        insertBtn.setBackground(new Color(0, 123, 255)); // Set button color to sexy blue
        insertBtn.setForeground(Color.WHITE); // Set button text color to white
        insertBtn.addActionListener(e -> insertContact()); // Add action listener to insert contact

        RoundedButton searchBtn = new RoundedButton("Search", 15);
        searchBtn.setBackground(new Color(0, 123, 255)); // Set button color to sexy blue
        searchBtn.setForeground(Color.WHITE); // Set button text color to white
        searchBtn.addActionListener(e -> searchContact()); // Add action listener to search contact

        RoundedButton displayAllBtn = new RoundedButton("Display All", 15);
        displayAllBtn.setBackground(new Color(0, 123, 255)); // Set button color to sexy blue
        displayAllBtn.setForeground(Color.WHITE); // Set button text color to white
        displayAllBtn.addActionListener(e -> displayAllContacts()); // Add action listener to display all contacts

        RoundedButton deleteBtn = new RoundedButton("Delete", 15);
        deleteBtn.setBackground(new Color(0, 123, 255)); // Set button color to sexy blue
        deleteBtn.setForeground(Color.WHITE); // Set button text color to white
        deleteBtn.addActionListener(e -> deleteContact()); // Add action listener to delete contact

        RoundedButton updateBtn = new RoundedButton("Update", 15);
        updateBtn.setBackground(new Color(0, 123, 255)); // Set button color to sexy blue
        updateBtn.setForeground(Color.WHITE); // Set button text color to white
        updateBtn.addActionListener(e -> updateContact()); // Add action listener to update contact

        RoundedButton analyzeBtn = new RoundedButton("Analyze", 15);
        analyzeBtn.setBackground(new Color(0, 123, 255)); // Set button color to sexy blue
        analyzeBtn.setForeground(Color.WHITE); // Set button text color to white
        analyzeBtn.addActionListener(e -> analyzeSearchEfficiency()); // Add action listener to analyze search efficiency

        // Add buttons to action panel
        actionPanel.add(insertBtn);
        actionPanel.add(searchBtn);
        actionPanel.add(displayAllBtn);
        actionPanel.add(deleteBtn);
        actionPanel.add(updateBtn);
        actionPanel.add(analyzeBtn);

        contentPanel.add(actionPanel, BorderLayout.SOUTH); // Add action panel to the south of the content panel

        return contentPanel; // Return the completed main page panel
    }

    // Method to switch from intro page to main page
    private void switchToMainPage() {
        CardLayout cl = (CardLayout) getContentPane().getLayout(); // Get CardLayout from the frame
        cl.show(getContentPane(), "Main"); // Show the main page
    }

    // Insert a contact into the LinkedList
    public void insertContact() {
        String name = nameField.getText(); // Get name from input field
        String phoneNumber = phoneField.getText(); // Get phone number from input field
        String email = emailField.getText(); // Get email from input field

        // Check if name and phone number are provided
        if (!name.isEmpty() && !phoneNumber.isEmpty()) {
            Contact newContact = new Contact(name, phoneNumber, email); // Create new contact
            contactList.add(newContact); // Add contact to the list
            tableModel.addRow(new Object[]{name, phoneNumber, email}); // Add contact to the table model
            clearFields(); // Clear input fields
            JOptionPane.showMessageDialog(this, "Contact added."); // Show success message
        } else {
            JOptionPane.showMessageDialog(this, "Name and Phone are required."); // Show error message
        }
    }

    // Search for a contact by name or phone number
    public void searchContact() {
        String query = JOptionPane.showInputDialog("Enter Name or Phone:"); // Prompt user for search query
        for (Contact contact : contactList) { // Loop through contacts in the list
            // Check if the query matches the contact's name or phone number
            if (contact.name.equalsIgnoreCase(query) || contact.phoneNumber.equals(query)) {
                JOptionPane.showMessageDialog(this, "Found: " + contact.name + " - " + contact.phoneNumber + " - " + contact.email); // Show found contact
                return; // Exit method after finding the contact
            }
        }
        JOptionPane.showMessageDialog(this, "Contact not found."); // Show not found message
    }

    // Display all contacts in the LinkedList
    public void displayAllContacts() {
        tableModel.setRowCount(0); // Clear the table model
        for (Contact contact : contactList) { // Loop through contacts
            tableModel.addRow(new Object[]{contact.name, contact.phoneNumber, contact.email}); // Add each contact to the table
        }
    }

    // Delete a contact by name or phone number
    public void deleteContact() {
        String query = JOptionPane.showInputDialog("Enter Name or Phone to Delete:"); // Prompt user for deletion query
        for (int i = 0; i < contactList.size(); i++) { // Loop through the contact list
            Contact contact = contactList.get(i); // Get current contact
            // Check if the query matches the contact's name or phone number
            if (contact.name.equalsIgnoreCase(query) || contact.phoneNumber.equals(query)) {
                contactList.remove(i); // Remove contact from the list
                tableModel.removeRow(i); // Remove contact from the table model
                JOptionPane.showMessageDialog(this, "Contact deleted."); // Show success message
                return; // Exit method after deletion
            }
        }
        JOptionPane.showMessageDialog(this, "Contact not found."); // Show not found message
    }

    // Update contact information
    public void updateContact() {
        String query = JOptionPane.showInputDialog("Enter Name or Phone to Update:"); // Prompt user for update query
        for (int i = 0; i < contactList.size(); i++) { // Loop through contact list
            Contact contact = contactList.get(i); // Get current contact
            // Check if the query matches the contact's name or phone number
            if (contact.name.equalsIgnoreCase(query) || contact.phoneNumber.equals(query)) {
                // Prompt user for new contact details
                String newName = JOptionPane.showInputDialog("Enter new Name:", contact.name);
                String newPhone = JOptionPane.showInputDialog("Enter new Phone:", contact.phoneNumber);
                String newEmail = JOptionPane.showInputDialog("Enter new Email:", contact.email);

                // Update contact details
                contact.name = newName;
                contact.phoneNumber = newPhone;
                contact.email = newEmail;

                // Update the table model with new values
                tableModel.setValueAt(newName, i, 0);
                tableModel.setValueAt(newPhone, i, 1);
                tableModel.setValueAt(newEmail, i, 2);
                JOptionPane.showMessageDialog(this, "Contact updated."); // Show success message
                return; // Exit method after update
            }
        }
        JOptionPane.showMessageDialog(this, "Contact not found."); // Show not found message
    }

    // Analyze search efficiency by measuring time taken for search
    public void analyzeSearchEfficiency() {
        String query = JOptionPane.showInputDialog("Enter Name or Phone to Search:"); // Prompt user for search query
        long startTime = System.nanoTime(); // Record start time

        for (Contact contact : contactList) { // Loop through contacts
            // Check if the query matches the contact's name or phone number
            if (contact.name.equalsIgnoreCase(query) || contact.phoneNumber.equals(query)) {
                long endTime = System.nanoTime(); // Record end time
                long duration = endTime - startTime; // Calculate duration
                JOptionPane.showMessageDialog(this, "Found: " + contact.name + "\nSearch Time: " + duration + " nanoseconds."); // Show found message with duration
                return; // Exit method after finding contact
            }
        }
        long endTime = System.nanoTime(); // Record end time for not found case
        long duration = endTime - startTime; // Calculate duration
        JOptionPane.showMessageDialog(this, "Contact not found. Search Time: " + duration + " nanoseconds."); // Show not found message with duration
    }

    // Clear input fields after adding or updating contact
    public void clearFields() {
        nameField.setText(""); // Clear name field
        phoneField.setText(""); // Clear phone number field
        emailField.setText(""); // Clear email field
    }

    // Main method to run the application
    public static void main(String[] args) {
        new Main(); // Create a new instance of the Main class
    }
}
