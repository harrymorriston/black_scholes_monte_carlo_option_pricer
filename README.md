# black_scholes_monte_carlo_option_pricer
Pricing a European Call Option on AAPL: A Comparison of Black-Scholes and Monte Carlo Methods

## Introduction and Background:

A stock option is a contract between a buyer who pays a premium for the right (not obligation) to buy or sell a stock at a fixed, predetermined price (known as the strike price) by a fixed date, and a seller (also known as a writer) who receives that premium in exchange for taking on the obligation to honour the deal if the buyer chooses to exercise it. Options are foundational building blocks in financial product structuring, an area of finance that is particularly interesting and involves using quantitative methods to design customised financial products.

In this project, I investigated European call options: A right to buy option that can only be exercised at maturity. American options and put options differ to this in how American options can be exercised any time before maturity, and put options are rights to sell. I chose to price it in 2 independent ways: Black-Scholes pricing and Monte Carlo pricing estimates. Through doing this, I could validate each method against the other and it allowed me to understand both the theoretical (Black-Scholes) and simulation-based approach (Monte Carlo) used in real-world derivatives pricing.

The Black-Scholes formula is a closed-form formula based on probability theory used to calculate the theoretical future price of a European option, whereas the Monte Carlo simulations use repeated random sampling to estimate the price of an option by finding the average payoff at maturity across all simulated futures. The Monte Carlo simulation method has particular importance in how for many real-world complex financial products, no exact closed-form formula exists exists at all – unlike the simpler options Black-Scholes can handle.

## Data:

For this project, the stock option I chose to price was AAPL (Apple Inc) as it is an extremely liquid stock, has years of clean data and is globally recognisable. It should be noted that this project prices a hypothetical European-style option for tractability; real-world AAPL options traded on exchanges are American-style which cannot be priced with the closed-form Black-Scholes formula alone. Many individual stock options (including those traded in UK markets) are essentially always American-style as a market convention. I chose AAPL as the underlying and modelled it as a European option specifically to allow the use of Black-Scholes, since this simplification would be needed for almost any individual stock’s option.

My source of Data was yfinance (Yahoo Finance) and I limited the selected price data of AAPL to the last 2 years of daily closing prices. I calculated the volatility by first finding the percentage changes between daily closing prices (this was the returns variable in my code), then calculating the standard deviation of these to find the daily volatility and finally calculating the annualised volatility as 28.92% by multiplying the daily volatility by the square root of 252 as standard deviation scales with the square root of time, and there are 252 trading days yearly as standard. The parameters I chose for my Black-Scholes and Monte Carlo call price calculators were as follows:
- S: Spot price, took the most recent closing price from the yfinance data
- K: Strike price, took it as 5% above the spot price - added an input prompt for user choice
- t: Maturity time of the option contract, took it as 6 months
- r: Risk-free rate, took the UK 1 year gilt yield (standard) as of the time of   running the program, 4.15% (16/09 at around 16:00) – added an input prompt for user ease
- σ: Volatility, used the annualised volatility I calculated (28.92%)
- Number of simulations (Monte Carlo): 200,000

## My Results:

Black-Scholes Call Price: £23.09
Monte Carlo Call Price: £23.12
Note: These call prices represent the premium – the upfront cost of the right to buy the stock at the strike price.

A key graph I created with my code was this one. It is a graph showing clearly how the Monte Carlo simulations converged to the Black-Scholes theoretical call price, highlighting how useful Monte Carlo simulations are in real world pricing.

My final calculation was the Delta value, which I found to be 0.489. The delta value is an important calculation as it answers the question “If the stock moves by £1, how much does the option’s price move?”, which has real-world value in delta hedging. If a bank sells a client a call option, the bank is now exposed. To protect itself, the bank buys a quantity of shares, equal to that of the delta value, for every option sold so that the gains on the stock offset the losses on the sold option. A hedge needs to be constantly adjusted because as stock price changes, so does the delta.

## Results Review:

The black-Scholes call price and the Monte Carlo call price were only different by around 0.1%. This is a valuable result, as it shows that the two independent pricing methods agree with each other which answers this project’s core idea of comparing the Black-Scholes and Monte Carlo methods.
Beyond the original run of my code, I decided to run a sensitivity test to investigate why the price behaves the way it does. I first nudged the value of sigma up by 2%, then down by 2%. Nudging it up resulted in the Black-Scholes call price and Monte Carlo call price increasing to £25.29 and £25.32 respectively, while nudging it down decreased them to £21.48 and £21.50 respectively. 

This, at face value, makes sense in the way that a more volatile market increases the chances of a “big win” (hence increase in price) while a less volatile market results in the opposite (hence decrease in price). In particular, the increase from a rise in volatility was larger than the decrease from a drop in the same percentage of volatility, showing the interesting result that option price sensitivity seems to respond more strongly to volatility increases rather than decreases (not a straight, linear function of volatility). Furthermore, the Black Scholes and Monte Carlo prices remained within a few pence of each other in both the higher and lower volatility scenarios, confirming the convergence holds beyond the base case.

Some of the limitations of this project are as follows:
- The pricing in this project was of a hypothetical European-style option (European version of AAPL) so that the Black-Scholes formula could be used. A further look at this is explored in the first paragraph in the data section.
- The project assumes certain things such as constant volatility and no dividends, which real-world markets do not fully satisfy.
The next practical step in this project would be to compare the calculated fair value with a real world quoted market price as this would indicate whether to buy or sell in practice.

## Reflection and Potential Extensions:

With more time, there are certain extras to this project I could have built. This project involved pricing a call option, but I could have also priced the equivalent put option alongside this which would have involved minor changes to my functions. Additionally, investigating and calculating other Greeks than just the Delta could’ve resulted in further useful insights (like Gamma which measures the change in delta for every move of 1 full market currency unit in the stock, or vega which measures how much an option changes when implied volatility shifts by 1%).

One of the most valuable realisations I had from the project was understanding that European-style pricing does not perfectly match how real AAPL options trade. A natural next step building on this would be to explore pricing models that can handle early exercise which are commonly used for American-style options.
