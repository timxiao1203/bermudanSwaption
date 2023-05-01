# Bermudan Swaption Introduction

The general structure of the pricing Bermudan swaption is split into the following sections: collection of attribute information, calculating exercise tree levels and valuation information corresponding to those levels, and calculation of the derivative with a binomial lattice.

The underlying variable as mentioned above is a portfolio of portfolios. For each exercise date the swap that can be entered is represented as a fixed and floating leg. Each pair of fixed and floating legs is contained in a separate portfolio. Each of these portfolios is contained in a master portfolio. This master portfolio entered as the “underlying” attribute.

The exercise dates of the Bermudan option will in general not fall exactly on the nodes of the binomial tree. As such, the exercise dates must be adjusted to the closest node on the tree. These exercisable node levels are recorded in the vector exercise dates.  By construction the last exercise date will correspond with the end of the lattice. Each exercise date is labelled with an exercise flag. 

The exercise flag can have a value of 0, 1, or 2. The name of this variable is ex_flag. ex_flag is a vector with one element for each exercise date. If the ex_flag has a value of 0 then the option is not exercisable at this date. If the ex_flag is 2 then the option is exercisable at this date. If the ex_flag is 1 then the option is exercisable at this date and at every other node between this date and the previous exercise date but not including the node associated with the previous exercise date. The exercisability of each node level is determined from the ex_falg attribute and recorded in the vector exFlag. exFlag has an element for each node level in the tree. Each entry is 0 or 1, where 0 indicates that the node level is not exercisable and 1 indicates that the node level is exercisable. 

The forward swap rate and the volatility for each exercise date is calculated for the actual exercise date. As such the values used on the tree must be interpolated since the node times are slightly different.  For the American nodes (ie. those nodes that are exercisable because of  a value of  1 on the ex_flag variable and don’t have a corresponding fixed and floating leg associated with it)  the swap structure must be interpolated from the structures of the swaps it falls between. The forward swap rate and volatility are also interpolated for these nodes.

Once the information in the previous section is calculated and recorded in the appropriate vectors the valuation using a binomial lattice is straight forward. The only difficulty is that the valuation input vectors are in general a different size than the number of tree steps and rolling back through the tree and rolling back through the valuation vectors must be carefully observed and implemented.

Let  ,…, , where  , be common Libor reset points.  Here we assume that all interest rate reset and Bermudan exercise times belong to the set of common reset points,  .

We consider an interest-rate swap consisting of respective receiver and payer legs.  Here the payer leg is specified by

•	reset points
•	an amount  

Here R  can be a fixed rate, a Libor or a CMS rate.  In the case of an  -period Libor rate, 

Where PV is the price at time   of a zero-coupon bond, which matures at   and has $1 face value.

In the case of a single period Libor rate, for example,  .

For an  -period CMS rate with frequency,   (where   is a whole number of consecutive common reset periods),

For example, if   (i.e., the CMS has reset times that correspond to consecutive common reset points), then  .

Let the receiver leg be similarly defined with respect to the reset points,  .

We now consider exercise points,  , such that  .  Here the Bermudan swaption can be exercised at any point belonging to  . If the option is exercised at time  , for some  , then 

•	the owner must pay all payer-leg quantities that set at points   such that  , and
•	the owner receives all receiver-leg quantities that set at points   such that  .

Reference:

https://finpricing.com/lib/EqVariance.html

