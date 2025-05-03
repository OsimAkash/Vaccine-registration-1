# Vaccine Registration System

This application is built using **Laravel 11.x** and provides a streamlined way to manage vaccination schedules. Users can register, select a vaccine center, and have their vaccination dates scheduled automatically. The project also includes an admin panel for managing user data and filtering based on vaccine status and centers.

## Features

### User Registration
- Users can register with the following details:
    - NID (National ID)
    - Email
    - Phone Number
    - Name
    - Vaccine Center selection

### Vaccine Centers
- Vaccine centers can only be added via Seeders.
- No CRUD operations are implemented for vaccine centers.
- Each vaccine center has a defined daily limit for scheduling vaccinations.

### Scheduling Vaccination Dates
- The system schedules vaccinations daily at **9 PM**.
- Users are scheduled based on first-come, first-served priority (registration time).
- Scheduling respects the daily limit set for each vaccine center.
- Scheduling skips weekends (Vaccination allowed only **Sunday to Thursday**).
- Email notifications are sent to users asynchronously via a queued job.

### User Status
- **Not Scheduled**: User has registered but not yet scheduled for vaccination.
- **Scheduled**: User has been scheduled for vaccination with date visible.
- **Vaccinated**: When the scheduled time passes, the user's status is automatically updated to Vaccinated with date.

### Admin Panel
An admin panel is implemented using **FilamentPHP**.

#### Features of the admin panel:
- Display a list of users.
- Filters (Multiple allowed):
    - Based on status (Not Scheduled, Scheduled, Vaccinated).
    - Based on vaccine center.

## Technical Details

### Key Technologies
- Laravel 11.x
- FilamentPHP for admin panel
- Email Notifications with queued jobs
- Seeders for preloading vaccine centers

### Scheduling Workflow
- A scheduled task runs daily at **9 PM** using Laravel's task scheduling.
- Users are selected in the order of registration, respecting the daily limit of their chosen vaccine center.
- Email notifications are sent asynchronously to notify users of their scheduled vaccination date.
- Weekend dates (**Sunday to Thursday**) are skipped automatically.

### Database Schema

#### Users Table
- `id`
- `nid`
- `email`
- `password` (hashed)
- `phone`
- `name`
- `vaccine_center_id`
- `status` (Not Scheduled, Scheduled, Vaccinated)
- `scheduled_at`
- `is_admin` (default: false)

#### Vaccine Centers Table
- `id`
- `name`
- `daily_limit`

**Admin is generated using seeder:**
- Admin Email: `admin@vaccine.com`
- Password: `password`

**Note**: By default, 1000 users and 5 vaccine centers are created. If you need to modify, check `UserSeeder.php` and `VaccineCenterSeeder.php`.

## Installation

### Prerequisites
- PHP 8.2+
- Laravel 11.x
- MySQL 8.0+ or SQLite 3.25+
- Composer
- Node.js and npm (for frontend assets, if required)
- Configure a mail server (e.g., Mailpit for local or SMTP for production) for email notification.

### Steps
1. Clone the repository:
     ```bash
     git clone https://github.com/OsimAkash/vaccine-registration-1.git
     cd vaccine-registration-system
     ```

2. Install dependencies:
     ```bash
     composer install
     npm install
     ```

3. Configure environment variables:
     ```bash
     cp .env.example .env
     php artisan key:generate
     ```

4. Update `.env`:
     - Update database settings.
     - Configure mail settings.

5. Run migrations and seed the database:
     ```bash
     php artisan migrate
     php artisan db:seed
     ```

6. Start the development server:
     ```bash
     php artisan serve
     ```

7. Start asset compilation:
     ```bash
     npm run dev
     ```

8. Run the queue worker:
     ```bash
     php artisan queue:listen
     ```

9. Add cron configuration to run the scheduler at regular intervals:
     ```bash
     * * * * * cd /path-to-your-project && php artisan schedule:run >> /dev/null 2>&1
     ```

10. Start the schedule worker:
        ```bash
        php artisan schedule:work
        ```

## Usage

### User Registration
- Visit the registration page.
- Fill in the required details and select a vaccine center.

### Admin Panel
- Access the admin panel at `/admin`.
- Use filters to manage user lists effectively.

### Scheduling Vaccinations
- Vaccination dates are scheduled automatically each day at **9 PM**.

## Contribution
Feel free to fork this repository and submit pull requests. For major changes, please open an issue first to discuss what you would like to change.

## Acknowledgments
Thanks to the Laravel and FilamentPHP communities for their excellent frameworks and tools.
