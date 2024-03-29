# Workout Program Data Analyst Project using __Python__
Data Analyst Project using Python with the Pandas, NumPy, and Seaborn libraries for a 12 week workout program that I made for myself. I will be using Git Hub and Git Bash for version control.

This project will be similar to the [this project](https://github.com/Romeeeo/Workout_Program_Data_Analyst_Project) that I did in the past. I will be using the same dataset, but will be using __Python__ to analyze the data.

## Introduction
In this case study, I will perform many real-world tasks of a junior data analyst at a fictional fitness company. In order to answer the key business questions, I will follow the steps of the data analysis process: [Ask](https://github.com/Romeeeo/Workout_Program_Python_Data_Analyst_Project/edit/main/README.md#ask), [Prepare](https://github.com/Romeeeo/Workout_Program_Python_Data_Analyst_Project/edit/main/README.md#prepare), [Process](https://github.com/Romeeeo/Workout_Program_Python_Data_Analyst_Project/edit/main/README.md#process), [Analyze](https://github.com/Romeeeo/Workout_Program_Python_Data_Analyst_Project/edit/main/README.md#analyze-and-share), [Share](https://github.com/Romeeeo/Workout_Program_Python_Data_Analyst_Project/edit/main/README.md#analyze-and-share), and [Act](https://github.com/Romeeeo/Workout_Program_Python_Data_Analyst_Project/edit/main/README.md#act).
### Quick Links
Data Source: For this project I took one of my 12 week workout programs and created a CSV from it. You can download the Data [Here](https://drive.google.com/uc?export=download&id=1AJovdTjjJM06A0DlkZkRYzWrUd2h6Tub)

Jupyter Notebook: [Jupyter Notebook](https://github.com/Romeeeo/Workout_Program_Python_Data_Analyst_Project/blob/main/workout_program_python.ipynb)


## Background

### Scenario
I am a junior data analyst working in the marketing analyst team at a fitness company. The director of marketing believes the company’s future success depends on maximizing the number people subscribing to their workout programas. Therefore, your team wants to understand the key components and valuable knowledge that is needed from a general 12 week program. From these insights, your team will design a new marketing strategy to bring in new subscribers. But first, the executives must approve your insights, so they must be backed up with compelling data insights and professional data visualizations.

## Ask
### Business Task
Devise insights on a 12 week program to allow people to better understand what is to come and what is needed.
### Analysis Questions
Three questions will guide the future marketing program:  
1. What are the key components of the 12 week workout program? 
2. What are the main equipments that are utilized in the program?
3. How do the different intensity levels differ in the program?  

The main objective is to provide insight on a simple 12 week program that everyone can follow.

The insights will help people ease in to a workout program with general valuble knowledge on what is to come, whether they are a beginner or are advanced.
## Prepare
### Data Source
For this project I took one of my 12 week workout programs and created a CSV from it. You can download the Data [Here](https://drive.google.com/uc?export=download&id=1AJovdTjjJM06A0DlkZkRYzWrUd2h6Tub)
### Data Organization
There is only one data file that consist of a workout program that spands over 12 weeks. The data consist of data such as the workout id, workout, muscle group, equipment, reps, number of sets, whether a workout is a superset or not, program week, date of a workout, and stage of the workout. The corresponding column names are workout_id, workout, muscle_group, equipment, reps, num_of_sets, superset, program_week, date_of_workout, stage.

## Process
I will be using the Jupyter Notebook with Python and utilizing both Pandas and NumPy for analyzing the data and Seaborn for data visualization.
### Reading in the CSV file
```
import pandas as pd
import numpy as np
import seaborn as sns

df = pd.read_csv('Data/12wk_Program_RM_new.csv')
```
### Data Exploration
```
df.head()
```

Displays first five rows of data.

```
df.info()
```

Tells us info about the data in the dataset. __#__ of columns, Column __names__, __Non-Null Count__ for each column, and __data types__ for each column
```
df.isnull().sum()
```
Tells us how many __Nulls__ each column contain.
We can make the following observations based on the work above:
- The data seems to contain all of the necessary columns that we need to complete our analysis
- There seems to be no missing/Null data values. There are 453 rows of data and 453 non-null values for each column
### Data Cleaning
__Identify Missing Values and Drop Unnecessary Columns__

Since we need all of the necessary columns in the dataset, we will not be dropping any columns.

We will also not need to reread the data in identifying the null values as `NaN` as there are no Null values in the data.

Here is what the dataset looks like now:

![w1](Images/w1.png)

Convert the data types:

```
df.convert_dtypes()
```

__Clean Column Names__
```
df.columns = df.columns.str.lower().str.strip().str.replace(' ', '_')
```
Running this line of code updates all column names to lowercase using `str.lower()`, remove leading and trailing whitespaces `str.strip()`, and replace all spaces with `_` using `str.replace(' ', '_')`

We can check the new column names by running:
```
df.columns
```

Here are the column names for the dataset:

![w2](Images/w2.png)

We now need to convert the `date_of_workout` column to __datetime__ as this is the only column that is of that datatype.

```
df['date_of_workout'] = pd.to_datetime(df['date_of_workout'])
```
__Create a new column__

We will also create a new column for the month that the workout takes place. There should only be four months (Mar, Apr, May, Jun).

```
# Convert the months into string values
def tranform_month(val):
    if val == '3':
        return "mar"
    elif val == '4':
        return "apr"
    elif val == '5':
        return "may"
    elif val == '6':
        return "jun"
    else:
        return np.nan
df['month'] = df['month'].apply(tranform_month)

# Quick check of the update
df['month'].value_counts()
```

Here are the value counts for the `month` column:

![w3](Images/w3.png)

I will also change the `workout_id` column to match the index using a function and applying it to that column.

```
# Change all of the workout id's to match the index
def chng_to_index(val):
    return val - 1

df['workout_id'] = df['workout_id'].apply(chng_to_index)
```

__Clean Row Values__

Change all of the values to be lowercase.

```
# Applying lowercase transformation to all values
df = df.applymap(lambda x: x.lower() if isinstance(x, str) else x)
```


__Filter the Data__

Remove all null values:

```
df.dropna()
```

Dropping all of the duplicates based on the column, `workout_id`:

```
df = df.drop_duplicates(subset=['workout_id'])
```

Check the unique values for the `muscle_group` column in the dataset.

```
df['muscle_group'].value_counts()
```

Here we can see that there are __7__ muscle groups that we work on in this program. The muscle groups are __legs__, __abs__, __back__, __chest__, __shoulders, triceps__, and __biceps__


We can check to see which equipment is used throughout the program:

```
df['equipment'].value_counts()
```

Here is the list of all the equipment used:

![w4](Images/w4.png)

List of the stages for the program:

```
df['stage'].value_counts()
```

![w5](Images/w5.png)

__Verify the Data__

We will now check the min and max of the number of sets for each workout, should be greater than __0__

```
# Check the range of num_of_sets column
min_value = df['num_of_sets'].min()
max_value = df['num_of_sets'].max()

print("Range for number of sets throughout the workout program {}-{}:".format(min_value,max_value))
```

Here we can see that the range is __2-6__.

Make sure that there is only 12 weeks in the program:

```
max_week = df['program_week'].max()

print("Max week:", max_week)
```

Check the ranges for `reps` also:

```
min_value = df['reps'].min()
max_value = df['reps'].max()

print("Range for number of reps throughout the workout program {}-{}:".format(min_value,max_value))
```

## Analyze and Share

Jupyter Notebook: [Jupyter Notebook](https://github.com/Romeeeo/Workout_Program_Python_Data_Analyst_Project/blob/main/workout_program_python.ipynb)

Now that the data is clean and ready to be analyzed, let's focus on the analysis question that we discussed in the beginning of this project.

<center>
What are the key components of the 12 week workout program and how do the intensity levels differ?
</center>

__Total number of workouts__

```
df.shape[0]
```

__Number of workouts__ each month:

```
df['month'].value_counts()
```

![w3](Images/w3.png)

__Number of workouts for each muscle group:__

```
# Number of workouts per muscle group throughout the program

# Creating a bar chart using Seaborn
sns.countplot(x='muscle_group', data=df, order=df['muscle_group'].value_counts().index)

# Adding labels and title
plt.ylabel('Number of workouts')
plt.xlabel('Muscle Group')
plt.title('Number of workout per Muscle Group')

# Displaying the plot
plt.show()
```

![w10](Images/w10.png)

Here is the percentage distribution:

```
df['muscle_group'].value_counts(normalize=True) * 100
```

![w6](Images/w6.png)

Here we can clearly see that __leg workouts__ make up the most of the workout program. We are able to understand that this program __focuses__ on `legs` more than any other muscle group. On the other hand `biceps` make up the __least__ amount of workouts in the program.

__Next lets find the average number of sets per workout:__

```
# Average number of sets per workout to the nearest whole number
avg_sets = df['num_of_sets'].mean().round()

print(avg_sets)
```

The result we get here is __3.0__, which gives us an average of 3 sets throughout the program.

__Number of workouts per stage:__

```
df['stage'].value_counts()
```

![w7](Images/w7.png)

As percentages:

![w8](Images/w8.png)

__Now let's check to see what eqipment is used throughout the program:__

```
df['equipment'].value_counts()
```

![w9](Images/w9.png)

```
# Number of workouts per equipment
# Creating a horizontal histogram using Seaborn
counts = df.groupby('equipment').size().reset_index(name='Count')

sns.barplot(x='Count', y='equipment', orientation='horizontal', data=counts, order=df['equipment'].value_counts().index)

# Adding labels and title
plt.xlabel('Number of workouts')
plt.ylabel('Equipment')
plt.title('Number of workouts per equipment')

# Displaying the plot
plt.show()
```

![w12](Images/w12.png)

Looking at the visual, we can observe that the top three equipment used in the program are `machine`, `dumbbells`, and `body weight` (body weight in the case is considered an equipment). On the other hand we can see that the `thick bar` is rarely used in the 12 week program, with only 1 workout using it out of all 453.

__Number of workouts per muscle group each month:__

```
# Number of workouts per muscle group each month

# create dataframe with count index
counts = df.groupby(['month', 'muscle_group']).size().reset_index(name='Count')

# custom order for the months
custom_order = ['mar', 'apr', 'may', 'jun']

# Sorting the DataFrame based on the custom order
df_sorted = counts[counts['month'].isin(custom_order)].sort_values(by='month', key=lambda x: [custom_order.index(e) for e in x])

# Creating a pointplot using Seaborn with counts on the y-axis and hue
sns.lineplot(x='month', y='Count', hue='muscle_group', data=df_sorted)

# Adding labels and title
plt.xlabel('Month')
plt.ylabel('Number of workouts')
plt.title('Number of workouts per muscle group each month')

# Displaying the plot
plt.show()
```

![w11](Images/w11.png)

In this visual we can see all see a peak in the number of workouts in the month of April and May. In this case, people should be prepared to get __more workouts__ in during those months, and provide sufficient time to complete each and all of the workouts.

On the otherhand, the number of workouts tend to be the lowest in March and June at the beginning of the program and towards the end of the program. This makes sense, as the workouts at the beginning are preparing you for the program and the workouts at the end tend to be more intense at the end of the program in the `shred` stage.

__Number of days off__

We first have to calculate the number of days we workout each month:

```
# Month of March
mar_month = df[df['month'] == 'mar'].copy()

count_of_workout_in_march = mar_month['date_of_workout'].value_counts().count()
```

```
# Month of April
apr_month = df[df['month'] == 'apr'].copy()

count_of_workout_in_april = apr_month['date_of_workout'].value_counts().count()
```

```
# Month of May
may_month = df[df['month'] == 'may'].copy()

count_of_workout_in_may = may_month['date_of_workout'].value_counts().count()
```

```
# Month of June'
jun_month = df[df['month'] == 'jun'].copy()

count_of_workout_in_june = jun_month['date_of_workout'].value_counts().count()
```

Then we will calculate the number of days off per month by subtracting the number of days working out by __30__.

```
# Calculate the number of days off
days_off_mar = 30 - count_of_workout_in_march
days_off_apr = 30 - count_of_workout_in_april
days_off_may = 30 - count_of_workout_in_may
days_off_jun = 30 - count_of_workout_in_june

print("Days off in the month of March:", days_off_mar)
print("Days off in the month of April:", days_off_apr)
print("Days off in the month of May:", days_off_may)
print("Days off in the month of June:", days_off_jun)
```

This is the result that we get:

![w13](Images/w13.png)

Next, lets plot this. We will create a new dataframe with the number of days working out per month:

```
data = {'month': ['mar', 'apr', 'may', 'jun'],
        'days_working_out': [count_of_workout_in_march, count_of_workout_in_april, count_of_workout_in_may, count_of_workout_in_june],}

df_months_worked_out = pd.DataFrame(data)
```

Then we will plot the dataframe:

```
sns.barplot(x='month', y='days_working_out', data=df_months_worked_out)

plt.xlabel('Month')
plt.ylabel('Days Working out')
plt.title(' Number of days working out per month')
```

![w14](Images/w14.png)

Let's now look at the total days of each stage the workout program consist of. The four stages in the program are `preperatory`, `hypertrophy`, `loading`, and `shred`.

We will follow the same steps as before to plot this out:

Figure out the number of days working out for each stage:

```
# Calculate the number of days working out for the preperatory stage
prep_stage = df[df['stage'] == 'preperatory'].copy()

prep_days_workout = prep_stage['date_of_workout'].value_counts().count()
```

```
# Calculate the number of days working out for the hypertrophy stage
hyp_stage = df[df['stage'] == 'hypertrophy'].copy()

hyp_days_workout = hyp_stage['date_of_workout'].value_counts().count()
```

```
# Calculate the number of days working out for the loading stage
load_stage = df[df['stage'] == 'loading'].copy()

load_days_workout = load_stage['date_of_workout'].value_counts().count()
```

```
# Calculate the number of days working out for the shred stage
shred_stage = df[df['stage'] == 'shred'].copy()

shred_days_workout = shred_stage['date_of_workout'].value_counts().count()
```

Then we will make a dataframe and plot it:

```
data = {'stage': ['preperatory', 'hypertrophy', 'loading', 'shred'],
        'days_working_out': [prep_days_workout, hyp_days_workout, load_days_workout, shred_days_workout]}

df_stage_workout = pd.DataFrame(data)

df_stage_workout
```

```
# Plot the dataframe
sns.barplot(x='stage', y='days_working_out', data=df_stage_workout)

# Labels
plt.xlabel('Stage')
plt.ylabel('Days working out')
plt.title('Number of days working out per Stage')
```

![w15](Images/w15.png)

Here we can see that the `hypertrophy` stage makes up most of the program. This makes sense, as this is where most of the muscle growing is done, after preparing for it. The `preperatory` stage makes up the __least__ amount of days in the program. This tells us that the ___majority___ of the stage consist of __muscle growing and losing fat__. The beginning of the porgram (`preperatory` stage) gets you __ready__ and your body __prepared__ for the rest of the program that is to come, hence the name.

Summary:
  
|Analysis and Insights|
|------|
|The program heavily focuses on building muscles for your legs.|
|The program tends to lean more towards the 8-12 rep range and 3-4 sets range for most workouts.|
|Workouts tend to increase in the month of April in the hypertrophy stage, where muscle growth and fat loss is most incetivised.|  
|THe program utilizes mahines, dumbbells, and your body weight for most of the workout.|
|Arms are less focused in this program (shoulders, triceps, and biceps).|
|Workouts tend to be repeated throughout the weeks.|
|The shred stage (towards the end of the program) is when the workout become more intense.|
  
## Act
After analyzing the 12 week program, we can suggest recommendations to people on subscribing to the program.

1. People will most likely need to have a gym memebership in order to utilize the equipment used in the program.
2. Legs will need to be more prepared than any other muscle group.
3. People will get more knowledge on workout and what muscle group they work on after the program is done, as workouts are easy to understand and they tend to repeat throughout the weeks.
4. The program is each to understand and gives valuable specifics.

