# Radar Legislacyjny

Interaktywna platforma do śledzenia procesów legislacyjnych z wbudowanym systemem konsultacji publicznych.

## Funkcjonalności

### Dla Obywateli
- 📋 Przeglądanie aktów prawnych i ich statusu
- 🔔 Subskrybowanie kategorii i projektów
- 💬 Dodawanie opinii w okresie konsultacji publicznych
- 📊 Przeglądanie własnych opinii i postępu procedury

### Dla Urzędników
- 📝 Tworzenie i edycja projektów aktów
- 👀 Przeglądanie wniosków od obywateli
- ✅ Zatwierdzanie opinii do konsultacji publicznych
- 📊 Zarządzanie postępem legislacyjnym

### Dla Administratorów
- 👥 **Zarządzanie kontami urzędników** (dodawanie, edytowanie, usuwanie)
- 🔐 **Generowanie bezpiecznych haseł** (12-znakowe z literami, cyframi, symbolami)
- 📊 **Statystyki systemowe** (liczba aktywnych urzędników, total projektów)
- 🔍 **Wyszukiwanie i filtrowanie** accounts w real-time
- 📋 **Kopiowanie danych do schowka** (email, hasło)
- 🔄 **Zarządzanie statusem** (aktywacja/deaktywacja kont)
- ⚙️ Administracja systemem
- 📋 Pełny dostęp do wszystkich funkcji

## Technologie

- **Frontend**: React + TypeScript + Vite
- **Styling**: Tailwind CSS + shadcn-ui
- **State Management**: React Context
- **Routing**: React Router v6
- **API Client**: TODO (do implementacji)

## Logowanie

### Obywatel
- Email: `obywatel@example.com`
- Hasło: `obywatel123`
- Tab logowania: **Obywatel**

### Urzędnik
- Email: `urzednik@gov.pl`
- Hasło: `urzednik123`
- Tab logowania: **Urzędnik**

### Administrator
- Email: `admin@gov.pl`
- Hasło: `admin123`
- Tab logowania: **Administrator**
- Dostęp: `/admin-zarzadzanie` - Panel zarządzania kontami urzędników

## Instalacja i uruchomienie

```sh
# Klonowanie repozytorium
git clone <URL_REPOZYTORIUM>
cd 4night

# Instalacja zależności
npm install

# Uruchomienie serwera deweloperskiego
npm run dev

# Build do produkcji
npm run build

# Podgląd build'u
npm run preview
```

## Struktura projektu

```
src/
├── components/
│   ├── acts/               # Komponenty powiązane z aktami
│   ├── home/              # Komponenty strony głównej
│   ├── layout/            # Layout i nagłówek
│   └── ui/                # Komponenty UI z shadcn-ui
├── contexts/              # React Context (Auth, Accessibility)
├── data/
│   ├── mockData.ts        # Dane mock'owe aktów
│   └── testData.ts        # Dane testowe do logowania
├── hooks/                 # Custom React hooks
├── lib/                   # Utility functions
├── pages/                 # Strony aplikacji
│   ├── Index.tsx          # Strona główna
│   ├── ActDetailPage.tsx  # Szczegóły aktu
│   ├── AllActsPage.tsx    # Lista wszystkich aktów
│   ├── EditorPage.tsx     # Edytor dla urzędników
│   ├── CitizenProfilePage.tsx    # Profil obywatela
│   ├── OfficerProjectsPage.tsx    # Projekty urzędnika
│   └── AdminManagementPage.tsx    # Zarządzanie kontami
└── App.tsx                # Główny komponent aplikacji
```

## Routing

| Ścieżka | Rola | Opis |
|--------|------|------|
| `/` | Wszyscy | Strona główna |
| `/wszystkie` | Wszyscy | Lista wszystkich aktów |
| `/akt/:id` | Wszyscy | Szczegóły aktu z konsultacjami |
| `/kategoria/:id` | Wszyscy | Akty w kategorii |
| `/edytor` | officer, admin | Tworzenie nowych aktów |
| `/moje-projekty` | officer, admin | Projekty utworzone przez urzędnika |
| `/obywatel` | citizen | Profil obywatela - subskrypcje i śledzenie |
| `/admin-zarzadzanie` | admin | Zarządzanie kontami urzędników |

## Integracja z Backendem

Poniżej znajduje się lista endpointów, które muszą być zaimplementowane w backendzie:

