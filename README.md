# ML-World-Rugby-Results 
![image](https://github.com/user-attachments/assets/33a0a7d2-b2bd-471d-88f1-735f74662436)

The main purpose of this project was to practice an initial **Machine Learning** model.

Specifically, a ***Logistic Regression*** model...along with a ***prediction*** showing the probability of the "***home_team***" to win the match.

The score of the ***Logistic Regression*** model was also compared to the returned by a ***KNN Classification*** model.

## Data Preparation

**1**. I looked for a dataset on Kaggle. 

I took one having info regarding the **"Rugby Results"** of most important teams (South Africa, Ireland, New Zealand, England, France, etc.) since 1871 up to mid August 2024.

**2**. I imported the ***.csv*** file in Python, and I did a first exploration of the data.
 
**3**. I added missing matches of 2024 -> From mid August 2024 up to December 2024.

![image](https://github.com/user-attachments/assets/0691b8ad-7027-4d13-a834-80fddd0eafac)

**4**. I created a new column "***score_diff***", being the subtraction of both columns "***home_score***" and "***away_score***".

**5**. I did few vizzes (Hitsplots and Boxplots) of the above mentioned columns in order to check the corresponding data distribution; and consider potential *Transformations*, *Normalizations*, etc.

![image](https://github.com/user-attachments/assets/71ba7d0c-21ff-4566-a909-9d93e6786bf4)
![image](https://github.com/user-attachments/assets/6437df33-9a66-4ee4-a270-7cf2bd44d244)
![image](https://github.com/user-attachments/assets/aaed2152-885d-4702-a0d5-160553222695)

After checking the final outcome of the project, I discarded *Transformations* in both columns since the difference wasn't significant; in addition, since column "***away_score***" was finally dropped for computation purposes.

**6**. I also explored data in columns "***stadium***", "***competition***" or "***country***"; such columns were standardized later on.

## WebScraping

**7**. I webscraped the site ***www.stadiumdb.com*** with ***BeautifulSoup*** in order to get the stadiums' capacities; and join such info in the original dataset:
  - The webscraping was done for each country having an stadium in the original dataset -> Argentina, South Africa, New Zealand, Japan, France, England, Italy, Singapore, etc.
  - I also webscraped historical stadiums, since some of them were appearing in the original dataset too.

![image](https://github.com/user-attachments/assets/84fbfce9-2d10-48f7-b98a-d2457819b741)
![image](https://github.com/user-attachments/assets/c4af8a45-a95c-4cdf-bed4-5b35712b2a0a)
![image](https://github.com/user-attachments/assets/02059562-aafd-4bb7-bc6f-1be9d2f96365)

**8**. Once all stadiums were webscraped (satdium's name, city and capacity), I created a DataFrame -***stadiums_final***- having all this info together.

![image](https://github.com/user-attachments/assets/380b47b8-c2e3-4e55-8c71-a5d384b29cb2)

## Data Standardization

**9**. Afterwards, I carried out some *standardization* tasks:
  - Converting stadiums' capacities into **int64**.
  - Converting dates into **datetime**.
  - Firstly dropping matches before 1987, since that's the year of the 1st Rugby World Cup.
  - Grouping competitions into 4 different classes -> **Test Matches**, **Rugby Championship**, **Six Nations** and **Rugby World Cup**. The main reason was because competitions' names vary along the years -although being essentially the same-. 
  - I renamed stadiums' names of the original dataset -according to the names I previously webscraped- to be able to merge the info in further steps.

![image](https://github.com/user-attachments/assets/2639704a-3b15-4c05-89aa-1d3cde2a9674)
![image](https://github.com/user-attachments/assets/a1b16219-fd23-40a7-960e-1637e83ac91a)
![image](https://github.com/user-attachments/assets/d372d4f6-d509-4286-9b12-b83e6f7832b4)

**10**. I created a **new** DataFrame -***wru_intermediate***- where I merged both datasets "*original*" and "*stadiums_final*", and I dropped some duplicated columns ("***competition***", "***stadium***" and "***city***").

![image](https://github.com/user-attachments/assets/a4ea70da-4681-462c-8a00-032e2825c64f)

**11**. I did few vizzes (Hitsplots and Scatterplots) between some independent potential variables (X) of the model; for instance, between column "***capacity***" and "***home_score***", "***away_score***" or "***score_diff***". I also did a **Correlation Matrix** for all variables having numerical data so far.

![image](https://github.com/user-attachments/assets/9db5ec05-b059-4b6f-8f54-e511f9a24732)

**12**. I created a new column "***jet_lag***" to consider some *jet-lag effect* in the final *Machine Learning* model -> If the "***away_team***" was playing the match in a country being of a different continent than its own -and not being a World Cup's match-, then such a team was suffering of jet-lag. Otherwise, it wasn't.

![image](https://github.com/user-attachments/assets/3d345436-aa45-470d-b247-d61f314d4f2e)

**13**. I also included 2 new columns, "***home_ranking***" and "***away_ranking***", where the ranking of each team was returned for each of the years a given match was played.

For simplicity reasons -not the best approach-, I took the ranking of each team at the end of such a given year.

Since the first available ranking is for 2003 -according to WRU-, I finally dropped all matches played before that year too; creating a new DataFrame -***wru_final***-.

![image](https://github.com/user-attachments/assets/ff5662ab-a411-474f-9c1c-83e7a9b2bec8)
![image](https://github.com/user-attachments/assets/85813098-27c4-44b2-9463-658463f75f90)

## Machine Learning

**14**. Once I had the ultimate dataset ready, I started considering the **Machine Learning** model -> ***Logistic Regression***:
  - **14.1**. I created a new column "***home_win***", where it returned *True* if the "***score_diff***" was positive; and it returned *False* otherwise. This variable was used as the ***target*** (y) of the model.

  ![image](https://github.com/user-attachments/assets/089914f6-11bd-4b28-9ebf-aa9c7d01e8b7)

  - **14.2**. I created **dummy** variables out of the previously standardized competitions' names -> **Test Matches**, **Rugby Championship**, **Six Nations** and **Rugby World Cup**.

  ![image](https://github.com/user-attachments/assets/678ed10c-044e-4247-8757-77191a729423)

  - **14.3**. I also did a couple of scatterplots between both the "***home_score***" and "***away_score***" (*independent variables*), and the "***home_win***" (*dependent variable*) to see the corresponding data distribution.

 ![image](https://github.com/user-attachments/assets/3f3bc2f1-8ed9-413c-be9e-af567199f8d1)
 ![image](https://github.com/user-attachments/assets/300c1e2e-d5e9-4894-9b73-83aa72e7fb46)

  - **14.4**. In order to get a reference score for the model, I computed the "***home_win***" / "***total matches***" ratio -> `59.32%`.
 
 ![image](https://github.com/user-attachments/assets/9e873965-df28-4d41-bef6-412e4ac3467d)

  - **14.5**. I did a version of the model, where from my ultimate dataset ***wru_final***:
    - I dropped some variables since they seemed to be not significant or redundant; for instance, "***home_score***", "***away_score***", "***world_cup***", "***score_diff***" or "****city***".

    ![image](https://github.com/user-attachments/assets/60315c06-a582-4bdd-bea1-201b88d1099d)

    - I **defined** both all *independent variables (X)* - all but "***home_win***"- and the *dependent variable (y)* -"***home_win***"-.
    - I **split** the data between *train* and *test* for both all *independent variables (X)* and the *dependent variable (y)*.

    ![image](https://github.com/user-attachments/assets/fff7bad0-4942-4959-8dcc-e03cb4f2c517)

    - I **defined** the model.

    ![image](https://github.com/user-attachments/assets/1a77a440-4884-4b38-9a82-018f1d020a3e)

    - I **scored** the model.

    ![image](https://github.com/user-attachments/assets/81111168-3684-4038-a291-2f32a91cd886)

    - I printed a **Confusion matrix** of the model.

    ![image](https://github.com/user-attachments/assets/7f2d0230-a2f5-4178-b5e2-393473a7fe7c)

    - I printed the **metrics** -Precision, Recall, F1-Score- of the model.

    ![image](https://github.com/user-attachments/assets/b3c4e9d8-284b-43a1-932e-657a4aa80899)

    - I printed the **ROC curve** of the model.

    ![image](https://github.com/user-attachments/assets/bceef6c7-ac89-4e1c-986f-37e764f7ec9e)

    - I finally played with some **predictions** of the model.

    ![image](https://github.com/user-attachments/assets/d3e2c62c-9f37-4ecc-a67e-c821ab88da6d)
    
    ![image](https://github.com/user-attachments/assets/39666b16-9144-4797-8c43-32e0bfa6dff7)
    ![image](https://github.com/user-attachments/assets/5471bb02-4c97-48ea-bc00-e8437e9b0d85)

**15**. I applyed a new model, based on ***KNN Classification***, in order to compare the resulting score with the previous ***Logistics Regression*** model; as well as with the initially computed "***home_win***" / "***total matches***" ratio. For such model, I applied the *StandardScaler()* to the variable "***home_score***".

![image](https://github.com/user-attachments/assets/eaf6c8fd-5959-4bc1-9828-dfbe3ca1d292)

According to the returned score (`0.7889908`), such ***KNN Classification*** model **doesn't improve** the score of the ***Logistics Regression*** model (`0.8302752`). 

## More insights

**16**. To finish this exercise, I also took some **insights** based on the data out of the ultimate dataset; for instance:
  - Split of the jet-lag effect by country.

  ![image](https://github.com/user-attachments/assets/cb549de1-86cb-4ad0-a83a-94ab476e81b7)

  - Count of "***home_win***" by "***home_team***".

  ![image](https://github.com/user-attachments/assets/941f4069-16d7-4bdc-a15c-a8be38afc820)

  - Sum of "***score_diff***" by "***home_team***".

  ![image](https://github.com/user-attachments/assets/40d949f5-22bc-48fb-ba37-bb1030d826ac)

  - Minimum, maximum and mean score by both "***home_team***" and "***away_team***".

  ![image](https://github.com/user-attachments/assets/48582127-4bd2-49cb-ae7d-e6dd5e3b8e60)
  ![image](https://github.com/user-attachments/assets/e3cf45d8-3ad8-4de3-b40d-170c42371160)

  - Percentage of both "***home_win***" and "***away_win***" by stadium's "***capacity***".

  ![image](https://github.com/user-attachments/assets/7d26fbad-6cfe-4620-9218-31341e500b85)
  
  - Minimum, maximum and mean "***score_diff***" by "***home_team***", and each "***away_team***".

  ![image](https://github.com/user-attachments/assets/b0416514-33c3-4343-a337-47bbd43d1c77)
