# Wallet Intelligent Stock Expert Trade (WISE Trade)- AI Trading System

This project collates the `TradingAgents` for trend prediction, communicating as an input with the Reinforcement Learning Agent, from its initial Research concept to the final web application.

The Reinforcement Learning Agent will utilize the LLM pipeline to output buy, hold, and sell options trading strategies, along with a corresponding profit chance.
The following content is a modified version of the Trading Agents and the add-on Reinforcement Learning-powered project.

This product is intended for academic research purposes only and does not constitute investment advice. 

---

## Table of Contents

- [Wallet Intelligent Stock Expert Trade (WISE Trade)- AI Trading System](#Wallet-Intelligent)
  - [Table of Contents](#table-of-contents)
  - [1. Project Overview](#1-project-overview)
    - [Core Concept](#core-concept)
    - [System Architecture](#system-architecture)
  - [2. Core Functional Modules](#2-core-functional-modules)
    - [Multi-Agent Analysis Framework](#multi-agent-analysis-framework)
    - [Options Trading Extension](#options-trading-extension)
    - [Reinforcement Learning Enhancement](#reinforcement-learning-enhancement)
  - [3. User Interfaces](#3-user-interfaces)
    - [Command-Line Interface (CLI)](#command-line-interface-cli)
    - [Professional Web Dashboard (V3)](#professional-web-dashboard-v3)
  - [4. Installation and Usage](#4-installation-and-usage)
    - [Environment Setup](#environment-setup)
    - [Running the CLI](#running-the-cli)
    - [Running the Web App](#running-the-web-app)
  - [5. Technical Implementation Details](#5-technical-implementation-details)
    - [State Encoder](#state-encoder)
    - [RL Model Architecture](#rl-model-architecture)
    - [Streaming Output](#streaming-output)

---

## 1. Project Overview

### Core Concept

`TradingAgents` is a multi-agent financial analysis framework that mimics the operational model of a real-world trading firm. It deploys multiple specialized, LLM-driven AI agents to collaboratively perform market analysis, strategy formulation, and risk management, ultimately delivering high-quality trading decisions. 

`RL Agent` takes the financial analysis advice and processes the self-trading to make a profit. RL will be able  to label 1/0, and it requires a specific transaction (with additional actions such as call, put, or hold).

### System Architecture

The system is composed of several collaborative agent teams:

1. **Analyst Team**:
    * `Fundamentals Analyst`: Analyzes company fundamentals.
    * `Technical Analyst`: Analyzes technical indicators and chart patterns.
    * `News Analyst`: Interprets news and macroeconomic events.
    * `Sentiment Analyst`: Assesses social media and public sentiment.
    * `Options Analyst`: (New) Focuses on options chains, Greeks, and volatility.

2. **Researcher Team**:
    * Critically evaluates analyst reports through a debate format, balancing risk and reward.

3. **RL Trader Agent**:
    * Synthesizes all information to formulate a concrete trading plan.
<img width="1186" height="542" alt="image" src="https://github.com/user-attachments/assets/656a5d2d-e0cc-454b-8177-9f55289a6ce2" />

---

## 2. Core Functional Modules

### Multi-Agent Analysis Framework

This is the core of the project, built with `LangGraph` to ensure flexibility and modularity. Each agent has a specific role, and through collaboration, they achieve a deep analysis of complex financial markets.

### Options Trading Extension

Adds powerful options analysis capabilities to the system.

* **Supported Strategies**:
  * **Single Leg**: `Call`, `Put`
  * **Spreads**: `Bull Call Spread`, `Bear Put Spread`
  * **Volatility**: `Straddle`, `Strangle`
  * **Income**: `Covered Call`, `Iron Condor`
* **Core Analysis**:
  * **Greeks Calculation**: Delta, Gamma, Theta, Vega, Rho.
  * **Implied Volatility (IV)**: Assesses market expectations of future volatility.
  * **Volume and Open Interest**: Analyzes market interest and liquidity.

### Reinforcement Learning Enhancement

Integrates a **Deep Q-Network (DQN)** model based on **PyTorch**, elevating the analysis to a new level.

* **Profitability Prediction**:
  * The system no longer just recommends "Buy/Sell" but calculates the **profitability probability** for a user's chosen trade.
  * Example: `Buy Call Option` -> `Profitability: 62%`.
* **State Encoder**:
  * Encodes all analyst reports (technical, fundamental, news, sentiment, options Greeks, etc.) into a **128-dimensional** state vector.
  * This vector is the sole basis for the RL model's decisions.
* **Reward Mechanism**:
  * The model is trained through historical simulations with a reward mechanism of: **Profit = +1, Loss = -1**.

---

## 3. User Interfaces

The project offers two ways to interact with the system.

### Command-Line Interface (CLI)

A full-featured, interactive command-line tool.

* **Launch Command**: `python -m cli.main`
* **Features**:
  * Completes all configurations (ticker, analysts, trading mode, etc.) through a Q&A format.
  * Displays real-time logs of the analysis process.
  * Fully supports the selection of **Options Trading** and **RL Enhancement** modes.

<p align="center">
  <img src="assets/cli/cli_init.png" width="80%">
</p>

### Professional Web Dashboard (V3)

A `Streamlit`-based web application with a professional financial terminal aesthetic.

* **Launch Command**: `streamlit run web_app_v3.py`
* **Core Features**:
  * **Professional UI/UX**: Dark theme, custom charts, and a serious, professional interface with all emojis removed.
  * **Gemini-Style Streaming Output**: Key information from the analysis process is displayed character-by-character, like a typewriter, providing an excellent interactive experience.
  * **User-Led Analysis**: The user inputs their desired trade (e.g., "Buy a call option"), and the system analyzes its profitability and risk, rather than directly recommending an action.
  * **Advanced Options Strategies**: Supports input and analysis of complex strategies like `Bull Call Spread`.
  * **Modular Layout**: A left sidebar for configuration and a main area for analysis and charts.

<p align="center">
  <img src="https://img.shields.io/badge/TradingAgents-Pro_V3-00d2ff?style=for-the-badge" >
</p>

---

## 4. Installation and Usage

### Environment Setup

1. **Clone the Repository**:
    ```bash
    git clone https://github.com/TauricResearch/TradingAgents.git
    cd TradingAgents
    ```
2. **Create a Virtual Environment**:
    ```bash
    conda create -n tradingagents python=3.13
    conda activate tradingagents
    ```
3. **Install Dependencies**:
    ```bash
    pip install -r requirements.txt
    ```
4. **Set API Keys**:
    Create a `.env` file in the project root and add your API keys:
    ```
    OPENAI_API_KEY="sk-..."
    ALPHA_VANTAGE_API_KEY="..."
    ```

### Running the CLI

```bash
python -m cli.main
```
Follow the interactive prompts to configure the analysis.

### Running the Web App

```bash
streamlit run web_app_v3.py
```
Your browser will automatically open to `http://localhost:8501`.

---

## 5. Technical Implementation Details

### State Encoder

The `StateEncoder` bridges the multi-agent analysis and the RL model by converting unstructured text reports into a standardized numerical vector.

* **Input**: Various text-based analysis reports.
* **Output**: A 128-dimensional `numpy` array.
* **Features Included**:
  * Technical Indicators (9 features)
  * Fundamental Metrics (6 features)
  * News Sentiment (3 features)
  * Social Sentiment (2 features)
  * Options Data (8 features)

### RL Model Architecture

A standard **Deep Q-Network (DQN)** is used.

```
Input (128-dim state vector)
  ↓
Dense (256) + ReLU + Dropout
  ↓
Dense (128) + ReLU + Dropout
  ↓
Dense (64) + ReLU + Dropout
  ↓
Output (3 actions: BUY, HOLD, SELL)
  ↓
Softmax → Probabilities
```

### Streaming Output

In `web_app_v3.py`, a Gemini-style typewriter effect is achieved by simulating `time.sleep()` and dynamically updating `st.markdown`, which significantly enhances the user experience.

```python
# Pseudocode
full_report = ""
placeholder = st.empty()

for message_part in analysis_stream:
    full_report += message_part
    placeholder.markdown(full_report)
    time.sleep(0.02)
```


**Disclaimer**: This project is for academic research purposes only and does not constitute any investment advice. Financial markets are risky, and investment requires caution.
