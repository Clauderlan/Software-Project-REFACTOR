# Software-Project-REFACTOR
## Design Patterns Used
**1. Creational Pattern – Factory Method** 
- **Antes**: A criação de processadores estava acoplada diretamente dentro da lógica da classe BookingSystem.
```
class PaymentProcessorFactory:
self.payment_processors: Dict[PaymentMethod, PaymentProcessor] = {
    PaymentMethod.CREDIT_CARD: CreditCardProcessor(),
    PaymentMethod.PIX: PixProcessor()
}

...

processor = self.payment_processors[payment_method]
if processor.process_payment(booking.total_price):
    booking.payment_status = PaymentStatus.COMPLETED
```
- **Depois**: Agora usamos uma fábrica isolada que retorna o processador apropriado:
```
class PaymentProcessorFactory:
@staticmethod
def create_processor(method: PaymentMethod) -> PaymentProcessor:
    if method == PaymentMethod.CREDIT_CARD:
        return CreditCardProcessor()
    elif method == PaymentMethod.PIX:
        return PixProcessor()
    else:
        raise ValueError(f"Unsupported payment method")

# Uso:
processor = PaymentProcessorFactory.create_processor(payment_method)
if processor.process_payment(booking.total_price):
    booking.payment_status = PaymentStatus.COMPLETED
```

**2. Structural Pattern – Facade** 
- **Antes**: Cada chamada envolvia múltiplos objetos e manipulações explícitas:
```
room = hotel.rooms.get(room_number)
if room.is_available:
    processor = PixProcessor()
    if processor.process_payment(room.base_price):
        room.is_available = False
        customer.bookings.append(room_number)
```
- **Depois**: Encapsulamos tudo dentro de uma facade de alto nível:
```
facade = HotelSystemFacade(hotel, customer)
facade.book_room("101", PaymentMethod.PIX)

Essa facade também cuida de:

facade.view_profile()
facade.leave_review(hotel.id, 5, "Excelente atendimento!")
```

**3. Behavioral Pattern – Strategy** 
- **Antes**: O cálculo de preços era direto e fixo:
```
total_price = hotel.rooms[room_number].base_price * nights
# Possível desconto fixo direto no código:
total_price = total_price * 0.9
```
- **Depois**: Agora o cálculo é configurável via estratégia:
```
class DiscountStrategy(ABC):
    def calculate_discount(self, price: float) -> float: pass

class LoyaltyDiscount(DiscountStrategy):
    def calculate_discount(self, price: float) -> float:
        return price * 0.9

booking_service = BookingService(LoyaltyDiscount())
final_price = booking_service.calculate_final_price(300.00)
```
## Funcionalidades que constavam no projeto:
### Hotel Listing Management:
Hotels can list and manage their properties, including photos and amenities; **Feito!**

### Room Booking and Cancellation:
Users can book, modify, and cancel reservations; **Feito!**

### Price and Availability Management:
Dynamic management of room pricing and availability; **Feito!**

### Customer Profile Management:
Creation and management of customer profiles; **Feito!**

### Payment Processing:
Secure processing of payments with various payment options; **Feito!**

### Reviews and Ratings:
Customers can leave reviews and ratings for hotels; **Feito!**

### Loyalty Program Integration:
Managing loyalty programs and rewards for frequent customers; **Feito, mas não foi integrado no Menu.**

### Multi-Language Support:
Offering the booking system in multiple languages;** Feito!**

### Customer Support Interface:
Providing support to customers for booking-related inquiries; **Não feito. Quando eu fiz não atendeu completamente a parte de suporte aos clientes.**

### Analytics and Reporting:
Generating reports on bookings, occupancy rates, and revenue. **Feito, mas só a parte de gerar relatório.**
