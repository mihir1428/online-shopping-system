import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

abstract class Product {
    private String name;
    private double price;

    public Product(String name, double price) {
        this.name = name;
        this.price = price;
    }

    public String getName() {
        return name;
    }

    public double getPrice() {
        return price;
    }

    public abstract void displayDetails();
}

class ElectronicProduct extends Product {
    private String brand;

    public ElectronicProduct(String name, double price, String brand) {
        super(name, price);
        this.brand = brand;
    }

    @Override
    public void displayDetails() {
        System.out.println("Electronic Product: " + getName());
        System.out.println("Brand: " + brand);
        System.out.println("Price: $" + getPrice());
    }
}

class User {
    private String email;
    private String password;

    public User(String email, String password) {
        this.email = email;
        this.password = password;
    }

    public String getEmail() {
        return email;
    }

    public String getPassword() {
        return password;
    }
}

class ShoppingCart {
    private List<Product> items;

    public ShoppingCart() {
        this.items = new ArrayList<>();
    }

    public void addProduct(Product product, int quantity) {
        for (int i = 0; i < quantity; i++) {
            items.add(product);
        }
        System.out.println(product.getName() + " (Quantity: " + quantity + ") added to the shopping cart.");
    }

    public void displayCart() {
        if (items.isEmpty()) {
            System.out.println("Your cart is empty.");
            return;
        }

        System.out.println("Shopping Cart Contents:");
        for (int i = 0; i < items.size(); i++) {
            System.out.println((i + 1) + ". ");
            items.get(i).displayDetails();
            System.out.println("-----------------------");
        }
    }

    public void deleteProduct(int itemNumber) {
        if (itemNumber >= 1 && itemNumber <= items.size()) {
            Product removedProduct = items.remove(itemNumber - 1);
            System.out.println(removedProduct.getName() + " removed from the shopping cart.");
        } else {
            System.out.println("Invalid item number.");
        }
    }

    public double calculateTotalAmount() {
        double totalAmount = 0;
        for (Product item : items) {
            totalAmount += item.getPrice();
        }
        return totalAmount;
    }

    public boolean isEmpty() {
        return items.isEmpty();
    }

    public void clearCart() {
        items.clear();
    }
}

public class OnlineShoppingSystem {
    private static List<User> registeredUsers = new ArrayList<>();
    private static User currentUser;

    public static void main(String[] args) {
        System.out.println("===============================================");
        System.out.println("|                                             |");
        System.out.println("|      Welcome to Online Shopping System      |");
        System.out.println("|                                             |");
        System.out.println("===============================================");

        List<Product> productList = new ArrayList<>();
        productList.add(new ElectronicProduct("Laptop", 1200.00, "XYZ"));
        productList.add(new ElectronicProduct("Smartphone", 699.99, "ABC"));
        productList.add(new ElectronicProduct("Earphone", 200.00, "XYZ"));
        productList.add(new ElectronicProduct("TV", 6995.99, "ABC"));
        productList.add(new ElectronicProduct("AC", 12000.00, "XYZ"));
        productList.add(new ElectronicProduct("PC", 2699.99, "ABC"));

        runOnlineShoppingSystem(productList);
    }

