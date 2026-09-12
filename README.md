# EXPERIMENT 4: DATA WRANGLING AND DATA VISUALIZATION
Name: Reese Cassandra T. Rapis <br> Section: 2ECE-C

This repository contains my **Programming Assignment 4** for **ECE 2112: Advanced Computer Programming and Algorithms**. The activity focuses on using Pandas and Matplotlib for three problems: **Visayas Communication DataFrame**, **Visayas Female DataFrame** and **Category-Average Visualization**.

## I. OBJECTIVES
- To filter tabular data using several categorical and numerical conditions.
- To construct focused DataFrames by selecting relevant features.
- To summarize the relationship between categorical features and a numerical variable.
- To communicate a data comparison using clear and correctly labeled plots.

## II. VISAYAS COMMUNICATION DATAFRAME
**Goal:** Create a DataFrame named `VisComm` containing students whose Hometown is Visayas and whose Track is `Communication`, retaining only the Name, Gender, Math, Electronics, and Average.

**How it works:**
```python
df = pd.read_excel("board2.xlsx")

df['Average'] = df[['Math', 'Electronics', 'GEAS', 'Communication']].mean(axis=1)

VisComm = df.loc[(df['Hometown']=='Visayas') & (df['Track']=='Communication'),
                  ['Name', 'Gender', 'Math', 'Electronics', 'Average']]
```
- `pd.read_excel("board2.xlsx")` loads the ECE Board Exam dataset into a DataFrame named df.
- Since the dataset does not include a pre-computed Average, `df[['Math', 'Electronics', 'GEAS', 'Communication']].mean(axis=1)` computes it as the row-wise mean of the four subject scores.
- `df['Hometown']=='Visayas'` and `df['Track']=='Communication'` create two Boolean conditions, combined with & so both must be true before any column is selected.
- `.loc[mask, columns]` applies the combined condition and keeps only the required columns in one step.

**Checking Values:**
```python
Number of rows: 5
```
- All filtering is done by value, not by row position, so the result reflects only students who satisfy both conditions regardless of their location in the dataset.

## III. VISAYAS FEMALE DATAFRAME
**Goal:** Create a DataFrame named `VisFemale` containing students whose Hometown is Visayas and whose Gender is `Female`, retaining only the Name, Track, GEAS, Electronics, and Average. Then display only the rows with an `Average` of at least 60, without overwriting VisFemale.

**How it works:**
```python
VisFemale = df.loc[(df['Hometown']=='Visayas') & (df['Gender']=='Female'),
                    ['Name', 'Track', 'GEAS', 'Electronics', 'Average']]

print(VisFemale[VisFemale['Average'] >= 60])
```
- The same two-condition Boolean filtering pattern as Part A is used, this time for Hometown and Gender.
- A second Boolean condition `(Average >= 60)` is applied directly when printing, without reassigning VisFemale, so the original filtered DataFrame stays intact for later use.
  
**Checking Values:**
- `VisFemale` contains every female student from Visayas; the second printout is a subset of that same DataFrame, confirming no data was overwritten in the process.

## IV. CATEGORY-AVERAGE VISUALIZATION
**Goal:** Compute the mean Average for every category under Track, Gender, and Hometown, display the three summary tables, then visualize them as bar charts in a single figure, and identify the highest-performing category for each feature.

**How it works:**
```python
track_means = df.groupby('Track')['Average'].mean()
gender_means = df.groupby('Gender')['Average'].mean()
hometown_means = df.groupby('Hometown')['Average'].mean()

fig, axes = plt.subplots(1, 3, figsize=(15, 5))
axes[0].bar(track_means.index, track_means.values, color='skyblue')
axes[1].bar(gender_means.index, gender_means.values, color='salmon')
axes[2].bar(hometown_means.index, hometown_means.values, color='lightgreen')
```
- `df.groupby(feature)['Average'].mean()` groups the dataset by each categorical feature and computes the mean Average per category, all derived directly from the dataset.
- `plt.subplots(1, 3, ...)` creates one figure with three side-by-side panels, and each `axes[i].bar(...)` plots one feature's category means as a bar chart, with titles and axis labels

**Checking Values:**
```python
Mean Average by Track:      Communication (67.975), Instrumentation (65.225), Microelectronics (67.500)
Mean Average by Gender:     Female (66.616667), Male (67.183333)
Mean Average by Hometown:   Luzon (68.083333), Mindanao (66.678571), Visayas (65.750000)
```
- Communication students, Male students, and students from Luzon each recorded the highest mean Average within their respective feature.
- These are descriptive comparisons of the sample means only; a higher mean Average within a category does not, by itself, establish that Track, Gender, or Hometown causes a higher board-exam score.

Thank you for reading.
  
