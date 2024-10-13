//importing of all the needed java Packages
import javax.swing.*;
import javax.swing.border.EmptyBorder;
import javax.swing.table.DefaultTableModel;//
import java.awt.*;
import java.awt.event.*;
import java.util.LinkedList;

//
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


class RoundedButton extends JButton {
    private final int radius; // Radius for rounded corners

    // Constructor for the rounded button
    RoundedButton(String text, int radius) {
        super(text);
        this.radius = radius; // Set the radius
        setBorder(new EmptyBorder(10, 10, 10, 10)); // Optional padding
        setContentAreaFilled(false); // Remove default button background
    }


    @Override
    protected void paintComponent(Graphics g) {
        if (getModel().isArmed()) {
            g.setColor(getBackground().darker());
        } else {
            g.setColor(getBackground()); //
        }

        g.fillRoundRect(0, 0, getWidth(), getHeight(), radius, radius);
        super.paintComponent(g); // Call the parent method
    }
}

// The Main class for the phonebook application
public class Main extends JFrame {
    LinkedList<Contact> contactList = new LinkedList<>(); // List to store contacts
    JTextField nameField, phoneField, emailField;
    JTable contactTable;
    DefaultTableModel tableModel;

    // The setting up the main application frame
    public Main() {
        setTitle("Phone Booker"); 
        setSize(400, 600);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(new CardLayout());

        //  Intro Page for the app
        JPanel introPanel = createIntroPage();
        add(introPanel, "Intro");


        JPanel contentPanel = createMainPage();
        add(contentPanel, "Main");

        setVisible(true);
    }

    // Method to create the intro page with a welcome message and a button
    private JPanel createIntroPage() {
        JPanel introPanel = new JPanel();
        introPanel.setLayout(new BorderLayout());
        introPanel.setBackground(new Color(211, 211, 211));

        // Create and set up the welcome label
        JLabel welcomeLabel = new JLabel("Welcome to PhoneBooker", SwingConstants.CENTER);
        welcomeLabel.setFont(new Font("Arial", Font.BOLD, 24));
        introPanel.add(welcomeLabel, BorderLayout.CENTER);


        RoundedButton proceedBtn = new RoundedButton("lETS GET STARTED ->", 15);
        proceedBtn.setBackground(new Color(0, 123, 255)); //blue colour of button
        proceedBtn.setForeground(Color.WHITE);
        proceedBtn.addActionListener(e -> switchToMainPage());


        JPanel buttonPanel = new JPanel();
        buttonPanel.add(proceedBtn);
        introPanel.add(buttonPanel, BorderLayout.SOUTH);

        return introPanel;
    }

 //  main content page
    private JPanel createMainPage() {
        JPanel contentPanel = new JPanel();
        contentPanel.setLayout(new BorderLayout()); // Set layout to BorderLayout
        contentPanel.setBackground(new Color(211, 211, 211)); // Set background color to light gray

        //  Header Panel for the mainn page
        JPanel headerPanel = new JPanel();
        JLabel headerLabel = new JLabel("Phonebook App", SwingConstants.CENTER);
        headerLabel.setFont(new Font("Arial", Font.BOLD, 24)); //  font for the header
        headerPanel.setBackground(new Color(51, 153, 255)); // header background color
        headerLabel.setForeground(Color.WHITE);
        headerPanel.add(headerLabel);
        contentPanel.add(headerPanel, BorderLayout.NORTH); //positioning the header panel


        JPanel inputPanel = new JPanel(new GridLayout(3, 2, 10, 10)); // grid layout that for the display of contacts
        inputPanel.setBorder(BorderFactory.createEmptyBorder(10, 10, 10, 10)); // Set border for input panel

     //fields of the grid panel
        nameField = new JTextField();
        phoneField = new JTextField();
        emailField = new JTextField();

        // labels for the panel
        inputPanel.add(new JLabel("Name:"));
        inputPanel.add(nameField);
        inputPanel.add(new JLabel("Phone Number:"));
        inputPanel.add(phoneField);
        inputPanel.add(new JLabel("Email:"));
        inputPanel.add(emailField);

        contentPanel.add(inputPanel, BorderLayout.NORTH);


        tableModel = new DefaultTableModel(new String[]{"Name", "Phone", "Email"}, 0);
        contactTable = new JTable(tableModel); // Create the table
        JScrollPane scrollPane = new JScrollPane(contactTable); //scroll pane for the table
        contentPanel.add(scrollPane, BorderLayout.CENTER); // scroll pane for the center of the content panel


        JPanel actionPanel = new JPanel(new GridLayout(2, 3, 10, 10));
        actionPanel.setBorder(BorderFactory.createEmptyBorder(10, 10, 10, 10));


        RoundedButton insertBtn = new RoundedButton("Add", 15);
        insertBtn.setBackground(new Color(0, 123, 255));
        insertBtn.setForeground(Color.WHITE);
        insertBtn.addActionListener(e -> insertContact()); // action listener for  insert contact

        RoundedButton searchBtn = new RoundedButton("Search", 15);
        searchBtn.setBackground(new Color(0, 123, 255));
        searchBtn.setForeground(Color.WHITE);
        searchBtn.addActionListener(e -> searchContact()); //  Action listener for Contacts

        RoundedButton displayAllBtn = new RoundedButton("Display All", 15);
        displayAllBtn.setBackground(new Color(0, 123, 255));
        displayAllBtn.setForeground(Color.WHITE);
        displayAllBtn.addActionListener(e -> displayAllContacts()); // action listener for  display all contacts

        RoundedButton deleteBtn = new RoundedButton("Delete", 15);
        deleteBtn.setBackground(new Color(0, 123, 255));
        deleteBtn.setForeground(Color.WHITE);
        deleteBtn.addActionListener(e -> deleteContact());

        RoundedButton updateBtn = new RoundedButton("Update", 15);
        updateBtn.setBackground(new Color(0, 123, 255));
        updateBtn.setForeground(Color.WHITE);
        updateBtn.addActionListener(e -> updateContact());

        RoundedButton analyzeBtn = new RoundedButton("Analyze", 15);
        analyzeBtn.setBackground(new Color(0, 123, 255));
        analyzeBtn.setForeground(Color.WHITE);
        analyzeBtn.addActionListener(e -> analyzeSearchEfficiency());

        // Add buttons to action panel
        actionPanel.add(insertBtn);
        actionPanel.add(searchBtn);
        actionPanel.add(displayAllBtn);
        actionPanel.add(deleteBtn);
        actionPanel.add(updateBtn);
        actionPanel.add(analyzeBtn);

        contentPanel.add(actionPanel, BorderLayout.SOUTH);

        return contentPanel;
    }


