# 🚀 Smart Elevator Control System - ER Design

## 📌 Overview

This system models a **multi-building elevator control platform** that manages:

* Buildings
* Floors
* Elevators
* Requests
* Rides
* Maintenance

It supports real-time elevator operations, request handling, and analytics.

---

# 🏢 Entities Explanation

---

## 🏢 `building`

```sql
building {
  id serial pk
  name text
  address text
  city text
}
```

**Represents:**
A physical building registered in the system.

**Foreign Keys:**
❌ No foreign keys

---

## 🛗 `elevator`

```sql
elevator {
  id serial pk
  building_id fk
  elevator_number int
  elevator_status enum[idle, moving, maintenance]
}
```

**Represents:**
An elevator operating inside a building.

**Foreign Keys:**

* `building_id` → links elevator to its building
  → Used to identify which building the elevator belongs to

---

## 🟨 `floor`

```sql
floor {
  id serial pk
  building_id fk
  floor_number varchar(10)
}
```

**Represents:**
A floor inside a building.

**Foreign Keys:**

* `building_id` → links floor to its building
  → Used to fetch all floors of a building

---

## 🔵 `elevator_floor`

```sql
elevator_floor {
  id serial pk
  floor_id fk
  elevator_id fk
}
```

**Represents:**
Mapping between elevators and floors they serve.

**Foreign Keys:**

* `floor_id` → links to floor
  → Used to know which floor is served
* `elevator_id` → links to elevator
  → Used to know which elevator serves that floor

---

## 🟢 `request`

```sql
request {
  id serial pk
  floor_id fk
  direction enum[up, down]
  status enum[pending, assigned, completed]
  created_at timestamp
}
```

**Represents:**
A user request generated from a floor.

**Foreign Keys:**

* `floor_id` → links request to floor
  → Used to track from which floor request originated

---

## 🔴 `maintenance`

```sql
maintenance {
  id serial pk
  elevator_id fk
  cost decimal
  start_at timestamp
  end_at timestamp
  reason text
}
```

**Represents:**
Maintenance records of an elevator.

**Foreign Keys:**

* `elevator_id` → links maintenance to elevator
  → Used to track maintenance history of each elevator

---

## 🟠 `ride`

```sql
ride {
  id serial pk
  elevator_id fk
  request_id fk
  source_floor_id fk
  destination_floor_id fk
  started_at timestamp
  ended_at timestamp
}
```

**Represents:**
An actual elevator trip fulfilling a request.

**Foreign Keys:**

* `elevator_id` → links ride to elevator
  → Used to track which elevator handled the ride

* `request_id` → links ride to request
  → Used to identify which request was fulfilled

* `source_floor_id` → links to floor
  → Used to identify ride starting point

* `destination_floor_id` → links to floor
  → Used to identify destination floor

---

# 🔗 Relationships

---

## 🏢 Building Relationships

* `building.id < elevator.building_id`
  → One building has many elevators

* `building.id < floor.building_id`
  → One building has many floors

---

## 🛗 Elevator Relationships

* `elevator.id < maintenance.elevator_id`
  → One elevator has many maintenance records

* `elevator.id < ride.elevator_id`
  → One elevator performs many rides

* `elevator.id < elevator_floor.elevator_id`
  → One elevator serves multiple floors

---

## 🟨 Floor Relationships

* `floor.id < request.floor_id`
  → One floor generates many requests

* `floor.id < elevator_floor.floor_id`
  → One floor can be served by multiple elevators

* `floor.id < ride.source_floor_id`
  → Floor can be source of many rides

* `floor.id < ride.destination_floor_id`
  → Floor can be destination of many rides

---

## 🔁 Request ↔ Ride Relationship

* `ride.request_id → request.id`

👉 **One-to-One Relationship**

* One request results in one ride
* One ride fulfills one request

---

# 🧠 Key Design Highlights

* ✅ Proper normalization (no redundancy)
* ✅ Supports many-to-many (elevator ↔ floor)
* ✅ Tracks full lifecycle (request → ride)
* ✅ Maintains history (maintenance & rides)
* ✅ Scalable for multiple buildings

---

# 🏁 Conclusion

This ER design fully supports:

* Real-time elevator request handling
* Ride tracking and analytics
* Maintenance history
* Multi-building scalability

---
