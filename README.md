# The-Hotel-Management-System-Java-
Java-Based Hotel Management System with GUI and MySQL 
import java.io.BufferedWriter;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.util.Scanner;

class Room {
    private int roomNumber;
    private boolean isBooked;
    private String guestName;
    private String phoneNumber;

    public Room(int roomNumber) {
        this.roomNumber = roomNumber;
        this.isBooked = false;
        this.guestName = "";
        this.phoneNumber = "";
    }

    public int getRoomNumber() {
        return roomNumber;
    }

    public boolean isBooked() {
        return isBooked;
    }

    public String getGuestName() {
        return guestName;
    }

    public String getPhoneNumber() {
        return phoneNumber;
    }

    public void bookRoom(String guestName, String phoneNumber) {
        this.guestName = guestName;
        this.phoneNumber = phoneNumber;
        isBooked = true;
    }

    public void checkoutRoom() {
        this.guestName = "";
        this.phoneNumber = "";
        isBooked = false;
    }

    @Override
    public String toString() {
        if (isBooked) {
            return "Room Number: " + roomNumber + ", Booked: Yes, Guest Name: " + guestName + ", Phone Number: "
                    + phoneNumber;
        } else {
            return "Room Number: " + roomNumber + ", Booked: No";
        }
    }
}

class Hotel {
    private Room[] rooms;

    public Hotel(int numRooms) {
        rooms = new Room[numRooms];
        for (int i = 0; i < numRooms; i++) {
            rooms[i] = new Room(i + 1);
        }
    }

    public boolean bookRoom(int roomNumber, String guestName, String phoneNumber) {
        if (roomNumber < 1 || roomNumber > rooms.length) {
            System.out.println("Invalid room number.");
            return false;
        }

        Room room = rooms[roomNumber - 1];
        if (!room.isBooked()) {
            room.bookRoom(guestName, phoneNumber);
            System.out.print("Room boocked successfully");
            return true;
        } else {
            System.out.println("Room " + roomNumber + " is already booked.");
            return false;
        }
    }

    public void checkoutRoom(int roomNumber) {
        if (roomNumber < 1 || roomNumber > rooms.length) {
            System.out.println("Invalid room number.");
            return;
        }

        Room room = rooms[roomNumber - 1];
        if (room.isBooked()) {
            room.checkoutRoom();
            System.out.println("Room " + roomNumber + " checked out successfully.");
        } else {
            System.out.println("Room " + roomNumber + " is not booked.");
        }
    }

    public void displayRoomStatus() {
        try{
            FileReader reader = new FileReader("Checkin.txt");
            for (Room room : rooms) {
                System.out.println(room);
            }
            reader.close();  
        }
        catch(IOException e){
            e.printStackTrace();
        }
    }

    
}

public class ClubMahindraHotelRoomsManagementSystem {
    static String NIYANT_FILE = "Checkin.txt";
    static String NIYANT1_FILE = "Checkout.txt";
    
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("number of rooms are 10 ");
        int numRooms = 10;

        Hotel hotel = new Hotel(numRooms);

        while (true) {
            System.out.println("\n1. Book a room");
            System.out.println("2. Checkout from a room");
            System.out.println("3. Display room status");
            System.out.println("4. Exit");
            
            System.out.print("Enter your choice: ");
            int choice = scanner.nextInt();

            switch (choice) {
                case 1:
                    System.out.print("types of rooms are:\n1.AC rooms\n2.non Ac rooms");
                    System.out.print("\nRoom number 1 to 5 are AC rooms and others non-AC\nenter your choice:");
                    int types = scanner.nextInt();
                    if(types>2 && types<1){
                        System.out.print("enter a valid number");
                        ClubMahindraHotelRoomsManagementSystem cl = new  ClubMahindraHotelRoomsManagementSystem();   
                    }
                    else{
                        switch(types)
                        {
                        case 1:
                            System.out.print("Enter room number to book: ");    
                            int bookRoomNumber = scanner.nextInt();
                            scanner.nextLine(); // Consume newline character
                            System.out.print("Enter guest name: ");
                            String guestName = scanner.nextLine();
                            System.out.print("Enter phone number: ");
                            String phoneNumber = scanner.nextLine();
                            System.out.println("enter the date in dd/mm/yyyy:");
                            String date = scanner.nextLine();
                            writeToFile(NIYANT_FILE, " Room Number: " + bookRoomNumber + " Guest Name: " + guestName +  " Phone Number: "
                            + phoneNumber+" Date:"+date+" AC Rooms");
                            hotel.bookRoom(bookRoomNumber, guestName, phoneNumber);
                            break;
                        case 2:
                            System.out.print("Enter room number to book: ");
                            bookRoomNumber = scanner.nextInt();
                            scanner.nextLine(); // Consume newline character
                            System.out.print("Enter guest name: ");
                            guestName = scanner.nextLine();
                            System.out.print("Enter phone number: ");
                            phoneNumber = scanner.nextLine();
                            System.out.println("enter the date in dd/mm/yyyy:");
                            date = scanner.nextLine();
                            writeToFile(NIYANT_FILE, " Room Number: " + bookRoomNumber + " Guest Name: " + guestName +  " Phone Number: "
                            + phoneNumber+" Date:"+date+" Non-AC Rooms");
                            hotel.bookRoom(bookRoomNumber, guestName, phoneNumber);
                            break;
                        }
                    }
                    break;
                    
                case 2:
                    
                    System.out.print("Enter room number to checkout: ");
                    int checkoutRoomNumber = scanner.nextInt();
                    hotel.checkoutRoom(checkoutRoomNumber);
                    writeToFile(NIYANT1_FILE, "Room Number: " + checkoutRoomNumber);
                    break;
                case 3:
                    hotel.displayRoomStatus();
                    break;
                
                case 4:
                    System.out.println("Exiting...");
                    System.exit(0);
                default:
                    System.out.println("Invalid choice. Please enter a number between 1 and 5.");
            }
        }
    }
    private static void writeToFile(String fileName, String content) {
        try (FileWriter writer = new FileWriter(fileName, true)) {
            writer.write(content + "\n");
        } catch (IOException e) {
            System.out.println("An error occurred while writing to file: " + e.getMessage());
        }
    }
}
