# 2024-Spring-HW2

Please complete the report problem below:

## Problem 1
Provide your profitable path, the amountIn, amountOut value for each swap, and your final reward (your tokenB balance).

> Solution
tokenB->tokenA 
    amountIn:5
    amountOut:5.655321988655322

tokenA->tokenE 
    amountIn:5.655321988655322
    amountOut:1.0583153138066885

tokenE->tokenD 
    amountIn:1.0583153138066885
    amountOut:2.4297862601422264

tokenD->tokenC 
    amountIn:2.4297862601422264
    amountOut:5.038996197252911

tokenC->tokenB 
    amountIn:5.038996197252911
    amountOut:20.042339589188174

final reward:20.042339589188174

## Problem 2
What is slippage in AMM, and how does Uniswap V2 address this issue? Please illustrate with a function as an example.

> Solution
The difference between the expected price and the final executed price is called slippage. When setting a minimum threshold for token exchange, if the expected quantity falls below this threshold, the transaction is reverted.

function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external virtual override ensure(deadline) returns (uint[] memory amounts) {
        amounts = UniswapV2Library.getAmountsOut(factory, amountIn, path);
        require(amounts[amounts.length - 1] >= amountOutMin, 'UniswapV2Router: INSUFFICIENT_OUTPUT_AMOUNT');
        TransferHelper.safeTransferFrom(
            path[0], msg.sender, UniswapV2Library.pairFor(factory, path[0], path[1]), amounts[0]
        );
        _swap(amounts, path, to);
    }

The function `require(amounts[amounts.length - 1] >= amountOutMin, 'UniswapV2Router: INSUFFICIENT_OUTPUT_AMOUNT');` is used to ensure that the final quantity obtained from the token exchange is not less than the minimum expected value.

## Problem 3
Please examine the mint function in the UniswapV2Pair contract. Upon initial liquidity minting, a minimum liquidity is subtracted. What is the rationale behind this design?

> Solution
This design is intended to ensure that the liquidity pool always maintains liquidity. The `MINIMUM_LIQUIDITY` tokens are permanently locked in the liquidity pool (allocated to `address(0)`), thereby providing a baseline level of liquidity.

## Problem 4
Investigate the minting function in the UniswapV2Pair contract. When depositing tokens (not for the first time), liquidity can only be obtained using a specific formula. What is the intention behind this?

> Solution
This formula is aimed at ensuring stability in the ratio of the two tokens. By using a specific formula (x*y=k), the addition of new liquidity tokens can maintain an appropriate proportion with the existing liquidity tokens. This helps prevent one asset from becoming too scarce or too abundant, ultimately enhancing the efficiency and stability of the transactions.

## Problem 5
What is a sandwich attack, and how might it impact you when initiating a swap?

> Solution
It may be caused by front-running, which is to understand the market and influence the market through rapid trading to take advantage of price changes; if a trader is subject to a sandwich attack, he may pay a higher price than expected or receive a lower price than expected.
