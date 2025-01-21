# lscs-tomo-api

This repository handles the backend for validating LSCS Members via barcode scanning. 

**For Tomo Coffee use**: checks if user is a LSCS Member, thus eligible for a discount within valid time bounds.

## Requirements
1. Requests are assumed as ID numbers (integer).

## Features
- **MySQL Database**
  - Stores the ID number, current date, and current time of each scan.
- **Validation**
  - Checks if the request is from a valid LSCS member using the LSCS Central Auth API.
  - Returns an error/fail response if the ID is invalid.
- **Time Restriction**
  - Ensures a minimum of 6 hours has elapsed since the last scan for the same ID number.
  - Returns an error/fail response if less than 6 hours have passed.

## TODO
- [x] **Database Setup**
  - Create a MySQL database to store:
    - ID number
    - Current date
    - Current time
- [x] **Validation**
  - Integrate with the LSCS Central Auth API to check membership validity.
  - Return appropriate error responses for invalid IDs.
- [x] **Time Restriction Logic**
  - Check the timestamp of the last scan for the given ID number.
  - Ensure at least 6 hours have passed before allowing a new scan.
  - Return error/fail response if the time condition is not met.

## Endpoints

### **POST `/validate`**
* requires `studentId` in req.body
* `request`:
```bash
curl -X POST http://tomo-scanner.app.dlsu-lscs.org/validate \
-H "Content-Type: application/json" \
-d '{"studentId": 12343765}'
```
* `response`:
```json
// successful scan
{
    "id": "12343765",
    "email": "rohann_dizon@dlsu.edu.ph",
    "full_name": "Rohann Gabriel Dizon",
    "committee_name": "Research and Development",
    "position_name": "Assistant Vice President",
    "division_name": "Internals"
}

// not a member
{ 
  "error": "Student ID number is not an LSCS member"
}

// availing discount within invalid time bound (6 hours from previous scan)
{
  "error": "The member has used up their discount within the last 6 hours."
}
```

* 
