<!-- <h1 align="center"> Flexible Constructivism Reflection for Long-Horizon Robotic Task Planning with Large Language Models </h1> -->

<!--
<div align='center'>
  <font size=4 color=black>IROS 2025</font>
</div>
-->

<!--
[author1](https://www.yuque.com/zhangjiatao-grdyv/rn49ht/lq7xzy4xmxgrpgz9), [author2](https://www.yuque.com/zhangjiatao-grdyv/rn49ht/vsarazgdts43o7y4)
-->

## Abstract
Autonomous error correction is critical for domestic robots to achieve reliable execution of complex long-horizon tasks. Prior work has explored self-reflection in Large Language Models (LLMs) for task planning error correction; however, existing methods are constrained by inflexible self-reflection mechanisms that limit their effectiveness. Motivated by these limitations and inspired by human cognitive adaptation, we propose the Flexible Constructivism Reflection Framework (FCRF), a novel Mentor-Actor architecture that enables LLMs to perform flexible self-reflection based on task difficulty, while constructively integrating historical valuable experience with failure lessons. We evaluated FCRF on diverse domestic tasks through simulation in AlfWorld and physical deployment in the real-world environment. Experimental results demonstrate that FCRF significantly improves overall performance and self-reflection flexibility in complex long-horizon robotic tasks. 

## Paper
<iframe  width="400" height="420" src="./FCRF_Flexible_Constructivism_Reflection_for_Long_Horizon_Robotic_Task_Planning_with_Large_Language_Models.pdf"></iframe>

## Video

<div align='center'>
  <video id="video" controls="" preload="none" poster="作者(图片地址)">
    <source id="mp4" src="./video1.mp4" type="video/mp4">
  </video>
</div>


## Results
Comparison of FCRF and baseline reflection method in an AlfWorld example:

<div align='center'>
  <img src="./sim_example_00.png">
</div>

## Methodology
The framework of FCRF. The planning process is executed by the Actor LLM, the reflection process is executed by the Mentor LLM. During
the self-reflection process, the difficulty level of the task is first assessed, according to which the Mentor flexibly selects reflection intensity, determine the proportion of simple experience retain and in-depth failure lessons extraction among all the reflection episodes. Combining the valuable experience and failure lesson, the Mentor performs a constructivism self-reflection to guide the next round of planning of the Actor. The reflection results and planning trajectories will be stored in the memory module for long-term management.

<div align='center'>
  <img src="./fig2_00.png">
</div>

<br/>

<div align='center'>
  <img src="./fig3_00.png">
</div>


## Appendix
### 1.Prompt of Valuable Experience Summary Process

    You will be given the history of a past experience in which you were placed in an environment and given a task to complete. 
    You were unsuccessful in completing the task.
    Now by aligning the task goal, summary the valuable actions you have gained in your past experience.
  
    Here are two examples:
    =================The first example=====================
    {experience[f'{v}_0']}
    
    =================The second example=====================
    {experience[f'{v}_1']}
    
    Here is the history of the past experience and your task:
    {scenario}
    
    Strictly follow the format of example, summarize the valuable actions directly in concise language, without adding your own divergent thoughts.
  

### 2.Prompt of Failure Lesson Summary Process
    You will be given the history of a past experience in which you were placed in an environment and given a task to complete. 
    You were unsuccessful in completing the task.
    Now to solve your mistake, I will give you a mentor lesson pool, in which listed typical constraints of your task environment.
    Analyze which tip in the mentor lesson pool might be the reason of your mistake, and then make failure lesson summarization refer to the form of examples given later. 
    The existed tips in the mentor lesson pool can be your inspirations, but don't be limited by them. Summary failure lessons according to your specific task experience.
    Pay attention, only choose ONE tip in the pool fits your task type, and only choose it when you think it might be reason of your mistake. 

    Here is the mentor lesson pool:
    {mentor_lesson_pool}
    
    Here is your task type, remember to extract one through tips in the pool which precisely contains your task type at the beginning:
    {v}

    Here are two examples:
    {lesson}

    Here is the history of the past experience and your task:
    {scenario}

    Strictly follow the format of example, directly extract one lesson from the pool in one sentence, without adding your own divergent thoughts.



### 3.Prompt of Comprehensive Construction Process
    You will be given the history of a past experience in which you were placed in an environment and given a task to complete. 
    You were unsuccessful in completing the task.
    Now I will give you valuable experience summarization and failure lesson summarization based on your previous experience. 
    Please create a plan for the next attempt with reference to specific actions that you should have taken, incorporating insights from these two analyses. 
    You need to output in the format of the example without any additional redundant content.
    I will give you some examples to help you better understand how to generate plan.
    
    Here are two examples:
    {plan_example}
    
    Here is the history you need for generating plan:
    {scenario}
    
    Here is the valuable experience you need for generating plan:
    {experience}
    
    Here is the failed lesson you need for generating plan:
    {lesson}
