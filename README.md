# Database-group-assignment-
-- Create the database
CREATE Database BookStore;
USE BookStore;

-- Create book_language table
CREATE TABLE book_language (
    language_id INT AUTO_INCREMENT PRIMARY KEY,
    language_code VARCHAR(8) NOT NULL,
    language_name VARCHAR(50) NOT NULL,
    UNIQUE KEY (language_code)
);

-- Create publisher table
CREATE TABLE publisher (
    publisher_id INT AUTO_INCREMENT PRIMARY KEY,
    publisher_name VARCHAR(100) NOT NULL,
    UNIQUE KEY (publisher_name)
);

-- Create author table
CREATE TABLE author (
    author_id INT AUTO_INCREMENT PRIMARY KEY,
    author_name VARCHAR(100) NOT NULL,
    UNIQUE KEY (author_name)
);

-- Create book table
CREATE TABLE book (
    book_id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(400) NOT NULL,
    isbn13 VARCHAR(13) UNIQUE,
    language_id INT NOT NULL,
    num_pages INT,
    publication_date DATE,
    publisher_id INT,
    price DECIMAL(10, 2) NOT NULL,
    stock_quantity INT NOT NULL DEFAULT 0,
    FOREIGN KEY (language_id) REFERENCES book_language(language_id),
    FOREIGN KEY (publisher_id) REFERENCES publisher(publisher_id)
);

-- Create book_author table (many-to-many relationship)
CREATE TABLE book_author (
    book_id INT NOT NULL,
    author_id INT NOT NULL,
    PRIMARY KEY (book_id, author_id),
    FOREIGN KEY (book_id) REFERENCES book(book_id),
    FOREIGN KEY (author_id) REFERENCES author(author_id)
);

-- Create country table
CREATE TABLE country (
    country_id INT AUTO_INCREMENT PRIMARY KEY,
    country_name VARCHAR(100) NOT NULL,
    UNIQUE KEY (country_name)
);

-- Create address_status table
CREATE TABLE address_status (
    status_id INT AUTO_INCREMENT PRIMARY KEY,
    address_status VARCHAR(30) NOT NULL,
    UNIQUE KEY (address_status)
);

-- Create address table
CREATE TABLE address (
    address_id INT AUTO_INCREMENT PRIMARY KEY,
    street_number VARCHAR(10) NOT NULL,
    street_name VARCHAR(100) NOT NULL,
    city VARCHAR(100) NOT NULL,
    country_id INT NOT NULL,
    postal_code VARCHAR(20) NOT NULL,
    FOREIGN KEY (country_id) REFERENCES country(country_id)
);

-- Create Customer table
CREATE TABLE Customer (
    customer_id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    phone VARCHAR(20),
    password_hash VARCHAR(255) NOT NULL,
    registration_date DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP
);

-- Create customer_address table
CREATE TABLE customer_address (
    customer_id INT NOT NULL,
    address_id INT NOT NULL,
    status_id INT NOT NULL,
    PRIMARY KEY (customer_id, address_id),
    FOREIGN KEY (customer_id) REFERENCES Customer(customer_id),
    FOREIGN KEY (address_id) REFERENCES address(address_id),
    FOREIGN KEY (status_id) REFERENCES address_status(status_id)
);

-- Create shipping_method table
CREATE TABLE shipping_method (
    method_id INT AUTO_INCREMENT PRIMARY KEY,
    method_name VARCHAR(100) NOT NULL,
    cost DECIMAL(6, 2) NOT NULL,
    UNIQUE KEY (method_name)
);

-- Create order_status table
CREATE TABLE order_status (
    status_id INT AUTO_INCREMENT PRIMARY KEY,
    status_value VARCHAR(20) NOT NULL,
    UNIQUE KEY (status_value)
);

-- Create cust_order table
CREATE TABLE cust_order (
    order_id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT NOT NULL,
    shipping_method_id INT NOT NULL,
    dest_address_id INT NOT NULL,
    order_date DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (customer_id) REFERENCES Customer(customer_id),
    FOREIGN KEY (shipping_method_id) REFERENCES shipping_method(method_id),
    FOREIGN KEY (dest_address_id) REFERENCES address(address_id)
);

-- Create order_line table
CREATE TABLE order_line (
    line_id INT AUTO_INCREMENT PRIMARY KEY,
    order_id INT NOT NULL,
    book_id INT NOT NULL,
    price DECIMAL(10, 2) NOT NULL,
    quantity INT NOT NULL DEFAULT 1,
    FOREIGN KEY (order_id) REFERENCES cust_order(order_id),
    FOREIGN KEY (book_id) REFERENCES book(book_id)
);

-- Create order_history table
CREATE TABLE order_history (
    history_id INT AUTO_INCREMENT PRIMARY KEY,
    order_id INT NOT NULL,
    status_id INT NOT NULL,
    status_date DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (order_id) REFERENCES cust_order(order_id),
    FOREIGN KEY (status_id) REFERENCES order_status(status_id)
);
-- Insert sample languages
INSERT INTO book_language (language_code, language_name) VALUES
('en', 'English'),
('es', 'Spanish'),
('fr', 'French'),
('de', 'German'),
('ja', 'Japanese');

-- Insert sample publishers
INSERT INTO publisher (publisher_name) VALUES
('Penguin Random House'),
('HarperCollins'),
('Simon & Schuster'),
('Macmillan'),
('Hachette');

-- Insert sample authors
INSERT INTO author (author_name) VALUES
('J.K. Rowling'),
('George R.R. Martin'),
('Stephen King'),
('Agatha Christie'),
('J.R.R. Tolkien');

-- Insert sample books
INSERT INTO book (title, isbn13, language_id, num_pages, publication_date, publisher_id, price, stock_quantity) VALUES
('The Hobbit', '9780007525508', 1, 310, '1937-09-21', 4, 12.99, 50),
('Harry Potter and the Philosopher''s Stone', '9780747532743', 1, 223, '1997-06-26', 1, 10.99, 100),
('A Game of Thrones', '9780553103540', 1, 694, '1996-08-01', 2, 14.99, 75),
('Murder on the Orient Express', '9780007119318', 1, 256, '1934-01-01', 3, 9.99, 60),
('The Shining', '9780307743657', 1, 447, '1977-01-28', 5, 11.99, 45);

-- Link books to authors
INSERT INTO book_author (book_id, author_id) VALUES
(1, 5),
(2, 1),
(3, 2),
(4, 4),
(5, 3);

-- Insert sample countries
INSERT INTO country (country_name) VALUES
('United States'),
('United Kingdom'),
('Canada'),
('Australia'),
('Germany');

-- Insert address statuses
INSERT INTO address_status (address_status) VALUES
('Current'),
('Previous'),
('Billing'),
('Shipping');

-- Insert sample shipping methods
INSERT INTO shipping_method (method_name, cost) VALUES
('Standard Shipping', 4.99),
('Express Shipping', 9.99),
('Overnight Shipping', 19.99),
('International Shipping', 14.99);

-- Insert order statuses
INSERT INTO order_status (status_value) VALUES
('Pending'),
('Processing'),
('Shipped'),
('Delivered'),
('Cancelled');
