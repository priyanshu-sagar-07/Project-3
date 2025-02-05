import java.util.ArrayList;
import java.util.Scanner;
import java.io.File;
import java.io.FileWriter;
import java.io.IOException;
import java.io.FileReader;
import java.io.BufferedReader;

class Contact {
    String name;
    String phoneNumber;
    String emailAddress;

    public Contact(String name, String phoneNumber, String emailAddress) {
        this.name = name;
        this.phoneNumber = phoneNumber;
        this.emailAddress = emailAddress;
    }
}

public class ContactManager {
    static ArrayList<Contact> contactList = new ArrayList<>();

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        loadContactsFromFile();

        while (true) {
            System.out.println("\nContact Manager");
            System.out.println("1. Add Contact");
            System.out.println("2. View Contacts");
            System.out.println("3. Edit Contact");
            System.out.println("4. Delete Contact");
            System.out.println("5. Exit");
            System.out.print("Enter your choice: ");

            int choice = scanner.nextInt();
            scanner.nextLine(); // Consume newline character

            switch (choice) {
                case 1:
                    addContact(scanner);
                    break;
                case 2:
                    viewContacts();
                    break;
                case 3:
                    editContact(scanner);
                    break;
                case 4:
                    deleteContact(scanner);
                    break;
                case 5:
                    saveContactsToFile();
                    System.out.println("Exiting program.");
                    System.exit(0);
                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        }
    }

    private static void addContact(Scanner scanner) {
        System.out.print("Enter contact name: ");
        String name = scanner.nextLine();
        System.out.print("Enter phone number: ");
        String phoneNumber = scanner.nextLine();
        System.out.print("Enter email address: ");
        String emailAddress = scanner.nextLine();

        Contact newContact = new Contact(name, phoneNumber, emailAddress);
        contactList.add(newContact);
        System.out.println("Contact added successfully.");
    }

    private static void viewContacts() {
        if (contactList.isEmpty()) {
            System.out.println("No contacts available.");
            return;
        }
        System.out.println("\nContact List:");
        for (int i = 0; i < contactList.size(); i++) {
            Contact contact = contactList.get(i);
            System.out.println((i + 1) + ". " + contact.name + " - " + contact.phoneNumber + " - " + contact.emailAddress);
        }
    }

    private static void editContact(Scanner scanner) {
        viewContacts();
        System.out.print("Enter the number of the contact to edit: ");
        int index = scanner.nextInt() - 1;
        scanner.nextLine(); // Consume newline character

        if (index < 0 || index >= contactList.size()) {
            System.out.println("Invalid contact number.");
            return;
        }

        Contact contact = contactList.get(index);
        System.out.print("Enter new name (leave blank to keep current: " + contact.name + "): ");
        String name = scanner.nextLine();
        System.out.print("Enter new phone number (leave blank to keep current: " + contact.phoneNumber + "): ");
        String phoneNumber = scanner.nextLine();
        System.out.print("Enter new email address (leave blank to keep current: " + contact.emailAddress + "): ");
        String emailAddress = scanner.nextLine();

        if (!name.isEmpty()) contact.name = name;
        if (!phoneNumber.isEmpty()) contact.phoneNumber = phoneNumber;
        if (!emailAddress.isEmpty()) contact.emailAddress = emailAddress;

        System.out.println("Contact updated successfully.");
    }

    private static void deleteContact(Scanner scanner) {
        viewContacts();
        System.out.print("Enter the number of the contact to delete: ");
        int index = scanner.nextInt() - 1;

        if (index < 0 || index >= contactList.size()) {
            System.out.println("Invalid contact number.");
            return;
        }

        contactList.remove(index);
        System.out.println("Contact deleted successfully.");
    }

    private static void loadContactsFromFile() {
        try (BufferedReader br = new BufferedReader(new FileReader("contacts.txt"))) {
            String line;
            while ((line = br.readLine()) != null) {
                String[] parts = line.split(",");
                if (parts.length == 3) {
                    contactList.add(new Contact(parts[0], parts[1], parts[2]));
                }
            }
        } catch (IOException e) {
            System.out.println("Error loading contacts: " + e.getMessage());
        }
    }

    private static void saveContactsToFile() {
        try (FileWriter writer = new FileWriter("contacts.txt")) {
            for (Contact contact : contactList) {
                writer.write(contact.name + "," + contact.phoneNumber + "," + contact.emailAddress + "\n");
            }
        } catch (IOException e) {
            System.out.println("Error saving contacts: " + e.getMessage());
        }
    }
}
