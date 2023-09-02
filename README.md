# Rethinking the Evaluation for Conversational Recommendation in the Era of Large Language Models

This repo provides the source code & data of our paper: Rethinking the Evaluation for Conversational Recommendation in the Era of Large Language Models.

## 😀 Overview

**Highlights**:
- 1️⃣ We are the first to examine ChatGPT in conversational recommendation systematically, the ability of which is **underestimated** in traditional evaluation approach.
- 💡 We propose a new interactive approach that employs LLM-based user simulators for evaluating CRSs.
- 🔝 The performance of chatgpt can be boosted with our new interactive evaluation approach, even surpassing the currently leading CRS baseline.

we propose an **i**nteractive **Eval**uation approach based on **LLM**s named **iEvaLM** that harnesses LLM-based user simulators. We take the ground-truth items from the example as the user preference through the interaction, and use them to set up the persona of the simulated users by LLMs through instructions. To further make a comprehensive evaluation, we consider two types of interaction: *attribute-based question answering* and *free-form chit-chat*.

<p align="center">
  <img src="./asset/eval.png" width="75%" height="75% title="Overview of iEvaLM-CRS" alt="">
</p>


## 🚀 Quick Start

### Requirements

- python == 3.9.16
- pytorch == 1.13.1
- transformers == 4.28.1
- pyg == 2.3.0
- accelerate == 0.18.0

### Download Models

You can download our fine-tuned models from the [link](https://drive.google.com/file/d/1XT6L6H7y2PyvAUf4JLx_tbo2nWz800XH/view?usp=sharing), which include recommendation and conversation models of **KBRD**, **BARCOR** and **UniCRS**. Please put the downloaded model into src/utils/model directory.

### Interact with the user simulator

- dataset: [redial, opendialkg]
- mode: [ask, chat]
- model: [kbrd, barcor, unicrs, chatgpt]

```bash
cd script
bash {dataset}/cache_item.sh 
bash {dataset}/{mode}_{model}.sh 
```

You can customize your iEvaLM-CRS by specifying these configs:
- `--api_key`: your API key
- `--turn_num`: number of conversation turns. We employ five-round interaction in iEvaLM-CRS.

After the execution, you will find detailed interaction information under "save_{turn_num}/{mode}/{model}/{dataset}/".

### Download intermediate results.

You can download the intermediate results from this [link](https://drive.google.com/drive/folders/1zXMaU5AVFEViZVcYNkE9K9pUgyitcMOz?usp=sharing).

### Evaluate

```bash
cd script
bash {dataset}/Rec_eval.sh
```

You can customize your iEvaLM-CRS by specifying these configs:
- `--turn_num`: number of conversation turns.
- `--mode`: [ask, chat]

After the execution, you will find evaluation results under "save_{turn_num}/result/{mode}/{model}/{dataset}.json".

### Result

<p align="center">Performance of CRSs and ChatGPT using different evaluation approaches.</p>
<table border="1" align="center">
  <tbody >
  <tr align="center">
    <td colspan="2">Model</td>
    <td colspan="3">KBRD</td>
    <td colspan="3">BARCOR</td>
    <td colspan="3">UniCRS</td>
    <td colspan="3">ChatGPT</td>
  </tr>
  <tr align="center">
    <td colspan="2">Evaluation Approach</td>
    <td>Original</td>
    <td>iEvaLM(attr)</td>
    <td>iEvaLM(free)</td>
    <td>Original</td>
    <td>iEvaLM(attr)</td>
    <td>iEvaLM(free)</td>
    <td>Original</td>
    <td>iEvaLM(attr)</td>
    <td>iEvaLM(free)</td>
    <td>Original</td>
    <td>iEvaLM(attr)</td>
    <td>iEvaLM(free)</td>
  </tr>
  <tr align="center">
    <td rowspan="3">ReDial</td>
    <td>R@1</td>
    <td>2.8</td>
    <td>3.9(±1.28)</td>
    <td>3.5(±1.26)</td>
    <td>3.1</td>
    <td>3.4(±0.62)</td>
    <td>3.4(±0.36)</td>
    <td>5.0</td>
    <td>5.3(±1.66)</td>
    <td>10.7(±2.49)</td>
    <td>3.7</td>
    <td><b>19.1(±1.02)</b></td>
    <td>14.6(±0.91)</td>
  </tr>
  <tr align="center">
    <td>R@10</td>
    <td>16.9</td>
    <td>19.6(±2.74)</td>
    <td>19.8(±2.86)</td>
    <td>17.0</td>
    <td>20.1(±1.70)</td>
    <td>19.0(±3.48)</td>
    <td>21.5</td>
    <td>23.8(±2.31)</td>
    <td>31.7(±2.65)</td>
    <td>13.0</td>
    <td><b>53.6(±2.79)</b></td>
    <td>44.0(±0.77)</td>
  </tr>
  <tr align="center">
    <td>R@50</td>
    <td>36.6</td>
    <td>43.6(±1.23)</td>
    <td>45.3(±1.99)</td>
    <td>37.2</td>
    <td>42.7(±1.63)</td>
    <td>46.7(±2.55)</td>
    <td>41.3</td>
    <td>52.0(±1.22)</td>
    <td>60.2(±1.25)</td>
    <td>21.8</td>
    <td><b>73.9</b></td>
    <td>70.5</td>
  </tr>
  <tr align="center">
    <td rowspan="3">OpenDialKG</td>
    <td>R@1</td>
    <td>23.1</td>
    <td>13.1(±3.68)</td>
    <td>23.4(±0.73)</td>
    <td>31.2</td>
    <td>26.4(±1.44)</td>
    <td>31.4(±2.40)</td>
    <td>30.8</td>
    <td>18.0(±0.54)</td>
    <td>31.4(±3.48)</td>
    <td>31.0</td>
    <td>29.9(±3.12)</td>
    <td><b>40.0(±2.93)</b></td>
  </tr>
  <tr align="center">
    <td>R@10</td>
    <td>0.423</td>
    <td>0.293(±0.60)</td>
    <td>0.431(±1.87)</td>
    <td>0.453</td>
    <td>0.423(±2.09)</td>
    <td>0.458(±2.07)</td>
    <td>0.513</td>
    <td>0.393(±4.25)</td>
    <td>0.538(±3.22)</td>
    <td>0.539</td>
    <td>0.604(±2.92)</td>
    <td><b>0.715(±1.87)</b></td>
  </tr>
  <tr align="center">
    <td>R@50</td>
    <td>49.2</td>
    <td>37.7(±1.47)</td>
    <td>50.9(±0.82)</td>
    <td>51.0</td>
    <td>48.2(±1.49)</td>
    <td>53.0(±1.96)</td>
    <td>57.4</td>
    <td>45.8(±3.49)</td>
    <td>60.9(±1.38)</td>
    <td>60.7</td>
    <td>70.8</td>
    <td><b>82.5</b></td>
  </tr>
  </tbody>

</table>