### 🔐 Autentykacja

**`POST /api/auth/login`**
- Logowanie użytkownika
- Request: `{ email: string, password: string, role: "citizen" | "officer" | "admin" }`
- Response: `{ success: boolean, user: User, token: string }`
- Lokalizacja kodu: `src/contexts/AuthContext.tsx:25-50`

**`POST /api/auth/logout`**
- Wylogowanie użytkownika
- Lokalizacja kodu: `src/contexts/AuthContext.tsx:62-66`

**`GET /api/auth/validate`**
- Weryfikacja tokenu
- Response: `{ valid: boolean, user: User }`

### 👤 Użytkownicy

**`GET /api/users/profile`**
- Pobranie profilu zalogowanego użytkownika
- Response: `User`
- Lokalizacja kodu: `src/contexts/AuthContext.tsx:25-50`

**`PUT /api/users/profile`**
- Aktualizacja profilu użytkownika
- Request: `Partial<User>`
- Response: `User`

**`GET /api/users/officers`** (admin)
- Lista wszystkich urzędników
- Response: `User[]`
- Lokalizacja kodu: `src/pages/AdminManagementPage.tsx:160-200`

**`POST /api/users/officers`** (admin)
- Tworzenie nowego konta urzędnika
- Request: `{ email: string, name: string, role: "officer" | "admin", status: "active" | "inactive" }`
- Response: `{ user: User, password: string }` (hasło wygenerowane na serwerze)
- Lokalizacja kodu: `src/pages/AdminManagementPage.tsx:70-130`
- Walidacja: Email unikalny, format email, wymagane pola

**`PUT /api/users/officers/:id`** (admin)
- Edycja danych urzędnika
- Request: `{ name?: string, status?: "active" | "inactive", role?: "officer" | "admin" }`
- Response: `User`
- Lokalizacja kodu: `src/pages/AdminManagementPage.tsx:140-155`

**`DELETE /api/users/officers/:id`** (admin)
- Usunięcie konta urzędnika
- Response: `{ success: boolean }`
- Lokalizacja kodu: `src/pages/AdminManagementPage.tsx:180-195`

**`POST /api/users/officers/:id/reset-password`** (admin)
- Zresetowanie hasła urzędnika
- Response: `{ password: string }` (nowe hasło)
- Lokalizacja kodu: `src/pages/AdminManagementPage.tsx:220-240`

### 📋 Akty Prawne

**`GET /api/acts`**
- Lista wszystkich aktów
- Query params: `?category=&status=&search=`
- Response: `LegislativeAct[]`
- Lokalizacja kodu: `src/pages/AllActsPage.tsx`, `src/pages/CategoryPage.tsx`

**`GET /api/acts/:id`**
- Szczegóły aktu
- Response: `LegislativeAct`
- Lokalizacja kodu: `src/pages/ActDetailPage.tsx`

**`POST /api/acts`** (officer, admin)
- Tworzenie nowego aktu
- Request: `LegislativeAct`
- Response: `LegislativeAct`
- Lokalizacja kodu: `src/pages/EditorPage.tsx:40-150`

**`PUT /api/acts/:id`** (officer, admin)
- Edycja aktu
- Request: `Partial<LegislativeAct>`
- Response: `LegislativeAct`

**`DELETE /api/acts/:id`** (officer, admin)
- Usunięcie aktu
- Response: `{ success: boolean }`

**`GET /api/acts/officer/:officerId`** (officer, admin)
- Akty utworzone przez urzędnika
- Response: `LegislativeAct[]`
- Lokalizacja kodu: `src/pages/OfficerProjectsPage.tsx:20-22`

### 🔔 Subskrypcje

**`GET /api/subscriptions`**
- Pobranie subskrypcji zalogowanego użytkownika
- Response: `{ categoryIds: string[], actIds: string[] }`
- Lokalizacja kodu: `src/pages/CitizenProfilePage.tsx:40-45`

**`POST /api/subscriptions`**
- Dodanie subskrypcji
- Request: `{ categoryIds: string[], actIds: string[] }`
- Response: `{ success: boolean }`
- Lokalizacja kodu: `src/contexts/AuthContext.tsx:70-87`

**`DELETE /api/subscriptions/:id`**
- Usunięcie subskrypcji
- Response: `{ success: boolean }`

