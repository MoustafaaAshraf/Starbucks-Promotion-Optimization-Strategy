#%% md

## Portfolio Exercise: Starbucks
<br>

<img src="https://opj.ca/wp-content/uploads/2018/02/New-Starbucks-Logo-1200x969.jpg" width="200" height="200">
<br>
<br>
 <h1>Table of Contents<span class="tocSkip"></span></h1>
<div class="toc"><ul class="toc-item"><li><span><a href="#Background-Information" data-toc-modified-id="Background-Information-1"><span class="toc-item-num">1&nbsp;&nbsp;</span>Background Information</a></span></li><li><span><a href="#Optimization-Strategy" data-toc-modified-id="Optimization-Strategy-2"><span class="toc-item-num">2&nbsp;&nbsp;</span>Optimization Strategy</a></span></li><li><span><a href="#How-To-Test-The-Strategy?" data-toc-modified-id="How-To-Test-The-Strategy?-3"><span class="toc-item-num">3&nbsp;&nbsp;</span>How To Test The Strategy?</a></span></li><li><span><a href="#Baseline-Model" data-toc-modified-id="Baseline-Model-4"><span class="toc-item-num">4&nbsp;&nbsp;</span>Baseline Model</a></span></li><li><span><a href="#Descriptive-Statistics" data-toc-modified-id="Descriptive-Statistics-5"><span class="toc-item-num">5&nbsp;&nbsp;</span>Descriptive Statistics</a></span></li><li><span><a href="#Isolating-Binomial-Evaluation-features" data-toc-modified-id="Isolating-Binomial-Evaluation-features-6"><span class="toc-item-num">6&nbsp;&nbsp;</span>Isolating Binomial-Evaluation features</a></span></li><li><span><a href="#Testing-50/50-split-balance-between-the-treatment-and-control-groups" data-toc-modified-id="Testing-50/50-split-balance-between-the-treatment-and-control-groups-7"><span class="toc-item-num">7&nbsp;&nbsp;</span>Testing 50/50 split balance between the treatment and control groups</a></span></li><li><span><a href="#Testing-Group-50/50-split-with-respect-to-the-evaluation-features" data-toc-modified-id="Testing-Group-50/50-split-with-respect-to-the-evaluation-features-8"><span class="toc-item-num">8&nbsp;&nbsp;</span>Testing Group 50/50 split with respect to the evaluation features</a></span></li><li><span><a href="#Testing-the-difference-in-mean-purchases-with-respect-to-each-evaluation-feature" data-toc-modified-id="Testing-the-difference-in-mean-purchases-with-respect-to-each-evaluation-feature-9"><span class="toc-item-num">9&nbsp;&nbsp;</span>Testing the difference in mean purchases with respect to each evaluation feature</a></span></li><li><span><a href="#Choosing-Most-effective-features-in-Increase-IIR-and-NIR" data-toc-modified-id="Choosing-Most-effective-features-in-Increase-IIR-and-NIR-10"><span class="toc-item-num">10&nbsp;&nbsp;</span>Choosing Most effective features in Increase IIR and NIR</a></span></li></ul></div>

#### Background Information

The dataset you will be provided in this portfolio exercise was originally used as a take-home assignment provided by Starbucks for their job candidates. The data for this exercise consists of about 120,000 data points split in a 2:1 ratio among training and test files. In the experiment simulated by the data, an advertising promotion was tested to see if it would bring more customers to purchase a specific product priced at $10. Since it costs the company 0.15 to send out each promotion, it would be best to limit that promotion only to those that are most receptive to the promotion. Each data point includes one column indicating whether or not an individual was sent a promotion for the product, and one column indicating whether or not that individual eventually purchased that product. Each individual also has seven additional features associated with them, which are provided abstractly as V1-V7.

#### Optimization Strategy

Your task is to use the training data to understand what patterns in V1-V7 to indicate that a promotion should be provided to a user. Specifically, your goal is to maximize the following metrics:

* **Incremental Response Rate (IRR)** 

IRR depicts how many more customers purchased the product with the promotion, as compared to if they didn't receive the promotion. Mathematically, it's the ratio of the number of purchasers in the promotion group to the total number of customers in the purchasers group (_treatment_) minus the ratio of the number of purchasers in the non-promotional group to the total number of customers in the non-promotional group (_control_).

$$ IRR = \frac{purch_{treat}}{cust_{treat}} - \frac{purch_{ctrl}}{cust_{ctrl}} $$


* **Net Incremental Revenue (NIR)**

NIR depicts how much is made (or lost) by sending out the promotion. Mathematically, this is 10 times the total number of purchasers that received the promotion minus 0.15 times the number of promotions sent out, minus 10 times the number of purchasers who were not given the promotion.

$$ NIR = (10\cdot purch_{treat} - 0.15 \cdot cust_{treat}) - 10 \cdot purch_{ctrl}$$

For a full description of what Starbucks provides to candidates see the [instructions available here](https://drive.google.com/open?id=18klca9Sef1Rs6q8DW4l7o349r8B70qXM).

Below you can find the training data provided.  Explore the data and different optimization strategies.

#### How To Test Your Strategy?

When you feel like you have an optimization strategy, complete the `promotion_strategy` function to pass to the `test_results` function.  
From past data, we know there are four possible outcomes:

Table of actual promotion vs. predicted promotion customers:  

<table>
<tr><th></th><th colspan = '2'>Actual</th></tr>
<tr><th>Predicted</th><th>Yes</th><th>No</th></tr>
<tr><th>Yes</th><td>I</td><td>II</td></tr>
<tr><th>No</th><td>III</td><td>IV</td></tr>
</table>

The metrics are only being compared for the individuals we predict should obtain the promotion – that is, quadrants I and II.  Since the first set of individuals that receive the promotion (in the training set) receive it randomly, we can expect that quadrants I and II will have approximately equivalent participants.  

Comparing quadrant I to II then gives an idea of how well your promotion strategy will work in the future.

### Workflow
1. Baseline Model
2. Descriptive statistics on features
3. Isolating Binomial-Evaluation features
4. Testing 50/50 split balance between the treatment and control groups
5. Testing Group 50/50 split with respect to the evaluation features
6. Testing the difference in mean purchases with respect to each evaluation feature
7. Choosing Most effective features in Increase IIR and NIR