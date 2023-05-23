## Deep Mechanism Optimization

In this example, we focus on the scenario where the auction mechanism (including the allocation rule and the payment rule) is learned through deep neural networks (DNNs). Specifically, one learns a network for allocation and a network for payment on the collected dataset, subject to the constraints on the desired property of the auction, e.g., incentive compatibility (IC). We refer the readers to the pioneering work RegretNet [1] for detailed algorithmic designs.

## Implementation in Our Framework

We initialize an auction mechanism with a payment rule specified by a deep payment network and a highest price allocation rule (for simplicity). 
We then run the dynamic environment multiple times to collect a dataset consisting of multiple rounds of auction.
Finally, we train the payment network to maximize the total payment on the dataset.

**Payment network**: Our payment network is implmented as a four-layer fully connected network (FCN) in PyTorch. The input is a $n$ dimension vector consisting of the bids of the $n$ players, and the output is another $n$ dimension vector consisting of the payment to the $n$ players. The three hidden layers are of size 128, 64, and 128.

**Dataset collection**: Our implementation is slightly different from that introduced in RegretNet [1]. Specifically, we collect different datasets as our payment network gets updated. The procedure can be understood as iterative refinement, where we alternate the steps of collecting the dataset using the current payment network, and updating the payment network using the collected dataset.

**Objective function and model training**: As in RegretNet [1], The goal of the optimization is to *maximize the revenue of the auctioneer*, which equals the total payment of the bidders. Thus, we take the sum of the payment from the bidders as the objective function, and update our network using gradient descent to maximize this objective.


---

#### References

[1] Dütting, Paul, et al. "Optimal auctions through deep learning." International Conference on Machine Learning. PMLR, 2019.


