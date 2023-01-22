# PoC: Uniswap's SwapRouter doesn't refund unspent ETH in partial swaps

This is a proof of concept of the bug I found in Uniswap's `SwapRouter`. Read the blog post for more details:
https://jeiwan.net/posts/public-bug-report-uniswap-swaprouter/

## Usage
1. Ensure that you have [Foundry](https://github.com/foundry-rs/foundry) installed.
1. Run `forge install` to install the deps (`forge-std`).
1. Set the `ETH_RPC_URL` env var to an Ethereum Mainnet RPC endpoint (e.g. use [Alchemy](https://www.alchemy.com/)).
1. Run:
    ```shell
    $ forge test --mc UniswapV3ETHRefundExploitTest
    ```

## Exploit Scenario
1. Alice wants to sell 1 ETH and buy some UNI. However, Alice wants her trade to be executed before the price X is reached.
1. Alice calls the `exactInputSingle` function of `SwapRouter`, sets the `sqrtPriceLimitX96` argument to the price X, and sends 1 ETH along with the transaction.
1. The router executes the swap via the ETH-UNI pool. The swap gets interrupted when the price X is reached.
1. Before reaching the price X, only 0.7 ETH of Alice were consumed to convert them to 100 UNI.
1. Alice receives 100 UNI while spending 1 ETH, the router contract keeps holding the remaining 0.3 ETH.
1. A MEV bot withdraws the 0.3 ETH by calling the `refundETH` function.