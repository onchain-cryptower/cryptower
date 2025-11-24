# Cryptower

![Banner](https://raw.githubusercontent.com/onchain-cryptower/cryptower/refs/heads/main/assets/cryptower-banner.png)

## Highligts

- Completely onchain game (no data or assets stored outside of blockchain)

## Introduction

Cryptower (CT) is a self-financed multiplayer browser game played by many in the real time, using different strategies to succeed. New players can enter the game at any time, and existing players can stop participating in the game at any time. There is some resemblance in behavior and goals of players in this game with activity of traders in the stock market, so we will use some stock market terminology. But you don’t need to be the stock market savvy, may be, even true the opposite, to understand and play the game.

The “crypto” part of the game name reflects the way how it is implemented (as a crypto block-chain), and word towers appeared in the name as visualization of poker table with towers of tokens/chips, or vertical bar on chart price/time. You can also visualize tower just as a basket with some distinct objects, let’s say, balls inside it.

In CT game players collectively build and destroy towers from blocks. Each tower T is simply a collection of blocks. The number of blocks, denoted as |T|, in the tower T at any moment of time defines the tower’s height. Because each block in the tower has the same monetary value α, we can say, that height also defines value or “price” of the tower. Different towers could be built from blocks of different monetary value. There are also tower’s banks to collect money associated with each tower.

```
α - monetary value of each block in the tower.
```

## How to play. Rules.

A player can make a move, which we call an actions, at any moment of time. There are only two kinds of elementary actions, namely, the first is adding one block, and the second is removing one block. Players make these actions by placing corresponding orders. To play in CT game all players need to have some amount of money, which they keep in their wallets. To keep records every player has a portfolio, that contains list of numbers of blocks in each tower, belonging to the player.

![Banner](https://raw.githubusercontent.com/onchain-cryptower/cryptower/refs/heads/main/assets/game-goals.png)

When a player adds one block to tower T(α) of height h the following events happen: height of the tower increases by one, system subtracts from player’s wallet amount of money α(h+1) and adds this number to the bank. System also makes record in player’s portfolio, adding one block of tower T to the number of blocks he has in that tower. When a player removes one block from a tower of height h, height of the tower decreases by one, amount αh is subtracted from the bank and added to the wallet. System also makes record in players portfolio: subtracting 1 block from the number of blocks that the player has in tower Tα.

![Crytower Interface](https://raw.githubusercontent.com/onchain-cryptower/cryptower/refs/heads/main/assets/interface-01.png)

It is also allowed to add or remove several blocks to the same tower in one compound transaction. In this case blocks are added/removed to the tower by a sequence of elementary transactions, which are performed one by one without interruptions, in the same manner, as if they are added/removed by different players. This leads to the cumulative effect in payments, which is reflected in the following formulas. When buying s number of blocks of tower Tα of the height h the payment to the bank is equal to
payment = α(sh + s(s + 1) / 2).

Here we used classic formula

```
1 + 2 + 3 + ... + n = ½n (n + 1), (1)
```

When selling s blocks player takes from the bank amount of money equal to

```
payment = α(sh - s(s - 1) / 2).
```

The system place money to player’s wallet.

There are two major feasibility restrictions, that are always applied: namely, for any order it must be enough amount of money in a player’s wallet to buy, and enough number of blocks in a player’s portfolio to sell. If feasibility restrictions are not satisfied, system rejects the order.

As it follows from the rules, if a player adds a block to a tower at some height, and then removes the block at the higher height, he gets profit. Removing block from the tower at the lower height than it was added to the tower, leads to a loss. Gain/loss from transactions of one block is sell payment minus buy payment: hsell — (hbuy+1). The goal of the CT game is to make a profit, and this will happen only if hsell is greater than hbuy. In other words, it follows the stock market mantra “buy low, sell high”. Though we say buy/sell, we see that we don’t need a buyer to buy or a seller to sell at the other end to make the transaction.

As it was mentioned earlier, player may send order of any described types of transactions at any time, and order, if not delayed, will be fulfilled. In case of a delay, order that came first for given tower is fulfilled, and all other orders for this tower are canceled. During the delay information on all blocks pending are gathered and provided. After the delay is ended the information about all orders that confirmed/fulfilled are provided.
As it is implemented now, every player may to start building a new tower and all towers, having the same parameter α, are born equal. But later, as process evolves, different towers will have different owners and could behave differently.


## Keeping records

This part is so closely resembles keeping track of trading stocks that we can use that language to describe our situation. The information that system holds: all transactions, names and coefficients alfas of all towers, current height of every tower, current amount of money in banks associated with towers, and main information about players: wallets and portfolios.
The main information for individual player: cash is the amount of money in his wallet, and the portfolio, that is the list of names of towers and the numbers of blocks in every tower, that belong to the player.

It is helpful to hold handy other things such as: at which price/height block were bought. Also, player can have list of his own transactions and include current heights of towers which blocks he owns.

## Analysis of the game

The CT game is a gambling for players, but does not involve any risk for a house, which is the organizer of the game. From this perspective it is like totalizator, lotteries, and some other types of self-financing gambling. But at the same time as it easy to see from the above description of the game, CT game is significantly different from the all the above-mentioned games and activities. CT game is not a collective bet on some random event as pari-mutuel system, or lottery based on, but rather game based on fluctuation of some random variables, which we call heights or prices. And fluctuations here as in the real market are made by players, by their sentiments, they are not artificially created or taken from some processes in the nature. They are pure product of players behavior.

There is a redistribution of money between players through transactions, but the law of conservation of money holds: the whole amount of money in the wallets and banks remains permanent (assuming that commissions are paid not from wallets). It follows from observation of elementary transactions, and the fact that compound transactions are equivalent to sequence of elementary transactions. Thus, all participants together do not win or loose money. At the same time some individuals, the lucky ones, win, and some loose.

Bank of tower of height h holds the amount of money which Is calculated by

```
Money in the Bank = αh(h + 1) / 2
```
the same formula (1).

![Crytower Interface](https://raw.githubusercontent.com/onchain-cryptower/cryptower/refs/heads/main/assets/interface-02.png)

## About strategies and tools

The single most important thing in this game is to buy low. That is, when height of some tower is low, i.e. significantly lower than average, it is good for making profit to add block to this tower. In such a case, mathematical expectation of profit is positive.

There is also an obvious advantage of being first, that is, to start a new tower, but this advantage is a short lived, because the only way to benefit from this situation it is to sell starting blocks with profits, and at that moment the advantage of being first disappears. In other words, it is just the very specific case of the strategy buying low.

To keep track of dynamics of changes in towers height it is convenient to draw charts of corresponding time series (hi, ti). For many purposes it is enough to keep and draw sequence hi, holding track only of changes in height. It could be useful also to keep track on volume, which is the number of transactions per time unit.

The points of interest in the chart are local minima — buy points, and local maxima — sell points. We want to choose only those extrema points (or points close to them), which are different significantly enough at least to beat commissions. So, one of the first tools is the procedure of calculating such subsequence of extrema with assigned threshold.

Technical analysis in the stock market provides vast number of tools to predict points when market chart changes direction. They are based in studying patterns of time series. In CT game in the future the new specific patterns would be found, and tools developed. We just want to mention calculations of average, moving averages, and extrema. For these calculations it is sufficient to have only sequence hi . Lack of trends simplifies the task and make these tools more reliable.

Since the price of the tower fluctuates under the influence of the players and this influence stand out in a pure form, it can be seen whether the fluctuations in the height of the tower obey the theory of random walks. The CT game hyperbolically underscores speculative nature of the stock market. Those speculations are mostly responsible for small, and, sometime not so small, fluctuations. Stochastic process (hi,ti) associated with CT game is one-dimensional random walk. We see, that by definition the process is non-negative. If we consider the process in the neighborhood of average value of h, then we have the process, which is close to a habitual random walk around 0 in two directions. 
