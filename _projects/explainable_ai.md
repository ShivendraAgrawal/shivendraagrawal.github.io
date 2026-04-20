---
layout: page
title: An Explainable Reinforcement Learning Approach for Enabling Robots to Coach Humans
description: Best Technical Paper Runner-up @ HRI 2019
img: assets/img/blog/RARE/fig_user_study1.jpg
# redirect: https://unsplash.com
importance: 2
date: 2020-05-20
category: work
---
    Team members:
    Aaquib Tabrez
    Shivendra Agrawal
    Bradley Hayes (Advisor)

<br /><br />
**Paper**
<div class="publications">
{% bibliography -f papers -q @*[topic=explainable_ai]* %}
</div>

While much of the existing robotics literature focuses on making robots better at performing tasks, this work explores how we can use that proficiency and expertise to allow robots to provide personalized feedback with the goal of coaching humans to improve task performance. We investigate the challenges faced by robots while coaching humans (e.g., providing corrective feedback) during collaboration and how explainable AI can be leveraged to alleviate these challenges for fostering trust, teamwork, transparency.

## **Robotic Coaching in HRI:**

In a collaborative task involving mixed human-robot teams, coaching helps to correct or improve the behavior of human collaborators by leveraging a robot teammate's capabilities. Correcting or repairing the behavior of a teammate is even more important in critical applications where missteps could lead to financial losses, catastrophic failures, or risks of physical harm.

In general, coaches engage in a range of communication skills including providing feedback, insights, and clarifications to help learners shift their perspectives about the task and thereby discover alternate and better ways to achieve their goals.

Here, we focus on two key innovations from our work that contribute toward enabling proficient autonomous coaches:  
1. Reward Augmentation and Repair through Explanation (RARE): A novel coaching framework for understanding and correcting an agent's decision-making process
2. Human subjects study results showing that justification is an important, potentially necessary factor for convincing people to accept an agent's advice.


## **Reward Augmentation and Repair through Explanation (RARE):**

Operating under the assumption that humans behave rationally to maximize reward when performing a task, our core insight is that *suboptimal behavior can be framed as an indicator of a malformed reward function* being used by an otherwise rational actor. Under this model, providing corrections to the actor's reward function will allow them to "reconverge their policy" to perform more optimally.

The process of Reward Augmentation and Repair through Explanation can be characterized through three interrelated challenges: 
1. Inferring the most likely reason for suboptimal behavior, by determining which parts of their reward function are malformed.
2. Determining a policy trade-off between task execution and intervention, and
3. Providing the necessary feedback and/or guidance to improve their performance. 

To make #1 tractable, we apply this work within a domain with sparse reward function (R(s) is only non-zero for a subset of states) and operate under the assumption that malformed reward functions occur only because the actor is missing information as opposed to using incorrect information (e.g., given actor reward function R and true reward function R*, R(s) = 0 for some values of s where R*(s) != 0. We do not address the more general problem of cases where R(s) = x and R*(s) = y for x != 0).

Here, we explain the intuition behind RARE through an example. Consider a grid world below (Figure 1) with two terminal states having rewards of 10 and 100. Given a knowledgeable, rational individual we would expect to see them collect the bigger reward. However, if we see a person going after the sub-optimal reward (10), then the only way to preserve the rationality assumption is to assume that they must not know about the bigger reward --- in other words, that their world model is incomplete/inaccurate. Our robotic coaching technique involves **_what_**, **_how_**, and **_when_** we can tell the human collaborator about the inaccuracies in their task comprehension (reward function) such that they will have the option to improve their behavior (policy).

<p align="center">
	<!-- <img align="center" width="75%" src="assets/img/blog/RARE/RARE-explain.gif"/> -->
	{% include figure.liquid path="assets/img/blog/RARE/RARE-explain.gif" class="img-fluid rounded" %}
	<br><em><b>Figure 1: RARE framework estimating the missing reward factor (100) based on human behavior (red line)</b></em>
</p>


