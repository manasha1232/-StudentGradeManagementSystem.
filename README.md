# -StudentGradeManagementSystem.

package gradesystem;

import java.util.HashMap;
import java.util.Map;

public class Learner {
    private String learnerId;
    private String learnerName;
    private Map<String, Double> subjectMarks;

    public Learner(String learnerId, String learnerName) {
        this.learnerId = learnerId;
        this.learnerName = learnerName;
        this.subjectMarks = new HashMap<>();
    }

    public void addMark(String subject, double mark) {
        subjectMarks.put(subject, mark);
    }

    public void displayMarks() {
        System.out.println("\nMarks for Learner: " + learnerName + " (ID: " + learnerId + ")");
        for (Map.Entry<String, Double> entry : subjectMarks.entrySet()) {
            System.out.println("Subject: " + entry.getKey() + ", Mark: " + entry.getValue());
        }
    }

    public double calculateAverage() {
        double total = 0;
        for (double mark : subjectMarks.values()) {
            total += mark;
        }
        return subjectMarks.size() > 0 ? total / subjectMarks.size() : 0.0;
    }

    // Getters
    public String getLearnerName() {
        return learnerName;
    }

    public String getLearnerId() {
        return learnerId;
    }
}


package gradesystem;

import java.util.ArrayList;
import java.util.List;

public class MarkManager {
    private List<Learner> learners;

    public MarkManager() {
        learners = new ArrayList<>();
    }

    public void addLearner(Learner learner) {
        learners.add(learner);
    }

    public void displayAllLearners() {
        if (learners.isEmpty()) {
            System.out.println("No learners found!");
            return;
        }
        for (Learner learner : learners) {
            learner.displayMarks();
            System.out.println("Average: " + String.format("%.2f", learner.calculateAverage()));
        }
    }

    public Learner findLearnerById(String id) {
        for (Learner learner : learners) {
            if (learner.getLearnerId().equals(id)) {
                return learner;
            }
        }
        return null;
    }
}


package gradesystem;

import java.util.Scanner;

public class MainApp {
    public static void main(String[] args) {
        MarkManager manager = new MarkManager();
        Scanner sc = new Scanner(System.in);
        boolean exit = false;

        while (!exit) {
            System.out.println("\n--- Learner Mark Management ---");
            System.out.println("1. Add Learner");
            System.out.println("2. Display All Learners");
            System.out.println("3. Search Learner");
            System.out.println("4. Exit");
            System.out.print("Choose option: ");
            int choice = sc.nextInt();
            sc.nextLine(); // consume newline

            switch (choice) {
                case 1:
                    System.out.print("Enter Learner ID: ");
                    String id = sc.nextLine();
                    System.out.print("Enter Learner Name: ");
                    String name = sc.nextLine();
                    Learner learner = new Learner(id, name);

                    System.out.print("Enter number of subjects: ");
                    int count = sc.nextInt();
                    sc.nextLine();

                    for (int i = 0; i < count; i++) {
                        System.out.print("Enter subject name: ");
                        String subject = sc.nextLine();
                        System.out.print("Enter mark: ");
                        double mark = sc.nextDouble();
                        sc.nextLine();
                        learner.addMark(subject, mark);
                    }
                    manager.addLearner(learner);
                    break;

                case 2:
                    manager.displayAllLearners();
                    break;

                case 3:
                    System.out.print("Enter learner ID to search: ");
                    String searchId = sc.nextLine();
                    Learner found = manager.findLearnerById(searchId);
                    if (found != null) {
                        found.displayMarks();
                        System.out.println("Average: " + String.format("%.2f", found.calculateAverage()));
                    } else {
                        System.out.println("Learner not found!");
                    }
                    break;

                case 4:
                    exit = true;
                    break;

                default:
                    System.out.println("Invalid choice!");
            }
        }
        sc.close();
    }
}
