## All-Pay Mechanism

In this example, we focus on another type of payment mechanism, all-pay mechanism, where all the players participating in the auction need to make a payment, no matter whether they win or not. This setting can find its 

To design a good all-pay mechanism that maximizes the auctioneer revenue (a.k.a. platform reward), Ian et al. [1] propose a sampling and approximation based approach. Concretely, they sample the weights for the payment rule, then simulate the auctions under the sampled payment rule, where the bidder strategy is learned by Fictitious Player (FP) [2]. After training the bidder strategies, they are able to obtain the platform reward associated with the payment rule. With these, they then train a regression network to learn the mapping between the payment rule (i.e., weights) and the platform reward. After obtaining the approximated mapping network, they are able to perform gradient descent on it to reach the best payment rule (weights) that gives the highest platform reward.


## Implementation in Our Framework

We aim to reproduce the results in Fig. 4 of Ian et al. [1] where there are $n=3$ bidders, due to the ease of visualization in a low-dimensional regime. We sample multiple weights, perform simulation under these weights, learn a network to fit the simulation results, and then obtain the best weight w.r.t. the learned network. We provide a more detailed description below.

**Payment rule**: For $n=3$, the payment rule can be conveniently expressed as $(w, 1-w, 0)$ parametrized by a single variable $w\in[1/2,1]$. We uniformly sample $w$ from the range and independently evaluate these different $w$'s.

**Simulation**: For a given weight (i.e., a given $w$), we initilize a dynamic environment with the payment rule specified by the weight $(w, 1-w, 0)$. We learn the bidding policy of the agents via MARL, where the individual policies can be realized by either $\varepsilon$-greedy or deep RL algorithms (e.g., DQN, PPO). After training the agents to convergence, we measure the platform revenue $R$, which is the revenue associated with the weight $w$. 


**Approximation**: After sampling multiple $w$'s and obtaining their corresponding $R$'s, we fit a network to learn the mapping from the highest-bidder weight $w$ to the platform revenue $R$. 

**Optimization**: The objective of finding the weight that leads to the highest platform revenue can be translated into the objective of finding the maximizer of the network. We then leverage standard optimization techniques (e.g., GD) to reach the optimizer.

## Example Commands

First enter the subfolder:

```bash
cd mechanism_design
```

Then run the simulation with a specified weight. 

```bash
python deep_mind_example.py --weights 0.8
```

A few notes for this part of the experiment:

- We randomly sample the weight $w$ from $[0.5, 1]$ and launch mutliple runs corresponding to the different $w$.
- For each $w$, we run independent experiments for ``round`` times (default=10) and then take the average over the obtained $R$'s.
- The exploration iterations and the total iterations can be configured in the code through the function ``easy_signal_setting(args)``.


## Experimental Results





---

## References

[1] Gemp, Ian, et al. "Designing all-pay auctions using deep learning and multi-agent simulation." Scientific Reports 12.1 (2022): 16937.