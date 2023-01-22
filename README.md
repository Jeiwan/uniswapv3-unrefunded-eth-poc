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