import com.opencsv.CSVReader;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

public class Main {
    private static Map<String, Employee> employeeDetails;

    public static void main(String[] args) throws IOException {
       employeeDetails = readEmployeeDetailsFromCSV("src/main/java/Employee Details.csv");

        Scanner scanner = new Scanner(System.in);
        Map<String, Runnable> menuOptions = new HashMap<>();
        menuOptions.put("1", Main::viewEmployeeDetails);
        menuOptions.put("2", Main::countWeeklyHoursAndSalary);
        menuOptions.put("3", Main::countMonthlyHours);
        menuOptions.put("4", Main::computeMonthlySalary);
        menuOptions.put("5", () -> System.out.println("Thank you for using MotorPh Portal!"));

        String userInput;
        do {
            System.out.print("Please enter your choice: ");
            userInput = scanner.nextLine();
            if (menuOptions.containsKey(userInput)) {
                menuOptions.get(userInput).run();
            } else {
                System.out.println("Invalid choice. Please try again.");
            }
        } while (!userInput.matches("[1-5]"));
    }

    private static Map<String, Employee> readEmployeeDetailsFromCSV(String fileName) throws IOException {
        Map<String, Employee> employeeDetails = new HashMap<>();

        try (Stream<String> lines = Files.lines(Paths.get(fileName))) {
            lines.skip(1) // skip header row
                    .map(line -> line.split(","))
                    .forEach(values -> {
                        String id = values[0];
                        String name = values[1];
                        String position = values[2];
                        double hourlyRate = Double.parseDouble(values[3]);
                        int weeklyHours = Integer.parseInt(values[4]);
                        int monthlyHours = Integer.parseInt(values[5]);

                        Employee employee = new Employee(id, name, position, hourlyRate, weeklyHours, monthlyHours);
                        employeeDetails.put(id, employee);
                    });
        }

        return employeeDetails;
    }

    private static void viewEmployeeDetails() {
        System.out.println("Enter the employee ID:");
Scanner scanner = new Scanner(System.in);
        String id = scanner.nextLine();

        Employee employee = employeeDetails.get(id);
        if (employee == null) {
            System.out.println("Employee not found.");
            return;
        }

        System.out.println("Employee Details:");
        System.out.println("ID: " + employee.getId());
        System.out.println("Name: " + employee.getName());
        System.out.println("Position: " + employee.getPosition());
        System.out.println("Hourly Rate: " + employee.getHourlyRate());
        System.out.println("Weekly Hours: " + employee.getWeeklyHours());
        System.out.println("Monthly Hours: " + employee.getMonthlyHours());
    }

    private static void countWeeklyHoursAndSalary() {
        // Code to get weekly hours & weekly salary
    }

    private static void countMonthlyHours() {
        // Code to get monthly hours
    }

    private static void computeMonthlySalary() {
        // Code to compute net monthly salary
    }
}

class Employee {
    private String id;
    private String name;
    private String position;
    private double hourlyRate;
    private int weeklyHours;
    private int monthlyHours;

    public Employee(String id, String name, String position, double hourlyRate, int weeklyHours, int monthlyHours) {
        this.id = id;
        this.name = name;
        this.position = position;
        this.hourlyRate = hourlyRate;
        this.weeklyHours = weeklyHours;
        this.monthlyHours = monthlyHours;
    }

    public String getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public String getPosition() {
        return position;
    }

    public double getHourlyRate() {
        return hourlyRate;
    }

    public int getWeeklyHours() {
        return weeklyHours;
    }

    public int getMonthlyHours() {
        return monthlyHours;
    }
}
