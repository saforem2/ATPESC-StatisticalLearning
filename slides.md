---
title: "Statistical Learning"
center: true
width: 960
height: 700
margin: 0.08
theme: black
transition: slide
preloadIframes: false
highlightTheme: 'monokai'
revealOptions:
   transition: 'slide'
css: 
- custom/graze-pro.css
- custom/font-awesome-animation-min.css
- css/animate.min.css
- https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css
---

<!-- .slide style="vertical-align:bottom!important;" -->

<grid drop="5 20" drag="90 25" style="font-family:'graze-pro';background-color:#303030;border-radius:8px!important;padding:auto;align:center;font-size=0.8em!important;">
# Statistical Learning<!-- .element style="color:#F8F8F8;" -->

#### ATPESC 2022 <!-- .element style="color:#757575;font-weight:600;font-family:JuliaMono" -->
</grid>

<grid drop="0 55" drag="100 30" style="line-height:0.6em;" align="top">
Sam Foreman <!-- .element style="font-size:1.5em;font-weight:600;color:#E0E0E0!important;font-family:JuliaMono" -->

2022-08-12 <!-- .element style="font-family:'JuliaMono'; font-size:0.8em;color:#505050;vertical-align:bottom;padding-top:5%;font-weight:400;" -->
</grid>

<grid drop="2 80" drag="10 10" align="bottom" style="">
<a href="https://www.samforeman.me"><i class="fas fa-home fa-1x" style="font-size:1.5em;" alt="`fas:Home`"/> </a>
</grid>

<grid drop="18 80" drag="10 10" align="bottom" style="">
<a href="https://www.github.com/saforem2/ATPESC-ML-Talk" ><i class="fab fa-github fa-1x" style="font-size:1.5em;" alt="`fas:Github`" />
</grid>

<grid drop="10 80" drag="10 10" align="bottom" style="">
<a href="https://www.twitter.com/saforem2"><i class="fab fa-twitter fa-1x" style="width=100%;align=right;font-size:1.5em;color;#00CCFF!important;" alt="`fas:Twitter`" /></a>
</grid>

<grid drop="70 75" drag="30 20" align="center" style="">
<a href="https://anl.gov"><img src="https://raw.githubusercontent.com/saforem2/anl-job-talk/main/docs/assets/anl.svg" width="100%" alt="Argonne National Laboratory" /></a>
</grid>

---
<!-- .slide style="text-align:left;text-size:100%;line-height:1.5em" -->
# First Steps

<div style="line-height:2.5em!important;">

