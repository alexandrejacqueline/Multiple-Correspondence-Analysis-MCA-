# Multiple-Correspondence-Analysis-MCA-

Function Description: analyse_profil
This function takes a crosstab as input and calculates various statistics such as total by row and by column, row profiles, marginal profile, and distance between pairs of row modalities. It also generates a heatmap to visualize the distance between pairs of modalities.

Function Parameters:

    D: A crosstab with the same variable.

Return:

    None.

How to Use:

    Make sure that you have imported the necessary packages, such as numpy and seaborn.
    Create a crosstab with the same variable.
    Call the function "analyse_profil" and pass the crosstab as a parameter.

Example:
Suppose you have a crosstab named "my_crosstab" that looks like this:

css

           A    B    C
Gender               
Male     100   50  150
Female    80   70  120

You can call the function as follows:

scss

analyse_profil(my_crosstab)

The function will generate a heatmap and print a dataframe that shows the distance between pairs of modalities, as well as other statistics.



Documentation for "chi2" function:

Function Description:
This function performs a Pearson's chi-squared test for independence on a contingency table and interprets the results. It prints the test statistics such as the chi-squared value and the p-value, as well as a conclusion about whether the variables are independent or dependent.

Function Parameters:

    cont: A contingency table.

Return:

    None.

How to Use:

    Make sure that you have imported the necessary packages, such as scipy.stats.
    Create a contingency table.
    Call the function "chi2" and pass the contingency table as a parameter.

Example:
Suppose you have a contingency table named "my_contingency_table" that looks like this:

yaml

          Yes   No
Male     100   50
Female    80   70

You can call the function as follows:

scss

chi2(my_contingency_table)

The function will perform a Pearson's chi-squared test and print the test statistics, as well as a conclusion about the independence of the variables.


Documentation for the function co_a(df_1,df_2)

The function co_a takes two dataframes df_1 and df_2 as input and performs a Correspondence Analysis (CA) on the contingency table generated by the two dataframes. It then returns a dataframe with the estimated Attraction-Repulsion Indices (IAR) for each pair of modalities in the contingency table.
Input

    df_1: a dataframe with rows corresponding to the first variable in the contingency table.
    df_2: a dataframe with rows corresponding to the second variable in the contingency table.

Output

    A pandas dataframe with the estimated Attraction-Repulsion Indices (IAR) for each pair of modalities in the contingency table.

Dependencies

    numpy
    pandas
    prince

Usage

python

import numpy as np
import pandas as pd
import prince
from ca import CA

def co_a(df_1,df_2): 
    ca = prince.CA(
    n_components=3,
    n_iter=3,
    copy=True,
    check_input=True,
    engine="auto",
    random_state=42)
    global X
    X = pd.crosstab(df_1,df_2 ,dropna=False,margins=False, normalize=False)
    ca = ca.fit(X)
    ax = ca.plot_coordinates(X=X, 
                        ax=None,
                        figsize=(10, 9),
                        x_component=0, 
                        y_component=1, 
                        show_row_labels=True, 
                        show_col_labels=True)    
    
    tot_lig = np.sum(X.values,axis=1)
    tot_col = np.sum(X.values,axis=0)
    n = np.sum(X.values)
    E = np.dot(np.reshape(tot_lig,(len(tot_lig),1)),
               np.reshape(tot_col,(1,len(tot_col))))/n
    IAR = X.values/E
    afc = CA(row_labels=X.index,col_labels=X.columns)
    afc.fit(X.values)
    estIAR = 1+np.dot(np.reshape(afc.row_coord_[:,0],(X.shape[0],1)),
                      np.reshape(afc.col_coord_[:,0],
                     (1,X.shape[1])))/np.sqrt(afc.eig_[0][0])
    return pd.DataFrame(estIAR,index=X.index,columns=X.columns)

To use this function, pass two dataframes df_1 and df_2 as arguments.
Example

python

import pandas as pd
import numpy as np
from ca import co_a

df = pd.DataFrame({'A': ['a', 'a', 'b', 'c', 'c', 'c', 'd', 'd', 'd'],
                   'B': ['x', 'y', 'z', 'y', 'x', 'z', 'y', 'x', 'y']})
print(co_a(df['A'], df['B']))

Output:

css

          x         y         z
a  1.060737  0.893211  0.778029
b  0.871518  0.808722  1.119758
c  0.801934  1.032475  0.902796
d  1.029363  0.890388  0.801818

The output shows the estimated Attraction-Repulsion Indices (IAR) for each pair of modalities in the contingency table. The higher the value, the more the modal



Function: rep(X)

python

def rep(X):

Input

    X: a pandas DataFrame with shape (n_rows, n_cols), where n_rows and n_cols represent the number of rows and columns in the dataset, respectively.

Output

    A horizontal stacked bar chart representing the row profiles of the input DataFrame.

Description

The rep function takes in a pandas DataFrame X and generates a horizontal stacked bar chart representing the row profiles of X. The row profile of each row is represented as a stacked bar, where each bar is partitioned into segments corresponding to the column proportions of the row.

The function first calculates the total row and column sums of the input DataFrame. It then calculates the row profiles by dividing each row by its corresponding row sum.

The stacked bar chart is generated using the plt.barh function from the matplotlib library. For each column, the function iteratively plots the corresponding segment for each row, positioning each segment starting from the previous row's right edge. Finally, the function sets the y-tick labels to the row names and adds a legend showing the column names.
Example Usage

python


This will generate a stacked bar chart showing the row profiles of the input DataFrame df. Each row is represented as a stacked bar, where each bar is partitioned into segments corresponding to the column proportions of the row. The y-axis represents the row names and the legend shows the column names.


Function: etude_prof(D)

This is a Python function etude_prof(D) that performs a profile analysis on a dataset D. Here is what the function does:

    Calculates the row and column totals of the dataset.
    Calculates the row profiles by dividing each row by its row total.
    Calculates the marginal row profile by dividing the column totals by the total number of observations.
    Calculates the distances between each pair of row profiles using the formula for squared Euclidean distance, weighted by the marginal row profile.
    Prints a table of the pairwise distances between row profiles.
    Displays a heatmap of the pairwise distances between row profiles.

The output of this function is useful for examining the similarities and differences between the row profiles in the dataset, and for identifying any underlying patterns or structures in the data.


Function :  MCA

This code defines a function called mca that takes a pandas DataFrame df as input. The function first extracts the values of the DataFrame into a numpy array X. Then it creates an instance of the MCA class from the fanalysis library, passing in the row labels and column labels of the DataFrame. The fit method of the MCA instance is then called with the numpy array X.

After fitting the MCA model, the function extracts the row and column factors into pandas DataFrames df_rows and df_cols, respectively. Then it creates scatter plots of the row and column factors, mapping the first two factors onto the x and y axes. It also creates a bar chart of the column contributions to the first axis, and a scatter plot of the column cosines squared on the first axis. Note that the function only shows the plots, and does not return any values.


Function :  mca_v2

This is a Python function that performs a Multiple Correspondence Analysis (MCA) using the prince package. MCA is a statistical method used to analyze and visualize the relationships between categorical variables.

The function takes a pandas DataFrame (df) as input and performs MCA on it. It uses the prince.MCA class to perform the analysis with 2 components and 3 iterations, and uses the plot_coordinates method to visualize the results.

The resulting plot shows the relationship between the categories of the variables in the DataFrame on two axes. The points represent the categories, and their position on the axes reflects their relationship to each other. The legend shows the variable labels for the categories.
