# US Medical Insurance Costs

## Overview
The purpose of this project is to explore data about medical costs in various patients with the goal of establishing relationships between factors and identifying the factors contributing the most to higher costs. The data includes patients' age, bmi, sex, smoking status, number of children, US region inhabited, and the insurance charges. Functions were written to automate the tasks needed for the data exploration, such as chart generation.

## Resources
Data Source: insurance.csv (from Codecademy)

Software: Python 3.7.6

## Tableau Dashboard
https://public.tableau.com/app/profile/ee1691/viz/Insurance_Costs/CostsDashboard

## Initial Data Exploration
The dataset was clean to begin with so there was no further cleaning needed. The first step in the data exploration was to check whether costs had any relationships with the other variables. This was achieved with simple scatterplots. While scatterplots are best for numerical (age/bmi) rather than categorical variables (sex/smoking status/region), they still were able to easily show how the patients compared against each other.

![This is an image](https://github.com/EricaEidelman/US_Medical_Insurance/blob/main/Images/Age_Costs.png)

![This is an image](https://github.com/EricaEidelman/US_Medical_Insurance/blob/main/Images/BMI_Costs.png)

![This is an image](https://github.com/EricaEidelman/US_Medical_Insurance/blob/main/Images/Sex_Costs.png)

![This is an image](https://github.com/EricaEidelman/US_Medical_Insurance/blob/main/Images/Children_Costs.png)

![This is an image](https://github.com/EricaEidelman/US_Medical_Insurance/blob/main/Images/Smoking_Costs.png)

![This is an image](https://github.com/EricaEidelman/US_Medical_Insurance/blob/main/Images/Region_Costs.png)

A few insights could be found from the generated plots. In terms of age, there appear to be three distinct groups of patients, indicating other variables had an impact on their medical costs, but all three groups still show that costs increase as age increases. BMI also appears to have a positive relationship with costs, albeit not a very strong one, also indicating that there are other factors affecting costs. And while smoking status is a categorical variable, the plot clearly indicates that the range of costs for smokers has a higher minimum and maximum cost than that of non-smokers.

The next step was to claculate average costs across the different variables. One function was used to find the average cost of fields in the dictionary of patient records, while another was used to generate bar charts. The two functions are shown below:

```
# Make function to find average costs
def find_avg_cost(dictionary, entry_list, list_field):
    cost_dictionary = {}
    for item in entry_list:
        total_cost = 0
        num_records = 0
        for record in dictionary:
            if dictionary[record][list_field] == item:
                total_cost += dictionary[record]["Charges"]
                num_records += 1
        average_cost = total_cost/num_records
        cost_dictionary[item] = round(average_cost,2)
    return cost_dictionary
```
```
# Make function to plot bar charts of average costs
def bar_chart(dictionary, x_axis, y_axis, type):
    x_value = []
    y_value = []
    for item in dictionary.keys():
        x_value.append(item)
    x_value.sort()
    for item in x_value:
        y_value.append(dictionary[item])
    plt.bar(x_value, y_value)
    plt.xlabel(x_axis)
    plt.ylabel(f"{type} {y_axis}")
    plt.title(f"{type} {y_axis} by {x_axis}")
    plt.show()
```

Because age and BMI are numerical variables, they needed to be converted to categories usable for a bar chart. The below function was used to create range buckets for age and BMI.

```
# Function to convert numbers to range buckets
def num_to_range(dictionary, list_field):
    for record in dictionary:
        if dictionary[record][list_field] < 10:
            dictionary[record][list_field] = "0-9"
        elif dictionary[record][list_field] < 20:
            dictionary[record][list_field] = "10-19"
        elif dictionary[record][list_field] < 30:
            dictionary[record][list_field] = "20-29"
        elif dictionary[record][list_field] < 40:
            dictionary[record][list_field] = "30-39"
        elif dictionary[record][list_field] < 50:
            dictionary[record][list_field] = "40-49"
        elif dictionary[record][list_field] < 60:
            dictionary[record][list_field] = "50-59"
        elif dictionary[record][list_field] < 70:
            dictionary[record][list_field] = "60-69"
        elif dictionary[record][list_field] < 80:
            dictionary[record][list_field] = "70-79"
        elif dictionary[record][list_field] < 90:
            dictionary[record][list_field] = "80-89"
        elif dictionary[record][list_field] < 100:
            dictionary[record][list_field] = "90-99"
        else:
            dictionary[record][list_field] = "100+"
    return dictionary
```

The resulting bar charts are shown below:

![This is an image](https://github.com/EricaEidelman/US_Medical_Insurance/blob/main/Images/Age_AvgCost.png)

![This is an image](https://github.com/EricaEidelman/US_Medical_Insurance/blob/main/Images/BMI_AvgCost.png)

![This is an image](https://github.com/EricaEidelman/US_Medical_Insurance/blob/main/Images/Sex_AvgCost.png)

![This is an image](https://github.com/EricaEidelman/US_Medical_Insurance/blob/main/Images/Children_AvgCost.png)

![This is an image](https://github.com/EricaEidelman/US_Medical_Insurance/blob/main/Images/Smoking_AvgCost.png)

![This is an image](https://github.com/EricaEidelman/US_Medical_Insurance/blob/main/Images/Smoking_AvgCost.png)

![This is an image](https://github.com/EricaEidelman/US_Medical_Insurance/blob/main/Images/Region_AvgCost.png)

Plotting the average costs for each variable revealed several insights. First, as age and BMI increases, costs generally rise. Also, males have a somewhat higher average cost than do females, and the average costs for smokers are just over three times as much as the average costs for non-smokers. Also, the different regions have different average costs, with charges highest in the southeast, followed by the northeast, and then the northwest and southwest. This last finding led to the question of which differences in lifestyle among the four regions lead to cost differences. 

## Data Exploration by Region
In order to graph the average values of age, bmi, and number of children by region, it was first necessary to define a function which would calculate that amount. Additionally, because the sex and smoker status variables can be considered "boolean" (having only one of two possible values), it would be better to graph the percentage of smokers and males in each region as those are the groups with the higher average costs. Anothe function was needed in order to calculate the percentages. The two functions are listed below.

```
# Function to find average amount of non-boolean independent variables in each region
def var_avg_amount(dictionary, variable):
    region_list = unique_list(medical_records, "Region")
    avgs_dictionary = {}
    for region in region_list:
        total = 0
        length = 0
        for record in dictionary:
            if dictionary[record]["Region"] == region:
                total += dictionary[record][variable]
                length += 1
        avg_value = round(total/length,2)
        avgs_dictionary[region] = avg_value
    return avgs_dictionary
```

```
# Function to find percentage of one of boolean choices
def bool_avg_prcnt(dictionary, variable, choice):
    region_list = unique_list(medical_records, "Region")
    prcnt_dict = {}
    for region in region_list:
        total = 0
        length = 0
        for record in dictionary:
            if dictionary[record]["Region"] == region:
                length += 1
                if dictionary[record][variable] == choice:
                    total += 1
        prcnt = round((total/length)*100,2)
        prcnt_dict[region] = prcnt
    return prcnt_dict
```

Once these functions were defined, they could be used in conjunction with the previously defined bar chart creation functions to achieve the following charts:

![This is an image](https://github.com/EricaEidelman/US_Medical_Insurance/blob/main/Images/Region_AvgAge.png)

![This is an image](https://github.com/EricaEidelman/US_Medical_Insurance/blob/main/Images/Region_AvgBMI.png)

![This is an image](https://github.com/EricaEidelman/US_Medical_Insurance/blob/main/Images/Region_AvgChildren.png)

![This is an image](https://github.com/EricaEidelman/US_Medical_Insurance/blob/main/Images/Region_PrcntSmoker.png)

![This is an image](https://github.com/EricaEidelman/US_Medical_Insurance/blob/main/Images/Region_PrcntMale.png)

From the created bar charts, it is visible that the southeast region has the highest BMI as well as the highest percentage of smokers and males, all of which were previously shown to be factors contributing to higher insurance costs. The northeast region, which has the second-highest costs, likewise has the second-highest percentage of smokers and males.

## Summary
The exploratory data analysis in this project revealed that some of the more significant factors contributing to higher insurance costs are a person's BMI, smoking status, and gender. These factors reveal why the regions of the US which have higher percentages of males and smokers and higher average BMIs then have higher average costs. One conclusion which can be drawn is that reducing the rate of smoking and BMI levels will contribute to a reduction in insurance charges. Going beyond a simple descriptive analysis, further research can be done into why males have higher insurance costs than females, with the goal of reducing costs in the highest-paying regions even further.
