# Reinforcement Learning (1)

## markov decision processes (MDP)：(S,A,P,R,γ)

### state transistion probability 

<a href="https://www.codecogs.com/eqnedit.php?latex=\dpi{120}&space;\large&space;P_{a}^{ss'}=P[S_{t=1}=s',A_{t}=a]" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\large&space;P_{a}^{ss'}=P[S_{t=1}=s',A_{t}=a]" title="\large P_{a}^{ss'}=P[S_{t=1}=s',A_{t}=a]" /></a>

### policy: π

<a href="https://www.codecogs.com/eqnedit.php?latex=\dpi{120}&space;\large&space;\pi(a|s)=p[A_{t}=a|S_{t}=s]" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\large&space;\pi(a|s)=p[A_{t}=a|S_{t}=s]" title="\large \pi(a|s)=p[A_{t}=a|S_{t}=s]" /></a>

### cumulative return:

<a href="https://www.codecogs.com/eqnedit.php?latex=\dpi{120}&space;\large&space;G_{t}=R_{t&plus;1}&plus;\gamma&space;R_{t&plus;2}&plus;...=\sum_{k=0}^{\infty}\gamma^{^{k}}R_{t&plus;k&plus;1}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\large&space;G_{t}=R_{t&plus;1}&plus;\gamma&space;R_{t&plus;2}&plus;...=\sum_{k=0}^{\infty}\gamma^{^{k}}R_{t&plus;k&plus;1}" title="\large G_{t}=R_{t+1}+\gamma R_{t+2}+...=\sum_{k=0}^{\infty}\gamma^{^{k}}R_{t+k+1}" /></a>

### state value function

When an agent use policy π， the cumulative return obey a distrubution. State value function is the expected value at state s.

<a href="https://www.codecogs.com/eqnedit.php?latex=\dpi{120}&space;\large&space;V_{\pi}(s)=E_{\pi}[\sum_{0}^{\infty}\gamma^{k}R_{t&plus;k&plus;1}|S_{t}=s]" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\large&space;V_{\pi}(s)=E_{\pi}[\sum_{0}^{\infty}\gamma^{k}R_{t&plus;k&plus;1}|S_{t}=s]" title="\large V_{\pi}(s)=E_{\pi}[\sum_{0}^{\infty}\gamma^{k}R_{t+k+1}|S_{t}=s]" /></a>

### state action value function

<a href="https://www.codecogs.com/eqnedit.php?latex=\dpi{120}&space;\large&space;q_{\pi}(s,a)=E_{\pi}[\sum_{0}^{\infty}\gamma^{k}R_{t&plus;k&plus;1}|S_{t}=s,A_{t}=a]" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\large&space;q_{\pi}(s,a)=E_{\pi}[\sum_{0}^{\infty}\gamma^{k}R_{t&plus;k&plus;1}|S_{t}=s,A_{t}=a]" title="\large q_{\pi}(s,a)=E_{\pi}[\sum_{0}^{\infty}\gamma^{k}R_{t+k+1}|S_{t}=s,A_{t}=a]" /></a>
