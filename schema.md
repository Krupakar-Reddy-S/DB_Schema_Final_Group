# Hospital Management System Database Schema

## Tables

**Table 1: patients**

- **Primary Key:** id
- **Fields:** full_name, date_of_birth, gender, address, phone_number

**Table 2: doctors**

- **Primary Key:** id
- **Fields:** full_name, specialty, phone_number

**Table 3: appointments**

- **Primary Key:** id
- **Fields:** patient_id, doctor_id, appointment_date, status
- **Indexes:** patient_id, doctor_id

**Table 4: medical_records**

- **Primary Key:** id
- **Fields:** patient_id, diagnosis, treatment, prescription, record_date
- **Indexes:** patient_id

**Table 5: billing**

- **Primary Key:** id
- **Fields:** patient_id, appointment_id, total_amount, billing_date
- **Indexes:** patient_id, appointment_id

**Table 6: rooms**

- **Primary Key:** id
- **Fields:** room_number, type, status
- **Indexes:** room_number

**Table 7: room_assignments**

- **Primary Key:** id
- **Fields:** patient_id, room_id, assignment_date, discharge_date
- **Indexes:** patient_id, room_id

## Schema Diagram - [DrawSQL link](https://drawsql.app/teams/krupakar-reddy/diagrams/hospital-management-system)

<iframe width="100%" height="500px" style="box-shadow: 0 2px 8px 0 rgba(63,69,81,0.16); border-radius:15px;" allowtransparency="true" allowfullscreen="true" scrolling="no" title="Embedded DrawSQL IFrame" frameborder="0" src="https://drawsql.app/teams/krupakar-reddy/diagrams/hospital-management-system/embed"></iframe>

<!-- <p align="center">
  <img src="schema.png" alt="Hospital Management System Database Schema" height="900px">
</p> -->


## Use Case Queries

**Query 1: List all appointments for a specific doctor on a given date:**

```sql
SELECT a.id, p.full_name AS patient_name, a.appointment_date, a.status
FROM appointments a
JOIN patients p ON a.patient_id = p.id
WHERE a.doctor_id = :doctor_id
        AND DATE(a.appointment_date) = :date;
```

**Query 2: Retrieve the medical records for a specific patient:**

```sql
SELECT mr.id, mr.diagnosis, mr.treatment, mr.prescription, mr.record_date
FROM medical_records mr
WHERE mr.patient_id = :patient_id;
```

**Query 3: Find the total amount billed to a specific patient:**

```sql
SELECT SUM(b.total_amount) AS total_billed
FROM billing b
WHERE b.patient_id = :patient_id;
```

**Query 4: List all available rooms of a specific type:**

```sql
SELECT r.room_number
FROM rooms r
WHERE r.type = :room_type
        AND r.status = 'available';
```

**Query 5: Calculate the average length of stay for patients in the hospital:**

```sql
SELECT AVG(DATEDIFF(ra.discharge_date, ra.assignment_date)) AS average_length_of_stay
FROM room_assignments ra;
```

**Query 6: List all patients currently admitted to the hospital:**

```sql
SELECT p.full_name, ra.assignment_date, r.room_number, r.type
FROM room_assignments ra
JOIN patients p ON ra.patient_id = p.id
JOIN rooms r ON ra.room_id = r.id
WHERE ra.discharge_date IS NULL;
```
