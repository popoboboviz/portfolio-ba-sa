# API (draft) — Страница брендов одежды

## 1) Brand-service

### GET /brands
Получить список брендов.

Query params (опционально):
- search — поиск по названию (пример: nike)
- sort — сортировка (пример: name,asc)
- limit, offset — пагинация

200 OK (пример ответа):
[
  {
    "id": "b1",
    "name": "Nike",
    "logoUrl": "https://cdn.example.com/brands/nike.png",
    "collectionId": "c10"
  }
]

Ошибки:
- 503 Service Unavailable — brand-service недоступен

---

## 2) Item-service

### GET /items?collectionId={collectionId}
Получить товары и баннеры бренда по коллекции.

200 OK (пример ответа):
{
  "items": [
    { "id": "i1", "title": "T-shirt", "price": 1990 }
  ],
  "banners": [
    { "id": "bn1", "imageUrl": "https://cdn.example.com/banners/bn1.png" }
  ]
}

Ошибки:
- 503 Service Unavailable — item-service недоступен

