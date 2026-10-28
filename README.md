# RideQuetta — Rickshaw Booking Web Application

RideQuetta is a responsive, lightweight web application for rickshaw booking in Quetta, Balochistan, Pakistan. Built using pure **HTML5**, **CSS3**, and **Vanilla JavaScript**, it runs directly in any modern browser without requiring `npm`, Node.js runtimes, or compilation steps.

---

## 🚀 Live Demo & Structure

- **Main File**: [`index.html`](index.html) — contains all markup, CSS styles, and client-side application logic.
- **External CDN Dependencies**:
  - Google Fonts (`Plus Jakarta Sans`)
  - Leaflet.js (`https://unpkg.com/leaflet@1.9.4`) for OpenStreetMap route visualization (with graceful offline fallback).

---

## ✨ Features Implemented

1. **Brand Identity & Header**:
   - Modern transportation startup aesthetic matching the original design.
   - Sticky header with glassmorphism blur backdrop.
   - Responsive navigation menu and mobile drawer.
   - Quick "Track Ride" status lookup and "Get started" CTAs.

2. **Hero Section & Quick Booking Widget**:
   - Quetta location badge: `• BUILT FOR QUETTA`.
   - Custom SVG illustration of a traditional Pakistani auto-rickshaw set against Quetta's iconic mountain silhouette (Koh-e-Murdar & Chiltan).
   - Floating Quick Booking Widget with real Quetta location autocomplete and instant ride-type switching.
   - Honest early-stage value highlights replacing unverified statistics:
     - Transparent Fares
     - Quetta-wide Local Focus
     - Direct / Cash Payment

3. **Popular Areas We Serve**:
   - 12 interactive cards representing real Quetta localities:
     *Jinnah Town, Satellite Town, Hazara Town, Brewery Road, Sariab Road, Quetta Cantt, Spinny Road, Sariab Phatak, Airport Road, Liaquat Bazaar, Kasi Road, Quarry Road*.
   - Each card features a one-click **"Set as Pickup"** action that autofills the booking form and smoothly scrolls to it.

4. **Transparent Fare Estimation Engine**:
   - Calibrated distance calculation for Quetta's road network using real geographical coordinates and a terrain curvature factor.
   - Real-time dynamic updates based on selected ride type:
     - **Standard**: Private rickshaw ride (Base PKR 80, PKR 35/km, Min PKR 120).
     - **Shared**: Route-sharing passenger option (Base PKR 80, 35% discount, Min PKR 80).
     - **Premium**: Enhanced comfort (Base PKR 80, 40% premium, Min PKR 180).
   - Clear disclaimer: *"Estimated fare only. Final fare may vary after route and service confirmation."*

5. **Booking Flow & Lifecycle**:
   - Autocomplete dropdown covering 20+ prominent Quetta landmarks and custom street addresses.
   - Pakistani mobile format validation (`03XX XXXXXXX` / `+92 3XX XXXXXXX`).
   - Pre-submission **Booking Review Modal** to confirm ride details and pricing.
   - Automatic generation of unique booking references (e.g., `RQ-849201`).
   - Initial honest status: `Request submitted — Awaiting confirmation`.
   - One-tap WhatsApp booking share button.

6. **Interactive Map**:
   - OpenStreetMap via Leaflet centered on Quetta (`30.1798° N, 66.9750° E`).
   - Custom pickup (P) and destination (D) markers with dashed route connector.
   - Switchable toggle between "Live Map" and "Dispatch View" (matching the original dotted card design).

7. **Drive With RideQuetta**:
   - Driver value proposition cards (Higher Earnings, Weekly Payouts, Safety & Support, 24/7 Support).
   - Complete Driver Registration form with vehicle type selection, licence status, service area checkboxes, and unique registration ID generation (`DRV-XXXXX`).

8. **Honest Mission Section ("What RideQuetta Aims to Improve")**:
   - Early-stage startup focus addressing real Quetta transit challenges instead of fake testimonials.

9. **Mobile App Announcement & Modals**:
   - "RideQuetta Is Starting Here" roadmap with iOS and Android alert modals.
   - Comprehensive modals for FAQs, Privacy Policy, Terms & Conditions, and Backend Configuration.

