# Design BookMyShow
75
varyani_dinesh's avatar
varyani_dinesh
174
Last Edit: August 30, 2023 1:01 AM

52.1K VIEWS

### Describe how would you design an online ticketing system that sells movie tickets like BookMyShow.

### Where: I was asked this problem for an interview for Oracle.

### Solution: I started with defining goals and requirement of the system. Few functional and non functional requirement are as follows -

### Functional Requirements:

The system should be able to list down cities where its cinemas are located.
Upon selecting the city, the system should display the movies released in that particular city to that user.
Once the user makes his choice of the movie, the system should display the cinemas running that movie and its available shows.
The user should be able to select the show from a cinema and book their tickets.
The system should be able to show the user the seating arrangement of the cinema hall.
The user should be able to select multiple seats according to their choice.
The user should be able to distinguish between available seats from the booked ones.
Users should be able to put a hold on the seats for 5/10 minutes before they make a payment to finalize the booking.
The system should serve the tickets First In First Out manner
Non-Functional Requirements:

The system would need to be highly concurrent as there will be multiple booking requests for the same seat at any particular point in time. The design should be such that system handles such ambiguity fairly.
Secure and ACID compliant.


### Database Model:

City
Movie
Cinema
Cinema Hall
Show
Seat
Booking
User
Payment


### Relationships between them:

A City can have multiple Cinemas.
A Cinema can have multiple halls.
A Movie can will have many Shows.
A Show will have multiple Bookings.
A User can have multiple bookings.


### How to handle concurrency such that no two users are able to book same seat?

Utilize transaction isolation levels.

###  SET TRANSACTION ISOLATION LEVEL SERIALIZABLE; for the seat and booking update. As Serializable level guarantees safety from Dirty, Nonrepeatable and Phantoms reads.