Let's look at the process of estimating the human collaborator's mental model in the same example. In the first step, the robot observes the human at state S1 (Figure 2). Note that each state is composed of the world state and additional latent boolean variables that indicate the robot's knowledge of the human's awareness of each of the reward factors (r1, r2) in the world. At this stage, the robot doesn't know if the human knows about either of the rewards. But at timestep 2, the robot can gather more information after observing the human's next action. At timestep 2 the human is at state S2, and the robot still doesn't know enough to take any action. But if the human goes to state S3 at timestep 3, the robot can infer that the human isn't aware of the greater reward at the upper right corner. This answers the "**_what_**'' (missing reward r2), but not the "**_when_**" or the "**_how_**".

To address the "**_when_**", we model the problem as a <a style="color:blue" href="https://people.csail.mit.edu/lpk/papers/aij98-pomdp.pdf">POMDP</a>. In this case, we have a robot who is coaching while collaborating (i.e., the robot is able to participate in tasks as well as coach through explanation). Latent variables in the POMDP state space consist of Boolean variables indicating whether or not the human's reward function contains each non-zero entry in R. While the robot's understanding of the human's mental model of reward is latent because we don't actually know them beforehand and can't observe them directly, we can gather clues to reduce state uncertainty by watching the human's policy in action. By solving the POMDP, the robotic coach can successfully resolve when to take task-productive actions from the domain or communicative, reward repairing actions.

<p align="center">
	<!-- <img align="center" width="75%" src="assets/img/blog/RARE/state-transition-diagram.gif"/> -->
	{% include figure.liquid path="assets/img/blog/RARE/state-transition-diagram.gif" class="img-fluid rounded" %}
	<br><em><b>Figure 2: Procedural visualization of the estimation of human collaborator's mental model by the robot using RARE.</b></em>
</p>

Finally, we are left with the challenge of resolving "**_how_**" the robot should convey the missing reward information to the human. While there are a number of choices, we investigate two classes of feedback: unjustified advice and advice with justification. An example of unjustified advice is to have the robot indicate the sub-optimality of the human's action to encourage exploration (e.g., "If you do that you won't get the best reward" or "That's a bad move"). Advice with justification would entail having the robot provide a description of the reward's location to supplement the advice (e.g., "If you do that won't get the best reward. There's a better reward in the top right corner." or "If you do that you won't get the best reward. There's a large negative reward in region X."). 

<p align="center">
	<!-- <img align="center" width="75%" src="assets/img/blog/RARE/policy-correction.gif"/> -->
	{% include figure.liquid path="assets/img/blog/RARE/policy-correction.gif" class="img-fluid rounded" %}
	<br><em><b>Figure 3 Caption: RARE enables robotic coaches to repair sub-optimal policy by informing the human about the missing reward from their mental model.</b></em>
</p>

<i>Note: We use previous work from <a style="color:blue" href="http://www.bradhayes.info/papers/hri17.pdf">Hayes and Shah</a> to describe state regions for natural language policy updates. </i>


<figure class="half" style="display:flex">
	{% include figure.liquid path="assets/img/blog/RARE/fig_option1.jpg" class="img-fluid rounded" %}
	{% include figure.liquid path="assets/img/blog/RARE/fig_option2.jpg" class="img-fluid rounded" %}
		<!-- <img align="center" width="50%" src="assets/img/blog/RARE/fig_option1.jpg"/>
		<img align="center" width="50%" src="assets/img/blog/RARE/fig_option2.jpg"/> -->
</figure>
<p align="center">
	<em><b>Figure 4: Options for providing reward coaching to improve the sub-optimal policy: 1) advice without justification and 2) advice with justification</b></em>
</p>




#### We performed a user study to test the strength of RARE and to figure out the best option to convey the missing reward.


## **Experimental evaluation and results:**

<figure class="half" style="display:flex">
	{% include figure.liquid path="assets/img/blog/RARE/fig_user_study1.jpg" class="img-fluid rounded" %}
	{% include figure.liquid path="assets/img/blog/RARE/fig_user_study2.jpg" class="img-fluid rounded" %}
		<!-- <img align="center" width="50%" src="assets/img/blog/RARE/fig_user_study1.jpg"/>
		<img align="center" width="50%" src="assets/img/blog/RARE/fig_user_study2.jpg"/> -->
