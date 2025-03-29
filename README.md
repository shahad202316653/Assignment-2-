# Assignment-2

    #Python Code

    # loyalty_program.py
    class LoyaltyProgram:
       """Handles guest loyalty data."""
        def __init__(self, program_id, points_earned=0, points_redeemed=0, 
    reward_level="Bronze", available_rewards=None):
            self.__program_id = program_id
            self.__points_earned = points_earned
            self.__points_redeemed = points_redeemed
            self.__reward_level = reward_level
            self.__available_rewards = available_rewards or []

        def earn_points(self, amount):
            self.__points_earned += amount

        def redeem_points(self, points):
            if points <= self.__points_earned:
                self.__points_earned -= points
                self.__points_redeemed += points
                return True
            return False

        def view_available_rewards(self):
            return self.__available_rewards

        def update_reward_level(self, level):
            self.__reward_level = level

        def get_points_balance(self):
            return self.__points_earned

        def set_reward_level(self, new_level):
            self.__reward_level = new_level

        def __str__(self):
            return f"LoyaltyProgram({self.__program_id}, {self.__points_earned} pts)"

    # guest.py
    class Guest:
    """Abstract base class for hotel guests."""
        def __init__(self, guest_id, name, email, phone, loyalty_status, account_creation_date):
            self.__guest_id = guest_id
            self.__name = name
            self.__email = email
            self.__phone = phone
            self.__loyalty_status = loyalty_status
            self.__account_creation_date = account_creation_date
            self.loyalty_program = None

        def create_account(self):
            pass

        def update_profile(self):
            pass

        def view_reservation_history(self):
            pass

        def get_loyalty_info(self):
            return str(self.loyalty_program) if self.loyalty_program else "No program"

        def request_service(self):
            pass

        def __str__(self):
            return f"Guest({self.__name})"


    class RegularGuest(Guest):
        def __init__(self, guest_id, name, email, phone, loyalty_status, account_creation_date,
                 newsletter_subscribed, last_stay_date, preferred_room_type, total_stays, feedback_given):
        super().__init__(guest_id, name, email, phone, loyalty_status, account_creation_date)
            self.__newsletter_subscribed = newsletter_subscribed
            self.__last_stay_date = last_stay_date
            self.__preferred_room_type = preferred_room_type
            self.__total_stays = total_stays
            self.__feedback_given = feedback_given

        def subscribe_newsletter(self):
            self.__newsletter_subscribed = True

        def cancel_newsletter(self):
            self.__newsletter_subscribed = False

        def get_stay_count(self):
            return self.__total_stays

        def leave_feedback(self):
            self.__feedback_given = True

        def get_type(self):
            return "Regular"

        def __str__(self):
            return f"RegularGuest({self._Guest__name})"

    # vip_guest.py
    class VIPGuest(Guest):
        def __init__(self, guest_id, name, email, phone, loyalty_status, account_creation_date,
                 vip_level, support_contact, discount_rate, anniversary_date, upgrade_eligible):
        super().__init__(guest_id, name, email, phone, loyalty_status, account_creation_date)
            self.__vip_level = vip_level
            self.__support_contact = support_contact
            self.__discount_rate = discount_rate
            self.__anniversary_date = anniversary_date
            self.__upgrade_eligible = upgrade_eligible

        def request_upgrade(self):
            return self.__upgrade_eligible

        def access_vip_lounge(self):
            return True

        def apply_vip_discount(self):
            return self.__discount_rate

        def get_vip_status(self):
            return self.__vip_level

        def contact_support(self):
            return self.__support_contact

        def __str__(self):
            return f"VIPGuest({self._Guest__name})"


    # booking.py
    class Booking:
    """Represents a room booking made by a guest."""
        def __init__(self, booking_id, check_in_date, check_out_date, booking_date, booking_status, number_of_guests):
            self.__booking_id = booking_id
            self.__check_in_date = check_in_date
            self.__check_out_date = check_out_date
            self.__booking_date = booking_date
            self.__booking_status = booking_status
            self.__number_of_guests = number_of_guests
            self.rooms = []
            self.invoice = None

        def confirm_booking(self):
            self.__booking_status = "Confirmed"

        def cancel_booking(self):
            self.__booking_status = "Cancelled"

        def assign_room(self, room):
            self.rooms.append(room)

        def generate_invoice(self, invoice):
            self.invoice = invoice

        def get_duration(self):
            return (self.__check_out_date - self.__check_in_date).days

        def __str__(self):
            return f"Booking({self.__booking_id}, Status: {self.__booking_status})"


    # room.py
    class Room:
    """Represents a hotel room."""
        def __init__(self, room_number, room_type, amenities, price_per_night, availability_status, floor_number):
            self.__room_number = room_number
            self.__type = room_type
            self.__amenities = amenities
            self.__price_per_night = price_per_night
            self.__availability_status = availability_status
            self.__floor_number = floor_number

        def check_availability(self):
            return self.__availability_status

        def update_room_status(self, status):
            self.__availability_status = status

        def display_room_info(self):
            return f"Room {self.__room_number}, Type: {self.__type}"

        def set_room_details(self, details):
            for key, value in details.items():
                setattr(self, f"_{self.__class__.__name__}__{key}", value)

        def is_available_on(self, date):
            return self.__availability_status  # Simplified logic

        def __str__(self):
            return f"Room({self.__room_number})"


    # invoice.py
    class Invoice:
    """Represents the invoice generated for a booking."""
        def __init__(self, invoice_id, nightly_rate, extra_charges, discount_amount, total_amount, invoice_date):
            self.__invoice_id = invoice_id
            self.__nightly_rate = nightly_rate
            self.__extra_charges = extra_charges
            self.__discount_amount = discount_amount
            self.__total_amount = total_amount
            self.__invoice_date = invoice_date

        def calculate_total(self):
            self.__total_amount = (self.__nightly_rate + self.__extra_charges) - self.__discount_amount

        def apply_discount(self, code):
            if code == "LOYALTY10":
                self.__discount_amount += 10

        def generate_pdf(self):
            return f"Invoice PDF for {self.__invoice_id}"

        def get_invoice_details(self):
            return f"Total: ${self.__total_amount}"

        def set_charges(self, charges):
            self.__extra_charges = charges

        def __str__(self):
            return f"Invoice({self.__invoice_id})"


    # payment.py
    class Payment:
    """Represents a payment linked to an invoice."""
        def __init__(self, payment_id, method, amount_paid, date, status, transaction_id):
            self.__payment_id = payment_id
            self.__payment_method = method
            self.__amount_paid = amount_paid
            self.__payment_date = date
            self.__payment_status = status
            self.__transaction_id = transaction_id

        def process_payment(self):
            self.__payment_status = "Completed"

        def validate_transaction(self):
            return True

        def refund_payment(self):
            self.__payment_status = "Refunded"

        def get_payment_details(self):
            return f"Paid: ${self.__amount_paid} via {self.__payment_method}"

        def mark_as_paid(self):
            self.__payment_status = "Paid"

        def __str__(self):
            return f"Payment({self.__payment_id}, Status: {self.__payment_status})"


    # test_cases.py
    import datetime

    # Test Case 1: Guest Account Creation
    print("Guest Account Creation: ")
    guest1 = RegularGuest("G001", "Shahad", "shahad@gmail.com", "1234567890", "Gold", 
    datetime.date.today(), True, datetime.date.today(), "Double", 3, False)
    guest2 = VIPGuest("G002", "Marwan", "marwan@gmail.com", "0987654321", "Silver", datetime.date.today(), "Platinum", "+18005551234", 15.0, datetime.date(2022, 6, 1), True)
    print(guest1)
    print(guest2)

    # Test Case 2: Searching for Available Rooms
    print("Searching for Available Rooms: ")
    room1 = Room("101", "Single", ["Wi-Fi"], 100.0, True, 1)
    room2 = Room("102", "Double", ["Wi-Fi", "TV"], 150.0, False, 1)
    print(room1.display_room_info() if room1.check_availability() else "Room not available")
    print(room2.display_room_info() if room2.check_availability() else "Room not available")

    # Test Case 3: Making a Room Reservation
    print("Making a Room Reservation: ")
    booking1 = Booking("B001", datetime.date(2024, 4, 10), datetime.date(2024, 4, 15), datetime.date.today(), "Pending", 2)
    booking1.assign_room(room1)
    print(booking1)

    booking2 = Booking("B002", datetime.date(2024, 5, 1), datetime.date(2024, 5, 5), datetime.date.today(), "Pending", 1)
    booking2.assign_room(room2)
    print(booking2)

    # Test Case 4: Booking Confirmation Notification
    print("Booking Confirmation Notification: ")
    booking1.confirm_booking()
    booking2.confirm_booking()
    print(f"Notification sent for: {booking1}")
    print(f"Notification sent for: {booking2}")

    # Test Case 5: Invoice Generation for a Booking
    print("Invoice Generation: ")
    invoice1 = Invoice("INV001", 100.0, 20.0, 10.0, 0.0, datetime.date.today())
    invoice1.calculate_total()
    booking1.generate_invoice(invoice1)
    print(invoice1.get_invoice_details())

    invoice2 = Invoice("INV002", 150.0, 30.0, 15.0, 0.0, datetime.date.today())
    invoice2.calculate_total()
    booking2.generate_invoice(invoice2)
    print(invoice2.get_invoice_details())

    # Test Case 6: Processing Different Payment Methods
    print("Processing Different Payment Methods: ")
    payment1 = Payment("P001", "Credit Card", 110.0, datetime.date.today(), "Pending", "TX123")
    payment1.process_payment()
    print(payment1.get_payment_details())

    payment2 = Payment("P002", "Mobile Wallet", 165.0, datetime.date.today(), "Pending", "TX456")
    payment2.process_payment()
    print(payment2.get_payment_details())

    # Test Case 7: Displaying Reservation History
    print("Displaying Reservation History: ")
    guest1.view_reservation_history()  # Stubbed in base class
    guest2.view_reservation_history()

    # Test Case 8: Cancellation of a Reservation
    print("Canceling a Reservation: ")
    booking1.cancel_booking()
    room1.update_room_status(True)
    print(f"{booking1} has been cancelled. Room status: {room1.check_availability()}")

    booking2.cancel_booking()
    room2.update_room_status(True)
    print(f"{booking2} has been cancelled. Room status: {room2.check_availability()}")
