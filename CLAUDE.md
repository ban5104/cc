# Crypto Financial Dashboard - Project Standards

## Commands
- Install: `npm install`
- Dev: `npm run dev`
- Build: `npm run build`
- Add Component: `npx shadcn@latest add [component]`

## Architecture
- Frontend: shadcn/ui + Next.js
- Backend: Next.js API + Prisma
- database: superbase
- deployment: docker containerization, and digital ocean

## Key Features
- Real-time crypto price data
- Portfolio tracking and analytics
- Interactive charts and graphs
- AI-powered market insights
- Price alerts and notifications
- Historical data analysis

## Conventions
- Style: Tailwind CSS utility classes, shadcn/ui components
- Git: Feature branches (`feature/price-alerts`, `refactor/chart-components`)
- API Keys: Store in environment variables, access via API routes only
- Data Updates: Real-time via API polling or WebSockets
- Charts: Use Recharts components with shadcn styling

## Critical Security
- **NEVER** expose exchange API keys to client-side
- All external API calls must go through `/api/` routes
- Use proper API rate limiting
- Validate all price data before display

## Component Priority for Refactor
1. **Dashboard Layout**: Header, sidebar, main grid
2. **Price Cards**: Current prices, 24h changes, market cap
3. **Charts**: Price charts, portfolio allocation, performance graphs
4. **Portfolio Table**: Holdings, P&L, allocation percentages
5. **AI Insights Panel**: Market analysis, recommendations
6. **Settings/Alerts**: User preferences, notification setup

## Do's
- Cache expensive API calls to avoid rate limits
- Use loading states for all data fetching
- Implement error boundaries for chart components
- Keep price updates smooth and non-jarring
- Test with both real and mock data
- Validate all numeric calculations (portfolio values, percentages)

## Don'ts
- Make direct API calls from client components
- Skip error handling for external API failures
- Hard-code cryptocurrency symbols or exchange endpoints
- Commit API keys or sensitive credentials
- Block UI while fetching price data

## Testing Focus
- Price data displays correctly across different market conditions
- Charts render properly with real-time updates
- Portfolio calculations are accurate
- AI insights load without breaking layout
- App handles API failures gracefully
- Responsive design works on mobile (important for price checking)

## Environment Variables Required
```
CRYPTO_API_KEY=
OPENAI_API_KEY=
NEXT_PUBLIC_APP_URL=
```

## Common Issues & Solutions
- **Chart not updating**: Check data format matches Recharts expectations
- **API rate limits**: Implement caching and request throttling
- **Price precision**: Use proper decimal handling for crypto values
- **Real-time updates**: Consider WebSocket connections for live data