    private void switchToMainPage() {
        CardLayout cl = (CardLayout) getContentPane().getLayout();
        cl.show(getContentPane(), "Main");
    }

    // Insert a contact into the LinkedList
    public void insertContact() {
        String name = nameField.getText(); // get name
        String phoneNumber = phoneField.getText(); // get phone number
        String email = emailField.getText(); // Get email from

        // Check if name and phone number are provided to continue
        if (!name.isEmpty() && !phoneNumber.isEmpty()) {
            Contact newContact = new Contact(name, phoneNumber, email); // Create new contact if conditions met
            contactList.add(newContact); // Add contact to the list of all contacts
            tableModel.addRow(new Object[]{name, phoneNumber, email}); // Add contact to the table
            clearFields(); // Clear input fields to get new input from the user
            JOptionPane.showMessageDialog(this, "Contact added."); // Show success message
        } else {
            JOptionPane.showMessageDialog(this, "Name and Phone are required.");
        }
    }

    // Search for a contact by name or phone number
    public void searchContact() {
        String query = JOptionPane.showInputDialog("Enter Name or Phone:");
        for (Contact contact : contactList) {
            // Check if the query matches the contact's name or phone number
            if (contact.name.equalsIgnoreCase(query) || contact.phoneNumber.equals(query)) {
                JOptionPane.showMessageDialog(this, "Found: " + contact.name + " - " + contact.phoneNumber + " - " + contact.email); // Show found contact
                return;
            }
        }
        JOptionPane.showMessageDialog(this, "Contact not found."); // Show not found message if contact not in list
    }

    // Display all contacts in the LinkedList
    public void displayAllContacts() {
        tableModel.setRowCount(0);
        for (Contact contact : contactList) { // Loop through contacts
            tableModel.addRow(new Object[]{contact.name, contact.phoneNumber, contact.email}); // Add each contact to the table
        }
    }

    // Delete a contact by name or phone number
    public void deleteContact() {
        String query = JOptionPane.showInputDialog("Enter Name or Phone to Delete:");
        for (int i = 0; i < contactList.size(); i++) { //loop through list for target contact
            Contact contact = contactList.get(i);
            // Check if the query matches the contact's name or phone number then
            if (contact.name.equalsIgnoreCase(query) || contact.phoneNumber.equals(query)) {
                contactList.remove(i); // Remove contact from the list
                tableModel.removeRow(i); // Remove contact from the table model
                JOptionPane.showMessageDialog(this, "Contact deleted.");
                return;
            }
        }
        JOptionPane.showMessageDialog(this, "Contact not found.");
    }

    // Update contact information
    public void updateContact() {
        String query = JOptionPane.showInputDialog("Enter Name or Phone to Update:");
        for (int i = 0; i < contactList.size(); i++) {
            Contact contact = contactList.get(i); // Get current contact

            if (contact.name.equalsIgnoreCase(query) || contact.phoneNumber.equals(query)) {
                // Prompt user for new contact details
                String newName = JOptionPane.showInputDialog("Enter new Name:", contact.name);
                String newPhone = JOptionPane.showInputDialog("Enter new Phone:", contact.phoneNumber);
                String newEmail = JOptionPane.showInputDialog("Enter new Email:", contact.email);

                // Update contact details with new details
                contact.name = newName;
                contact.phoneNumber = newPhone;
                contact.email = newEmail;


                tableModel.setValueAt(newName, i, 0);
                tableModel.setValueAt(newPhone, i, 1);
                tableModel.setValueAt(newEmail, i, 2);
                JOptionPane.showMessageDialog(this, "Contact updated.");
                return;
            }
        }
        JOptionPane.showMessageDialog(this, "Contact not found."); //
    }

    // Analyze search efficiency by measuring time taken for search
    public void analyzeSearchEfficiency() {
        String query = JOptionPane.showInputDialog("Enter Name or Phone to Search:"); // Prompt user for search item
        long startTime = System.nanoTime(); // Record start time

        for (Contact contact : contactList) { // Loop through contacts // Check if the query matches the contact's name or phone number

            if (contact.name.equalsIgnoreCase(query) || contact.phoneNumber.equals(query)) {
                long endTime = System.nanoTime(); // Record end time and calculate the time it took
                long duration = endTime - startTime;
                JOptionPane.showMessageDialog(this, "Found: " + contact.name + "\nSearch Time: " + duration + " nanoseconds."); // Show found message with duration
                return;
            }
        }
        long endTime = System.nanoTime();
        long duration = endTime - startTime;
        JOptionPane.showMessageDialog(this, "Contact not found. Search Time: " + duration + " nanoseconds.");
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
