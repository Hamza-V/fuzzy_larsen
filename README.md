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



Reminder:
Larsen Fuzzy Model
If a fuzzy rule base with n rules for input membership functions A_i and B_i with the universe of discourse X and Y, respectively and the output membership function C_i with the universe of discourse Z is defined as,
IF x is A_i     AND/OR    y is B_i   THEN  z is C_i

Where  i=1,2,3,…,n  and fuzzy sets A_i,B_i  and C_i are expressed as,
A_i=∫_(x∈X)▒〖μ_(A_i ) (x)/x 〗;B_i=∫_(y∈Y)▒〖μ_(B_i ) (y)/y  ;〗   C_i=∫_(z∈Z)▒〖μ_(C_i ) (z)/z 〗  

The firing weight of i^th-rule is defined as
w_i=μ_(A_i ) (x)  ×μ_(B_i ) (y)    for max-product composition
Then, the membership function for fuzzy output C' will be:
μ_(c^' ) (z)= ∨_(i=1)^n (w_i×μ_(c_i ) (z))
Hence, the final output will be:
C^'=∫_(z∈Z)▒〖μ_(c^' ) (z)/z〗
So for Larsen we use Product operation rule of fuzzy implication
R_L=A×B

Explaining the source code: (full code in the end of the document) 
	First we need to import the proper libraries:
import numpy as np
import skfuzzy as fuzz
import matplotlib.pyplot as plt
from skfuzzy import control as ctrl
Then we define “Larsen” function to use it in the end, this function takes five parameters; (Market value, Location, Asset, Income, Interest)
def larsen(my_market_value, my_location,my_asset, my_income, my_interest):

Then we define the arrays for each factor.




First we define the fuzzy sets of the linguistic variable (Market Value)
market_value = np.arange(0, 1100, 50)

 

Fuzzy sets of the linguistic variable (Location):
    location = np.arange(0, 11, 0.5)

 
And so on for the rest …
    asset = np.arange(0, 1100, 50)
    income =np.arange(0, 110, 5)
    interest = np.arange(0, 11,0.5)

    house = np.arange(0,11,1)
    applicant = np.arange(0,11,1)
    credit = np.arange(0,550,25)
whereas the function “numpy.arange” generates values and return an array of evenly spaced values with the parameters (start value, stop value, steps).
Now after we defined our Fuzzy sets variables, we need to generate fuzzy membership functions for each fuzzy set.
And by doing that, we use the following function:
skfuzzy.membership.trapmf(x, abcd)
from what we see from the graph, the shape of the membership function is Trapezoidal, which means like this:
 
Parameters:	x : 1d array
Independent variable.
abcd : 1d array, length 4
Four-element vector. Ensure a <= b <= c <= d.
Returns:	y : 1d array
Trapezoidal membership function.

And for the triangle shapes we use another function which is :
skfuzzy.membership.trimf(x, abc)
Triangular membership function generator.
Parameters:	x : 1d array
Independent variable.
abc : 1d array, length 3
Three-element vector controlling shape of triangular function. Requires a <= b <= c.
Returns:	y : 1d array (Triangular membership function.)

The fuzzy membership functions will be like this:
    market_valueL_mf = fuzz.trapmf(market_value, [0,0,50,100])
    market_valueM_mf = fuzz.trapmf(market_value, [50,100, 200,250])
    market_valueH_mf = fuzz.trapmf(market_value, [200,300,650,850])
    market_valueVH_mf = fuzz.trapmf(market_value, [650,850,1000,1000])
    
    locationB_mf = fuzz.trapmf(location , [0,0,1.5,4])
    locationF_mf  = fuzz.trapmf(location , [2.5 , 5 , 6, 8.5])
    locationE_mf  = fuzz.trapmf(location , [6,8.5,10,10])
    
    houseVL_mf = fuzz.trimf(house, [0,0,3])
    houseL_mf = fuzz.trimf(house, [0,3,6])
    houseM_mf = fuzz.trimf(house, [2,5,8])
    houseH_mf = fuzz.trimf(house, [4,7,10])
    houseVH_mf = fuzz.trimf(house, [7,10,10])
    
    assetL_mf = fuzz.trapmf(asset, [0,0,0,150])
    assetM_mf = fuzz.trapmf(asset, [50,250,450,650])
    assetH_mf = fuzz.trapmf(asset, [500,700,1000,1000])

    incomeL_mf = fuzz.trapmf(income, [0,0,10,25])
    incomeM_mf = fuzz.trapmf(income, [15,35,35,55])
    incomeH_mf = fuzz.trapmf(income, [40,60,60,80])
    incomeVH_mf = fuzz.trapmf(income, [60,80,100,100])

    interestL_mf = fuzz.trapmf(interest, [0,0,2,5])
    interestM_mf = fuzz.trapmf(interest, [2,4,6,8])
    interestH_mf = fuzz.trapmf(interest, [6,8.5,10,10])

    applicantL_mf = fuzz.trapmf(applicant, [0,0,2,4])
    applicantM_mf = fuzz.trapmf(applicant, [2,5,5,8])
    applicantH_mf = fuzz.trapmf(applicant, [6,8,10,10])

    creditVL_mf = fuzz.trimf(credit, [0,0,125])
    creditL_mf = fuzz.trimf(credit, [0,125,250])
    creditM_mf = fuzz.trimf(credit, [125,250,375])
    creditH_mf = fuzz.trimf(credit, [250,375,500])
    creditVH_mf = fuzz.trimf(credit, [375,500,500])
    

