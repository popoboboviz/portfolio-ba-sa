# Модель данных — Бронирование переговорки

## Сущности

### Room (Переговорка)
- `room_id` (UUID)
- `name` (string)
- `capacity` (int)
- `location` (string, опционально: этаж/крыло)
- `equipment` (array[string] или отдельная таблица)
- `status` (enum): `AVAILABLE`, `UNAVAILABLE`
- `unavailable_reason` (string, nullable)
- `created_at` (datetime)
- `updated_at` (datetime)

### Booking (Бронь)
- `booking_id` (UUID)
- `room_id` (UUID, FK -> Room)
- `created_by` (string/UUID) — сотрудник
- `title` (string, nullable)
- `start_at` (datetime)
- `end_at` (datetime)
- `status` (enum): `BOOKED`, `CANCELLED`
- `created_at` (datetime)
- `updated_at` (datetime)
- `cancelled_at` (datetime, nullable)
- `cancel_reason` (string, nullable)

### BookingParticipant (Участники брони)
- `booking_id` (UUID, FK -> Booking)
- `participant` (string) — email/логин сотрудника
- `role` (enum, optional): `ORGANIZER`, `ATTENDEE`

## Ограничения/правила на данных
- Для одной `room_id` интервалы бронирований со статусом `BOOKED` не должны пересекаться.
- `end_at` > `start_at`.
- Максимальная длительность брони: 2 часа (правило уровня приложения).
- Ограничение активных броней на пользователя: не более 3 (правило уровня приложения).
- Отмена разрешена, если до `start_at` >= 10 минут (правило уровня приложения).
