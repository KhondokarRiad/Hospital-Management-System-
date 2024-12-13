# Hospital-Management-System-
A simple Java-based Hospital Management System implementing core OOP principles. Features include patient registration, doctor management, appointment scheduling, and role-based access. Built using file-based storage and designed for easy scalability. Ideal for learning and small-scale healthcare operations.
import java.io.*;
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

// Patient Class
class Patient implements Serializable {
    @SuppressWarnings("FieldMayBeFinal")
    @SuppressWarnings("FieldMayBeFinal")
    private String id;
    private String name;
    private int age;
    private String ailment;

    public Patient(String id, String name, int age, String ailment) {
        this.id = id;
        this.name = name;
        this.age = age;
        this.ailment = ailment;
    }

    // Getters and Setters
    public String getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public String getAilment() {
        return ailment;
    }

    @Override
    public String toString() {
        return "Patient ID: " + id + ", Name: " + name + ", Age: " + age + ", Ailment: " + ailment;
    }
}

// Doctor Class
class Doctor implements Serializable {
    private String id;
    private String name;
    private String specialization;

    public Doctor(String id, String name, String specialization) {
        this.id = id;
        this.name = name;
        this.specialization = specialization;
    }

    // Getters and Setters
    public String getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public String getSpecialization() {
        return specialization;
    }

    @Override
    public String toString() {
        return "Doctor ID: " + id + ", Name: " + name + ", Specialization: " + specialization;
    }
}

// Appointment Class
class Appointment implements Serializable {
    private String patientId;
    private String doctorId;
    private String date;

    public Appointment(String patientId, String doctorId, String date) {
        this.patientId = patientId;
        this.doctorId = doctorId;
        this.date = date;
    }

    @Override
    public String toString() {
        return "Appointment -> Patient ID: " + patientId + ", Doctor ID: " + doctorId + ", Date: " + date;
    }
}

// Utility Class for File Handling
class FileUtility {
    public static <T> void saveToFile(String filename, List<T> list) {
        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(filename))) {
            oos.writeObject(list);
        } catch (IOException e) {
            System.out.println("Error saving to file: " + e.getMessage());
        }
    }

    @SuppressWarnings("unchecked")
    public static <T> List<T> loadFromFile(String filename) {
        try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream(filename))) {
            return (List<T>) ois.readObject();
        } catch (IOException | ClassNotFoundException e) {
            return new ArrayList<>();
        }
    }
}

// Manager Classes
class PatientManager {
    private final List<Patient> patients;
    private final String fileName;

    public PatientManager(String fileName) {
        this.fileName = fileName;
        this.patients = FileUtility.loadFromFile(fileName);
    }

    public void registerPatient(String id, String name, int age, String ailment) {
        patients.add(new Patient(id, name, age, ailment));
        FileUtility.saveToFile(fileName, patients);
        System.out.println("Patient registered successfully!");
    }

    public void viewPatients() {
        if (patients.isEmpty()) {
            System.out.println("No patients registered.");
        } else {
            for (Patient patient : patients) {
                System.out.println(patient);
            }
        }
    }
}

class DoctorManager {
    private final List<Doctor> doctors;
    private final String fileName;

    public DoctorManager(String fileName) {
        this.fileName = fileName;
        this.doctors = FileUtility.loadFromFile(fileName);
    }

    public void registerDoctor(String id, String name, String specialization) {
        doctors.add(new Doctor(id, name, specialization));
        FileUtility.saveToFile(fileName, doctors);
        System.out.println("Doctor registered successfully!");
    }

    public void viewDoctors() {
        if (doctors.isEmpty()) {
            System.out.println("No doctors registered.");
        } else {
            for (Doctor doctor : doctors) {
                System.out.println(doctor);
            }
        }
    }
}

class AppointmentManager {
    private final List<Appointment> appointments;
    private final String fileName;

    public AppointmentManager(String fileName) {
        this.fileName = fileName;
        this.appointments = FileUtility.loadFromFile(fileName);
    }

    public void scheduleAppointment(String patientId, String doctorId, String date) {
        appointments.add(new Appointment(patientId, doctorId, date));
        FileUtility.saveToFile(fileName, appointments);
        System.out.println("Appointment scheduled successfully!");
    }

    public void viewAppointments() {
        if (appointments.isEmpty()) {
            System.out.println("No appointments scheduled.");
        } else {
            for (Appointment appointment : appointments) {
                System.out.println(appointment);
            }
        }
    }
}

// Main System
public class HospitalManagementSystem {
    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) {
        PatientManager patientManager = new PatientManager("patients.dat");
        DoctorManager doctorManager = new DoctorManager("doctors.dat");
        AppointmentManager appointmentManager = new AppointmentManager("appointments.dat");

        while (true) {
            System.out.println("\nHospital Management System");
            System.out.println("1. Register Patient");
            System.out.println("2. View Patients");
            System.out.println("3. Register Doctor");
            System.out.println("4. View Doctors");
            System.out.println("5. Schedule Appointment");
            System.out.println("6. View Appointments");
            System.out.println("7. Exit");
            System.out.print("Enter your choice: ");
            int choice = scanner.nextInt();
            scanner.nextLine(); // Consume newline

            switch (choice) {
                case 1 -> {
                    System.out.print("Enter Patient ID: ");
                    String id = scanner.nextLine();
                    System.out.print("Enter Patient Name: ");
                    String name = scanner.nextLine();
                    System.out.print("Enter Patient Age: ");
                    int age = scanner.nextInt();
                    scanner.nextLine(); // Consume newline
                    System.out.print("Enter Patient Ailment: ");
                    String ailment = scanner.nextLine();
                    patientManager.registerPatient(id, name, age, ailment);
                }
                case 2 -> patientManager.viewPatients();
                case 3 -> {
                    System.out.print("Enter Doctor ID: ");
                    String id = scanner.nextLine();
                    System.out.print("Enter Doctor Name: ");
                    String name = scanner.nextLine();
                    System.out.print("Enter Doctor Specialization: ");
                    String specialization = scanner.nextLine();
                    doctorManager.registerDoctor(id, name, specialization);
                }
                case 4 -> doctorManager.viewDoctors();
                case 5 -> {
                    System.out.print("Enter Patient ID: ");
                    String patientId = scanner.nextLine();
                    System.out.print("Enter Doctor ID: ");
                    String doctorId = scanner.nextLine();
                    System.out.print("Enter Appointment Date (yyyy-mm-dd): ");
                    String date = scanner.nextLine();
                    appointmentManager.scheduleAppointment(patientId, doctorId, date);
                }
                case 6 -> appointmentManager.viewAppointments();
                case 7 -> {
                    System.out.println("Exiting the system...");
                    return;
                }
                default -> System.out.println("Invalid choice. Please try again.");
            }
        }
    }
}