if we print the membership function on screen we get something like this: 
    print( market_valueL_mf)
output:
[1. 1. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0.]
The first two values represent the first two steps in the fuzzy set, which is the membership function of Low Market value, Since we are moving 50 steps at a time in that fuzzy set specifically.
 
The next step is generating interpolated discrete membership function u(x)  at a specific value x given by the user.
We use the following function:
skfuzzy.interp_membership(x, xmf, xx)
this function finds the degree of membership u(xx) for a given value of x= xx.
Parameters:	x : 1d array
Independent discrete variable vector.
xmf : 1d array
Fuzzy membership function for x. Same length as x.
xx : float
Discrete singleton value on universe x.
Returns:	xxmf : float
Membership function value at xx, u(xx)

Code:
   market_valueL = fuzz.interp_membership(market_value, market_valueL_mf , my_market_value)
    market_valueM = fuzz.interp_membership(market_value, market_valueM_mf , my_market_value)
    market_valueH = fuzz.interp_membership(market_value, market_valueH_mf , my_market_value)
    market_valueVH = fuzz.interp_membership(market_value, market_valueVH_mf , my_market_value)
    
    locationB = fuzz.interp_membership(location, locationB_mf, my_location)
    locationF = fuzz.interp_membership(location, locationF_mf, my_location)
    locationE = fuzz.interp_membership(location, locationE_mf, my_location)
    
    assetL = fuzz.interp_membership(asset, assetL_mf, my_asset)
    assetM = fuzz.interp_membership(asset, assetM_mf, my_asset)
    assetH = fuzz.interp_membership(asset, assetH_mf, my_asset)
    
    incomeL = fuzz.interp_membership(income, incomeL_mf, my_income)
    incomeM = fuzz.interp_membership(income, incomeM_mf, my_income)
    incomeH = fuzz.interp_membership(income, incomeH_mf, my_income)
    incomeVH = fuzz.interp_membership(income, incomeVH_mf, my_income)
    
    interestL = fuzz.interp_membership( interest, interestL_mf, my_interest )
    interestM = fuzz.interp_membership( interest, interestM_mf, my_interest )
    interestH = fuzz.interp_membership( interest, interestH_mf, my_interest )

we write the function for each membership function in each fuzzy set. 

Now, We take our rules and apply them as follows:
	For example: for the following rule:
If (Location is Bad) and (Market_value is Medium) then (House is Low)
We write the proper function for this rule like this:
    locationBad_MarketMedium = np.dot(locationB, market_valueM)
    house_low1 = np.dot(locationBad_MarketMedium, houseL_mf)

we used here the function dot() from numpy
which does a product of two arrays, and for Larsen, we need to use Product operation rule of fuzzy implication, instead of Mini-operation inf the Mamdani case.
And in the same time we can write the following line and get the same result:
    locationBad_MarketMedium = location * market_valueM

