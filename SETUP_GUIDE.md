# SaaS Project Setup Guide - Vercel Database Connection

## Project Overview
This is a Next.js SaaS starter with:
- Next.js 15 with TypeScript
- Drizzle ORM with PostgreSQL
- Stripe payment integration
- Authentication with JWT
- Dashboard with CRUD operations

## Step-by-Step Installation & Setup

### Step 1: Install Dependencies

Run the following command in your terminal from the project root:

```bash
pnpm install
```

### Step 2: Set Up Vercel Postgres Database

1. **Create a Vercel Account** (if you don't have one):
   - Go to https://vercel.com and sign up

2. **Create a Vercel Storage (Postgres) Database**:
   - Go to https://vercel.com/dashboard
   - Click on "Storage" or go to https://vercel.com/dashboard/stores
   - Click "Create Database" → "Create New" → Select "Postgres"
   - Choose a name and region
   - Click "Create"

3. **Get Your Database URL**:
   - After creation, go to the "Connection string" section
   - Copy the connection string with `pgbouncer` or the standard PostgreSQL URL
   - Look for the line that starts with `POSTGRES_URL=`

### Step 3: Configure Environment Variables

1. **Update the `.env` file** in the project root with your actual values:

```env
# Your Vercel Postgres connection string
POSTGRES_URL=postgresql://default:[PASSWORD]@[HOST]:5432/verceldb

# Stripe Configuration (optional for local development)
STRIPE_SECRET_KEY=sk_test_your_key
STRIPE_WEBHOOK_SECRET=whsec_your_secret

# Application Settings
BASE_URL=http://localhost:3000
AUTH_SECRET=your_random_secret
```

**To generate AUTH_SECRET**:
- On macOS/Linux: `openssl rand -base64 32`
- On Windows PowerShell: Use any secure random string or visit https://generate-random.org/

### Step 4: Set Up Stripe (Optional, for Payment Features)

1. **Create a Stripe Account**:
   - Go to https://stripe.com
   - Sign up and get API keys from https://dashboard.stripe.com/apikeys

2. **Update .env with Stripe Keys**:
   ```env
   STRIPE_SECRET_KEY=sk_test_...
   STRIPE_WEBHOOK_SECRET=whsec_...
   ```

3. **Set up Stripe CLI** (for local webhook testing):
   ```bash
   # Install Stripe CLI from https://stripe.com/docs/stripe-cli
   stripe login
   stripe listen --forward-to localhost:3000/api/stripe/webhook
   ```

### Step 5: Run Database Migrations

```bash
# Generate migrations from schema
pnpm db:generate

# Run migrations against your Vercel database
pnpm db:migrate

# Seed the database with sample data (creates test@test.com / admin123)
pnpm db:seed
```

### Step 6: Start Development Server

```bash
pnpm dev
```

The application will be available at `http://localhost:3000`

## Default Test Credentials

After running `pnpm db:seed`, you can log in with:
- **Email**: test@test.com
- **Password**: admin123

## Database Management

### View Database in Studio
```bash
pnpm db:studio
```

### Generate New Migrations
```bash
pnpm db:generate
```

### Run Migrations
```bash
pnpm db:migrate
```

## Available Scripts

- `pnpm dev` - Start development server with Turbopack
- `pnpm build` - Build for production
- `pnpm start` - Start production server
- `pnpm db:setup` - Interactive setup script
- `pnpm db:seed` - Seed database with sample data
- `pnpm db:generate` - Generate migrations
- `pnpm db:migrate` - Run migrations
- `pnpm db:studio` - Open Drizzle Studio

## Troubleshooting

### Connection Refused
- Verify your `POSTGRES_URL` is correct
- Check that your Vercel database is accessible
- Ensure you're not behind a firewall that blocks the database port

### Migration Errors
- Run `pnpm db:generate` first to create migrations
- Check that schema changes are valid in `lib/db/schema.ts`

### Missing Environment Variables
- Make sure all variables in `.env.example` are present in `.env`
- Restart the development server after updating `.env`

## Production Deployment

When deploying to Vercel:

1. Push your code to GitHub
2. Import project to Vercel
3. Add environment variables in Vercel project settings:
   - `POSTGRES_URL`
   - `STRIPE_SECRET_KEY`
   - `STRIPE_WEBHOOK_SECRET`
   - `BASE_URL` (your production domain)
   - `AUTH_SECRET`

4. Vercel will automatically run migrations on deployment

## File Structure

```
├── app/                      # Next.js 15 app directory
│   ├── (dashboard)/         # Protected dashboard routes
│   ├── (login)/             # Auth routes
│   └── api/                 # API routes
├── lib/
│   ├── db/                  # Database configuration
│   │   ├── schema.ts        # Drizzle schema
│   │   ├── migrations/      # Generated migrations
│   │   ├── setup.ts         # Interactive setup script
│   │   └── seed.ts          # Database seeding
│   ├── auth/                # Authentication logic
│   └── payments/            # Stripe integration
├── components/              # React components
├── .env                     # Environment variables (local)
└── drizzle.config.ts        # Drizzle configuration
```

## Support & Resources

- [Next.js Documentation](https://nextjs.org/docs)
- [Drizzle Documentation](https://orm.drizzle.team/)
- [Vercel Postgres Docs](https://vercel.com/docs/storage/vercel-postgres)
- [Stripe Documentation](https://stripe.com/docs)
- [Shadcn/ui Components](https://ui.shadcn.com/)

---

**Next Steps:**
1. Install dependencies: `pnpm install`
2. Update `.env` with your Vercel Postgres URL
3. Run migrations: `pnpm db:migrate`
4. Seed database: `pnpm db:seed`
5. Start dev server: `pnpm dev`
