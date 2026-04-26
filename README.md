# TERMINUS — Indian F&O Trading Terminal

A production-ready, AI-powered Indian Futures & Options trading terminal built with TanStack Start and deployed on Netlify. Features a cyber-grid aesthetic with glassmorphism UI panels, real-time simulated market data, and Gemini 2.5 Flash AI for trading intelligence.

## Features

- **Real-time market simulation** — Live price feeds for all major Indian indices (NIFTY 50, BANKNIFTY, SENSEX, FINNIFTY, MIDCPNIFTY) and 40+ F&O stocks with random-walk price simulation
- **Animated cyber-grid background** — Full-screen animated grid with scan line effect and glassmorphism panels
- **TradingView chart integration** — Embedded TradingView advanced chart widget with interval switching and symbol selection
- **AI Brain Panel** — Gemini 2.5 Flash AI via secure server-side proxy for F&O strategy, Greeks, and market analysis
- **Signal Panel** — Automated BUY/SELL/HOLD signals with RSI, MACD, EMA, VWAP indicators
- **Pattern Recognition Engine** — 14 candlestick patterns detected across multiple timeframes
- **Option Chain** — Full option chain with OI heatmap, IV, volume, ATM highlighting
- **Portfolio Tracker** — Real-time P&L tracking with live position updates
- **Kill Switch** — Automatic trading halt at 5% drawdown (manual override available)
- **Toast Notifications** — Event-driven alerts for kill switch, drawdown thresholds, system events
- **F&O Ticker Marquee** — Auto-scrolling live price feed for all F&O instruments
- **Market Monitor** — India VIX, PCR ratio, FII/DII flows, live news feed

## Tech Stack

| Layer | Technology |
|-------|------------|
| Framework | TanStack Start v1 |
| Frontend | React 19, TanStack Router v1 |
| Build | Vite 7 |
| Styling | Tailwind CSS 4 + custom CSS |
| Charts | TradingView widget (embedded), Chart.js |
| AI | Google Gemini 2.5 Flash via server-side API route |
| Deployment | Netlify |

## Running Locally

```bash
npm install
npm run dev
```

The dev server starts on `http://localhost:3000`. For Netlify features (including the AI endpoint), use:

```bash
netlify dev
```

This runs on `http://localhost:8888` with full Netlify edge function emulation.

## AI Configuration

Set your Gemini API key as a Netlify environment variable:

```
GEMINI_API_KEY=your_key_here
```

The AI proxy endpoint is available at `/api/ai-chat`. Without the key, the AI panel shows a configuration prompt — all other terminal features work normally.

To get a Gemini API key: [Google AI Studio](https://aistudio.google.com/app/apikey)

## Zerodha Integration (Optional)

The terminal is designed to work standalone with simulated data. To connect real Zerodha market data, replace the `randomWalk` simulation in `src/components/trading/useTradingData.ts` with calls to the Kite Connect API. The data shape is already defined in `src/components/trading/types.ts`.

## Disclaimer

This is a demonstration/educational application. Simulated prices are not real market data. Not intended for actual trading decisions.
