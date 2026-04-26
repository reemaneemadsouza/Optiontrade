# AGENTS.md

This document describes the architecture of the TERMINUS Indian F&O Trading Terminal for AI agents working on this codebase.

## Project Overview

A production-ready Indian F&O trading terminal with AI-powered analysis, real-time simulated market data, TradingView chart integration, and automated risk management. Built with TanStack Start (React 19 + Vite 7), deployed on Netlify.

## Directory Structure

```
src/
├── routes/
│   ├── __root.tsx          # Root layout: page title, global body styles
│   ├── index.tsx           # Main trading terminal (TradingTerminal component)
│   └── api/
│       └── ai-chat.ts      # Server API route: Gemini 2.5 Flash proxy
├── components/
│   └── trading/
│       ├── types.ts         # All TypeScript interfaces
│       ├── data.ts          # Static data: Indian indices, F&O stocks, news, patterns
│       ├── useTradingData.ts # Core hook: price simulation, option chain, portfolio
│       ├── ToastSystem.tsx  # useToasts hook + ToastContainer component
│       ├── CyberBackground.tsx  # Animated cyber-grid + scan line
│       ├── IndicesBar.tsx   # Top bar: index prices, clock, market status
│       ├── TickerMarquee.tsx # Auto-scrolling F&O ticker
│       ├── TradingViewWidget.tsx # TradingView chart embed
│       ├── SignalPanel.tsx  # BUY/SELL/HOLD signals + pattern detection tabs
│       ├── AIBrainPanel.tsx # Gemini AI chat panel
│       ├── PortfolioPanel.tsx # Position tracker, P&L, drawdown, kill switch
│       ├── NewsPanel.tsx    # VIX, PCR, FII/DII, news feed
│       └── OptionChainPanel.tsx # Full option chain with OI heatmap
├── router.tsx              # TanStack Router setup
└── styles.css              # Global styles: cyber grid, glassmorphism, animations
```

## Architecture Decisions

### Data Simulation
Real-time prices use a `randomWalk` function in `useTradingData.ts` applied every 1 second via `setInterval`. To connect real data (e.g. Zerodha Kite Connect), replace the interval blocks with WebSocket/REST calls, preserving the `IndexQuote`/`StockQuote` types from `types.ts`.

### AI Integration
The Gemini API key never touches the browser. `src/routes/api/ai-chat.ts` is a TanStack Start server route (runs server-side on Netlify) that reads `process.env['GEMINI_API_KEY']` and proxies requests to `generativelanguage.googleapis.com`. The frontend calls `/api/ai-chat` via fetch.

### Kill Switch Logic
In `useTradingData.ts`, portfolio `drawdown` is recalculated on every price tick. When `drawdown >= 5`, `portfolio.killed` becomes `true`. The main `index.tsx` fires a kill switch toast via `useEffect`. Manual reset is available via `resetKillSwitch`.

### Styling
Tailwind CSS 4 for layout + custom CSS in `styles.css` for terminal aesthetics:
- `.cyber-grid` / `.scan-line` — animated background
- `.glass-panel` — glassmorphism with gradient border
- `.signal-buy` / `.signal-sell` / `.signal-hold` — neon badges
- `.ticker-track` — CSS scroll animation
- CSS variables for neon colors: `--neon-green`, `--neon-cyan`, `--neon-red`, `--neon-yellow`

### TradingView Widget
`TradingViewWidget.tsx` injects the TradingView `embed-widget-advanced-chart.js` script via `useEffect`. Symbol/interval changes trigger re-injection. Symbol mapping is in `data.ts` (`CHART_SYMBOLS`).

## Coding Conventions

- TypeScript strict mode; all types in `types.ts`
- `@/` path alias resolves to `src/`
- PascalCase components, camelCase hooks
- No external state library — React hooks only
- No new npm dependencies required

## Key Environment Variables

| Variable | Purpose | Required |
|----------|---------|----------|
| `GEMINI_API_KEY` | Gemini 2.5 Flash API key | Optional (AI panel only) |

## F&O Coverage

- **Indices**: NIFTY 50, BANKNIFTY, SENSEX, FINNIFTY, MIDCPNIFTY, NIFTYIT
- **F&O Stocks**: 40 stocks including RELIANCE, TCS, INFY, HDFCBANK, ICICIBANK, SBIN, BAJFINANCE, TATAMOTORS, and more
- Add more stocks in `data.ts` → `FNO_STOCKS` array
