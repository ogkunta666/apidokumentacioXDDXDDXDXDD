# Reservation API Documentation

## Bevezetés

Ez a dokumentáció a Reservation API végpontjait ismerteti. Az API RESTful elveket követve lehetővé teszi foglalások kezelését, valamint felhasználói regisztrációt és bejelentkezést.

**Base URL:** `{{localhost/reservationKisuj/public/api}}`

---

## Tartalomjegyzék

1. [Autentikáció nélküli végpontok](#autentikáció-nélküli-végpontok)
   - [Teszt végpont](#teszt-végpont)
   - [Regisztráció](#regisztráció)
   - [Bejelentkezés](#bejelentkezés)
2. [Autentikált végpontok](#autentikált-végpontok)
   - [Kijelentkezés](#kijelentkezés)
   - [Összes foglalás lekérése](#összes-foglalás-lekérése)
   - [Egy foglalás lekérése](#egy-foglalás-lekérése)
   - [Új foglalás létrehozása](#új-foglalás-létrehozása)
   - [Foglalás teljes frissítése](#foglalás-teljes-frissítése)
   - [Foglalás részleges frissítése](#foglalás-részleges-frissítése)
   - [Foglalás törlése](#foglalás-törlése)

---

## Autentikáció nélküli végpontok

### Teszt végpont

Egyszerű teszt végpont az API elérhetőségének ellenőrzésére.

**Endpoint:** `GET /hello`

#### Headers
Nincsenek specifikus header-ök szükségesek.

#### Request Body
Nincs request body.

#### Válasz
- **Status Code:** `200 OK`
- **Response Body:** Üdvözlő üzenet

---

### Regisztráció

Új felhasználó regisztrálása az alkalmazásban.

**Endpoint:** `POST /register`

#### Headers
```
Accept: application/json
Content-Type: application/json
```

#### Request Body
```json
{
  "name": "Pelda Janos",
  "email": "peldajanos@example.com",
  "password": "Super_Secure_Password2025",
  "password_confirmation": "Super_Secure_Password2025"
}
```

**Mező leírások:**
- `name` (string, kötelező): A felhasználó neve
- `email` (string, kötelező): Egyedi email cím
- `password` (string, kötelező): Jelszó (min. 8 karakter ajánlott)
- `password_confirmation` (string, kötelező): Jelszó megerősítése (meg kell egyeznie a `password`-del)

#### Válasz
- **Status Code:** `201 Created` vagy `200 OK`
- **Response Body:** A létrehozott felhasználó adatai és autentikációs token

---

### Bejelentkezés

Bejelentkezés egy meglévő felhasználói fiókkal.

**Endpoint:** `POST /login`

#### Headers
```
Accept: application/json
Content-Type: application/json
```

#### Request Body
```json
{
  "email": "peldajanos@example.com",
  "password": "Super_Secure_Password2025"
}
```

**Mező leírások:**
- `email` (string, kötelező): A felhasználó email címe
- `password` (string, kötelező): A felhasználó jelszava

#### Válasz
- **Status Code:** `200 OK`
- **Response Body:** 
```json
{
  "token": "2|GQKKHAdOHkS1x7OOHULgdUQEzWbupd8pSsBCT5im5fdc7412",
  "user": {
    "id": 1,
    "name": "John Doe",
    "email": "John.Doe@example.com"
  }
}
```

**Fontos:** A visszakapott `token` értéket használd az autentikált végpontok eléréséhez.

---

## Autentikált végpontok

Az alábbi végpontok használatához Bearer token autentikáció szükséges. A tokent az `Authorization` headerben kell elküldeni.

**Példa Authorization header:**
```
Authorization: Bearer 2|dvoJA10sB2ABXjmaqF4YHAKXkpVgXJLlJaPdOWhUce01a0e3
```

---

### Kijelentkezés

Aktuális felhasználói munkamenet lezárása és a token invalidálása.

**Endpoint:** `POST /logout`

#### Headers
```
Accept: application/json
Content-Type: application/json
Authorization: Bearer {your_token}
```

#### Request Body
```json
{
  "email": "peldajanos@example.com",
  "password": "Super_Secure_Password2025"
}
```

**Megjegyzés:** A request body tartalma nem szükséges a kijelentkezéshez, elég a Bearer token.

#### Válasz
- **Status Code:** `200 OK`
- **Response Body:** Sikerességet jelző üzenet

---

### Összes foglalás lekérése

Az összes foglalás lekérése a rendszerből.

**Endpoint:** `GET /reservations`

#### Headers
```
Accept: application/json
Content-Type: application/json
Authorization: Bearer {your_token}
```

#### Request Body
Nincs request body.

#### Válasz
- **Status Code:** `200 OK`
- **Response Body:** 
```json
[
  {
    "id": 1,
    "reservation_time": "2025-10-02T18:43:30",
    "guests": 4,
    "note": "Lorem ipsum dolor sit amet",
    "user_id": 1,
    "created_at": "2025-11-20T10:30:00",
    "updated_at": "2025-11-20T10:30:00"
  },
  {
    "id": 2,
    "reservation_time": "2025-10-03T19:00:00",
    "guests": 2,
    "note": "Ablak melletti asztal kérése",
    "user_id": 1,
    "created_at": "2025-11-21T14:20:00",
    "updated_at": "2025-11-21T14:20:00"
  }
]
```

---

### Egy foglalás lekérése

Egyetlen foglalás részletes adatainak lekérése ID alapján.

**Endpoint:** `GET /reservations/{id}`

**Példa:** `GET /reservations/2`

#### Headers
```
Accept: application/json
Content-Type: application/json
Authorization: Bearer {your_token}
```

#### Request Body
Nincs request body.

#### Válasz
- **Status Code:** `200 OK` vagy `201 Created`
- **Response Body:** 
```json
{
  "id": 2,
  "reservation_time": "2025-10-03T19:00:00",
  "guests": 2,
  "note": "Ablak melletti asztal kérése",
  "user_id": 1,
  "created_at": "2025-11-21T14:20:00",
  "updated_at": "2025-11-21T14:20:00"
}
```

---

### Új foglalás létrehozása

Új foglalás rögzítése a rendszerben.

**Endpoint:** `POST /reservations`

#### Headers
```
Accept: application/json
Content-Type: application/json
Authorization: Bearer {your_token}
```

#### Request Body
```json
{
  "reservation_time": "2025-10-02 18:43:30",
  "guests": "4",
  "note": "Lorem Ipsum dolor sit amet"
}
```

**Mező leírások:**
- `reservation_time` (string/datetime, kötelező): A foglalás időpontja (formátum: `YYYY-MM-DD HH:MM:SS`)
- `guests` (string/integer, kötelező): Vendégek száma
- `note` (string, opcionális): Megjegyzés a foglaláshoz

#### Válasz
- **Status Code:** `200 OK`, `201 Created`, vagy `204 No Content`
- **Response Body:** 
```json
{
  "id": 11,
  "reservation_time": "2025-10-02T18:43:30",
  "guests": 4,
  "note": "Lorem ipsum dolor sit amet",
  "user_id": 1,
  "created_at": "2025-11-26T10:30:00",
  "updated_at": "2025-11-26T10:30:00"
}
```

---

### Foglalás teljes frissítése

Egy meglévő foglalás összes adatának felülírása (PUT metódus).

**Endpoint:** `PUT /reservations/{id}`

**Példa:** `PUT /reservations/11`

#### Headers
```
Accept: application/json
Content-Type: application/json
Authorization: Bearer {your_token}
```

#### Request Body
```json
{
  "reservation_time": "2025-10-02 18:43:30",
  "guests": "4",
  "note": "Lorem ipsum dolor sit amet"
}
```

**Mező leírások:**
- `reservation_time` (string/datetime, kötelező): A foglalás időpontja
- `guests` (string/integer, kötelező): Vendégek száma
- `note` (string, kötelező): Megjegyzés a foglaláshoz

**Fontos:** PUT kérésnél minden mezőt meg kell adni, mert felülírja a teljes erőforrást.

#### Válasz
- **Status Code:** `200 OK`
- **Response Body:** 
```json
{
  "id": 11,
  "reservation_time": "2025-10-02T18:43:30",
  "guests": 4,
  "note": "Lorem ipsum dolor sit amit",
  "user_id": 1,
  "created_at": "2025-11-26T10:30:00",
  "updated_at": "2025-11-26T12:45:00"
}
```

---

### Foglalás részleges frissítése

Egy meglévő foglalás egyes mezőinek frissítése (PATCH metódus).

**Endpoint:** `PATCH /reservations/{id}`

**Példa:** `PATCH /reservations/11`

#### Headers
```
Accept: application/json
Content-Type: application/json
Authorization: Bearer {your_token}
```

#### Request Body
```json
{
  "note": "Something smart."
}
```

**Fontos:** PATCH kérésnél csak azokat a mezőket kell megadni, amelyeket frissíteni szeretnél. A többi mező változatlan marad.

**Frissíthető mezők:**
- `reservation_time` (string/datetime, opcionális)
- `guests` (string/integer, opcionális)
- `note` (string, opcionális)

#### Válasz
- **Status Code:** `200 OK`
- **Response Body:** 
```json
{
  "id": 11,
  "reservation_time": "2025-10-02T18:43:30",
  "guests": 4,
  "note": "Lorem ipsum dolor sit amet",
  "user_id": 1,
  "created_at": "2025-11-26T10:30:00",
  "updated_at": "2025-11-26T13:20:00"
}
```

---

### Foglalás törlése

Egy foglalás végleges törlése a rendszerből.

**Endpoint:** `DELETE /reservations/{id}`

**Példa:** `DELETE /reservations/11`

#### Headers
```
Accept: application/json
Content-Type: application/json
Authorization: Bearer {your_token}
```

#### Request Body
Nincs request body.

#### Válasz
- **Status Code:** `200 OK` vagy `204 No Content`
- **Response Body:** 
```json
{
  "message": "Reservation deleted successfully"
}
```

---

## Hibakezelés

Az API szabványos HTTP státuszkódokat használ a hibák jelzésére:

- **400 Bad Request:** Hibás kérés (pl. hiányzó kötelező mező)
- **401 Unauthorized:** Hiányzó vagy érvénytelen autentikációs token
- **403 Forbidden:** Nincs jogosultság a kért művelethez
- **404 Not Found:** A keresett erőforrás nem található
- **422 Unprocessable Entity:** Validációs hiba
- **500 Internal Server Error:** Szerver oldali hiba

### Példa hibaüzenet:
```json
{
  "message": "The given data was invalid.",
  "errors": {
    "email": [
      "The email field is required."
    ],
    "password": [
      "The password must be at least 8 characters."
    ]
  }
}
```

---

## Használati példák

### 1. Teljes munkafolyamat: Regisztráció és foglalás létrehozása

```bash
# 1. Regisztráció
curl -X POST {{base_url}}/register \
  -H "Accept: application/json" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Teszt Felhasználó",
    "email": "teszt@example.com",
    "password": "JelszoTest123",
    "password_confirmation": "JelszoTest123"
  }'

# 2. Bejelentkezés
curl -X POST {{base_url}}/login \
  -H "Accept: application/json" \
  -H "Content-Type: application/json" \
  -d '{
    "email": "teszt@example.com",
    "password": "JelszoTest123"
  }'

# Válasz: { "token": "YOUR_TOKEN_HERE", ... }

# 3. Új foglalás létrehozása (használd a kapott tokent)
curl -X POST {{base_url}}/reservations \
  -H "Accept: application/json" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_TOKEN_HERE" \
  -d '{
    "reservation_time": "2025-12-01 19:00:00",
    "guests": "6",
    "note": "Születésnapi vacsora"
  }'
```

### 2. Foglalás módosítása

```bash
# Részleges frissítés (PATCH)
curl -X PATCH {{base_url}}/reservations/1 \
  -H "Accept: application/json" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_TOKEN_HERE" \
  -d '{
    "guests": "8"
  }'
```

---

## Megjegyzések

1. **Token kezelés:** A bejelentkezéskor kapott tokent biztonságosan tárold. Minden autentikált kéréshez csatold az `Authorization` headerben.

2. **Dátum formátum:** Az `reservation_time` mező esetén használj `YYYY-MM-DD HH:MM:SS` formátumot.

3. **PUT vs PATCH:** 
   - **PUT:** Teljes erőforrás-frissítés, minden mezőt meg kell adni
   - **PATCH:** Részleges frissítés, csak a módosítani kívánt mezőket add meg

4. **Vendégek száma:** Bár a példákban string formátumban van, ajánlott integer típusként küldeni.


