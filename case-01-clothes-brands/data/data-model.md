# Data model (минимально)

## Entities

Brand:
- id
- name (unique)
- logoUrl
- collectionId
- isActive (optional)

Collection:
- id
- title

Item:
- id
- collectionId
- title
- price

Banner:
- id
- brandId
- imageUrl
- sortOrder (optional)

## Relations
- Brand 1 — 1 Collection
- Collection 1 — N Item
- Brand 1 — N Banner