    private static void runOnlineShoppingSystem(List<Product> productList) {
        Scanner scanner = new Scanner(System.in);

        // Registration
        System.out.print("Enter your email address to register: ");
        String email = scanner.nextLine();
        System.out.print("Create a password: ");
        String password = scanner.nextLine();
        registerUser(email, password);

        // Login
        System.out.print("\nEnter your email address to log in: ");
        String loginEmail = scanner.nextLine();
        System.out.print("Enter your password: ");
        String loginPassword = scanner.nextLine();
        loginUser(loginEmail, loginPassword);

        ShoppingCart cart = new ShoppingCart();

        while (true) {
            System.out.println("\n1. Add Order");
            System.out.println("2. Display Cart");
            System.out.println("3. Delete Item from Cart");
            System.out.println("4. Buy Product");
            System.out.println("5. Make Payment");
            System.out.println("6. Exit");
            System.out.print("Enter your choice: ");
            int choice = scanner.nextInt();

            switch (choice) {
                case 1:
                    displayProductList(productList);
                    System.out.print("Enter the product number to add to the cart: ");
                    int productNumber = scanner.nextInt();
                    if (productNumber >= 1 && productNumber <= productList.size()) {
                        Product selectedProduct = productList.get(productNumber - 1);
                        System.out.print("Enter the quantity: ");
                        int quantity = scanner.nextInt();
                        cart.addProduct(selectedProduct, quantity);
                        displayProductList(productList);
                    } else {
                        System.out.println("Invalid product number.");
                    }
                    break;

                case 2:
                    cart.displayCart();
                    System.out.println("Total Amount: $" + cart.calculateTotalAmount());
                    break;

                case 3:
                    System.out.print("Enter the item number to delete from the cart: ");
                    int itemNumber = scanner.nextInt();
                    cart.deleteProduct(itemNumber);
                    break;

                case 4:
                    cart.displayCart();
                    double totalAmount = cart.calculateTotalAmount();
                    if (cart.isEmpty()) {
                        System.out.println("Your cart is empty. Please add items first.");
                    } else {
                        System.out.println("Total Amount to Pay: $" + totalAmount);
                        System.out.print("Do you want to proceed to payment? (yes/no): ");
                        scanner.nextLine(); // Consume newline
                        String confirm = scanner.nextLine();
                        if (confirm.equalsIgnoreCase("yes")) {
                            System.out.println("Redirecting to payment...");
                            System.out.println("Please select 'Make Payment' option to complete your purchase.");
                        } else {
                            System.out.println("Order not placed.");
                        }
                    }
                    break;

                case 5:
                    if (cart.isEmpty()) {
                        System.out.println("Your cart is empty. Add items before making a payment.");
                    } else {
                        System.out.println("Choose Payment Method:");
                        System.out.println("1. Debit Card");
                        System.out.println("2. Credit Card");
                        System.out.print("Enter choice: ");
                        int paymentChoice = scanner.nextInt();
                        scanner.nextLine(); // Consume newline

                        System.out.print("Enter Card Number: ");
                        String cardNumber = scanner.nextLine();

                        if (paymentChoice == 1) {
                            System.out.println("Processing payment via Debit Card...");
                        } else if (paymentChoice == 2) {
                            System.out.println("Processing payment via Credit Card...");
                        } else {
                            System.out.println("Invalid payment method.");
                            break;
                        }

                        System.out.println("Validating card ending with " + cardNumber.substring(cardNumber.length() - 4) + "...");
                        System.out.println("Payment of $" + cart.calculateTotalAmount() + " successful! ✅");
                        cart.clearCart();
                    }
                    break;

                case 6:
                    System.out.println("Exiting the Online Shopping System. Goodbye!");
                    System.exit(0);
                    break;

                default:
                    System.out.println("Invalid choice. Please enter a valid option.");
            }
        }
    }

    private static void registerUser(String email, String password) {
        if (isValidEmail(email)) {
            User newUser = new User(email, password);
            registeredUsers.add(newUser);
            System.out.println("User registered successfully!");
        } else {
            System.out.println("Invalid email address. Registration failed.");
            System.exit(0);
        }
    }

    private static void loginUser(String email, String password) {
        User foundUser = findUserByEmail(email);
        if (foundUser != null && foundUser.getPassword().equals(password)) {
            currentUser = foundUser;
            System.out.println("Login successful! Welcome, " + currentUser.getEmail() + "!");
        } else {
            System.out.println("Invalid email or password. Login failed.");
            System.exit(0);
        }
    }

    private static boolean isValidEmail(String email) {
        return email.matches("[a-zA-Z0-9._-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,4}");
    }

    private static User findUserByEmail(String email) {
        for (User user : registeredUsers) {
            if (user.getEmail().equals(email)) {
                return user;
            }
        }
        return null;
    }

    private static void displayProductList(List<Product> productList) {
        System.out.println("\nAvailable Products:");
        for (int i = 0; i < productList.size(); i++) {
            Product product = productList.get(i);
            System.out.println((i + 1) + ". " + product.getName() + " - $" + product.getPrice());
        }
    }
}
