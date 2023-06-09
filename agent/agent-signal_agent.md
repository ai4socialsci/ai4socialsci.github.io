### Introduction
We also build another classical agent named "signal agent" where agent will receive the signals on each step. We seperate the signal into public signal and private signal [1] , where the signal game environment will deliver both the private signal and public signal to bidders (See Fig1). 

![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1684316183582-3117229f-fd04-4ff2-9622-d171ba635eac.png#clientId=u9b17d421-8bc0-4&from=paste&height=244&id=ua5b28348&originHeight=244&originWidth=542&originalType=binary&ratio=1&rotation=0&showTitle=false&size=100860&status=done&style=none&taskId=ua81f8a11-f551-4428-b5aa-b358843b1b0&title=&width=542)<br />Fig1. Signal game interface.

As a results, we build a signal agent inherit from the base agent and extend more basic funtion required in the signal game (See Fig2).<br /> <br />![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1684316871492-54c0e792-f3e2-4c7d-a233-d901e4506b34.png#clientId=uca3ccd8c-e0f2-4&from=paste&height=233&id=u43c56a4e&originHeight=233&originWidth=482&originalType=binary&ratio=1&rotation=0&showTitle=false&size=85270&status=done&style=none&taskId=u3d91b289-79bb-43aa-8b09-86a6625b358&title=&width=482)<br />Fig1. Signal agent class.
### Basic prosperity
We introduce the basic prosperity in the signal agent, where the class name is  "signal_agent " , and the detail codes are located in "./agent/signal_agent.py". 
```python
class signal_agent(base_agent):
    """
    signal agent generates its action based on certain greedy 'self.algorithm'.
    his public signal and private signal devote to his true value
    """

    def __init__(self, agent_name='', obs_public_signal_dim=2, max_public_dim=5, public_signal_range=10,
                 private_signal_generator=None):
        super(signal_agent, self).__init__(agent_name)

        # public signal
        self.obs_public_signal_dim = obs_public_signal_dim  # how much dim of the public signal is observed

        self.public_signal_dim_list = None
        self.max_public_dim = max_public_dim
        self.public_signal_range = public_signal_range  # user observed maximal signal upper bound

        # private signal
        self.private_signal_generator = private_signal_generator

        # other profile
        self.last_encode_signal = None
        self.last_public_signal_realization = None
```


The extended function is listed below.

| **Function** | **Explanation** |
| --- | --- |
| `set_public_signal_dim (observed_dim_list ,rnd)    ` | Set the received public signal format. |
| `encode_signal  (public_signal ,private_signal )    ` | From signal to a value (or state), encode for the algorithm optimization |

#### 
Detailed implement

We provide the implement on the function"encode_signal", more details can be found in the "./agent/signal_agent.py".
```python
    def encode_signal(self, public_signal, private_signal=None):
        """
        from signal to a value (or state)
        encode for the algorithm optimization
        in signal , None = not observed
        in state ,  v1: no deal with not observed signal = no idea about further information on non-observed signal
        """
        state = 0

        if private_signal is None:
            cnt = 0
            for obs_dim in (self.public_signal_dim_list):
                obs_signal = public_signal[obs_dim]
                # check if it is none:
                if obs_signal is None:
                    print('exist None in ' + str(self.agent_name) + 'signal_dim is ' + str(obs_signal))
                else:
                    state += obs_signal * ((self.public_signal_range + 1) ** cnt) #[0-H]
                cnt += 1

        return state
```


---

### Reference
[1] Lundholm R J. Public signals and the equilibrium allocation of private information[J]. Journal of Accounting Research, 1991, 29(2): 322-349.
