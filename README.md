# B2Gov Project Dashboard

Full-stack Laravel 13 application with an Inertia React frontend. The Laravel backend handles authentication, project records, service points, agencies, and document storage; React retains the existing dashboard interface.

## Requirements

- PHP 8.3 or newer with SQLite, OpenSSL, Mbstring, Fileinfo, and ZIP extensions
- Composer 2
- Node.js 20.19+ (or 22.12+) and npm

## Local development

1. Copy `.env.example` to `.env`, then set `ADMIN_EMAIL`, `ADMIN_NAME`, and a strong `ADMIN_PASSWORD`.
2. Set `FILESYSTEM_DISK=local` for local document storage, or configure the R2 credentials and keep `FILESYSTEM_DISK=r2`.
3. Install dependencies: `composer install` and `npm install`.
4. Initialize the app: `php artisan key:generate`, then `php artisan migrate --seed`.
5. Start Laravel and Vite together with `composer run dev`.
6. Open `http://127.0.0.1:8000` and log in using the admin credentials from `.env`.

To build frontend assets, run `npm run build`. Run the backend workflow tests with `php artisan test`.

## Database and documents

The default database is SQLite at `database/database.sqlite`. Migrations create the singleton support admin, agencies, projects, project documents, and service points. The seeder inserts the 13 supported agencies and initializes the admin using the environment credentials.

Documents are limited to 10 MB and the file types accepted by the dashboard. Use the local disk for development, or configure a private Cloudflare R2 bucket with `R2_ACCOUNT_ID`, `R2_ACCESS_KEY_ID`, `R2_SECRET_ACCESS_KEY`, and `R2_BUCKET_NAME` for shared/private object storage.

## Deployment

The repository includes a Docker setup and Render Blueprint for the Laravel app. Connect this repository to Render and deploy the Blueprint from `render.yaml`. The container builds the frontend assets, serves Laravel from `public/`, and runs database migrations and the seeder at startup.

The Blueprint uses Render Free resources for testing. Free web services can sleep and have ephemeral local files; free PostgreSQL databases expire after 30 days. Do not use this setup for production data. Use paid persistent resources before storing important project records or documents.

For persistent private document storage, configure a Cloudflare R2 bucket, set `FILESYSTEM_DISK=r2`, and add `R2_ACCOUNT_ID`, `R2_ACCESS_KEY_ID`, `R2_SECRET_ACCESS_KEY`, and `R2_BUCKET_NAME` as Render environment variables. Keep `.env` and credentials out of version control. Set `ADMIN_EMAIL` and a strong `ADMIN_PASSWORD` in the Render dashboard before logging in.
