CREATE DATABASE IF NOT EXISTS cinema_db;
USE cinema_db;

DROP TABLE IF EXISTS Bill_Snack, Apply_Discount, Review, CashPayment, CardPayment, Payment, Booking, Ticket, Snack, PreOrderSnack, SeasonalDiscount, Discount, NowShowingMovie, UpcomingMovie, Movie, Employee, Customer, User, CustomerReview;

CREATE TABLE User (
    username VARCHAR(100) PRIMARY KEY,
    password VARCHAR(100)
);

CREATE TABLE Customer (
    customerID INT PRIMARY KEY,
    username VARCHAR(100),
    FOREIGN KEY (username) REFERENCES User(username)
);

CREATE TABLE Employee (
    employeeID INT PRIMARY KEY,
    username VARCHAR(100),
    FOREIGN KEY (username) REFERENCES User(username)
);

CREATE TABLE Movie (
    movieID INT PRIMARY KEY,
    title VARCHAR(100),
    genre VARCHAR(50),
    rate DOUBLE
);

CREATE TABLE Ticket (
    ticketNumber INT PRIMARY KEY,
    type VARCHAR(50),
    price DOUBLE
);

CREATE TABLE Discount (
    discountID INT PRIMARY KEY,
    discountPercentage FLOAT,
    validityStartDate DATE,
    validityEndDate DATE
);

CREATE TABLE Booking (
    bookingID INT PRIMARY KEY,
    customerID INT,
    movieID INT,
    ticketNumber INT,
    discountID INT,
    bookingDate DATE,
    FOREIGN KEY (customerID) REFERENCES Customer(customerID),
    FOREIGN KEY (movieID) REFERENCES Movie(movieID),
    FOREIGN KEY (ticketNumber) REFERENCES Ticket(ticketNumber),
    FOREIGN KEY (discountID) REFERENCES Discount(discountID)
);

CREATE TABLE Payment (
    Bill_Id INT PRIMARY KEY,
    bookingID INT,
    method VARCHAR(50),
    Amount DOUBLE,
    FOREIGN KEY (bookingID) REFERENCES Booking(bookingID)
);

CREATE TABLE CashPayment (
    Bill_Id INT PRIMARY KEY,
    receivedBy VARCHAR(100),
    FOREIGN KEY (Bill_Id) REFERENCES Payment(Bill_Id)
);

CREATE TABLE CardPayment (
    Bill_Id INT PRIMARY KEY,
    cardType VARCHAR(50),
    cardNumber INT,
    cardHolderName VARCHAR(100),
    FOREIGN KEY (Bill_Id) REFERENCES Payment(Bill_Id)
);

CREATE TABLE Review (
    reviewID INT PRIMARY KEY,
    customerID INT,
    movieID INT,
    rating INT,
    reviewText TEXT,
    FOREIGN KEY (customerID) REFERENCES Customer(customerID),
    FOREIGN KEY (movieID) REFERENCES Movie(movieID)
);

CREATE TABLE CustomerReview (
    customerID INT,
    movieID INT,
    PRIMARY KEY (customerID, movieID),
    FOREIGN KEY (customerID) REFERENCES Customer(customerID),
    FOREIGN KEY (movieID) REFERENCES Movie(movieID)
);

CREATE TABLE Snack (
    snackID INT PRIMARY KEY,
    name VARCHAR(100),
    price DOUBLE
);

CREATE TABLE PreOrderSnack (
    snackID INT PRIMARY KEY,
    isPrepared BOOLEAN,
    FOREIGN KEY (snackID) REFERENCES Snack(snackID)
);

CREATE TABLE SeasonalDiscount (
    discountID INT PRIMARY KEY,
    season VARCHAR(50),
    FOREIGN KEY (discountID) REFERENCES Discount(discountID)
);

CREATE TABLE NowShowingMovie (
    movieID INT PRIMARY KEY,
    availableSeats INT,
    showTime VARCHAR(50),
    endDate DATE,
    runningDays INT,
    FOREIGN KEY (movieID) REFERENCES Movie(movieID)
);

CREATE TABLE UpcomingMovie (
    movieID INT PRIMARY KEY,
    expectedRelease DATE,
    FOREIGN KEY (movieID) REFERENCES Movie(movieID)
);

CREATE TABLE Bill_Snack (
    Bill_Id INT,
    snackID INT,
    PRIMARY KEY (Bill_Id, snackID),
    FOREIGN KEY (Bill_Id) REFERENCES Payment(Bill_Id),
    FOREIGN KEY (snackID) REFERENCES Snack(snackID)
);

-- Insert sample data with corrected column names
INSERT INTO User (username, password) VALUES
('Fatima', 'fatma123'),
('Salem', 'salem456'),
('Ahmed', 'ahmed789'),
('Nora', 'nora000'),
('Khaled', 'khaled555'),
('Saeed', 'saeed777'),
('Mona', 'mona888'),
('Yousef', 'yousef999');

INSERT INTO Customer (customerID, username) VALUES
(1, 'Fatima'),
(2, 'Ahmed'),
(3, 'Nora'),
(4, 'Khaled'),
(5, 'Saeed'),
(6, 'Mona');

INSERT INTO Employee (employeeID, username) VALUES
(1001, 'Salem'),
(1002, 'Yousef');

INSERT INTO Movie (movieID, title, genre, rate) VALUES
(101, 'Al-Feel Al-Azraq', 'Horror', 8.2),
(102, 'Torab Al-Mas', 'Crime', 7.8),
(103, 'Al-Kanz', 'Adventure', 7.5),
(104, 'Casablanca', 'Action', 8.0),
(105, 'Hepta', 'Romance', 7.6),
(106, 'Waqfa Regal', 'Comedy', 7.2);

INSERT INTO Ticket (ticketNumber, type, price) VALUES
(1, 'Regular', 30.00),
(2, 'VIP', 60.00),
(3, 'Child', 15.00),
(4, '3D', 50.00),
(5, 'Student', 25.00);

-- Add sample discount data
INSERT INTO Discount (discountID, discountPercentage, validityStartDate, validityEndDate) VALUES
(1, 10.0, '2025-01-01', '2025-12-31'),
(2, 20.0, '2025-05-01', '2025-06-30');

INSERT INTO Booking (bookingID, customerID, movieID, ticketNumber, discountID, bookingDate) VALUES
(1, 1, 101, 2, 1, '2025-05-10'),
(2, 2, 102, 1, NULL, '2025-05-11'),
(3, 3, 104, 4, 2, '2025-05-12'),
(4, 4, 105, 5, NULL, '2025-05-12'),
(5, 5, 106, 3, 1, '2025-05-13'),
(6, 6, 103, 1, NULL, '2025-05-14');

INSERT INTO Payment (Bill_Id, bookingID, method, Amount) VALUES
(1, 1, 'Credit Card', 60.00),
(2, 2, 'Cash', 30.00),
(3, 3, 'Mada', 50.00),
(4, 4, 'Bank Transfer', 25.00),
(5, 5, 'E-Wallet', 15.00),
(6, 6, 'Credit Card', 60.00);

INSERT INTO Snack (snackID, name, price) VALUES
(1, 'Popcorn', 10.00),
(2, 'Orange Juice', 8.00),
(3, 'Nachos', 12.00),
(4, 'Burger Sandwich', 20.00),
(5, 'Chocolate', 7.00),
(6, 'Water', 5.00);

INSERT INTO Bill_Snack (Bill_Id, snackID) VALUES
(1, 1),
(1, 2),
(2, 3),
(3, 4),
(4, 5),
(5, 6),
(6, 1),
(6, 3);
