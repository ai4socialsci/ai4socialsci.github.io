#### Summary 
In the auction theory, the mechanism design is mainly concentrated on the payment rule and allocation rule as well as other constraints. In our AI4SS platform, we provide two basic class of "payment rule" and "allocation rule" in the  "./env/payment_rule.py" and "./env/allocation_rule.py" wrapped by a mechanism class named "Policy" in the "./env/policy.py".

![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1684236550738-69838d1c-a1d7-4708-a92f-606adc79053e.png#clientId=u4aee9180-b408-4&from=paste&height=185&id=ubaac96c0&originHeight=185&originWidth=268&originalType=binary&ratio=1&rotation=0&showTitle=false&size=43211&status=done&style=none&taskId=ucb4cfa99-974c-4c6d-ba4d-085b298bb2f&title=&width=268)<br />Fig 1. The illustration of the mechanism design in AI4SS.

#### Second Price Mechanism 
Second price mechanism is the mechanism that the **highest** bidder wins the item and pays the **second** highest bid. From the Myerson's optmial auction theory[1], the second price auction with virtual bid and reserve price is the optimal auction. <br />In our platform, we implement the "Second Price" mechanism with default reserve price equalling to 0. <br />When interested in the second price, use the following hyperparameters:

| **Args** | **Explanation** |
| --- | --- |
| `arg.mechanism  = second_price ` | platform applyed mechanism in the mini-env environment |

#### 
#### First Price Mechanism
First price mechanism is one of the most popluar mechansim in the auction scenarioes, where the real-time bidding switched from the "Second Price" to the "First Price" [2]. The First price mechanism is the highest bidder wins the auction and then pay as his bid. <br />The first price mechanism will lead to the equilibrium without truthful bidding and the theoretical bidding policies exists only if all the bidders are symmetirc bidders.

In our platform, we implement the "First Price" mechanism with default reserve price equalling to 0. <br />When interested in the second price, use the following hyperparameters:

| **Args** | **Explanation** |
| --- | --- |
| `arg.mechanism  = first_price ` | platform applyed mechanism in the mini-env environment |

#### 

#### 
#### Custom Classical Mechanism
We implement the classic "First price" and the "Second price" mechanism along with "All-pay" mechanism. However, the existence of apossible equilibrium under a custom mechanism can also be evaluated through our platform. The implement of custom mechansim is simple. We divide the auction mechanism into two part: "payment rule" and the "allocation rule", and use "Policy" class to describe it.(See "/env/policy.py" )
```python
policy=Policy(allocation_mode=args.allocation_mode, 
                                                 payment_mode=args.mechanism,
                                                 action_mode=args.action_mode, 
                                                 args=args),
```

Noted that all the mini-envs follow the same interface, which means you can easily switch mechanisms among all the types of the auctions.
##### payment rule
The payment rule is defined in the file (/env/payment_rule.py).<br />We have implement multiple payment rule, including "First/Second/Third Price" as well as multi-item "VCG" and other payment rule.  After the definiation, add the custom payment rule into the Policy.py-compute_payment   func.

The adjust of payment rule can directly adjust Policy class, and the mechnism can changed when payment rule changes during the auction running process. We show an example as below. 
```python
policy.assign_payment_mode(payment_mode='pay_by_submit')

```

##### allocation rule 
The most common allocation rule is the winner allocated. However, random or other allocation rule is also allowed. The allocation rule is defined in the file(). After the definiation, add the custom allocation rule into the Policy.py-compute_allocation  func.<br />The allocation can also adjust through the Policy class.
```python
policy.assign_allocation_mode(allocation='random')

```

---

#### Deep Mechansim Design 

Using deep nerual network to train a deep mechanism is proposed in [3,4], and one illustration is displayed in the following plot.

![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1684235611707-5461ce3f-e0d5-46f8-ad08-f76f14955369.png#clientId=u4aee9180-b408-4&from=paste&height=540&id=u979f1e86&originHeight=540&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=501760&status=done&style=none&taskId=ub9367b16-ac58-4ae3-8493-827177342c1&title=&width=1400)<br />Fig 2. The illustration of the deep mechanism design in the [3]

Generally speaking, the payment rule and allocation is adopted by a deep neural network and we also provide a interface towards the deep mechanism design. (See "./mechansim_design/deep_mechanism.py")<br /> The key implement is the method of "compute_payment" in the payment rule, and the method of "compute_allocation" in the allocation rule. <br />We provide a Pytorch implement example in the "./mechansim_design/deep_allocation.py" and the "./mechansim_design/deep_payment.py" where the final allocation and payment results is computed from a deep neural network output. The model updating details can be found in the "./mechansim_design/mechanism_optimization.py". More details about this example can be found in the document "Deep Mechanism Optmization" in the "Examples" folder. 





---

### Reference
[1]Myerson R B. Optimal auction design[J]. Mathematics of operations research, 1981, 6(1): 58-73.<br />[2][https://support.google.com/adsense/answer/10858748?hl=en&ref_topic=1628432](https://support.google.com/adsense/answer/10858748?hl=en&ref_topic=1628432)<br />[3] Dütting P, Feng Z, Narasimhan H, et al. Optimal auctions through deep learning[C]//International Conference on Machine Learning. PMLR, 2019: 1706-1715.<br />[4] Gemp I, Anthony T, Kramar J, et al. Designing all-pay auctions using deep learning and multi-agent simulation[J]. Scientific Reports, 2022, 12(1): 16937.