### 💬 Komentarze (Konsultacje Publiczne)

**`GET /api/acts/:actId/comments`**
- Lista komentarzy do aktu
- Query params: `?approved=true` (dla urzędników)
- Response: `Comment[]`
- Lokalizacja kodu: `src/components/acts/ActComments.tsx:30-60`

**`POST /api/acts/:actId/comments`** (citizen)
- Dodanie komentarza/opinii
- Request: `{ content: string }`
- Response: `Comment` (ze statusem pending)
- Lokalizacja kodu: `src/components/acts/ActComments.tsx:90-130`

**`PATCH /api/comments/:id/approve`** (officer, admin)
- Zatwierdzenie komentarza
- Response: `Comment` (ze statusem approved)
- Lokalizacja kodu: `src/components/acts/ActComments.tsx:140-150`

**`DELETE /api/comments/:id`** (officer, admin)
- Usunięcie komentarza
- Response: `{ success: boolean }`
- Lokalizacja kodu: `src/components/acts/ActComments.tsx:155-170`

## Zmienne Środowiskowe

Stwórz plik `.env` w głównym katalogu:

```env
VITE_API_URL=http://localhost:3000/api
VITE_APP_NAME=4Night - Radar Legislacyjny
```

## Notatki dla Developera

### Admin Panel (AdminManagementPage)
- **Lokalizacja**: `/admin-zarzadzanie`
- **Dostęp**: Tylko dla ról `admin`
- **Funkcje**:
  - Dodawanie nowych urzędników/administratorów
  - Edytowanie danych (nazwa, rola, status)
  - Usuwanie kont
  - Wyszukiwanie real-time po email/nazwisko
  - Filtrowanie po statusie (aktywni/nieaktywni)
  - Kopiowanie do schowka (email, hasło)
  - Wyświetlanie statystyk
  - Tabela z kolumnami: Email, Nazwa, Rola, Status, Data Utworzenia, Akcje

### Generowanie Haseł
- Hasła są generowane na frontendzie w trybie testowym
- Format: 12 znaków, mieszanka A-Z, a-z, 0-9, !@#$%
- Na produkcji: Backend powinien generować hasła bezpośrednio
- Hasła powinny być wysyłane na email zaraz po utworzeniu

### Logowanie (Header Component)
- **Tabs**: Obywatel | Urzędnik | Administrator
- Każda rola ma oddzielne pole logowania
- Dynamiczny handleLogin na podstawie activeLoginTab
- Wyświetlanie danych testowych dla każdej roli

### Autentykacja
1. **Mock Data**: Aktualnie aplikacja używa danych mock'owych z `src/data/mockData.ts`
2. **AuthContext**: Waliduje dane testowe. Przed podłączeniem backendu zakomentuj linię walidacji w `AuthContext.tsx:32-45` i odkomentuj sekcję z TODO
3. **Token Storage**: Token powinien być przechowywany w `localStorage` pod kluczem `token`
4. **CORS**: Backend powinien obsługiwać CORS dla developmentu
5. **Błędy**: Użyj biblioteki `sonner` do wyświetlania notyfikacji (już zaimplementowana)

### Role-Based Access Control (RBAC)
- **citizen**: `/`, `/wszystkie`, `/akt/:id`, `/kategoria/:id`, `/obywatel`
- **officer**: citizen routes + `/edytor`, `/moje-projekty`
- **admin**: officer routes + `/admin-zarzadzanie`

## TODO - Integracja Backendu

- [ ] Podłączenie API do autentykacji
- [ ] Podłączenie API do pobierania aktów
- [ ] Podłączenie API do tworzenia/edycji aktów
- [ ] Podłączenie API do komentarzy
- [ ] Podłączenie API do subskrypcji
- [ ] Podłączenie API do zarządzania kontami (GET, POST, PUT, DELETE, password reset)
- [ ] Migracja danych testowych do bazy danych
- [ ] Implementacja refresh tokena
- [ ] Handling błędów API
- [ ] Optimistic updates
- [ ] Caching responses
- [ ] Email notifications na utworzenie nowego konta
- [ ] Audit logging dla akcji admina
- [ ] 2FA/MFA dla administratorów

## Build i Deployment

```sh
# Production build
npm run build

# Podgląd build'u lokalnie
npm run preview

# Linting
npm run lint
```

## Licencja

Projekt opracowany dla HackNations 2025.
