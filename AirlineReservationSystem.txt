import java.util.*;

// Class to represent a flight
class Flight {
    private String flightNumber;
    private String departureAirport;
    private String arrivalAirport;
    private Date departureTime;
    private Date arrivalTime;
    private int totalSeats;
    private int availableSeats;
    private Map<Integer, Boolean> seatMap; // Seat number -> Availability

    public Flight(String flightNumber, String departureAirport, String arrivalAirport, Date departureTime, Date arrivalTime, int totalSeats) {
        this.flightNumber = flightNumber;
        this.departureAirport = departureAirport;
        this.arrivalAirport = arrivalAirport;
        this.departureTime = departureTime;
        this.arrivalTime = arrivalTime;
        this.totalSeats = totalSeats;
        this.availableSeats = totalSeats;
        this.seatMap = new HashMap<>();
        for (int i = 1; i <= totalSeats; i++) {
            seatMap.put(i, true); // Initialize all seats as available
        }
    }

    public String getFlightNumber() {
        return flightNumber;
    }

    public String getDepartureAirport() {
        return departureAirport;
    }

    public String getArrivalAirport() {
        return arrivalAirport;
    }

    public Date getDepartureTime() {
        return departureTime;
    }

    public Date getArrivalTime() {
        return arrivalTime;
    }

    public int getTotalSeats() {
        return totalSeats;
    }

    public int getAvailableSeats() {
        return availableSeats;
    }

    public Map<Integer, Boolean> getSeatMap() {
        return seatMap;
    }

    public void bookSeat(int seatNumber) {
        if (seatMap.containsKey(seatNumber) && seatMap.get(seatNumber)) {
            seatMap.put(seatNumber, false); // Mark seat as unavailable
            availableSeats--;
            System.out.println("Seat " + seatNumber + " booked successfully.");
        } else {
            System.out.println("Seat " + seatNumber + " is not available.");
        }
    }
}

// Class to manage flight operations
class FlightManager {
    private List<Flight> flights;

    public FlightManager() {
        this.flights = new ArrayList<>();
    }

    public void addFlight(Flight flight) {
        flights.add(flight);
    }

    public List<Flight> searchFlights(String departureAirport, String arrivalAirport) {
        List<Flight> searchResults = new ArrayList<>();
        for (Flight flight : flights) {
            if (flight.getDepartureAirport().equals(departureAirport) && flight.getArrivalAirport().equals(arrivalAirport)) {
                searchResults.add(flight);
            }
        }
        return searchResults;
    }
}

// Class to represent the Airline Reservation System
public class AirlineReservationSystem {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        FlightManager flightManager = new FlightManager();

        // Sample flights
        Flight flight1 = new Flight("ABC123", "JFK", "LAX", new Date(), new Date(), 100);
        Flight flight2 = new Flight("XYZ456", "LAX", "JFK", new Date(), new Date(), 150);
        flightManager.addFlight(flight1);
        flightManager.addFlight(flight2);

        // Flight search
        System.out.println("Enter departure airport: ");
        String departureAirport = scanner.nextLine();
        System.out.println("Enter arrival airport: ");
        String arrivalAirport = scanner.nextLine();
        List<Flight> searchResults = flightManager.searchFlights(departureAirport, arrivalAirport);
        if (!searchResults.isEmpty()) {
            System.out.println("Available Flights:");
            for (Flight flight : searchResults) {
                System.out.println("Flight Number: " + flight.getFlightNumber() + ", Departure Time: " + flight.getDepartureTime() + ", Available Seats: " + flight.getAvailableSeats());
            }

            // Select a flight and book a seat
            System.out.println("Enter the flight number to book a seat: ");
            String selectedFlightNumber = scanner.nextLine();
            Flight selectedFlight = null;
            for (Flight flight : searchResults) {
                if (flight.getFlightNumber().equals(selectedFlightNumber)) {
                    selectedFlight = flight;
                    break;
                }
            }
            if (selectedFlight != null) {
                System.out.println("Available seats on flight " + selectedFlight.getFlightNumber() + ":");
                for (Map.Entry<Integer, Boolean> entry : selectedFlight.getSeatMap().entrySet()) {
                    if (entry.getValue()) {
                        System.out.print(entry.getKey() + " ");
                    }
                }
                System.out.println("\nEnter the seat number to book: ");
                int selectedSeat = scanner.nextInt();
                selectedFlight.bookSeat(selectedSeat);
            } else {
                System.out.println("Invalid flight number.");
            }
        } else {
            System.out.println("No flights available for the selected route.");
        }

        scanner.close();
    }
}
