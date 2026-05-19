import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.Scanner;

public class PasswordAnalyzer {

    // Simulated database of old or common weak passwords
    private static final List<String> OLD_PASSWORDS_DATABASE = Arrays.asList(
        "password123", "admin2024", "qwerty!", "welcome1", "thiranex2026"
    );

    public static void evaluatePassword(String password) {
        int score = 0;
        List<String> feedback = new ArrayList<>();

        // 1. Check Uniqueness / Reuse
        if (OLD_PASSWORDS_DATABASE.contains(password.toLowerCase())) {
            System.out.println("\n--- Evaluation Results ---");
            System.out.println("Strength Assessment : **WEAK (REUSED / COMMON)**");
            System.out.println("Internal Score      : 0/6");
            System.out.println("\nSuggestions / Feedback:");
            System.out.println("- This password has been used before. Please choose a completely unique password.");
            return;
        }

        // 2. Check Length
        int length = password.length();
        if (length >= 12) {
            score += 2;
        } else if (length >= 8) {
            score += 1;
        } else {
            feedback.add("Increase length to at least 12 characters (Current: " + length + ").");
        }

        // 3. Complexity Checks using Regular Expressions
        if (password.matches(".*[A-Z].*")) {
            score++;
        } else {
            feedback.add("Add at least one uppercase letter (A-Z).");
        }

        if (password.matches(".*[a-z].*")) {
            score++;
        } else {
            feedback.add("Add at least one lowercase letter (a-z).");
        }

        if (password.matches(".*\\d.*")) {
            score++;
        } else {
            feedback.add("Add at least one numerical digit (0-9).");
        }

        if (password.matches(".*[!@#$%^&*(),.?\":{}|<>_+-].*")) {
            score++;
        } else {
            feedback.add("Add at least one special character (e.g., !, @, #, $, %).");
        }

        // 4. Determine Strength Rating
        String strength;
        if (score >= 5) {
            strength = "STRONG";
        } else if (score >= 3) {
            strength = "MEDIUM";
        } else {
            strength = "WEAK";
        }

        // Print Results
        System.out.println("\n--- Evaluation Results ---");
        System.out.println("Strength Assessment : **" + strength + "**");
        System.out.println("Internal Score      : " + score + "/6");
        
        System.out.println("\nSuggestions / Feedback:");
        if (feedback.isEmpty()) {
            System.out.println("- Excellent! Your password meets all security criteria.");
        } else {
            for (String hint : feedback) {
                System.out.println("- " + hint);
            }
        }
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println("=========================================");
        System.out.println("      PASSWORD STRENGTH ANALYZER         ");
        System.out.println("=========================================\n");

        while (true) {
            System.out.print("Enter a password to test (or type 'exit' to quit): ");
            String input = scanner.nextLine();

            if (input.equalsIgnoreCase("exit")) {
                break;
            }

            evaluatePassword(input);
            System.out.println("-----------------------------------------\n");
        }
        scanner.close();
        System.out.println("Program terminated.");
    }
}
