# Reinforcement Learning (1)

## markov decision processes (MDP)：(S,A,P,R,γ)

### 1. state transistion probability 

<a href="https://www.codecogs.com/eqnedit.php?latex=\dpi{150}&space;\large&space;P_{a}^{ss'}=P[S_{t=1}=s',A_{t}=a]" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\large&space;P_{a}^{ss'}=P[S_{t=1}=s',A_{t}=a]" title="\large P_{a}^{ss'}=P[S_{t=1}=s',A_{t}=a]" /></a>

### 2. policy: π

<a href="https://www.codecogs.com/eqnedit.php?latex=\dpi{150}&space;\large&space;\pi(a|s)=p[A_{t}=a|S_{t}=s]" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;\large&space;\pi(a|s)=p[A_{t}=a|S_{t}=s]" title="\large \pi(a|s)=p[A_{t}=a|S_{t}=s]" /></a>

### 3. cumulative return:

<a href="https://www.codecogs.com/eqnedit.php?latex=\dpi{150}&space;\large&space;G_{t}=R_{t&plus;1}&plus;\gamma&space;R_{t&plus;2}&plus;...=\sum_{k=0}^{\infty}\gamma^{^{k}}R_{t&plus;k&plus;1}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;\large&space;G_{t}=R_{t&plus;1}&plus;\gamma&space;R_{t&plus;2}&plus;...=\sum_{k=0}^{\infty}\gamma^{^{k}}R_{t&plus;k&plus;1}" title="\large G_{t}=R_{t+1}+\gamma R_{t+2}+...=\sum_{k=0}^{\infty}\gamma^{^{k}}R_{t+k+1}" /></a>

### 4. state value function

When an agent use policy π， the cumulative return obey a distrubution. State value function is the expected value at state s.

<a href="https://www.codecogs.com/eqnedit.php?latex=\dpi{150}&space;\large&space;V_{\pi}(s)=E_{\pi}[\sum_{0}^{\infty}\gamma^{k}R_{t&plus;k&plus;1}|S_{t}=s]" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;\large&space;V_{\pi}(s)=E_{\pi}[\sum_{0}^{\infty}\gamma^{k}R_{t&plus;k&plus;1}|S_{t}=s]" title="\large V_{\pi}(s)=E_{\pi}[\sum_{0}^{\infty}\gamma^{k}R_{t+k+1}|S_{t}=s]" /></a>

### 5. state action value function

<a href="https://www.codecogs.com/eqnedit.php?latex=\dpi{150}&space;\large&space;q_{\pi}(s,a)=E_{\pi}[\sum_{0}^{\infty}\gamma^{k}R_{t&plus;k&plus;1}|S_{t}=s,A_{t}=a]" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;\large&space;q_{\pi}(s,a)=E_{\pi}[\sum_{0}^{\infty}\gamma^{k}R_{t&plus;k&plus;1}|S_{t}=s,A_{t}=a]" title="\large q_{\pi}(s,a)=E_{\pi}[\sum_{0}^{\infty}\gamma^{k}R_{t+k+1}|S_{t}=s,A_{t}=a]" /></a>

### 6. Bellman equation for state value function

<a href="https://www.codecogs.com/eqnedit.php?latex=\dpi{150}&space;V(S{_{t}})=E(R_{t&plus;1}&plus;\gamma&space;V(S_{t&plus;1}))" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;V(S{_{t}})=E(R_{t&plus;1}&plus;\gamma&space;V(S_{t&plus;1}))" title="V(S{_{t}})=E(R_{t+1}+\gamma V(S_{t+1}))" /></a>

Specific Calculation Process:

<a href="https://www.codecogs.com/eqnedit.php?latex=\dpi{150}&space;v_{\pi}(s)=\sum_{a\in&space;A}\pi(a|s)(R_{s}^{a}&plus;\gamma\sum_{s'\in&space;S}P_{a}^{ss'}v_{\pi}(s'))" target="_blank"><img src="https://latex.codecogs.com/gif.latex?v_{\pi}(s)=\sum_{a\in&space;A}\pi(a|s)(R_{s}^{a}&plus;\gamma\sum_{s'\in&space;S}P_{a}^{ss'}v_{\pi}(s'))" title="v_{\pi}(s)=\sum_{a\in A}\pi(a|s)(R_{s}^{a}+\gamma\sum_{s'\in S}P_{a}^{ss'}v_{\pi}(s'))" /></a>