To explain furthermore, let’s say we have:
a_1=μ_A (X_0 )   which represents (Location is Bad)
a_2=μ_B (Y_0 )   which represents (Market value is Medium)
Thus, 
μ_(C_1^' ) (z)=a_1.μ_C (z)   which represents the following line:
    locationBad_MarketMedium = np.dot(locationB, market_valueM)

And,
μ_(C_2^' ) (z)=a_2.μ_C (z) which represents the following line:
    house_low1 = np.dot(locationBad_MarketMedium, houseL_mf)


we apply that in our code:
    locationBad_MarketMedium = np.dot(locationB, market_valueM)
    locationFair_MarketLow = np.dot(locationF , market_valueL)
    locationBad_MarketLow = np.dot(locationB, market_valueL)
    locationBad_marketHigh = np.dot(locationB, market_valueH)
    locationFair_marketMedium = np.dot(locationF, market_valueM)
    locationEx_marketLow = np.dot(locationE , market_valueL)
    locationBad_marketVeryHigh = np.dot(locationB, market_valueVH)
    locationFair_marketHigh = np.dot(locationF, market_valueH)
    locationEx_marketMedium = np.dot(locationE, market_valueM)
    locationFair_marketVeryHigh = np.dot(locationF, market_valueVH)
    locationEx_marketHigh = np.dot(locationE, market_valueH)
    locationEx_marketVeryHigh = np.dot(locationE, market_valueVH)
    ####################################
    assetLow_incomeLow = np.dot(assetL , incomeL)
    assetLow_incomeMedium = np.dot(assetL, incomeM)
    assetMedium_incomeLow = np.dot(assetM, incomeL)
    assetLow_incomeHigh = np.dot(assetL , incomeH)
    assetMedium_incomeMedium = np.dot(assetM , incomeM)
    assetHigh_incomeLow = np.dot(assetH , incomeL)
    assetHigh_incomeMedium = np.dot(assetH, incomeM)
    assetLow_incomeVeryHigh = np.dot(assetL, incomeVH)
    assetMedium_incomeHigh = np.dot(assetM , incomeH)
    assetMedium_incomeVeryHigh = np.dot(assetM , incomeVH)
    assetHigh_incomeHigh = np.dot(assetH , incomeH )
    assetHigh_incomeVeryHigh = np.dot(assetH , incomeVH)

    # Now we apply a product operation for the larsen Inference method
    # membership function with `np.dot`
    house_low1 = np.dot(locationBad_MarketMedium, houseL_mf) 
    house_low2 = np.dot(market_valueL, houseL_mf)
    house_low3 = np.dot(locationB , houseL_mf)
    house_low4 = np.dot(locationFair_MarketLow , houseL_mf)
    house_very_low = np.dot(locationBad_MarketLow , houseVL_mf)
    house_medium1 = np.dot(locationBad_marketHigh, houseM_mf)
    house_medium2 = np.dot(locationFair_marketMedium, houseM_mf)
    house_medium3 = np.dot(locationEx_marketLow, houseM_mf)
    house_high1 = np.dot(locationBad_marketVeryHigh, houseH_mf)
    house_high2 = np.dot(locationFair_marketHigh, houseH_mf)
    house_high3 = np.dot(locationEx_marketMedium, houseH_mf)
    house_very_high1 = np.dot(locationFair_marketVeryHigh, houseVH_mf)
    house_very_high2 = np.dot(locationEx_marketHigh, houseVH_mf)
    house_very_high3 = np.dot(locationEx_marketVeryHigh, houseVH_mf)
    ####################################
    applicant_low1 = np.dot(assetLow_incomeLow, applicantL_mf)
    applicant_low2 = np.dot(assetLow_incomeMedium , applicantL_mf)
    applicant_low3 = np.dot(assetMedium_incomeLow, applicantL_mf)
    applicant_medium1 = np.dot(assetLow_incomeHigh, applicantM_mf)
    applicant_medium2 = np.dot(assetMedium_incomeMedium, applicantM_mf)
    applicant_medium3 = np.dot(assetHigh_incomeLow, applicantM_mf)
    applicant_medium4 = np.dot(assetHigh_incomeMedium, applicantM_mf)
    applicant_high1 = np.dot(assetLow_incomeVeryHigh, applicantH_mf)
    applicant_high2 = np.dot(assetMedium_incomeHigh , applicantH_mf)
    applicant_high3 = np.dot(assetMedium_incomeVeryHigh, applicantH_mf)
    applicant_high4 = np.dot(assetHigh_incomeHigh , applicantH_mf)
    applicant_high5 = np.dot(assetHigh_incomeVeryHigh , applicantH_mf)
The following function returns an array of zeros with the same shape and type as a given array, this helps us in the defuzzification process.
    house0 = np.zeros_like(house)
    applicant0 = np.zeros_like(applicant)



Now we have our rule base. 
 
Then next step is to aggregate all the output membership functions together and do the Inference process.
    aggregated_applicant = np.fmax(applicant_low1,
                            np.fmax(applicant_low2,
                            np.fmax(applicant_low3,
                            np.fmax(applicant_medium1,
                            np.fmax(applicant_medium2,
                            np.fmax(applicant_medium3,
                            np.fmax(applicant_medium4,
                            np.fmax(applicant_high1,
                            np.fmax(applicant_high2,
                            np.fmax(applicant_high3,                                  np.fmax(applicant_high4,applicant_high5)))))))))))
    
    
    aggregated_house = np.fmax(house_low1,
                         np.fmax(house_low2,
                         np.fmax(house_low3,
                         np.fmax(house_low4,
                         np.fmax(house_very_low,
                         np.fmax(house_medium1,
                         np.fmax(house_medium2,
                         np.fmax(house_medium3,
                         np.fmax(house_high1,
                         np.fmax(house_high2,
                         np.fmax(house_high3,
                         np.fmax(house_very_high1,
                   np.fmax(house_very_high2,house_very_high3)))))))))))))

The linguistic inputs, which are represented by analog voltages distributed on data buses, are fed into each inference engine in parallel. The results inferred from the rules are aggregated by a MAX block, which implements the function of the connective “also” as a union operation ∪ .aggregation can be implemented as max. The output of each rule is aggregated into the single value.

Then we calculate the defuzzified result:
    applicant_result = fuzz.defuzz(applicant, aggregated_applicant, 'centroid')
    applicant_result_activation = fuzz.interp_membership(applicant, aggregated_applicant, applicant_result)
    
    house_result = fuzz.defuzz(house, aggregated_house, 'centroid')
    house_result_activation = fuzz.interp_membership(house, aggregated_house, house_result) 




we use the function :
skfuzzy.defuzz(x, mfx, mode)
the function feature is to do a defuzzification of a membership function, returning a defuzzified value of the function at x.
Parameters:	x : 1d array or iterable, length N
Independent variable.
mfx : 1d array of iterable, length N
Fuzzy membership function.
mode : string
Controls which defuzzification method will be used. * ‘centroid’: Centroid of area * ‘bisector’: bisector of area * ‘mom’ : mean of maximum * ‘som’ : min of maximum * ‘lom’ : max of maximum
Returns:	u : float or int
Defuzzified result.

 
The next step is to visualize these universes and membership function as plots on screen.
fig, ( ax3,ax4) = plt.subplots(nrows=2, figsize=(9,4))

    ax4.plot(applicant, applicantL_mf, 'g', linewidth=0.5, label='Low')
    ax4.plot(applicant, applicantM_mf, 'r', linewidth=0.5, label='Medium')
    ax4.plot(applicant, applicantH_mf, 'c', linewidth=0.5, label='High')
    ax4.fill_between(applicant, applicant0, aggregated_applicant, facecolor='Orange', alpha=0.7)
    ax4.plot([applicant_result, applicant_result], [0, applicant_result_activation], 'k', linewidth=1.5, alpha=0.9)
    ax4.plot([0, applicant_result], [applicant_result_activation, applicant_result_activation], 'k', linewidth=1.5, alpha=0.9)
    ax4.set_title('Applicant')
    ax4.text(applicant_result , applicant_result_activation ,"{}".format(int(applicant_result)))
    ax4.text(0 , applicant_result_activation,"{}".format(format(applicant_result_activation , '.2f')))
    ax4.legend()
    
    ax3.plot(house, houseVL_mf, 'b', linewidth=0.5,label='Very Low' )
    ax3.plot(house, houseL_mf, 'g', linewidth=0.5, label='Low')
    ax3.plot(house, houseM_mf, 'r', linewidth=0.5, label='Medium')
    ax3.plot(house, houseH_mf, 'c', linewidth=0.5, label='High')
    ax3.plot(house, houseVH_mf, 'k', linewidth=0.5,label='Very High')
    ax3.fill_between(house, house0, aggregated_house, facecolor='Orange', alpha=0.7)
    ax3.plot([house_result, house_result], [0, house_result_activation], 'k', linewidth=1.5, alpha=0.9)
    ax3.plot([0, house_result], [house_result_activation, house_result_activation], 'k', linewidth=1.5, alpha=0.9)
    ax3.set_title('House')
    ax3.text(house_result,house_result_activation,"{}".format(house_result))
    ax3.text(0 , house_result_activation,"{}".format(format(house_result_activation , '.2f')))
    ax3.legend()


 
to test the previous code we manually enter hard-coded input values:
my_market_value=333
my_location= 2
my_asset=200
my_income=35

The output will be :
 
The previous values can be categorized like this:
-Market value is 300 (High)
-Location is 2 (Bad)
	If (Location is Bad) and (Market_value is High) then (House is Medium)
	We can see the House value has been inferred between Medium and Low)

-Asset is 200 (Medium)
-Income is 35 (Medium)
	If (Asset is Medium) and (Income is Medium) then (Applicant is Medium)
	We can see that the degree of membership is 0.75 and not all the way to 1. Because the asset at 200 its degree of membership is 0.8 and not 1.
Now for the next step, we need to do all the previous step for the house membership function and fuzzy set. 
And then we take the singleton value from the output of the previous defuzzification process. And we use those values to do the inference process.
 
Because we need to calculate the credit’s amount for the applicant using the evaluation of the applicant and the evaluation of the house together with the applicant’s income and the interest rate using the hierarchal structure.
We use the same code method, but instead of getting the values of the interpolated membership functions from the user directly, we use the result values that we got as output.
For example:
    houseVL =  fuzz.interp_membership( house , houseVL_mf, house_result )
and :
    applicantL = fuzz.interp_membership( applicant , applicantL_mf, applicant_result )


