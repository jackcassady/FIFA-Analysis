# FIFA-Analysis
Analysis of all the players and clubs from FIFA 2019

## Data

The data contains information about players from the FIFA 2019 game. Included in this data are the categories for each player of:
* ID - corresponding to a players ID number within the game
* Name
* Age
* Nationality
* Overall - the rating of the player in the game
* Club - the football club for which the player plays
* Value - monetary value of the player in Euros
* Wage - How much player is paid on a weekly basis in Euros
* Preferred Foot
* Jersey Number
* Height
* Weight

## Functions

Converting string of money into a number:

```python
def money(raw_wage):
    if raw_wage == '€ 0':
        return 0
    else:
        wage = float(raw_wage.strip(raw_wage[-1]).strip(raw_wage[0]))
        if raw_wage[-1] is 'K':
            return wage * 1000
        elif raw_wage[-1] is 'M':
            return wage * 1000000
        elif raw_wage[-1] is 'B':
            return wage * 1000000000
```

Return all values in a given column:

```python
def get_column(col_name):
    """Returns items in player_data of the specified column name"""
    values = []
    for i in range(len(player_data)):
        col = player_data[i][header.index(col_name)]
        values.append(col)
    return values
```

Count the number of players of a certain nationality:

```python
def player_count(country):
    """Counts the number of players per country."""
    count = 0
    nation = get_column('Nationality')
    for i in range(len(nation)):
        if nation[i] == country:
            count += 1
    return count
```

Convert player data into a dictionary:
```python
def player_to_dict(ID):
    """Finds player using ID number and returns a dict of values"""
    keys = header
    player = {}
    for i in range(len(player_data)):
        if player_data[i][0] == ID:
            vals = player_data[i]
            for i in range(len(keys)):
                player[keys[i]] = vals[i]
    return player
```

## Data Analysis

We can find the oldest player, highest paid player, highest valued player, and highest valued club using similar code like I used below. I looped through the data creating a dictionary with player names or club as the key and either age, wage, or value for the value. Then, looping through this dict, I compared the value at the index with the previous highest value defined by a variable.

```python
oldest = None
ages = {}
for i in range(len(player_data)):
    age = player_data[i][header.index('Age')]
    name = player_data[i][header.index('Name')]
    ages[name] = age

for player in ages:
    if oldest is None or ages[player] > ages[oldest]:
        oldest = player
oldest
```

Utilizing the code above, I found that `O. Perez` was the oldest player in the dataset. `L. Messi` is the highest paid player in the dataset. `Neymar Jr` is the highest valued player and `Paris Saint-German` is the highest valued club in the dataset. 

Next, I found the average player age, and average player value by looping through the dataset and adding all of the values for each player then dividing by the number of players using the `len()` function. 

```python
total = 0
total_vals = get_column('Value')
for i in range(len(total_vals)):
    total += money(total_vals[i])
avg_value = total / len(total_vals)
avg_value
```

Giving the average age of `25.122205745043114` and an average value of `2410695.8861976163`

We can use the `player_count()` function listed above in order to find the number of players each country has in the dataset. 

* Portugal has `322` players in the dataset
* Brazil has `827` players
* England has the most players in the dataset with `1662`

The top ten countries in terms of player representation are:

Country | # of players
------- | ------------
England | 1662
Germany | 1198
Spain | 1072
Argentina | 937
France | 914
Brazil | 827
Italy | 702
Colombia | 618
Japan | 478
Netherlands | 453

We can use the same code to see what are the most popular jersey numbers among players:

```python
jersey_numbers = {}
jersey = get_column('Jersey Number')
for i in range(len(jersey)):
    if jersey[i] not in jersey_numbers:
        jersey_numbers[jersey[i]] = 1
    else:
        jersey_numbers[jersey[i]] += 1
jersey_numbers
```

The ten most popular jersey numbers among players in the dataset are as follows:

Jersey Number | # of players
------------- | ------------
8 | 612
7 | 604
10 | 593
11 | 590
6 | 586
5 | 579
9 | 577
4 | 573
20 | 568
1 | 566

Using the number of players for each jersey number, I found the average rating for each jersey number.

```python
jersey = get_column('Jersey Number')
rating = get_column('Overall')
jersey_numbers #denominator
jersey_rating_totals = {}
result = {}
for i in range(len(jersey)):
    if jersey[i] not in jersey_rating_totals:
        jersey_rating_totals[jersey[i]] = rating[i]
    else:
        jersey_rating_totals[jersey[i]] += rating[i]

for i in jersey_rating_totals:
    result[i] = jersey_rating_totals[i] / jersey_numbers[i]
result
```
* The highest average rated jersey number in the dataset is `79`

The ten highest rated jersey numbers based on the dataset are:

Jersey Number | Rating
------------- | ------
79 | 71.5
10 | 70.38617200674537
9 | 69.28769497400347
63 | 69.0
92 | 68.9
7 | 68.87251655629139
8 | 68.83006535947712
69 | 68.66666666666667
5 | 68.49740932642487
87 | 68.35714285714286

I then found that the club that paid the highest average wage was `Real Madrid` using the code:

```python
clubs = {}
club = get_column('Club')
for i in range(len(club)):
    if club[i] not in clubs:
        clubs[club[i]] = 1
    else:
        clubs[club[i]] += 1

wage = get_column('Wage')
clubs #denominator
club_wage_totals = {}
highest_wages = {}
for i in range(len(club)):
    if club[i] not in club_wage_totals:
        club_wage_totals[club[i]] = money(wage[i])
    else:
        club_wage_totals[club[i]] += money(wage[i])

for i in club_wage_totals:
    highest_wages[i] = club_wage_totals[i] / clubs[i]
highest_wages

highest_avg_wage = None
for i in highest_wages:
    if highest_avg_wage == None or highest_wages[i] > highest_wages[highest_avg_wage]:
        highest_avg_wage = i
highest_avg_wage
```