1. Login and submit an interactive job (on Polaris):
  ```shell
  $ ssh <username>@polaris
  $ # ATPESC queue: -q R313446 (in general: -q prod)
  $ qsub -I -q R313446 -l select=32 -l walltime=01:00:00
  ```
  this will launch a job with $32$ ranks ($\times 4 = 128$ GPUs) for $1$ hour [(more on queues / scheduling)](https://argonne-lcf.github.io/user-guides/polaris/queueing-and-running-jobs/job-and-queue-scheduling/)

2. From the interactive job, clone the github repo:<br>
  [<i class="fab fa-github fa-1x" alt="`fas:Github`"/> ATPESC_MachineLearning](https://github.com/argonne-lcf/ATPESC_MachineLearning)
  ```shell
  $ hostname
  x3002c0s31b0n0
  $ git clone https://github.com/argonne-lcf/ATPESC_MachineLearning
  $ cd ATPESC_MachineLearning/00_statisticalLearning/
  ```

</div>

---
<!-- .slide style="text-align:left;text-size:70%;" -->

# Setup / Install

 ```shell
  $ tree ATPESC_MachineLearning/00_statisticalLearning/
  📁 src/
  └── 📁 atpesc/
      ├── 🐍 common.py
      ├── 📁 notebooks/
      │   └── 📕 statistical_learning.ipynb
      └── 📁 utils/
          └── 🐍 plots.py
  ```

<div id="note">

- <span style="color:#63ff5b;">**Goal:**</span> Use functions located in `common.py` and `utils.py` from within our Jupyter notebook.

</div>


- To do this we:
	1. Create a python `venv` which we will use to launch our `jupyter notebook`
	2. From within this `venv`, perform a local (editable) install `python3 -m pip install -e .`

note:
- In order to use the functions located in the `common.py` and `utils/plots.py` modules, 
- In order to use the functions located in the modules `common.py`, `utils/plots.py`, we can perform a local (editable) install
- To make sure that these packages are also `import`-able from our Jupyter notebook, we create a `venv` 
  we can perform a local (editable) install from within a `venv`.

---
<!-- .slide style="text-align:left;text-size:70%;" -->

## Jupyter Notebooks

1. Load `base` conda environment (as a starting point):
  ```shell
  $ module load conda/2022-07-19  # from our interactive job
  $ conda activate base
  ```

2. Create (isolated) `venv` and perform local install:
  ```shell
  $ cd ATPESC_MachineLearning/00_statisticalLearning/
  $ python3 -m venv venv --system-site-packages
  $ source venv/bin/activate
  $ # will install `atpesc` into the `venv`
  $ python3 -m pip install -e .
  ```

4. Install Jupyter kernel and launch notebook
  ```shell
  $ python3 -m pip install ipykernel
  $ python3 -m ipykernel install --user --name="2022-07-19-ATPESC" \
      --display-name="2022-07-19 ATPESC"
  $ jupyter notebook --port=8899 --no-browser > /tmp/jlab8899.log 2>&1 &
  $ hostname
  x3002c0s31b0n0
  ```

---
<!-- .slide style="text-align:left;text-size:60%;" -->

## Port Forwarding

![](work/talks/ATPESC/assets/port-forwarding.svg) <!-- .element align="stretch" -->


---
<!-- .slide style="text-align:left;text-size:60%;" -->

### Port Forwarding

Connect `localhost` to compute node running Jupyter.

6. Starting from your <span style="color:#00CCFF">**local machine**</span>
  ```bash
  # 1: localhost <--> polaris-login-01
  ssh -L localhost:8899:localhost:8899 <username>@polaris.alcf.anl.gov
  # 2: polaris-login-01 <--> compute node
  ssh -L localhost:8899:localhost:8899 <username>@x3002c0s31b0n0
  ```

7. From a web browser on your local machine, navigate to:
  [`https://localhost:8899/`](https://localhost:8899)

<div style="font-size:0.75em;" align="center">

> [!warning] Warning!
> Only **one** port (`8899` in this example) can be used at a time.
> Because of this, if someone is already using the port you try and specify, your connection **WILL NOT WORK**
> To remedy this, choose a different port (e.g. `8891`, `8873`, etc.)

</div>

---
<!-- .slide style="text-align:left;" -->

# <span style="border-bottom:8px solid #00CCFF;"> Now, on with the show...</span>

---
<!-- .slide style="text-align:left;" -->

# Line Fitting

Linear regression via [_stochastic gradient descent_](https://en.wikipedia.org/wiki/Stochastic_gradient_descent) (SGD).

1. Load data into pandas DataFrame:
  ```python
  import pandas as pd
  from pathlib import Path
  from atpesc.common import DATA_DIR
  data_file = Path(DATA_DIR).joinpath('realestate_train.csv')
  df = pd.read_csv(data_file)
  ```

2. Extract total home square footage
  ```python
  area = sum([
      df['1stFlrSF'],
      df['2ndFlrSF'],
      df['TotalBsmtSF']  
  ])
  area.name = 'SqFt'
  price = df['SalePrice']
  plot = sns.jointplot(x=area, y=price, alpha=0.33)
  ```

---

Sale Price vs. Square Footage

![](./assets/price_vs_area.svg)  <!-- .element align="stretch" -->

---
<!-- .slide style="text-align:left;" -->

# Linear Regression

- We can fit the data with a line in order to estimate future sale prices based on home size.

- Assume a simple linear relationship ($y = m\cdot x$)

<div align="center">

$\mathrm{price} = m \cdot \mathrm{area}$

</div>

> [!faq] How does our prediction fit the data?
> In order to evaluate how well our predictions match the data, we can use the [Mean Squared Error](https://en.wikipedia.org/wiki/Mean_squared_error) (MSE):
> <div align="center">
> $\mathrm{MSE} \equiv \delta(y, \hat{y}) = \frac{1}{N}\sum_{i=1}^{N} \left(y_{i} - \hat{y}\right)^{2}$
> </div>

---
<!-- .slide style="text-align:left;" -->

# Linear Regression
```python
def predict_price(slope, area):
    return slope * area

def evaluate(slope, area, true_price):
    prediction = predict_price(slope, area)
    return np.mean((true_price - prediction) ** 2)
```

---
<!-- .slide style="text-align:left;font-size:80%;" -->

## Linear Regression
- Recall our prediction is given by $\textcolor{#63ff5b}{y} = \textcolor{#F92672}{m}\cdot \textcolor{#00CCFF}{x}$, where:
	- $\textcolor{#F92672}{m}$ is the <span style="color:#F92672;">**slope**</span> (randomly initialized)
	- $\textcolor{#00CCFF}{x}$ is the <span style="color:#00CCFF;">input area</span>
	- $\textcolor{#63ff5b}{y}$ is the <span style="color:#63ff5b;">predicted price</span>

- We update our slope $m$ using the update policy:
  $$
  \begin{align}
  \textcolor{#F92672}{m} &\gets  \textcolor{#F92672}{m} - \textcolor{#AE81FF}{\alpha} \,\nabla\, \delta\left(\textcolor{#63ff5b}{y}, \textcolor{#FFFF00}{\hat{y}}\right) \\
  & = \textcolor{#F92672}{m} - \frac{\textcolor{#AE81FF}{\alpha}}{n} \sum_{i=1}^{n} 2 \left(\textcolor{#63ff5b}{y_{i}} - \textcolor{#FFFF00}{\hat{y}}\right)\cdot \frac{\partial \textcolor{#63ff5b}{y_{i}}}{\partial \textcolor{#F92672}{m}} \\
  & = \textcolor{#F92672}{m} - \frac{\textcolor{#AE81FF}{\alpha}}{n}\sum_{i=1}^{n} 2 \left(\textcolor{#63ff5b}{y_{i}} - \textcolor{#FFFF00}{\hat{y}}\right)\cdot \textcolor{#00CCFF}{x}
  \end{align}
  $$
  here:
  - $\textcolor{#FFFF00}{\hat{y}}$ is the <span style="color:#FFFF00">true price</span>
  - $\textcolor{#AE81FF}{\alpha}$ is the <span style="color:#AE81FF">learning rate</span>

---
<!-- .slide style="text-align:left;" -->

## Linear Regression

```python
def learn(
    area,
    slope,
    true_price,
    lr = 0.000001,
):
    prediction = predict_price(slope, area)
    dfdx = 2. * np.mean((prediction - true_price) * area)
    new_slope = slope - learning_rate * dfdx
    return new_slope

---
<!-- .slide style="text-align:left;font-size:80%;" -->

## Stochastic Gradient Descent

- Each application of the `learn` function updates the slope and the `learning_rate` dampens that update.
	- This iterative method helps to find the value of $x$ that _minimizes_ the gradient $\tfrac{df}{dx}$.
- The `learning_rate` controls the size of the update step when updating $x$.
	- If **too large**, you may miss the minimum
	- If **too small**, you may never reach it


![](./assets/learning-rate.svg) <!-- .element align="stretch" -->

---
## Linear Regression

![](./assets/best-fit.svg) <!-- .element align="stretch" -->

---

<!-- .slide style="text-align:left;font-size:150%;" -->
# <span style="border-bottom:8px solid #F92672;">Data Clustering</span>

---
# Data Clustering</span>

![](./assets/clusters2d.svg) <!-- .element align="stretch" -->

</div>

---
<!-- .slide style="text-align:left;font-size:80%;" -->

## Data Clustering

<div align="center" style="font-size:0.8em;">

> [!info] Normalization
> Always a good idea to normalize your data

</div>

```python
from sklearn.preprocessing import StandardScaler
xn = StandardScaler().fit_transform(x)
```

![](./assets/clusters-normalized.svg)  <!-- .element align="stretch" -->


<style>

:root {
    --r-heading-text-transform: none;
    --r-heading-margin: 0 0 20px 0;
    --r-heading-font: "graze-pro", 'Inter', "OpenSans-Bold", "Open Sans", Helvetica, Impact, sans-serif;
    --r-main-background-color: #1c1c1c;
    --r-main-font: "Inter", "graze-pro", "Open Sans", "Coming Soon", "SourceSansPro", Helvetica Neue, sans-serif;
    --r-main-font-size: 34px;
    --r-block-margin: 5px;
    --r-heading-margin: 0 0 20px 0;
    --r-heading-line-height: 1.2;
    --r-heading-letter-spacing: -0.45px;
    --r-heading-word-spacing: 0.5px;
    --r-heading-text-transform: none;
    --r-heading-text-shadow: none;
    --r-heading-font-weight: 800;
    --r-heading1-text-shadow: none;
    --r-heading1-size: 1.5em;
    --r-heading2-size: 1.25em;
    --r-heading3-size: 1.2em;
    --r-heading4-size: 1.1em;
    --r-heading5-size: 1.05em;
    --r-heading6-size: 1.025em;
	--r-monospace-size: 1em;
    --r-code-font: "JuliaMono", "agave Nerd Font", monospace;
    --r-link-color: #007DFF;
    --r-link-color-dark: #f92672;
    --r-link-color-hover: #63ff51;
    --r-controls-color: #228BE6;
    --r-progress-color: #404040;
    --r-selection-background-color: rgba(255, 255, 0, 0.15);
    --r-selection-color: rgb(255, 255, 0);
    --r-main-color: #c8c8c8;
    --text-muted: #757575;
    --text-faint: #404040;
    --r-heading-color: #FFF;
    --r-background-color: #fff;
    -webkit-font-smoothing:subpixel-antialiased;
}

.reveal pre {
  display: block;
  margin: auto;
  text-align: left;
  width: auto;
  font-family: var(--r-code-font);
  line-height: 1.2em;
  word-wrap: break-word;
  max-width:99%;
  margin-right: 1%;
  padding: auto;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.15);
}

.reveal {
    font-family: var(--r-main-font), sans-serif;
    font-size: var(--r-main-font-size);
    font-weight: normal;
    color: var(--r-main-color);
    background-color: var(--r-main-background-color);
}

.reveal h1,
.reveal h2,
.reveal h3,
.reveal h4 {
    margin: var(--r-heading-margin);
    color: var(--r-heading-color);
    font-family: var(--r-heading-font);
    line-height: var(--r-heading-line-height);
    word-spacing: var(--r-heading-word-spacing);
    text-transform: var(--r-heading-text-transform);
    text-shadow: var(--r-heading-text-shadow);
    word-wrap: break-word;
}

.reveal h1 {
    font-weight: 800;
    font-size: var(--r-heading1-size);
}

.reveal h2 {
    font-weight: 700;
    font-size: var(--r-heading2-size);
}

.reveal h3 {
    font-weight: 700;
    font-size: var(--r-heading3-size);
}

.reveal h4 {
    font-weight: 700;
    font-size: var(--r-heading4-size);
}

.reveal h5 {
    font-weight: 600;
    font-size: var(--r-heading5-size);
}
.reveal h6 {
    font-weight: 500;
    font-size: var(--r-heading6-size);
}

.reveal ul ul,
.reveal ul ol,
.reveal ol ol,
.reveal ol ul {
  margin-bottom: 10px;
}

.reveal ul:not(:last-child) {
    margin-bottom: 2px;
}

.reveal ul:last-child {
    margin-bottom: 20px;
}

.reveal ul ul {
    list-style: circle;
}
.container {
  position: relative;
}

.make-it-pop {
  filter: drop-shadow(0 0 10px purple);
}

.bottomright {
  position: absolute;
  bottom: 8px;
  right: 16px;
  font-size: 18px;
}

@media (max-width: 95%) {
  section {
    -webkit-flex-direction: column;
    flex-direction: column;
  }
}

.row {
  display: flex;
}

.column {
  flex: 50%;
}


#left {
  margin: 0 0 5px 5px;
  text-align: left;
  float: left;
  z-index: -10;
  width: 48%;
  font-size: 0.85em;
}

#right {
  margin: 0 0 5px 0;
  float: right;
  max-width: 48%;
  text-align: left;
  z-index: -10;
  width: 48%;
  font-size: 0.85em;
}

#darkBack {
    background-color: #1c1c1c;
    color: #efefef;
    .reveal a {
        color: #F92672;
        transition: color 0.15s ease;
    }
    .reveal a:hover {
        color: var(--r-link-color-hover);
    }
}

.multiCol {
    display: table;
    table-layout: fixed; /* don't fudge depending on content */
    width: 100%;
    text-align: left; /* matter of taste, makes imho sense */
    .col {
        display: table-cell;
        vertical-align: top;
        width: 50%;
        padding: 2% 0 2% 3%; /* some vertical, and between columns */
        &:first-of-type { padding-left: 0; } /* there's nothing before col1 */
    }
}

.reveal blockquote:before {
  content: "❝";
  font-size: 2.5em;
  margin-right: .05em;
  line-height: 0.1em;
  vertical-align: -0.3em;
  text-align: left;
  color: var(--text-faint);
  font-style: normal !important;
}

.reveal blockquote p {
  color: var(--text-muted);
  font-style: normal !important;
  font-align: left;
  display: inline;
  text-align: left;
}

.reveal blockquote em{
  color: var(--text-muted);
  text-align: left;
}

.reveal blockquote {
  border-radius: 8px !important;
  margin: 0.5rem 0rem 0.5rem 0rem;
  text-align: left;
  padding-top: 1rem;
  padding-left: 2rem;
  padding-bottom: 1rem;
  padding-right: 2rem;
  width: auto;
  <!-- max-width: 90%; -->
  font-style: normal !important;
}

.hljs {
    background: #202020 !important;
    border-radius: 8px !important;
}

#blue {
    color: #00CCFF;
}
#bright {
    color: #00A2FF;
}
#cyan {
    color: #00CCFF;
}
#purple {
    color: #AE81ff;
}
#green {
    color: #63FF51;
}
#yellow {
    color: #FFFF00;
}
#lightpink {
    color: #E64980;
}
#pink {
    color: #F92672;
}
#red {
    color: #FA5252;
}
#grey {
    color: #666666;
}

#noteinverse {
    background-color: #1c1c1c;
    border-radius: 5px;
    border-color: #1c1c1c;
    color: #efefef;
    padding: auto;
    margin: auto;
}

#note {
    background-color: rgba(255, 255, 255, 0.1);
    padding: 10px;
    border-radius: 8px;
    border-color: rgba(240, 240, 240, 1.0);
    color: rgb(255, 255, 255);
    margin: auto;
}

#mark {
    margin: auto;
    padding: auto;
    background-color: #FFFF00;
    color: #1c1c1c;
    font-weight: 700;
    border-radius: 7px;
}

#dark {
    background-color: #1c1c1c;
    color: #efefef;
    --r-link-color: #00CCFF!important;
    --r-header-color: #666666;
    -webkit-font-smoothing:subpixel-antialiased;
}

#halfsize {
    font-size: 0.5em;
}

#large {
  font-size: 1.25em;
}

.horizontal_dotted_line{
  border-bottom: 2px dotted gray;
} 
.footer {
  font-size: 60%;
  vertical-align:bottom;
  color:#bdbdbd;
  font-weight:400;
  margin-left:-5px;
}
.note {
  color:#f8f8f8;
  border-radius:8px;
  background-color:#35353540;
  border-color:#66666640;
  padding: auto;
  margin:auto;
}

.callout {
  border-left: 4px solid rgb(var(--callout-color));
  border-radius: 2px;
  font-size: 0.9em;
  background-color: #35353540;
  margin: 1em 0;
  text-align:left;
}
.callout-title {
  padding: 10px;
  display: flex;
  gap: 10px;
  text-align:left;
  color: #f8f8f8;
  font-size: 0.95em;
  line-height: 1em;
  background-color: rgba(var(--callout-color), 0.3);
}


</style>