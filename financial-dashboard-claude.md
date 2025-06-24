# Crypto Financial Dashboard

## Quick Start
```bash
# Local Development
docker-compose up -d
docker-compose exec app npm run dev
# Access at http://localhost:3000
```

## Architecture
- Frontend: shadcn/ui + Next.js (App Router)
- Backend: Next.js API Routes + Prisma
- Database: PostgreSQL (local) → Supabase (production)
- Deployment: Docker locally → DigitalOcean
- Charts: Recharts with shadcn styling
- Real-time: WebSocket for price updates

## Common Commands
```bash
# Docker
docker-compose up -d                    # Start all services
docker-compose exec app npm run dev     # Run dev server
docker-compose exec app npx prisma studio  # Database GUI

# Development
npm run dev                            # Local development
npx shadcn@latest add [component]      # Add UI components
npx prisma migrate dev                 # Run migrations
```

## Project Structure
- `/app/api/*` - Protected API routes for external calls
- `/components/ui/*` - shadcn components only
- `/lib/prisma.ts` - Database client
- `/docker-compose.yml` - Local stack configuration

## Critical Security Rules
- **NEVER** expose API keys to client-side
- ALL crypto API calls through `/app/api/*` routes
- Validate price data before display
- Use environment variables for secrets

## Code Style
- Use existing shadcn/ui components - don't create custom UI
- Cache expensive API calls to avoid rate limits
- Show loading states for all data fetching
- Handle API failures gracefully
- Test with both real and mock data

## Environment Setup
```env
# .env.local
DATABASE_URL="postgresql://postgres:postgres@localhost:5432/crypto_db"
CRYPTO_API_KEY="your-key"
OPENAI_API_KEY="your-key"
```

## Component Priority
1. Dashboard Layout (header, sidebar, grid)
2. Price Cards (current prices, 24h changes)
3. Charts (price history, portfolio allocation)
4. Portfolio Table (holdings, P&L)
5. AI Insights Panel
6. Settings/Alerts

## Common Issues
- **Chart not updating**: Check Recharts data format
- **API rate limits**: Implement caching layer
- **Price precision**: Use Decimal.js for crypto values
- **WebSocket disconnects**: Add reconnection logic

## Docker Workflow
```bash
# Fresh start
docker-compose down -v
docker-compose up -d
docker-compose exec app npx prisma migrate dev
docker-compose exec app npm run dev
```

## Testing
- Verify price displays across market conditions
- Check portfolio calculations accuracy
- Test API failure scenarios
- Ensure responsive design on mobile