# API — Бронирование переговорки

Базовый URL: /api/v1

1) Получить переговорки (с фильтрами)
GET /rooms?start_at=...&end_at=...&capacity=...&equipment=...

Query params:
- start_at (required, ISO datetime)
- end_at (required, ISO datetime)
- capacity (optional, int)
- equipment (optional, string, можно повторять)

200 OK
[
  {
    "room_id": "uuid",
    "name": "Room A",
    "capacity": 8,
    "location": "Floor 5",
    "equipment": ["TV", "Whiteboard"],
    "status": "AVAILABLE"
  }
]

---

2) Создать бронь
POST /bookings

Request:
{
  "room_id": "uuid",
  "title": "Weekly sync",
  "start_at": "2026-01-10T10:00:00",
  "end_at": "2026-01-10T11:00:00",
  "participants": ["a@corp.com", "b@corp.com"]
}

Правила:
- end_at > start_at
- длительность <= 2 часа
- не более 3 активных броней у создателя
- отсутствие конфликта по room_id на интервале

201 Created:
{
  "booking_id": "uuid",
  "status": "BOOKED"
}

409 Conflict (комната занята):
{
  "error": "ROOM_BUSY",
  "message": "Room is already booked for the selected time interval"
}

400 Bad Request (ограничения):
{
  "error": "VALIDATION_ERROR",
  "message": "Booking duration must be <= 2 hours"
}

---

3) Получить мои брони
GET /bookings/my?from=...&to=...

200 OK
[
  {
    "booking_id": "uuid",
    "room_id": "uuid",
    "title": "Weekly sync",
    "start_at": "2026-01-10T10:00:00",
    "end_at": "2026-01-10T11:00:00",
    "status": "BOOKED"
  }
]

---

4) Отменить бронь
POST /bookings/{booking_id}/cancel

Request:
{
  "reason": "Meeting canceled"
}

Правила:
- отмена разрешена, если до начала >= 10 минут

200 OK:
{
  "booking_id": "uuid",
  "status": "CANCELLED"
}

403 Forbidden (слишком поздно):
{
  "error": "CANCEL_FORBIDDEN",
  "message": "Too late to cancel"
}

---

5) Админ: изменить статус переговорки (например, закрыть на ремонт)
PATCH /rooms/{room_id}

Request:
{
  "status": "UNAVAILABLE",
  "unavailable_reason": "Repair"
}

200 OK:
{
  "room_id": "uuid",
  "status": "UNAVAILABLE"
}
