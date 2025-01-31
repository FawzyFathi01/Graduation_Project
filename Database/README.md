# **Rentora Database Schema**

## **1. Users Table**

|Column Name|Data Type|Constraints|Description|
|---|---|---|---|
|UserID|INT|PRIMARY KEY, AUTO_INCREMENT|Unique user identifier|
|FirstName|VARCHAR(50)|NOT NULL|First name|
|LastName|VARCHAR(50)|NOT NULL|Last name|
|Email|VARCHAR(100)|UNIQUE, NOT NULL|User's email|
|PasswordHash|VARCHAR(255)|NOT NULL|Encrypted password|
|PhoneNumber|VARCHAR(15)|NOT NULL|User's phone number|
|NationalID|VARCHAR(20)|UNIQUE, NOT NULL|User's national ID|
|ProfileImage|VARCHAR(255)|NULL|Profile picture URL|
|Address|TEXT|NULL|User's full address|
|Role|ENUM('User', 'Admin')|NOT NULL|User role|
|CreatedAt|DATETIME|DEFAULT CURRENT_TIMESTAMP|Account creation date|

---

## **2. Products Table**

|Column Name|Data Type|Constraints|Description|
|---|---|---|---|
|ProductID|INT|PRIMARY KEY, AUTO_INCREMENT|Unique product identifier|
|Name|VARCHAR(100)|NOT NULL|Product name|
|Description|TEXT|NOT NULL|Detailed product description|
|PricePerHour|DECIMAL(10,2)|NOT NULL|Rental price per hour|
|CategoryID|INT|FOREIGN KEY|Product category reference|
|Location|VARCHAR(255)|NULL|Product location|
|Latitude|DECIMAL(9,6)|NULL|GPS Latitude|
|Longitude|DECIMAL(9,6)|NULL|GPS Longitude|
|Status|ENUM('Available', 'Rented', 'Under Maintenance')|NOT NULL|Product status|
|OwnerID|INT|FOREIGN KEY (Users)|Owner of the product|
|CreatedAt|DATETIME|DEFAULT CURRENT_TIMESTAMP|Product creation date|

---

## **3. Categories Table**

|Column Name|Data Type|Constraints|Description|
|---|---|---|---|
|CategoryID|INT|PRIMARY KEY, AUTO_INCREMENT|Unique category identifier|
|Name|VARCHAR(100)|NOT NULL, UNIQUE|Category name|

---

## **4. Rentals Table** (For tracking rentals)

|Column Name|Data Type|Constraints|Description|
|---|---|---|---|
|RentalID|INT|PRIMARY KEY, AUTO_INCREMENT|Unique rental identifier|
|ProductID|INT|FOREIGN KEY (Products)|Rented product reference|
|RenterID|INT|FOREIGN KEY (Users)|User who rented the product|
|StartTime|DATETIME|NOT NULL|Rental start time|
|EndTime|DATETIME|NOT NULL|Rental end time|
|TotalCost|DECIMAL(10,2)|NOT NULL|Total rental cost|
|Status|ENUM('Active', 'Completed', 'Cancelled')|NOT NULL|Rental status|

---

## **5. Payments Table** (For tracking payments)

|Column Name|Data Type|Constraints|Description|
|---|---|---|---|
|PaymentID|INT|PRIMARY KEY, AUTO_INCREMENT|Unique payment identifier|
|RentalID|INT|FOREIGN KEY (Rentals)|Associated rental transaction|
|Amount|DECIMAL(10,2)|NOT NULL|Amount paid|
|PaymentMethod|ENUM('Credit Card', 'PayPal', 'Cash')|NOT NULL|Payment method|
|PaymentDate|DATETIME|DEFAULT CURRENT_TIMESTAMP|Payment timestamp|

---

## **6. Reviews Table** (User reviews & ratings)

|Column Name|Data Type|Constraints|Description|
|---|---|---|---|
|ReviewID|INT|PRIMARY KEY, AUTO_INCREMENT|Unique review identifier|
|ProductID|INT|FOREIGN KEY (Products)|Reviewed product reference|
|UserID|INT|FOREIGN KEY (Users)|Reviewer reference|
|Rating|INT|CHECK (Rating BETWEEN 1 AND 5)|Product rating (1-5)|
|Comment|TEXT|NULL|User's review comment|
|CreatedAt|DATETIME|DEFAULT CURRENT_TIMESTAMP|Review creation date|

---

## **7. Favorites Table** (For user wishlists)

|Column Name|Data Type|Constraints|Description|
|---|---|---|---|
|FavoriteID|INT|PRIMARY KEY, AUTO_INCREMENT|Unique favorite identifier|
|UserID|INT|FOREIGN KEY (Users)|User who favorited the item|
|ProductID|INT|FOREIGN KEY (Products)|Favorited product reference|

---

## **8. Notifications Table** (For system notifications)

|Column Name|Data Type|Constraints|Description|
|---|---|---|---|
|NotificationID|INT|PRIMARY KEY, AUTO_INCREMENT|Unique notification ID|
|UserID|INT|FOREIGN KEY (Users)|Recipient of the notification|
|Message|TEXT|NOT NULL|Notification message|
|Status|ENUM('Unread', 'Read')|NOT NULL|Notification status|
|CreatedAt|DATETIME|DEFAULT CURRENT_TIMESTAMP|Notification timestamp|

---

## **Database Relationships**

- **Users (1) → (M) Products**: A user can own multiple products.
- **Users (1) → (M) Rentals**: A user can rent multiple products.
- **Users (1) → (M) Reviews**: A user can write multiple reviews.
- **Products (1) → (M) Rentals**: A product can be rented multiple times.
- **Products (1) → (M) Reviews**: A product can have multiple reviews.
- **Rentals (1) → (1) Payments**: Each rental has one payment.
- **Users (1) → (M) Favorites**: A user can favorite multiple products.

---