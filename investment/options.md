## option 基础

### option characteristic
1. option has expiration date 到期日
2. options has a strike price  执行价格
3. option contract multiplier 合约乘数 (x100)
种类:
1. call option  看涨期权
2. put option 看跌期权

## important principle
YOU WILL ALMOST NEVER exercise an option

## value of option
* intrinsic value
* extrinsic value (time value)

1. in the money -> has intrinsic value
2. out the money -> has 0 intrinsic value

## short the option
because extrinsic value decay natualy with the time, so it makes sense to short the option
if you short the option, you don't pay the premium, you collect the premium

because short call option collecting the premium, the breakeven point point is higher than strike price


## implied volatility 
expected magnitude of stock price changes
losing money factor
* change of stock price
* decay of extrinsic value
* change of impliied volatility

## exercise and assignment
* exercise option when you own the option(buy)
  1. call: buy 100 shares at strike price
  2. put: sell 100 shares at strike price
* assign ed when you short the option (short)
  1. short call: assign -100 shares at the call's strike price
  2. short put: assign 100 shares at the put's strike price

## which stocks for options trading
* option trading volume
* open interest

SPY, IWM,QQQ (INDEX ETFS)
vix Index (options)

## vertical spread strategy
1. purchasing an option at one strike price
2. shorting an option at another strike price

### bull call spread (debit)
1. buy a call option at one strike price
2. short anohter call at higher strike price  
### bull put spread (high probability strategy) (credit)
1. short a put option at one strike price
2. buy another put option at lower strike price

breakeven = strike +/- ( collected premium -  paid premium )

### bear call spread ( high probability strategy) (credit)
1. short a call option at one strike price
2. buy another call option at a higher strike price
## bear put spread (debit)
1. buy a put options at one strike price
2. short a put option at lower strike price

hte max profit potential occurs from all of the extrinsic value coming out of the vertical spreads options

### implied volatility <-> choose vertical spread cycle

choose longer expiration means longer time to wait extrinsic value coming out , (30-60 = happy medium )

### debit spreads are not low implied volatility trades
for credit spread logic , ideal when IV is high becuase option-selling strategies benefit from the decrese in IV ( decrese in extricsic value)
可以在重大事件前采用 credit spread 压注，一方面可以在方向上获利，一方面在IV上获利。

注意: debit spread 卖出并不能从high IV 获利, 它也是从IV decrease 获利

如果价格走势和压注方向相反， 则IV增加对自己有利

### selecting strike prices for vertical spreads

#### debit spread case
* buy the in the money spread
you will have more risk than reward potential ( but high probability trading strategy)
* buy the in the money option and selling out out of the money option ( 比较平衡的策略)
* buy the spread out of money ( low probability & higher profit rate)

#### credit spread case
* sell the in the money put option
?

### veritical spread trade management "triggers"
1. profit/loss percentages
2. time-based exits
3. delta-based exits (**avoid holding trades to expiration**)