### 7. Bellman equation for state action value function

<a href="https://www.codecogs.com/eqnedit.php?latex=\dpi{150}&space;q_{\pi}(s,a)=E_{\pi}(R_{t&plus;1}&plus;\gamma&space;q(S_{t&plus;1},A_{t&plus;1})|S_{t}=s,A_{t}=a))" target="_blank"><img src="https://latex.codecogs.com/gif.latex?q_{\pi}(s,a)=E_{\pi}(R_{t&plus;1}&plus;\gamma&space;q(S_{t&plus;1},A_{t&plus;1})|S_{t}=s,A_{t}=a))" title="q_{\pi}(s,a)=E_{\pi}(R_{t+1}+\gamma q(S_{t+1},A_{t+1})|S_{t}=s,A_{t}=a))" /></a>

Specific Calculation Process:

<a href="https://www.codecogs.com/eqnedit.php?latex=\dpi{150}&space;q_{\pi}(s,a)=R_{s}^{a}&plus;\gamma\sum_{s'\in&space;S}P_{ss'}^{a}\sum_{a'\in&space;A}\pi&space;(a'|s')q_{\pi}(s',a')" target="_blank"><img src="https://latex.codecogs.com/gif.latex?q_{\pi}(s,a)=R_{s}^{a}&plus;\gamma\sum_{s'\in&space;S}P_{ss'}^{a}\sum_{a'\in&space;A}\pi&space;(a'|s')q_{\pi}(s',a')" title="q_{\pi}(s,a)=R_{s}^{a}+\gamma\sum_{s'\in S}P_{ss'}^{a}\sum_{a'\in A}\pi (a'|s')q_{\pi}(s',a')" /></a>

### 8. Frequently used random strategy

(1) greedy strategy

<a href="https://www.codecogs.com/eqnedit.php?latex=\dpi{150}&space;$$&space;\pi_{*}(a|s)=\left\{&space;\begin{array}{rcl}&space;1&space;&&space;&&space;{if\,&space;\:&space;a=\underset{a\in&space;A}{argmax}\,&space;q_{*}(s,a)}\\&space;0&space;&&space;&&space;{otherwise}\\&space;\end{array}&space;\right.&space;$$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?$$&space;\pi_{*}(a|s)=\left\{&space;\begin{array}{rcl}&space;1&space;&&space;&&space;{if\,&space;\:&space;a=\underset{a\in&space;A}{argmax}\,&space;q_{*}(s,a)}\\&space;0&space;&&space;&&space;{otherwise}\\&space;\end{array}&space;\right.&space;$$" title="$$ \pi_{*}(a|s)=\left\{ \begin{array}{rcl} 1 & & {if\, \: a=\underset{a\in A}{argmax}\, q_{*}(s,a)}\\ 0 & & {otherwise}\\ \end{array} \right. $$" /></a>

(2) ϵ-greedy stratedy

<a href="https://www.codecogs.com/eqnedit.php?latex=\dpi{150}&space;$$&space;\pi_{*}(a|s)=\left\{&space;\begin{array}{rcl}&space;1-\varepsilon&plus;\frac{\varepsilon}{|A(s)|}&space;&&space;&&space;{if\,&space;\:&space;a=\underset{a\in&space;A}{argmax}\,&space;q_{*}(s,a)}\\&space;\frac{\varepsilon}{|A(s)|}&space;&&space;&&space;{otherwise}\\&space;\end{array}&space;\right.&space;$$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;$$&space;\pi_{*}(a|s)=\left\{&space;\begin{array}{rcl}&space;1-\varepsilon&plus;\frac{\varepsilon}{|A(s)|}&space;&&space;&&space;{if\,&space;\:&space;a=\underset{a\in&space;A}{argmax}\,&space;q_{*}(s,a)}\\&space;\frac{\varepsilon}{|A(s)|}&space;&&space;&&space;{otherwise}\\&space;\end{array}&space;\right.&space;$$" title="$$ \pi_{*}(a|s)=\left\{ \begin{array}{rcl} 1-\varepsilon+\frac{\varepsilon}{|A(s)|} & & {if\, \: a=\underset{a\in A}{argmax}\, q_{*}(s,a)}\\ \frac{\varepsilon}{|A(s)|} & & {otherwise}\\ \end{array} \right. $$" /></a>
