# Simple-Book-Library
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract BookLibrary {
    struct Book {
        string title;
        string author;
        bool isAvailable;
    }

    Book[] public books;
    mapping(uint256 => address) public borrowedBy;

    function addBook(string memory title, string memory author) public {
        books.push(Book(title, author, true));
    }

    function borrowBook(uint256 bookId) public {
        require(bookId < books.length, "Book does not exist");
        require(books[bookId].isAvailable, "Book not available");
        books[bookId].isAvailable = false;
        borrowedBy[bookId] = msg.sender;
    }

    function returnBook(uint256 bookId) public {
        require(borrowedBy[bookId] == msg.sender, "Not borrowed by you");
        books[bookId].isAvailable = true;
        borrowedBy[bookId] = address(0);
    }

    function getBookCount() public view returns (uint256) {
        return books.length;
    }
}
