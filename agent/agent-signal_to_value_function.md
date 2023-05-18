Normally, bidders in the auction environment will observe the item valuation before auction. However, in the real world for example, bidders may only observe the item signal or the attribution before allocated. One detailed example is that on the E-commercial platform, you may only observe multiple attributions rather than the detailed item itself and finally realizes the item valuation for you after received.<br />In such situation where bidders only observe signals or the attributions, we may adopt a signal to value function to compute the expected value or the expected revenue before bidding. As in the signal game, bidders only observe the signal from the platform, the true value may know or unknown before win the item. We implement a function called "signal to value" (S2V) function, where this function is unknown from the bidder perspective by default. The mathmatircal formualtion is denoted as below,<br />![](https://intranetproxy.alipay.com/skylark/lark/__latex/c9a06242d1a4e1db7d703dbc02948c2f.svg#card=math&code=V%28%5Ctextbf%7Bs%7D%29%20%3D%20%5Cphi%28s_1%2Cs_2..s_n%29&id=LjHCc)<br />where ![](https://intranetproxy.alipay.com/skylark/lark/__latex/f547e08b7db926659f19deee4b4363d1.svg#card=math&code=%5Cphi&id=Gpgpo) is the S2V function. 

 We take public signals as an exmaple. In the common value auction with public information (signal), the final value is determined by the whole public information[1], and we implement it as below with public S2V function. <br />The default **public signal** S2V function is defined in the base_signal_mini_env.py 
```python
    def platform_public_signal_to_value(self,signal, func='sum', weights=None):
        #
        # denotes the function for signal->value [v=f(s)]
        #
        if signal is None or len(signal) == 0:
            return 0

        if func == 'sum':
            return sum(signal)
        if func == 'avg':
            return sum(signal) / len(signal)

        if func == 'weighted':
            return weighted_sum(signal, weights)
```

In the example, we adjust the public signal S2V function in the test_signal_game.py by 
```python
    dynamic_env.set_env_public_signal_to_value(
            platform_public_signal_to_value=platform_public_signal_to_value
        )
```
where the function platform_public_signal_to_value is defined in the test_signal_game_example_function.py, where you can re-write this function in any names.
```python
def platform_public_signal_to_value(self,signal, func='sum', weights=None):
    #

    value = int( sum(signal) / 2)
    #print('new signal func')
    return value


def platform_public_signal_to_value_mode3(self,signal, func='sum', weights=None):
    #
    #print('new signal func')
    #print(self)
    if max(signal) ==0 :

        signal = self.saved_signal
    if signal is None:
        return 0

    value = int( sum(signal))
    #print('new signal func')

    return value
```

---

### Reference
[1]Kagel J H, Levin D. The winner's curse and public information in common value auctions[J]. The American economic review, 1986: 894-920.