</figure>
<figure class="half" style="display:flex">
	{% include figure.liquid path="assets/img/blog/RARE/fig_user_study3.jpg" class="img-fluid rounded" %}
	{% include figure.liquid path="assets/img/blog/RARE/fig_user_study4.jpg" class="img-fluid rounded" %}
		<!-- <img align="center" width="50%" src="assets/img/blog/RARE/fig_user_study3.jpg"/>
		<img align="center" width="50%" src="assets/img/blog/RARE/fig_user_study4.jpg"/> -->
</figure>
<p align="center">
	<em><b>
			Figure 5 (Clockwise from top left)
		</b>
		<ol>
			<li>Our experimental setup where the robot and the human collaborator are simultaneously and collaboratively solving a modified Sudoku game.</li>
			<li>We conducted a between-subject study with 3 test conditions: control, justification, and no interruption.</li>
			<li>Our modified Sudoku game uses a 6x6 grid with 6 different colors available (instead of 9 numbers). The goal is to fill this grid collaboratively such that no rules are violated. Each player fills cells on the board from right to left, nearest to the farthest, while filling their share of the rows. There are no turns and a player is supposed to play their turn whenever they are ready. </li>
			<li>We can clearly see that due to these constraints, past moves have long term consequences for the success of the game, making this an incredibly difficult reward function to keep in working memory. Consequently, a player also needs to update their policy/moves based on the collaborator's moves making, the game even more cognitively demanding.</li>
		</ol>
	</em>
</p>


To determine the effectiveness of RARE and the usefulness of justification, we came up with a difficult game for people to play with a robot. We adapted the game of Sudoku and made it more challenging by putting it in a collaborative setting (see the above Figure 5 for details). 

The subjective evaluation of our study shows that robots that provide justification for advice are perceived as more useful, helpful, and intelligent coaches compared to the control condition in which no explanation for failure was given. Another surprising result which emerged during the study was that people barely followed the robot's advice (only 20% listened, while the other 80% of participants ignored advice and failed to solve the puzzle) when the robot did not provide any justification for its recommendation. In contrast, nearly everyone followed the robot's advice (80% successfully completed the game) when justification was provided. Furthermore, we saw from open questionnaire responses that the participants had a more positive user experience in the justification condition. 


<p align="center">
	{% include figure.liquid path="assets/img/blog/RARE/quotes.gif" class="img-fluid rounded" %}
	<!-- <img align="center" width="75%" src="assets/img/blog/RARE/quotes.gif"/> -->
	<br><em><b>Figure 6: Some of the participant feedback from the user study showing the contrast between the user experience in both condition</b></em>
</p>


## **Conclusion:**

To summarize, we developed a novel framework enabling an expert robot to coach a novice collaborator within a reinforcement learning context. We evaluated our framework in a challenging collaborative cognitive game with a human and a robot. We found that using justification during coaching makes robots more useful, helpful, and intelligent coaches.  

<hr>

<!-- <p align="center">
This blog post is based on the following paper:
</p>

<b>Explanation-based Reward Coaching to Improve Human Performance via Reinforcement Learning.</b>
<a style="color:red" href="http://humanrobotinteraction.org/2019/wp-content/uploads/2019/05/HRI-2019_Award_0314-Full-paper-only.pdf">(Runner-up, Best Technical Paper Award)</a> 
Proceedings of the 2019 ACM/IEEE International Conference on Human Robot Interaction (HRI 2019). Daegu, South Korea. 
<a style="color:blue" href="http://www.bradhayes.info/papers/hri19.pdf">\<Paper link\></a>

```
@inproceedings{tabrez2019explanation,
  title={Explanation-based reward coaching to improve human performance via reinforcement learning},
  author={Tabrez, Aaquib and Agrawal, Shivendra and Hayes, Bradley},
  booktitle={2019 14th ACM/IEEE International Conference on Human-Robot Interaction (HRI)},
  pages={249--257},
  year={2019},
  organization={IEEE}
}
``` -->
