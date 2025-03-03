---
sidebar_position: 2
---

# API Integration

This document explains how the Crypto Price Tracker integrates with cryptocurrency APIs to fetch and display real-time data.

## API Provider: CoinGecko

We chose the [CoinGecko API](https://www.coingecko.com/en/api/documentation) for our cryptocurrency data because:

- It's free to use with generous rate limits
- It provides comprehensive cryptocurrency data
- It has reliable uptime and performance
- It doesn't require API keys for basic usage

## API Implementation

### API Client

We've created a simple API client that handles fetching data from the CoinGecko API:

```typescript
const fetchCryptos = async () => {
  const res = await fetch(
    "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd/coins/markets?vs_currency=usd&order=market_cap_desc&page=1&price_change_percentage=24h"
  );
  if (!res.ok) {
    throw new Error("Failed to fetch data");
  }
  return res.json();
};
```

### Data Fetching Strategy

We use the following strategy for data fetching:

1. **Initial Load**: Data is fetched when the component mounts
2. **Manual Refresh**: Users can trigger a refresh by clicking the "Refresh" button
3. **Error Handling**: API errors are caught and displayed to the user

### API Parameters

The CoinGecko API endpoint we use is `/coins/markets` with the following parameters:

- `vs_currency=usd`: Return prices in USD
- `order=market_cap_desc`: Order by market cap (highest first)
- `page=1`: Get the first page of results
- `price_change_percentage=24h`: Include 24-hour price change percentage

## Caching and Revalidation

We use Next.js's built-in data fetching capabilities along with React Query to implement an efficient caching strategy:

2. **Client-Side Caching**: React Query maintains a client-side cache with configurable stale times

This approach provides several benefits:

- Reduced API calls to avoid rate limiting
- Improved performance by serving cached data
- Fresh data through periodic revalidation
- Optimistic UI updates when refreshing data

## Error Handling

Our error handling strategy includes:

1. **Try/Catch Blocks**: All API calls are wrapped in try/catch blocks
2. **Error Display**: User-friendly error messages are shown in the UI
3. **Fallback UI**: The UI gracefully handles error states

## Future Improvements

Potential improvements to our API integration:

1. **Websockets**: Implement real-time updates using websockets
2. **Multiple Sources**: Add fallback API providers (e.g., CoinCap, CryptoCompare)
