
# Solana Rivals: AI Agent Trading Competition 

> Build, customize & unleash AI trading agents in a gamified battle! Track whales, analyze markets & climb the leaderboard for epic rewards. Trade like an expert—earn like a champion!

## About

Solana Rivals merges cutting-edge AI technology with competitive gaming to revolutionize decentralized trading. Born at the intersection of autonomous AI agents, decentralized markets, and gamification, our platform transforms trading strategies into an exciting battle between legendary playable avatars.

## Features

### 🔗 Wallet Integration & Customization

> **Users begin by connecting their Solana wallets using a sleek wallet adapter. Once in, they enter an interactive onboarding process where they tailor their agent’s trading behavior based on on-chain analysis and market analytics. For instance, if you’re all about tracking whale wallet activity, you can assign a higher weight to that factor—ensuring your agent primarily copies large-scale trades.**


### 🎮 Gamified Experience & Leaderboards

>**To celebrate creativity and competition, users select an avatar inspired by historic warriors like Vikings, legionnaires, and samurais. These avatars are displayed on a dynamic leaderboard that tracks performance, showing key metrics such as total profit/loss.**

### 🤖 Autonomous Agent Swarm

Our sophisticated AI orchestrator coordinates three specialized sub-agents:
### Market Trends Agent

```
const startTrendingTokenInterval = () => {
  setInterval(async () => {
    try {
      const tokenData = await getAllTrendingTokensData();
      
      await clearTrendingTokenData();

      for (const data of tokenData) {
        const { token, tokenInfo } = data;
        const bestPair = tokenInfo.pairs?.[0];
        
        if (bestPair) {
          // Convert values with proper type handling
          const price = typeof bestPair.priceUsd === 'string' 
            ? parseFloat(bestPair.priceUsd) 
            : bestPair.priceUsd;
          
          const volume = typeof bestPair.volume?.h24 === 'string'
            ? parseFloat(bestPair.volume.h24)
            : bestPair.volume?.h24;
          
          const liquidity = typeof bestPair.liquidity?.usd === 'string'
            ? parseFloat(bestPair.liquidity.usd)
            : bestPair.liquidity?.usd;
          
          const marketCap = typeof bestPair.marketCap === 'string'
            ? parseFloat(bestPair.marketCap)
            : bestPair.marketCap;

          const txns = bestPair.txns || {};
          const h1 = txns.h1 || { buys: 0, sells: 0 };
          const m5 = txns.m5 || { buys: 0, sells: 0 };

          await insertTrendingTokenData(
            token.tokenAddress,
            price || null,
            volume || null,
            liquidity || null,
            marketCap || null,
            h1.buys,
            h1.sells,
            m5.buys,
            m5.sells,
            token.description
          );
        }
      }

    } catch (error) {
    }
  }, 120000);
};
```

### Whale Tracker Agent
>**Monitors influential whale wallets, analyzing trade frequencies and volumes of the hottest wallets.**

```
const startWhaleTokenInterval = () => {
  setInterval(async () => {
    try {
      const fetcher = new TransactionFetcher();
      await fetcher.analyzeMultipleWallets(WALLET_ADDRESSES);
      
      const tokenData = fetcher.getTokenFrequencies();
      
      await clearWhaleTokenData();

      for (const [mint, data] of tokenData) {
        await insertWhaleTokenData(
          mint,
          data.frequency,
          data.totalVolume
        );
      }

    } catch (error) {
    }
  }, 180000);
};
```

### Orchestrator Agent
>**Synthesizes market data and user parameters to provide actionable insights. Together, they empower the main agent to autonomously decide when to buy or sell tokens, with a dedicated sell agent managing exits based on user-defined risk profiles.**

```
const systemPrompt = `You are a trading agent. Given token data and preferences, output only a valid Solana token address to buy, or "SKIP". Consider diversification as a plus but prioritize token quality. Use all fields in the JSON data for analysis.`;

const humanPrompt = `Preferences: ${JSON.stringify(preferences)}
		                  Tokens: ${tokenDataJson}
		                  Whale data: ${whaleDataJson}
		                  Output only the token address or "SKIP".`;

const response = await bot.invoke([
   new SystemMessage(systemPrompt),
   new HumanMessage(humanPrompt)
]);
```

## Technical Architecture

### Backend

-   Express Server (ts-node)
-   Docker containerization
-   Solana and Helius API integration
-   DexScreener and CoinGecko API integration
-   OpenAI API

### Frontend

-   Next.js
-   Tailwind CSS
-   shadcn UI
-   Phantom Wallet Adapter

### Data Layer

-   Supabase database

## Social Impact & Future Potential

Solana Rivals is committed to:

-   Democratizing trading strategies
-   Driving financial inclusion
-   Inspiring the next generation of traders
-   Catalyzing broader change in DeFi
-   Continuous data-driven optimization