---

## 🛠️ How to Run Locally

You do not need to install Node.js, npm, or any build tool:

1. Clone or download this folder to your machine.
2. Double-click `index.html` to open it in Google Chrome, Microsoft Edge, Firefox, or Safari.
3. Alternatively, serve it via any static server:
   ```bash
   # Using Python 3:
   python -m http.server 8000

   # Or using npx:
   npx serve .
   ```
4. Open `http://localhost:8000` in your browser.

---

## 📦 GitHub & Vercel Deployment Instructions

Because RideQuetta is a pure static single-page website, deploying it is instantaneous and 100% free.

### Step 1: Create a GitHub Repository
1. Log in to [GitHub](https://github.com/).
2. Click the **+** icon in the top right corner and select **New repository**.
3. Name your repository (e.g., `ridequetta-web`).
4. Choose **Public** or **Private**, keep other options unchecked, and click **Create repository**.

### Step 2: Push `index.html` to GitHub
In your local project folder (`C:\Users\Hp\.gemini\antigravity\scratch\ridequetta`), run:
```bash
git init
git add index.html README.md
git commit -m "Initial commit: Complete RideQuetta static web app"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/ridequetta-web.git
git push -u origin main
```
*(Alternatively, you can upload `index.html` and `README.md` directly through GitHub's web interface via the "Upload files" button).*

### Step 3: Deploy to Vercel
1. Log in to [Vercel](https://vercel.com/) (sign in with your GitHub account).
2. Click **Add New...** &rarr; **Project**.
3. Find and select your `ridequetta-web` repository, then click **Import**.
4. In the **Configure Project** screen:
   - **Framework Preset**: Select `Other` (or leave default).
   - **Root Directory**: `./` (leave default).
   - **Build Command**: Leave empty / disabled (no build required).
   - **Output Directory**: Leave empty / disabled.
5. Click **Deploy**.
6. Within 10–20 seconds, your website will be live with a production HTTPS URL (e.g., `https://ridequetta-web.vercel.app/`).

---

## 🗄️ Backend Configuration (Supabase / REST API)

In the default static deployment, the application runs in **Demo Mode**, persisting bookings and driver registrations to browser `localStorage` and explicitly notifying users that rides are logged for demonstration.

### Connecting Supabase (Production Setup)
To connect a real Supabase backend:

1. Create a project at [supabase.com](https://supabase.com).
2. Create two tables in the SQL Editor:

```sql
-- Bookings Table
create table public.bookings (
  id uuid default gen_random_uuid() primary key,
  booking_reference text unique not null,
  pickup text not null,
  destination text not null,
  ride_type text not null,
  passenger_name text not null,
  phone text not null,
  notes text,
  estimated_fare text not null,
  status text default 'Request submitted — Awaiting confirmation',
  created_at timestamp with time zone default timezone('utc'::text, now()) not null
);

-- Driver Applications Table
create table public.driver_applications (
  id uuid default gen_random_uuid() primary key,
  driver_reference text unique not null,
  full_name text not null,
  phone text not null,
  cnic text,
  vehicle_plate text not null,
  vehicle_type text not null,
  licence_status text not null,
  submitted_at timestamp with time zone default timezone('utc'::text, now()) not null
);

-- Enable Row Level Security (RLS)
alter table public.bookings enable row level security;
alter table public.driver_applications enable row level security;

-- Allow anonymous inserts from web clients
create policy "Allow anonymous ride submissions" on public.bookings for insert with check (true);
create policy "Allow anonymous driver applications" on public.driver_applications for insert with check (true);
```

3. Open `index.html` and locate the configuration object around line 775:
```javascript
const RideQuettaConfig = {
  // ...
  supabase: {
    url: 'https://YOUR_PROJECT_ID.supabase.co',
    anonKey: 'YOUR_SUPABASE_ANON_PUBLIC_KEY'
  }
};
```
4. Insert your Supabase project URL and public Anon Key. The app will automatically post requests to your live database.

---

## ⚖️ License & Attribution
- Built for the people of Quetta, Balochistan, Pakistan.
- OpenStreetMap data &copy; [OpenStreetMap contributors](https://www.openstreetmap.org/copyright).
