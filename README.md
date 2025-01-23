// import java.sql.*;
import java.util.Scanner;

public class JDBCAllExample {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println("Choose an operation:");
        System.out.println("1. Insert Data");
        System.out.println("2. Delete Data");
        System.out.println("3. View Data");
        System.out.println("4. Update Data");
        int choice = scanner.nextInt();

        switch (choice) {
            case 1:
                insertData();
                break;
            case 2:
                deleteData();
                break;
            case 3:
                viewData();
                break;
            case 4:
                updateData();
                break;
            default:System.out.println("Invalid choice. Please select a valid operation.");
        }
        scanner.close();
    }

    private static void insertData() {
        try {
            Connection connection = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/SEM4", "root", "1arun@");
            String query = "INSERT INTO STUDENTS (ID,name, age,    ADDRESS) VALUES (?,?, ?, ?)";
            PreparedStatement preparedStatement = connection.prepareStatement(query);

            Scanner scanner = new Scanner(System.in);
            System.out.println("Enter ID: ");
            int ID = scanner.nextInt();
            System.out.println("Enter Name: ");
            String name = scanner.next();
            System.out.println("Enter Age: ");
            int age = scanner.nextInt();
            System.out.println("Enter Address: ");
            String ADDRESS = scanner.next();
            preparedStatement.setInt(1,ID);
            preparedStatement.setString(2, name);
            preparedStatement.setInt(3, age);
            preparedStatement.setString(4, ADDRESS);
            preparedStatement.executeUpdate();

            System.out.println("Data inserted successfully.");
            connection.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    private static void deleteData() {
        try {
            Connection connection = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/SEM4", "root", "1arun@");
            String query = "DELETE FROM STUDENTS WHERE ID = ?";
            PreparedStatement preparedStatement = connection.prepareStatement(query);
            Scanner scanner = new Scanner(System.in);
            System.out.println("Enter ID to delete: ");
            int ID = scanner.nextInt();

            preparedStatement.setInt(1, ID);
            preparedStatement.executeUpdate();
            System.out.println("Data deleted successfully.");
            connection.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    private static void viewData() {
        try {
            Connection connection = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/SEM4", "root", "1arun@");
            String query = "SELECT * FROM STUDENTS";
            Statement statement = connection.createStatement();
            ResultSet resultSet = statement.executeQuery(query);

            while (resultSet.next()) {
                int ID = resultSet.getInt("ID");
                String name = resultSet.getString("name");
                int age = resultSet.getInt("age");
                String ADDRESS = resultSet.getString("ADDRESS");
                System.out.println("ID: " + ID);
                System.out.println("Name: " + name);
                System.out.println("Age: " + age);
                System.out.println("ADDRESS: " + ADDRESS);
            }
            connection.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    private static void updateData() {
        try {
            Connection connection = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/SEM4", "root", "1arun@");
            Scanner scanner = new Scanner(System.in);

            System.out.println("Enter ID toupdate: ");
            int ID = scanner.nextInt();

            System.out.println("Do you want to update the Name? (yes/no)");
            String updateName = scanner.next();
            String name = null;
            if (updateName.equalsIgnoreCase("yes")) {
                System.out.println("Enter new Name: ");
                name = scanner.next();
            }

            System.out.println("Do you want to update the Age? (yes/no)");
            String updateAge = scanner.next();
            Integer age = null;
            if (updateAge.equalsIgnoreCase("yes")) {
                System.out.println("Enter new Age: ");
                age = scanner.nextInt();
            }
            System.out.println("Do you want to update the ADDRESS? (yes/no)");
            String updateADDRESS = scanner.next();
            String ADDRESS = null;
            if (updateADDRESS.equalsIgnoreCase("yes")) {
                System.out.println("Enter new ADDRESS: ");
                ADDRESS = scanner.next();
            }

            String query = "UPDATE STUDENTS SET ";
            boolean firstField = true;
            if (name != null) {
                query += "name = ?";
                firstField = false;
            }
            if (age != null) {
                if (!firstField) query += ", ";
                query += "age = ?";
                firstField = false;
            }
            if (ADDRESS != null) {
                if (!firstField) query += ", ";
                query += "ADDRESS = ?";
            }
            query += " WHERE ID = ?";

            PreparedStatement preparedStatement = connection.prepareStatement(query);
            int parameterIndex = 1;
            if (name != null) {
                preparedStatement.setString(parameterIndex++, name);
            }
            if (age != null) {
                preparedStatement.setInt(parameterIndex++, age);
            }
            if (ADDRESS != null) {
                preparedStatement.setString(parameterIndex++, ADDRESS);
            }
            preparedStatement.setInt(parameterIndex, ID);
            preparedStatement.executeUpdate();

            System.out.println("Data updated successfully.");
            connection.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
//
