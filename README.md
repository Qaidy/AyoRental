# AyoRental: Premium Car Marketplace & Rental

AyoRental is a single-page web app for browsing, listing, saving, and financing cars. It is built with plain HTML, JavaScript, and Tailwind CSS, with Supabase as the backend (auth and database).

## Features

- **Home page**: hero section, smart search widget (Search / Body Type tabs), featured listings, browse by brand (with logos) and by category, "Why AyoRental" section, testimonial carousel, and a financing banner.
- **Browse Cars**: dedicated catalog page with a sidebar filter (text search, make, body type, transmission, fuel, max price) and sorting (newest, price, year).
- **Listing detail**: large photo, specs, seller info, and a credit application button.
- **Authentication**: sign up and sign in with email and password via Supabase Auth.
- **Favorites**: logged-in users can save and remove cars, with a live counter badge.
- **Seller dashboard**: add new listings, view your own listings, and delete them.
- **Financing application**: pre-approval form saved to the database.
- **Testimonial carousel**: dots, swipe on mobile, and auto-slide.

## Tech Stack

| Layer | Technology |
|-------|------------|
| Markup / Logic | HTML5, Vanilla JavaScript (ES6+) |
| Styling | Tailwind CSS v3 (CDN) with a custom brand theme |
| Icons | Font Awesome 6 |
| Font | Inter (Google Fonts) |
| Backend | Supabase (PostgreSQL, Auth, Row Level Security) |
| Brand logos | [car-logos-dataset](https://github.com/filippofilip95/car-logos-dataset) via jsDelivr |
| Routing | Custom hash-based router (`#/`, `#/browse`, ...) |

## Project Structure

```
.
├── index.html     # The entire app (markup, styles, and logic)
├── favicon.svg    # Site icon
└── README.md
```

## Routes

| Route | Description |
|-------|-------------|
| `#/` | Home page |
| `#/browse` | Car catalog with filters |
| `#/listing/:id` | Listing detail page |
| `#/login` | Sign in / Sign up |
| `#/favorites` | Saved cars (login required) |
| `#/dashboard-seller` | Seller dashboard (login required) |

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/<your-username>/ayorental.git
cd ayorental
```

### 2. Set up Supabase

1. Create a project at [supabase.com](https://supabase.com).
2. Open the SQL Editor and create the tables below.
3. Copy your **Project URL** and **anon public key** from *Project Settings → API*.

### 3. Configure credentials

In `index.html`, find the following lines and replace them with your own values:

```javascript
const SUPABASE_URL = "https://YOUR-PROJECT.supabase.co";
const SUPABASE_ANON_KEY = "YOUR-ANON-PUBLIC-KEY";
```

### 4. Run locally

No build step is required. Open `index.html` directly, or serve it with any static server:

```bash
npx serve .
# or
python3 -m http.server 8000
```

Then visit `http://localhost:8000`.

## Database Schema

The schema below is based on the columns the app reads and writes. Adjust types and constraints to your needs.

```sql
-- User profiles (linked to Supabase Auth)
create table profiles (
  id uuid primary key references auth.users(id) on delete cascade,
  full_name text,
  avatar_url text
);

-- Car listings
create table cars (
  id uuid primary key default gen_random_uuid(),
  seller_id uuid references profiles(id) on delete cascade,
  make text not null,
  model text not null,
  year int not null,
  price numeric not null,
  mileage int not null,
  location text not null,
  transmission text,        -- 'Auto' | 'Manual'
  fuel_type text,           -- 'Gas' | 'Electric' | 'Hybrid'
  body_type text,           -- 'Sedan' | 'SUV' | 'Truck' | 'Coupe' | 'Hatchback' | 'Convertible'
  image_url text,
  created_at timestamptz default now()
);

-- Saved cars
create table favorites (
  user_id uuid references profiles(id) on delete cascade,
  car_id uuid references cars(id) on delete cascade,
  primary key (user_id, car_id)
);

-- Financing / credit applications
create table financing_applications (
  id uuid primary key default gen_random_uuid(),
  user_id uuid references profiles(id) on delete cascade,
  car_id uuid references cars(id) on delete set null,
  full_name text,
  email text,
  phone text,
  monthly_income numeric,
  status text default 'pending',
  created_at timestamptz default now()
);
```

### Auto-create a profile on sign up

The app reads from `profiles`, so create a trigger that inserts a row whenever a new user signs up:

```sql
create function public.handle_new_user()
returns trigger as $$
begin
  insert into public.profiles (id, full_name)
  values (new.id, new.raw_user_meta_data->>'full_name');
  return new;
end;
$$ language plpgsql security definer;

create trigger on_auth_user_created
  after insert on auth.users
  for each row execute procedure public.handle_new_user();
```

### Row Level Security (important)

The `anon` key is visible in the browser, so **RLS policies are what actually protect your data**. At minimum:

```sql
alter table profiles enable row level security;
alter table cars enable row level security;
alter table favorites enable row level security;
alter table financing_applications enable row level security;

-- Anyone can read listings and seller names
create policy "Public read cars" on cars for select using (true);
create policy "Public read profiles" on profiles for select using (true);

-- Sellers manage only their own listings
create policy "Insert own cars" on cars for insert with check (auth.uid() = seller_id);
create policy "Delete own cars" on cars for delete using (auth.uid() = seller_id);

-- Users manage only their own favorites
create policy "Own favorites" on favorites for all
  using (auth.uid() = user_id) with check (auth.uid() = user_id);

-- Users see and create only their own applications
create policy "Own applications select" on financing_applications for select using (auth.uid() = user_id);
create policy "Own applications insert" on financing_applications for insert with check (auth.uid() = user_id);
```

> **Note:** The sign-up flow assumes users are logged in immediately. In Supabase, go to *Authentication → Providers → Email* and disable **Confirm email**, or handle the email confirmation step in the app.

## Customization

- **Brand colors / fonts**: edit the `tailwind.config` block at the top of `index.html`.
- **Add a car brand**: add one entry to `brandList`, e.g. `{ name: 'Hyundai', label: 'Hyundai', slug: 'hyundai' }`. The `name` must match the `make` value stored in the database.
- **Add a testimonial**: duplicate a `.testimonial-slide` block. The slide count and dots update automatically.

## Known Limitations

- The whole app lives in one HTML file, so it is best suited to prototypes and small projects. Consider splitting into modules or moving to a framework as it grows.
- Tailwind is loaded via the CDN script, which is not recommended for production. Use the Tailwind CLI or a build step for a smaller, faster bundle.
- Filters on the home page only show the first 5 matching cars. Use the Browse page for full results.
- Car brand logos are trademarks of their respective owners. For commercial use, verify licensing or use official press-kit assets.
- The "Rental" part of the brand is not yet implemented. The current flow covers buying, selling, and financing.

## Roadmap Ideas

- Image upload via Supabase Storage (instead of an image URL)
- Rental booking flow with date picker and availability calendar
- Edit listings in the seller dashboard
- Payment calculator for monthly installments
- Multi-language support (EN / ID)

## License

This project is licensed under the MIT License. See `LICENSE` for details.