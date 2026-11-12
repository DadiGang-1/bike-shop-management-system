# MySQL Database Design

### Table: clients
- id: INT, Primary Key, Auto Increment
- first_name: VARCHAR(255), Not Null
- last_name: VARCHAR(255), Not Null
- date_of_birth: DATE, Not Null
- gender: VARCHAR(20), Nullable
- email: VARCHAR(255), Unique, Nullable
- phone: VARCHAR(20), Nullable
- address: VARCHAR(255), Nullable
- created_at: DATETIME, Not Null

### Table: specialists
- id: INT, Primary Key, Auto Increment
- first_name: VARCHAR(255), Not Null
- last_name: VARCHAR(255), Not Null
- email: VARCHAR(255), Unique, Not Null
- phone: VARCHAR(20), Nullable
- location_id: INT, Foreign Key → shop_locations(id)
- created_at: DATETIME, Not Null

### Table: appointments
- id: INT, Primary Key, Auto Increment
- specialist_id: INT, Foreign Key → specialists(id)
- client_id: INT, Foreign Key → clients(id)
- location_id: INT, Foreign Key → shop_locations(id)
- appointment_time: DATETIME, Not Null
- status: INT (0 = Scheduled, 1 = Completed, 2 = Cancelled)
- created_at: DATETIME, Not Null

### Table: admin
- id: INT, Primary Key, Auto Increment
- username: VARCHAR(100), Not Null
- email: VARCHAR(255), Unique, Not Null
- password_hash: VARCHAR(255), Not Null
- role: VARCHAR(50), Not Null
- created_at: DATETIME, Not Null

### Table: shop_locations
- id: INT, Primary Key, Auto Increment
- name: VARCHAR(255), Not Null
- address: VARCHAR(255), Not Null
- city: VARCHAR(100), Not Null
- postal_code: VARCHAR(20), Not Null
- country: VARCHAR(100), Not Null
- phone: VARCHAR(20), Nullable
- created_at: DATETIME, Not Null
  
### Table: payments
- id: INT, Primary Key, Auto Increment
- client_id: INT, Foreign Key (clients.id), Not Null
- appointment_id: INT, Foreign Key (appointments.id), Nullable
- amount: DECIMAL(10,2), Not Null
- currency: VARCHAR(3), Not Null
- payment_method: VARCHAR(50), Not Null
- status: VARCHAR(50), Not Null
- transaction_reference: VARCHAR(255), Unique, Nullable
- payment_date: DATETIME, Nullable
- created_at: DATETIME, Not Null

# MongoDB Collection Design

### Collection: prescriptions
```json
{
  "_id": "ObjectId('64abc123456')",
  "clientName": "John Smith",
  "appointmentId": 51,
  "order": "complete bike fitting",
  "specialistNote": "Move saddle +1/2mm per week until reaching 78.5cm",
  "bike": {
    "type": "Bike",
    "name": "Orbea",
    "location": "Market Street"
    ...
  }
}
```

### Collection: feedback
```json
{
  "_id": "ObjectId('64abc123457')",
  "clientId": 12,
  "appointmentId": 51,
  "specialistId": 7,
  "rating": 5,
  "comment": "Very professional and helpful.",
  "category": "bike fitting",
  "createdAt": "2026-11-11T10:00:00Z"
}
```
### Collection: logs
```json
{
  "_id": "ObjectId('64abc123458')",
  "userId": 3,
  "userType": "admin",
  "action": "CREATE_APPOINTMENT",
  "entity": "appointment",
  "entityId": 51,
  "details": "Appointment created successfully.",
  "ipAddress": "192.168.1.10",
  "createdAt": "2026-11-11T10:15:00Z"
}
```

### Collection: messages
```json
{
  "_id": "ObjectId('64abc123459')",
  "senderId": 12,
  "senderType": "client",
  "receiverId": 7,
  "receiverType": "specialist",
  "appointmentId": 51,
  "content": "Hello xxxx, I have a question about my appointment.",
  "isRead": false,
  "createdAt": "2026-11-11T10:30:00Z"
}
```
