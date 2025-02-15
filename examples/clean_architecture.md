```java
    // Entities Layer - Domain Model with Business Logic
    class ParkingLot {
        private String id;
        private List<ParkingSpot> spots;
        private ParkingRate rate;
        private int capacity;

        // Domain business rules
        public ParkingTicket parkVehicle(Vehicle vehicle) {
            if (isFull()) {
                throw new ParkingFullException("No spots available");
            }

            ParkingSpot spot = findAvailableSpot(vehicle.getType());
            if (spot == null) {
                throw new NoSuitableSpotException("No suitable spot for vehicle type");
            }

            spot.occupy(vehicle);
            return new ParkingTicket(vehicle, spot, LocalDateTime.now());
        }

        public Payment exitVehicle(ParkingTicket ticket) {
            validateTicket(ticket);
            ParkingSpot spot = ticket.getParkingSpot();
            Duration parkingDuration = Duration.between(ticket.getEntryTime(), LocalDateTime.now());
            
            BigDecimal fee = rate.calculateFee(parkingDuration);
            spot.vacate();
            
            return new Payment(ticket, fee);
        }

        private ParkingSpot findAvailableSpot(VehicleType type) {
            return spots.stream()
                       .filter(spot -> spot.canPark(type) && spot.isAvailable())
                       .findFirst()
                       .orElse(null);
        }

        private boolean isFull() {
            return spots.stream().noneMatch(ParkingSpot::isAvailable);
        }
    }

    // Use Cases Layer
    interface ParkingService {
        ParkingTicket parkVehicle(Vehicle vehicle);
        Payment processExit(String ticketId);
        List<ParkingSpot> getAvailableSpots();
    }

    class ParkingLotService implements ParkingService {
        private final ParkingLotRepository parkingLotRepository;
        private final TicketRepository ticketRepository;

        @Override
        public ParkingTicket parkVehicle(Vehicle vehicle) {
            ParkingLot parkingLot = parkingLotRepository.findActive();
            ParkingTicket ticket = parkingLot.parkVehicle(vehicle);
            ticketRepository.save(ticket);
            return ticket;
        }
    }

    // Interface Adapters Layer
    class ParkingController {
        private final ParkingService parkingService;

        @RestController
        @RequestMapping("/api/parking")
        public class ParkingController {
            @PostMapping("/park")
            public ParkingTicketDTO parkVehicle(@RequestBody VehicleDTO vehicleDTO) {
                Vehicle vehicle = vehicleMapper.toEntity(vehicleDTO);
                ParkingTicket ticket = parkingService.parkVehicle(vehicle);
                return ticketMapper.toDTO(ticket);
            }
        }
    }

    // Frameworks & Drivers Layer
    @Repository
    class JpaParkingLotRepository implements ParkingLotRepository {
        private final JpaRepository<ParkingLotEntity, String> jpaRepository;

        @Override
        public ParkingLot findActive() {
            ParkingLotEntity entity = jpaRepository.findByStatus(Status.ACTIVE);
            return parkingLotMapper.toEntity(entity);
        }
    }
    ```