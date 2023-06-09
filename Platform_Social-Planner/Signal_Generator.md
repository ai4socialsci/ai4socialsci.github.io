### Summary
As denoted in the figure, a classic value based auction consist of the following component. The detailed auction format defination is located in  ("/mini_env/base_signal_mini_env.py"). <br />![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1681286556050-4a1f1e27-fa35-4b98-8bff-ab282b435b42.png#clientId=u05dcf58c-1745-4&from=paste&height=316&id=pYdOz&originHeight=316&originWidth=755&originalType=binary&ratio=1&rotation=0&showTitle=false&size=155873&status=done&style=none&taskId=u659a2a12-fc5c-40f3-9611-78ff1a60f9e&title=&width=755)<br />Fig. Signal-based Mini-env Setting 

The mini-env holds the similar configuration and classic configuration adjustments can read the Page of "custom configuration". We introduce adjustment towards the signal definiation.

### custom signal 
We follow the classic economical hypothesis that the observed signal can be divided into a public signal and a private signal (can be None) and we implement these two signal seperated. The final sigal realization is defined in the base_signal_mini_env.py by update_env_public_signal  function to generate each round signal.  The update function is build upon two signal generator : public signal generator and private signal generator, where we can custom signal by custom these two signal generator.

##### custom public signal 
In the example, we apply a custom public signal through a custom signal generator named platform_Signal_v2_rnd, which is defined in the "/pricing_signal/plat_deliver_signal.py"

The custom signal inherit from the base class Signal, which is defined in the "/agent_utils/signal.py"). The main signal configuration is the whole dimension of the public signal and the generate distribution.The generate distribution is defined in the generate_signal  func (see /pricing_signal/plat_deliver_signal.py). In the example, we apply Uniform distrbution, and other distribution is also supported.
```python
    def generate_signal(self, init=True, data_type='int',upper_bound=None):
        if init:
            self.signal_init()

        if upper_bound is not None:
            cur_upper_bound=upper_bound
        else:
            cur_upper_bound=self.upper_bound

        if self.dtype == "Discrete":
            if self.generation_method == 'uniform':
                for i in range(self.feature_dim):
                    rnd_signal = np.random.uniform(self.lower_bound, cur_upper_bound)  # [low , high]
                    if data_type == 'int':
                        rnd_signal = int(rnd_signal)
                    self.signal_realization[i] = rnd_signal

        return
```



Then we set the public signal generator through the python code (see in the test_signal_game.py)
```python
        public_signal_generator = platform_Signal_v2_rnd(dtype='Discrete', feature_dim=args.public_signal_dim,
                                                     lower_bound=1, upper_bound=args.public_signal_range,
                                                     generation_method='uniform',
                                                     value_to_signal=args.value_to_signal,
                                                     public_signal_asym=args.public_signal_asym
                                                     )  # assume upperbound

        dynamic_env.set_public_signal_generator(public_signal_generator)
        #set s->v func
        dynamic_env.set_env_public_signal_to_value(
            platform_public_signal_to_value=platform_public_signal_to_value_mode3
        )
        #set the update policy
        dynamic_env.set_update_env_public_signal(update_env_public_signal = test_update_env_public_signal)
```

##### custom private signal
Private signal class can also inherit from the base class "Signal" and different agents possess different private signal generator. 

##### custom signal distribute method 
In our implement, each bidder can only witness part or the whole signal from the public signal, which is determined by the platform (or the auctionee). We apply "agent_obs_dim_list" to control different agent observed signal dimension.(see init_agent_obs_dim_list  in the base_signal_mini_env.py)

In order to custom signal distrbute method, we can adjust different user observed signal or even adjust signal itself. <br />The observed dimension list can be adjust through the func (called by dynamic_env.set_agent_obs_dim)
```python
    def set_agent_obs_dim(self,agent_name,observed_dim_list):
        
        self.agt_list[agent_name].set_public_signal_dim(observed_dim_list=observed_dim_list)
```

The reshape of public signal is also supported, which is called from the base_signal_mini_env.py by  set_agent_signal_observation defined in /mini_env/mini_env_utils.py 
```python
            public_signal = set_agent_signal_observation(
                public_signal_generator = self.public_signal_generator,
                agent_name=agent_name,
                agt=agt,
                mode='default'

            )
```

You can custom any signal intervention through the function. We show the default function as following,
```python
def set_agent_signal_observation(
        public_signal_generator=None,
        agent_name='',
        agt=None,
        mode='default'
):
    #use default partial observation list
    if mode =='default' :
        return public_signal_generator.get_partial_signal_realization(
            observed_dim_list = agt.get_public_signal_dim_list()
        )
    elif mode =='all':
        #denote as return all signal
        return public_signal_generator.get_whole_signal_realization()

    elif mode =='modify':
        #apply noise or other method
        original_signal = public_signal_generator.get_partial_signal_realization(
            observed_dim_list = agt.get_public_signal_dim_list()
        )
        if agent_name =='??':
            noise = 1
        else:
            noise=0


        return original_signal + noise
    else:
        return None

```

---

### Reference
