# fuzzy_larsen
DEVELOPING A FUZZY LOGIC SYSTEM FOR BANK OFFICERS THAT DETERMINES THE AMOUNT OF HOUSING CREDIT FOR AN APPLICANT USING LARSEN INFERENCE METHOD

1. Market Value of the House:


![image](https://user-images.githubusercontent.com/37450251/132398075-2ff6b96f-ad6a-4d85-9db6-7b54929c5c7e.png)


2. Location of the House:


![image](https://user-images.githubusercontent.com/37450251/132398152-513ce209-1258-49f9-bc67-e4abef17e676.png)


3. Asset of the Applicant:


![image](https://user-images.githubusercontent.com/37450251/132398185-048b272c-b37e-4811-a640-f64c588271ea.png)


4. Income of the Applicant:


![image](https://user-images.githubusercontent.com/37450251/132398213-a83cf1dc-1144-47df-aabd-2f904a99b5b9.png)


5. Interest:


![image](https://user-images.githubusercontent.com/37450251/132398225-74dd70c3-1f9a-4e0f-9f78-814b4a16a44e.png)



Based on the information provided by the applicant, your program first calculates an evaluation for the house and
and an evaluation for the applicant using the hierarchical structure given in Figure 1.
1. House:

![image](https://user-images.githubusercontent.com/37450251/132398241-652f2239-761c-46f2-b634-23619503274b.png)

2. Applicant:



![image](https://user-images.githubusercontent.com/37450251/132398256-02144e4c-6e11-4ac8-9a97-f5214e9f6bba.png)


3. Credit Amount


![image](https://user-images.githubusercontent.com/37450251/132398267-24fe0e8f-ef7f-46e6-819b-38f04d3df5d8.png)



Our program then calculates the credit’s amount for the applicant using the evaluation of the applicant and the
evaluation of the house together with the applicant’s income and the interest rate using the hierarchical structure
given in Figure 1.


![image](https://user-images.githubusercontent.com/37450251/132398291-4f2ab667-4619-4d2a-8e47-077ff22c7c23.png)
Firgure 1. The hiearchical structure of the program



The rule set:

![image](https://user-images.githubusercontent.com/37450251/132398342-77d07118-9058-4e07-ae67-91ab9a31a124.png)


![image](https://user-images.githubusercontent.com/37450251/132398377-6d81a30d-5101-4e6f-a8b9-aac8df599c42.png)


![image](https://user-images.githubusercontent.com/37450251/132398397-7f00b0f4-9c1b-4aa3-b54b-25258b46056d.png)






-----------------------------------------------------------------




# **Reminder:**

**Larsen Fuzzy Model**

If a fuzzy rule base with rules for input membership functions and with the universe of discourse , respectively and the output membership function with the universe of discourse is defined as,

Where and fuzzy sets are expressed as,

The firing weight of -rule is defined as

for max-product composition

Then, the membership function for fuzzy output will be:

Hence, the final output will be:

So for Larsen we use _ **Product** _ operation rule of fuzzy implication

**Explaining the source code:** (full code in the end of the document)

First we need to import the proper libraries:

import **numpy**** as ****np**

import **skfuzzy**** as ****fuzz**

import **matplotlib.pyplot**** as ****plt**

from **skfuzzy**** import **control** as** ctrl

Then we define &quot;Larsen&quot; function to use it in the end, this function takes five parameters; (Market value, Location, Asset, Income, Interest)

**def** larsen(my\_market\_value,my\_location,my\_asset,my\_income,my\_interest):

Then we define the arrays for each factor.

First we define the fuzzy sets of the linguistic variable **(Market Value)**

market\_value=np.arange(0,1100,50)

![](RackMultipart20210907-4-j48m7v_html_97a41a98cfe86512.png)

Fuzzy sets of the linguistic variable **(Location):**

location=np.arange(0,11,0.5)

![](RackMultipart20210907-4-j48m7v_html_b13db1cb80c265ef.png)

And so on for the rest …

asset=np.arange(0,1100,50)

income=np.arange(0,110,5)

interest=np.arange(0,11,0.5)

house=np.arange(0,11,1)

applicant=np.arange(0,11,1)

credit=np.arange(0,550,25)

whereas the function &quot; **numpy.arange**&quot; generates values and return an array of evenly spaced values with the parameters (start value, stop value, steps).

Now after we defined our Fuzzy sets variables, we need to generate **fuzzy membership functions** for each fuzzy set.

And by doing that, we use the following function:

skfuzzy.membership. **trapmf**** ( **_x_** ,  **_abcd_** )**

from what we see from the graph, the shape of the membership function is **Trapezoidal** , which means like this:

![](RackMultipart20210907-4-j48m7v_html_5e699e552438510b.png)

| **Parameters:** | **x**  : 1d arrayIndependent variable. **abcd**  : 1d array, length 4Four-element vector. Ensure a \&lt;= b \&lt;= c \&lt;= d. |
| --- | --- |
| **Returns:** | **y**  : 1d arrayTrapezoidal membership function. |

And for the triangle shapes we use another function which is :

skfuzzy.membership. **trimf**** ( **_x_** ,  **_abc_** )**

Triangular membership function generator.

| **Parameters:** | **x**  : 1d arrayIndependent variable. **abc**  : 1d array, length 3Three-element vector controlling shape of triangular function. Requires a \&lt;= b \&lt;= c. |
| --- | --- |
| **Returns:** | **y**  : 1d array (Triangular membership function.) |

The fuzzy membership functions will be like this:

market\_valueL\_mf=fuzz.trapmf(market\_value,[0,0,50,100])

market\_valueM\_mf=fuzz.trapmf(market\_value,[50,100,200,250])

market\_valueH\_mf=fuzz.trapmf(market\_value,[200,300,650,850])

market\_valueVH\_mf=fuzz.trapmf(market\_value,[650,850,1000,1000])

locationB\_mf=fuzz.trapmf(location,[0,0,1.5,4])

locationF\_mf=fuzz.trapmf(location,[2.5,5,6,8.5])

locationE\_mf=fuzz.trapmf(location,[6,8.5,10,10])

houseVL\_mf=fuzz.trimf(house,[0,0,3])

houseL\_mf=fuzz.trimf(house,[0,3,6])

houseM\_mf=fuzz.trimf(house,[2,5,8])

houseH\_mf=fuzz.trimf(house,[4,7,10])

houseVH\_mf=fuzz.trimf(house,[7,10,10])

assetL\_mf=fuzz.trapmf(asset,[0,0,0,150])

assetM\_mf=fuzz.trapmf(asset,[50,250,450,650])

assetH\_mf=fuzz.trapmf(asset,[500,700,1000,1000])

incomeL\_mf=fuzz.trapmf(income,[0,0,10,25])

incomeM\_mf=fuzz.trapmf(income,[15,35,35,55])

incomeH\_mf=fuzz.trapmf(income,[40,60,60,80])

incomeVH\_mf=fuzz.trapmf(income,[60,80,100,100])

interestL\_mf=fuzz.trapmf(interest,[0,0,2,5])

interestM\_mf=fuzz.trapmf(interest,[2,4,6,8])

interestH\_mf=fuzz.trapmf(interest,[6,8.5,10,10])

applicantL\_mf=fuzz.trapmf(applicant,[0,0,2,4])

applicantM\_mf=fuzz.trapmf(applicant,[2,5,5,8])

applicantH\_mf=fuzz.trapmf(applicant,[6,8,10,10])

creditVL\_mf=fuzz.trimf(credit,[0,0,125])

creditL\_mf=fuzz.trimf(credit,[0,125,250])

creditM\_mf=fuzz.trimf(credit,[125,250,375])

creditH\_mf=fuzz.trimf(credit,[250,375,500])

creditVH\_mf=fuzz.trimf(credit,[375,500,500])

if we print the membership function on screen we get something like this:

print( market\_valueL\_mf)

output:

[1. 1. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0.]

The first two values represent the first two steps in the fuzzy set, which is the membership function of **Low Market value,** Since we are moving 50 steps at a time in that fuzzy set specifically.

![](RackMultipart20210907-4-j48m7v_html_40bae4360354441.png)

The next step is generating interpolated discrete membership function at a specific value given by the user.

We use the following function:

skfuzzy.**interp\_membership(**_x_ **, ** _xmf_ **, ** _xx_**)**

this function finds the degree of membership u(xx) for a given value of x= xx.

| **Parameters:** | **x**  : 1d arrayIndependent discrete variable vector. **xmf**  : 1d arrayFuzzy membership function for x. Same length as x. **xx**  : floatDiscrete singleton value on universe x. |
| --- | --- |
| **Returns:** | **xxmf**  : floatMembership function value at xx, u(xx) |

Code:

market\_valueL=fuzz.interp\_membership(market\_value,market\_valueL\_mf,my\_market\_value)

market\_valueM=fuzz.interp\_membership(market\_value,market\_valueM\_mf,my\_market\_value)

market\_valueH=fuzz.interp\_membership(market\_value,market\_valueH\_mf,my\_market\_value)

market\_valueVH=fuzz.interp\_membership(market\_value,market\_valueVH\_mf,my\_market\_value)

locationB=fuzz.interp\_membership(location,locationB\_mf,my\_location)

locationF=fuzz.interp\_membership(location,locationF\_mf,my\_location)

locationE=fuzz.interp\_membership(location,locationE\_mf,my\_location)

assetL=fuzz.interp\_membership(asset,assetL\_mf,my\_asset)

assetM=fuzz.interp\_membership(asset,assetM\_mf,my\_asset)

assetH=fuzz.interp\_membership(asset,assetH\_mf,my\_asset)

incomeL=fuzz.interp\_membership(income,incomeL\_mf,my\_income)

incomeM=fuzz.interp\_membership(income,incomeM\_mf,my\_income)

incomeH=fuzz.interp\_membership(income,incomeH\_mf,my\_income)

incomeVH=fuzz.interp\_membership(income,incomeVH\_mf,my\_income)

interestL=fuzz.interp\_membership(interest,interestL\_mf,my\_interest)

interestM=fuzz.interp\_membership(interest,interestM\_mf,my\_interest)

interestH=fuzz.interp\_membership(interest,interestH\_mf,my\_interest)

we write the function for each membership function in each fuzzy set.

Now, We take our rules and apply them as follows:

For example: for the following rule:

If (Location is Bad) and (Market\_value is Medium) then (House is Low)

We write the proper function for this rule like this:

locationBad\_MarketMedium=np.dot(locationB,market\_valueM)

house\_low1=np.dot(locationBad\_MarketMedium,houseL\_mf)

we used here the function **dot()** from _numpy_

which does a product of two arrays, and for **Larsen** , we need to use **Product**  **operation** rule of fuzzy implication, instead of Mini-operation inf the Mamdani case.

And in the same time we can write the following line and get the same result:

locationBad\_MarketMedium=location \*market\_valueM

To explain furthermore, let&#39;s say we have:

which represents **(Location is Bad)**

which represents **(Market value is Medium)**

Thus,

which represents the following line:

locationBad\_MarketMedium=np.dot(locationB,market\_valueM)

And,

which represents the following line:

house\_low1=np.dot(locationBad\_MarketMedium,houseL\_mf)

we apply that in our code:

locationBad\_MarketMedium=np.dot(locationB,market\_valueM)

locationFair\_MarketLow=np.dot(locationF,market\_valueL)

locationBad\_MarketLow=np.dot(locationB,market\_valueL)

locationBad\_marketHigh=np.dot(locationB,market\_valueH)

locationFair\_marketMedium=np.dot(locationF,market\_valueM)

locationEx\_marketLow=np.dot(locationE,market\_valueL)

locationBad\_marketVeryHigh=np.dot(locationB,market\_valueVH)

locationFair\_marketHigh=np.dot(locationF,market\_valueH)

locationEx\_marketMedium=np.dot(locationE,market\_valueM)

locationFair\_marketVeryHigh=np.dot(locationF,market\_valueVH)

locationEx\_marketHigh=np.dot(locationE,market\_valueH)

locationEx\_marketVeryHigh=np.dot(locationE,market\_valueVH)

_####################################_

assetLow\_incomeLow=np.dot(assetL,incomeL)

assetLow\_incomeMedium=np.dot(assetL,incomeM)

assetMedium\_incomeLow=np.dot(assetM,incomeL)

assetLow\_incomeHigh=np.dot(assetL,incomeH)

assetMedium\_incomeMedium=np.dot(assetM,incomeM)

assetHigh\_incomeLow=np.dot(assetH,incomeL)

assetHigh\_incomeMedium=np.dot(assetH,incomeM)

assetLow\_incomeVeryHigh=np.dot(assetL,incomeVH)

assetMedium\_incomeHigh=np.dot(assetM,incomeH)

assetMedium\_incomeVeryHigh=np.dot(assetM,incomeVH)

assetHigh\_incomeHigh=np.dot(assetH,incomeH)

assetHigh\_incomeVeryHigh=np.dot(assetH,incomeVH)

_# Now we apply a product operation for the larsen Inference method_

_# membership function with `np.dot`_

house\_low1=np.dot(locationBad\_MarketMedium,houseL\_mf)

house\_low2=np.dot(market\_valueL,houseL\_mf)

house\_low3=np.dot(locationB,houseL\_mf)

house\_low4=np.dot(locationFair\_MarketLow,houseL\_mf)

house\_very\_low=np.dot(locationBad\_MarketLow,houseVL\_mf)

house\_medium1=np.dot(locationBad\_marketHigh,houseM\_mf)

house\_medium2=np.dot(locationFair\_marketMedium,houseM\_mf)

house\_medium3=np.dot(locationEx\_marketLow,houseM\_mf)

house\_high1=np.dot(locationBad\_marketVeryHigh,houseH\_mf)

house\_high2=np.dot(locationFair\_marketHigh,houseH\_mf)

house\_high3=np.dot(locationEx\_marketMedium,houseH\_mf)

house\_very\_high1=np.dot(locationFair\_marketVeryHigh,houseVH\_mf)

house\_very\_high2=np.dot(locationEx\_marketHigh,houseVH\_mf)

house\_very\_high3=np.dot(locationEx\_marketVeryHigh,houseVH\_mf)

_####################################_

applicant\_low1=np.dot(assetLow\_incomeLow,applicantL\_mf)

applicant\_low2=np.dot(assetLow\_incomeMedium,applicantL\_mf)

applicant\_low3=np.dot(assetMedium\_incomeLow,applicantL\_mf)

applicant\_medium1=np.dot(assetLow\_incomeHigh,applicantM\_mf)

applicant\_medium2=np.dot(assetMedium\_incomeMedium,applicantM\_mf)

applicant\_medium3=np.dot(assetHigh\_incomeLow,applicantM\_mf)

applicant\_medium4=np.dot(assetHigh\_incomeMedium,applicantM\_mf)

applicant\_high1=np.dot(assetLow\_incomeVeryHigh,applicantH\_mf)

applicant\_high2=np.dot(assetMedium\_incomeHigh,applicantH\_mf)

applicant\_high3=np.dot(assetMedium\_incomeVeryHigh,applicantH\_mf)

applicant\_high4=np.dot(assetHigh\_incomeHigh,applicantH\_mf)

applicant\_high5=np.dot(assetHigh\_incomeVeryHigh,applicantH\_mf)

The following function returns an array of zeros with the same shape and type as a given array, this helps us in the defuzzification process.

house0=np.zeros\_like(house)

applicant0=np.zeros\_like(applicant)

Now we have our rule base.

![](RackMultipart20210907-4-j48m7v_html_bf72dfe276cfbb7f.gif)

Then next step is to aggregate all the output membership functions together and do the Inference process.

aggregated\_applicant=np.fmax(applicant\_low1,

np.fmax(applicant\_low2,

np.fmax(applicant\_low3,

np.fmax(applicant\_medium1,

np.fmax(applicant\_medium2,

np.fmax(applicant\_medium3,

np.fmax(applicant\_medium4,

np.fmax(applicant\_high1,

np.fmax(applicant\_high2,

np.fmax(applicant\_high3,np.fmax(applicant\_high4,applicant\_high5)))))))))))

aggregated\_house=np.fmax(house\_low1,

np.fmax(house\_low2,

np.fmax(house\_low3,

np.fmax(house\_low4,

np.fmax(house\_very\_low,

np.fmax(house\_medium1,

np.fmax(house\_medium2,

np.fmax(house\_medium3,

np.fmax(house\_high1,

np.fmax(house\_high2,

np.fmax(house\_high3,

np.fmax(house\_very\_high1,

np.fmax(house\_very\_high2,house\_very\_high3)))))))))))))

The linguistic inputs, which are represented by analog voltages distributed on data buses, are fed into each inference engine in parallel. The results inferred from the rules are aggregated by a MAX block, which implements the function of the connective &quot;also&quot; as a union operation .aggregation can be implemented as max. The output of each rule is aggregated into the single value.

Then we calculate the defuzzified result:

applicant\_result=fuzz.defuzz(applicant,aggregated\_applicant,&#39;centroid&#39;)

applicant\_result\_activation=fuzz.interp\_membership(applicant,aggregated\_applicant,applicant\_result)

house\_result=fuzz.defuzz(house,aggregated\_house,&#39;centroid&#39;)

house\_result\_activation=fuzz.interp\_membership(house,aggregated\_house,house\_result)

we use the function :

skfuzzy.**defuzz(**_x_ **, ** _mfx_ **, ** _mode_**)**

the function feature is to do a defuzzification of a membership function, returning a defuzzified value of the function at x.

| **Parameters:** | **x**  : 1d array or iterable, length NIndependent variable. **mfx**  : 1d array of iterable, length NFuzzy membership function. **mode**  : stringControls which defuzzification method will be used. \* &#39;centroid&#39;: Centroid of area \* &#39;bisector&#39;: bisector of area \* &#39;mom&#39; : mean of maximum \* &#39;som&#39; : min of maximum \* &#39;lom&#39; : max of maximum |
| --- | --- |
| **Returns:** | **u**  : float or intDefuzzified result. |

The next step is to visualize these universes and membership function as plots on screen.

fig,(ax3,ax4)=plt.subplots(nrows=2,figsize=(9,4))

ax4.plot(applicant,applicantL\_mf,&#39;g&#39;,linewidth=0.5,label=&#39;Low&#39;)

ax4.plot(applicant,applicantM\_mf,&#39;r&#39;,linewidth=0.5,label=&#39;Medium&#39;)

ax4.plot(applicant,applicantH\_mf,&#39;c&#39;,linewidth=0.5,label=&#39;High&#39;)

ax4.fill\_between(applicant,applicant0,aggregated\_applicant,facecolor=&#39;Orange&#39;,alpha=0.7)

ax4.plot([applicant\_result,applicant\_result],[0,applicant\_result\_activation],&#39;k&#39;,linewidth=1.5,alpha=0.9)

ax4.plot([0,applicant\_result],[applicant\_result\_activation,applicant\_result\_activation],&#39;k&#39;,linewidth=1.5,alpha=0.9)

ax4.set\_title(&#39;Applicant&#39;)

ax4.text(applicant\_result,applicant\_result\_activation,&quot; **{}**&quot;.format(int(applicant\_result)))

ax4.text(0,applicant\_result\_activation,&quot; **{}**&quot;.format(format(applicant\_result\_activation,&#39;.2f&#39;)))

ax4.legend()

ax3.plot(house,houseVL\_mf,&#39;b&#39;,linewidth=0.5,label=&#39;Very Low&#39;)

ax3.plot(house,houseL\_mf,&#39;g&#39;,linewidth=0.5,label=&#39;Low&#39;)

ax3.plot(house,houseM\_mf,&#39;r&#39;,linewidth=0.5,label=&#39;Medium&#39;)

ax3.plot(house,houseH\_mf,&#39;c&#39;,linewidth=0.5,label=&#39;High&#39;)

ax3.plot(house,houseVH\_mf,&#39;k&#39;,linewidth=0.5,label=&#39;Very High&#39;)

ax3.fill\_between(house,house0,aggregated\_house,facecolor=&#39;Orange&#39;,alpha=0.7)

ax3.plot([house\_result,house\_result],[0,house\_result\_activation],&#39;k&#39;,linewidth=1.5,alpha=0.9)

ax3.plot([0,house\_result],[house\_result\_activation,house\_result\_activation],&#39;k&#39;,linewidth=1.5,alpha=0.9)

ax3.set\_title(&#39;House&#39;)

ax3.text(house\_result,house\_result\_activation,&quot; **{}**&quot;.format(house\_result))

ax3.text(0,house\_result\_activation,&quot; **{}**&quot;.format(format(house\_result\_activation,&#39;.2f&#39;)))

ax3.legend()

to test the previous code we manually enter hard-coded input values:

my\_market\_value=333

my\_location= 2

my\_asset=200

my\_income=35

The output will be :

![](RackMultipart20210907-4-j48m7v_html_67ef235affa11ec6.png)

The previous values can be categorized like this:

-Market value is 300 **(High)**
 -Location is 2 **(Bad)**

- If (Location is **Bad** ) and (Market\_value is **High** ) then (House is **Medium** )
- We can see the House value has been inferred between **Medium** and **Low** )

-Asset is 200 ( **Medium** )
 -Income is 35 ( **Medium** )

- If (Asset is **Medium** ) and (Income is **Medium** ) then (Applicant is **Medium** )
- We can see that the degree of membership is **0.75** and not all the way to **1.** Because the asset at 200 its degree of membership is **0.8** and not **1**.

Now for the next step, we need to do all the previous step for the house membership function and fuzzy set.

And then we take the singleton value from the output of the previous defuzzification process. And we use those values to do the inference process.

![](RackMultipart20210907-4-j48m7v_html_63ba96a53bb377ad.png)

Because we need to calculate the credit&#39;s amount for the applicant using the evaluation of the applicant and the evaluation of the house together with the applicant&#39;s income and the interest rate using the hierarchal structure.

We use the same code method, but instead of getting the values of the interpolated membership functions from the user directly, we use the result values that we got as output.

For example:

houseVL=fuzz.interp\_membership(house,houseVL\_mf,house\_result)

and :

applicantL=fuzz.interp\_membership(applicant,applicantL\_mf,applicant\_result)

# **Testing the Final code:**

# **Testing the Final code:**
First Test:

Input:

my\_market\_value=333

my\_location= 2

my\_asset=200

my\_income=35

my\_interest=5

output:

Value of house = 4.000000000000001

Value of applicant = 5

And the interest = %5

The Credit Amount is 187.50000000000006 x 10^3 = $187500.00000000006

![](RackMultipart20210907-4-j48m7v_html_ad9a86e0f47a0ef7.png)

![](RackMultipart20210907-4-j48m7v_html_ed6a8c904c4fd484.png)

**Second test:**

Input:

my\_market\_value=900

my\_location= 7

my\_asset=600

my\_income=70

my\_interest=7

output:

Value of house = 9.0

Value of applicant = 8

And the interest = %7

The Credit Amount is 423.93617021276594 x 10^3 = $423936.1702127659

![](RackMultipart20210907-4-j48m7v_html_ce85143f6e3a8aa3.png)

![](RackMultipart20210907-4-j48m7v_html_ae26eee5cc398692.png)

# **Third test:**

input:

my\_market\_value=600

my\_location= 5

my\_asset=800

my\_income=90

my\_interest=9

output:

Value of house = 7.000000000000001

Value of applicant = 8

And the interest = %9

The Credit Amount is 374.9999999999999 x 10^3 = $374999.9999999999

![](RackMultipart20210907-4-j48m7v_html_ca28ece52a0042bc.png)

![](RackMultipart20210907-4-j48m7v_html_811f0dace81b0520.png)

