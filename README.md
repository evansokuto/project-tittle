# project-tittle
question 1


-- Library Management System Database Schema
-- Created: 2025-05-12

-- Use or create the database
CREATE DATABASE IF NOT EXISTS LibraryDB;
USE LibraryDB;

-- Authors table: stores author details
CREATE TABLE Authors (
  author_id INT AUTO_INCREMENT PRIMARY KEY,
  first_name VARCHAR(100) NOT NULL,
  last_name VARCHAR(100) NOT NULL
);

-- Genres table: stores book genres
CREATE TABLE Genres (
  genre_id INT AUTO_INCREMENT PRIMARY KEY,
  genre_name VARCHAR(100) NOT NULL UNIQUE
);

-- Books table: stores book information
CREATE TABLE Books (
  book_id INT AUTO_INCREMENT PRIMARY KEY,
  title VARCHAR(255) NOT NULL,
  published_year YEAR,
  genre_id INT,
  is_available BOOLEAN NOT NULL DEFAULT TRUE,
  CONSTRAINT fk_genre FOREIGN KEY (genre_id) REFERENCES Genres(genre_id)
);

-- Junction table for many-to-many relationship between Books and Authors
CREATE TABLE BookAuthors (
  book_id INT NOT NULL,
  author_id INT NOT NULL,
  PRIMARY KEY (book_id, author_id),
  CONSTRAINT fk_book FOREIGN KEY (book_id) REFERENCES Books(book_id) ON DELETE CASCADE,
  CONSTRAINT fk_author FOREIGN KEY (author_id) REFERENCES Authors(author_id) ON DELETE CASCADE
);

-- Members table: stores library member details
CREATE TABLE Members (
  member_id INT AUTO_INCREMENT PRIMARY KEY,
  first_name VARCHAR(100) NOT NULL,
  last_name VARCHAR(100) NOT NULL,
  email VARCHAR(255) NOT NULL UNIQUE,
  phone VARCHAR(20),
  membership_date DATE NOT NULL
);

-- Borrowings table: tracks book loans (1-M relationship)
CREATE TABLE Borrowings (
  borrowing_id INT AUTO_INCREMENT PRIMARY KEY,
  book_id INT NOT NULL,
  member_id INT NOT NULL,
  borrow_date DATE NOT NULL,
  return_date DATE,
  CONSTRAINT fk_borrow_book FOREIGN KEY (book_id) REFERENCES Books(book_id),
  CONSTRAINT fk_borrow_member FOREIGN KEY (member_id) REFERENCES Members(member_id)
);

-- Indexes for performance optimization
CREATE INDEX idx_books_genre ON Books(genre_id);
CREATE INDEX idx_borrowings_book ON Borrowings(book_id);
CREATE INDEX idx_borrowings_member ON Borrowings(member_id);
