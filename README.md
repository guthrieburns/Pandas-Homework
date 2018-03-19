

```python
# Environment Setup
# ----------------------------------------------------------------
# Dependencies
import csv
import pandas as pd
import random
import numpy as np
# Output File Name
file_output_players = "generated_data/players_complete.csv"
file_output_items = "generated_data/items_complete.csv"
file_output_purchases_json = "generated_data/purchase_data_3.json"
file_output_purchases_csv = "generated_data/purchase_data_3.csv"
# Convert the Players List to a Data Frame
players = pd.read_csv("raw_data/players.csv", dtype="str", header=0)
total_players = len(players)
items = pd.read_table("raw_data/items.txt", delimiter="\t", dtype="str")
total_items = len(items)
# Generator Conditions (Change as Needed)

# ----------------------------------------------------------------
# Population Counts
total_purchase_count = 78
player_count = len(players) - 27
item_count = len(items) - 6
# Player Weight
genders = ["Male", "Female", "Other / Non-Disclosed"]
gender_weights = [0.82, 0.16, 0.02]
age_ranges = [7, 15, 20, 25, 30, 35, 40, 45]
age_weights = [0.01, 0.09, 0.20, 0.46, 0.10, 0.08, 0.05, 0.01]
# Item Prices
low_price = 1
high_price = 5
# Generate Players
# ----------------------------------------------------------------
# Generate all gender probabilities
gender_probabilities = zip(genders, gender_weights)
gender_profiles = []
# Generate a sufficient number of genders
for gender in gender_probabilities:
    gender_profiles = gender_profiles + \
        [gender[0]] * int(gender[1] * total_players)
# Generate random ages
age_probabilities = zip(age_ranges, age_weights)
age_counts = []
age_profiles = []
for age in age_probabilities:
    age_counts = age_counts + [int(age[1] * total_players)]
age_probabilities = zip(age_counts, age_ranges)
# Generate right number of random numbers
prev_age = age_ranges[0]
for age in age_probabilities:
    for x in range(age[0]):
        age_profiles = age_profiles + [random.randint(prev_age, age[1])]
    prev_age = age[1]
random.shuffle(gender_profiles)
random.shuffle(age_profiles)
# Convert lists into pandas data frames
gender_profiles_pd = pd.Series(gender_profiles)
age_profiles_pd = pd.Series(age_profiles)
# Combine the data frames to match the number of players
players = players[0:player_count]
gender_profiles_pd = gender_profiles_pd[0:player_count]
age_profiles_pd = age_profiles_pd[0:player_count]
# Combine all datasets
players["Age"] = age_profiles_pd.values
players["Gender"] = gender_profiles_pd.values
# Export as JSON
# players.to_json(file_output_players, orient="records")
players.to_csv(file_output_players, index_label="Player ID")
# Generate the Item Data
# ----------------------------------------------------------------
items["Price"] = np.random.uniform(low_price, high_price, items.shape[0])
items = items.round({"Price": 2})
# Export the Items to a CSV
items.to_csv(file_output_items, index_label="Item ID")
# Generate the Purchases
# ----------------------------------------------------------------
# Setup the purchases data frame
index = ["Purchase ID"]
columns = ["SN", "Item ID", "Item Name", "Item Price"]
purchases = pd.DataFrame(index=index, columns=columns)
purchased_items = []
for x in range(total_purchase_count):
    print("Now Rendering: %s of %s records..." % (x, total_purchase_count))
    rand_player = np.random.randint(0, player_count)
    rand_item = np.random.randint(0, item_count)
    selected_user = players.iloc[rand_player]["SN"]
    selected_user_age = players.iloc[rand_player]["Age"]
    selected_user_gender = players.iloc[rand_player]["Gender"]
    selected_item_id = rand_item
    selected_item_name = items.iloc[rand_item]["Item Name"]
    selected_item_price = items.iloc[rand_item]["Price"]
    purchased_items = purchased_items + list(zip([selected_user],
                                                 [selected_user_age],
                                                 [selected_user_gender],
                                                 [selected_item_id],
                                                 [selected_item_name],
                                                 [selected_item_price]))
purchased_items_pd = pd.DataFrame(purchased_items, columns=["SN",
                                                            "Age",
                                                            "Gender",
                                                            "Item ID",
                                                            "Item Name",
                                                            "Price"])
# Export the Purchase List as CSVs and JSONs
purchased_items_pd.to_csv(file_output_purchases_csv, index_label="Purchase ID")
purchased_items_pd.to_json(file_output_purchases_json, orient="records")

purchased_items_pd
```

    Now Rendering: 0 of 78 records...
    Now Rendering: 1 of 78 records...
    Now Rendering: 2 of 78 records...
    Now Rendering: 3 of 78 records...
    Now Rendering: 4 of 78 records...
    Now Rendering: 5 of 78 records...
    Now Rendering: 6 of 78 records...
    Now Rendering: 7 of 78 records...
    Now Rendering: 8 of 78 records...
    Now Rendering: 9 of 78 records...
    Now Rendering: 10 of 78 records...
    Now Rendering: 11 of 78 records...
    Now Rendering: 12 of 78 records...
    Now Rendering: 13 of 78 records...
    Now Rendering: 14 of 78 records...
    Now Rendering: 15 of 78 records...
    Now Rendering: 16 of 78 records...
    Now Rendering: 17 of 78 records...
    Now Rendering: 18 of 78 records...
    Now Rendering: 19 of 78 records...
    Now Rendering: 20 of 78 records...
    Now Rendering: 21 of 78 records...
    Now Rendering: 22 of 78 records...
    Now Rendering: 23 of 78 records...
    Now Rendering: 24 of 78 records...
    Now Rendering: 25 of 78 records...
    Now Rendering: 26 of 78 records...
    Now Rendering: 27 of 78 records...
    Now Rendering: 28 of 78 records...
    Now Rendering: 29 of 78 records...
    Now Rendering: 30 of 78 records...
    Now Rendering: 31 of 78 records...
    Now Rendering: 32 of 78 records...
    Now Rendering: 33 of 78 records...
    Now Rendering: 34 of 78 records...
    Now Rendering: 35 of 78 records...
    Now Rendering: 36 of 78 records...
    Now Rendering: 37 of 78 records...
    Now Rendering: 38 of 78 records...
    Now Rendering: 39 of 78 records...
    Now Rendering: 40 of 78 records...
    Now Rendering: 41 of 78 records...
    Now Rendering: 42 of 78 records...
    Now Rendering: 43 of 78 records...
    Now Rendering: 44 of 78 records...
    Now Rendering: 45 of 78 records...
    Now Rendering: 46 of 78 records...
    Now Rendering: 47 of 78 records...
    Now Rendering: 48 of 78 records...
    Now Rendering: 49 of 78 records...
    Now Rendering: 50 of 78 records...
    Now Rendering: 51 of 78 records...
    Now Rendering: 52 of 78 records...
    Now Rendering: 53 of 78 records...
    Now Rendering: 54 of 78 records...
    Now Rendering: 55 of 78 records...
    Now Rendering: 56 of 78 records...
    Now Rendering: 57 of 78 records...
    Now Rendering: 58 of 78 records...
    Now Rendering: 59 of 78 records...
    Now Rendering: 60 of 78 records...
    Now Rendering: 61 of 78 records...
    Now Rendering: 62 of 78 records...
    Now Rendering: 63 of 78 records...
    Now Rendering: 64 of 78 records...
    Now Rendering: 65 of 78 records...
    Now Rendering: 66 of 78 records...
    Now Rendering: 67 of 78 records...
    Now Rendering: 68 of 78 records...
    Now Rendering: 69 of 78 records...
    Now Rendering: 70 of 78 records...
    Now Rendering: 71 of 78 records...
    Now Rendering: 72 of 78 records...
    Now Rendering: 73 of 78 records...
    Now Rendering: 74 of 78 records...
    Now Rendering: 75 of 78 records...
    Now Rendering: 76 of 78 records...
    Now Rendering: 77 of 78 records...





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>SN</th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Pheuthello28</td>
      <td>25</td>
      <td>Male</td>
      <td>59</td>
      <td>Lightning, Etcher of the King</td>
      <td>4.46</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Eulanurin88</td>
      <td>36</td>
      <td>Male</td>
      <td>8</td>
      <td>Purgatory, Gem of Regret</td>
      <td>4.30</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Iskjaskan81</td>
      <td>20</td>
      <td>Male</td>
      <td>121</td>
      <td>Massacre</td>
      <td>2.10</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Rarallo90</td>
      <td>15</td>
      <td>Male</td>
      <td>117</td>
      <td>Heartstriker, Legacy of the Light</td>
      <td>1.70</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Chamimla73</td>
      <td>7</td>
      <td>Male</td>
      <td>160</td>
      <td>Azurewrath</td>
      <td>3.81</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Chanassa48</td>
      <td>20</td>
      <td>Male</td>
      <td>53</td>
      <td>Vengeance Cleaver</td>
      <td>4.60</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Lirtosiast35</td>
      <td>7</td>
      <td>Male</td>
      <td>122</td>
      <td>Unending Tyranny</td>
      <td>2.02</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Chamadar61</td>
      <td>9</td>
      <td>Male</td>
      <td>102</td>
      <td>Avenger</td>
      <td>4.86</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Lisimsta66</td>
      <td>25</td>
      <td>Other / Non-Disclosed</td>
      <td>157</td>
      <td>Spada, Etcher of Hatred</td>
      <td>1.02</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Eudai71</td>
      <td>21</td>
      <td>Male</td>
      <td>102</td>
      <td>Avenger</td>
      <td>4.86</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Assassasda84</td>
      <td>7</td>
      <td>Female</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.92</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Assilsan72</td>
      <td>26</td>
      <td>Female</td>
      <td>142</td>
      <td>Righteous Might</td>
      <td>3.79</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Ilenmol62</td>
      <td>32</td>
      <td>Male</td>
      <td>140</td>
      <td>Striker</td>
      <td>2.54</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Qilalista41</td>
      <td>15</td>
      <td>Male</td>
      <td>146</td>
      <td>Warped Iron Scimitar</td>
      <td>3.10</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Chamilsala65</td>
      <td>7</td>
      <td>Male</td>
      <td>111</td>
      <td>Misery's End</td>
      <td>3.37</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Aisurria69</td>
      <td>17</td>
      <td>Male</td>
      <td>136</td>
      <td>Ghastly Adamantite Protector</td>
      <td>1.95</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Marast30</td>
      <td>24</td>
      <td>Male</td>
      <td>80</td>
      <td>Dreamsong</td>
      <td>4.89</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Qilatie51</td>
      <td>25</td>
      <td>Female</td>
      <td>178</td>
      <td>Oathbreaker, Last Hope of the Breaking Storm</td>
      <td>4.53</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Ilosiast59</td>
      <td>24</td>
      <td>Male</td>
      <td>4</td>
      <td>Bloodlord's Fetish</td>
      <td>3.39</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Rairith81</td>
      <td>27</td>
      <td>Male</td>
      <td>110</td>
      <td>Suspension</td>
      <td>2.70</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Indanuard75</td>
      <td>19</td>
      <td>Male</td>
      <td>25</td>
      <td>Hero Cane</td>
      <td>2.20</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Frichaststa81</td>
      <td>27</td>
      <td>Male</td>
      <td>154</td>
      <td>Feral Katana</td>
      <td>3.87</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Yathe48</td>
      <td>24</td>
      <td>Male</td>
      <td>40</td>
      <td>Second Chance</td>
      <td>2.88</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Iasursti71</td>
      <td>16</td>
      <td>Male</td>
      <td>85</td>
      <td>Malificent Bag</td>
      <td>1.08</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Eollym91</td>
      <td>25</td>
      <td>Female</td>
      <td>68</td>
      <td>Storm-Weaver, Slayer of Inception</td>
      <td>3.38</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Lisassasda39</td>
      <td>21</td>
      <td>Male</td>
      <td>45</td>
      <td>Glinting Glass Edge</td>
      <td>1.74</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Lassjask63</td>
      <td>21</td>
      <td>Male</td>
      <td>49</td>
      <td>The Oculus, Token of Lost Worlds</td>
      <td>1.58</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Sundim98</td>
      <td>20</td>
      <td>Female</td>
      <td>73</td>
      <td>Ritual Mace</td>
      <td>2.11</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Chamjasknya65</td>
      <td>35</td>
      <td>Male</td>
      <td>152</td>
      <td>Darkheart</td>
      <td>3.37</td>
    </tr>
    <tr>
      <th>29</th>
      <td>Tyaelly53</td>
      <td>24</td>
      <td>Male</td>
      <td>61</td>
      <td>Ragnarok</td>
      <td>1.70</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>48</th>
      <td>Aestysu37</td>
      <td>35</td>
      <td>Male</td>
      <td>175</td>
      <td>Woeful Adamantite Claymore</td>
      <td>2.18</td>
    </tr>
    <tr>
      <th>49</th>
      <td>Mindossa76</td>
      <td>39</td>
      <td>Male</td>
      <td>32</td>
      <td>Orenmir</td>
      <td>2.44</td>
    </tr>
    <tr>
      <th>50</th>
      <td>Chanjaskan43</td>
      <td>39</td>
      <td>Male</td>
      <td>3</td>
      <td>Phantomlight</td>
      <td>3.34</td>
    </tr>
    <tr>
      <th>51</th>
      <td>Undimsya85</td>
      <td>22</td>
      <td>Male</td>
      <td>45</td>
      <td>Glinting Glass Edge</td>
      <td>1.74</td>
    </tr>
    <tr>
      <th>52</th>
      <td>Alallo58</td>
      <td>16</td>
      <td>Male</td>
      <td>46</td>
      <td>Hopeless Ebon Dualblade</td>
      <td>4.47</td>
    </tr>
    <tr>
      <th>53</th>
      <td>Farenon57</td>
      <td>30</td>
      <td>Female</td>
      <td>12</td>
      <td>Dawne</td>
      <td>2.31</td>
    </tr>
    <tr>
      <th>54</th>
      <td>Siri75</td>
      <td>22</td>
      <td>Male</td>
      <td>164</td>
      <td>Exiled Doomblade</td>
      <td>4.44</td>
    </tr>
    <tr>
      <th>55</th>
      <td>Aerithnuphos61</td>
      <td>28</td>
      <td>Male</td>
      <td>64</td>
      <td>Fusion Pummel</td>
      <td>3.68</td>
    </tr>
    <tr>
      <th>56</th>
      <td>Aithelis62</td>
      <td>42</td>
      <td>Male</td>
      <td>160</td>
      <td>Azurewrath</td>
      <td>3.81</td>
    </tr>
    <tr>
      <th>57</th>
      <td>Airi27</td>
      <td>22</td>
      <td>Male</td>
      <td>68</td>
      <td>Storm-Weaver, Slayer of Inception</td>
      <td>3.38</td>
    </tr>
    <tr>
      <th>58</th>
      <td>Jiskimsta59</td>
      <td>22</td>
      <td>Female</td>
      <td>125</td>
      <td>Whistling Mithril Warblade</td>
      <td>3.50</td>
    </tr>
    <tr>
      <th>59</th>
      <td>Eusri44</td>
      <td>22</td>
      <td>Male</td>
      <td>40</td>
      <td>Second Chance</td>
      <td>2.88</td>
    </tr>
    <tr>
      <th>60</th>
      <td>Lisossala30</td>
      <td>21</td>
      <td>Female</td>
      <td>16</td>
      <td>Restored Bauble</td>
      <td>2.54</td>
    </tr>
    <tr>
      <th>61</th>
      <td>Filrion44</td>
      <td>21</td>
      <td>Male</td>
      <td>166</td>
      <td>Thirsty Iron Reaver</td>
      <td>3.09</td>
    </tr>
    <tr>
      <th>62</th>
      <td>Sundosiasta28</td>
      <td>32</td>
      <td>Male</td>
      <td>21</td>
      <td>Souleater</td>
      <td>4.12</td>
    </tr>
    <tr>
      <th>63</th>
      <td>Chanjaskan43</td>
      <td>39</td>
      <td>Male</td>
      <td>127</td>
      <td>Heartseeker, Reaver of Souls</td>
      <td>4.94</td>
    </tr>
    <tr>
      <th>64</th>
      <td>Chamirrasya33</td>
      <td>22</td>
      <td>Male</td>
      <td>151</td>
      <td>Severance</td>
      <td>4.84</td>
    </tr>
    <tr>
      <th>65</th>
      <td>Hirirap39</td>
      <td>27</td>
      <td>Male</td>
      <td>56</td>
      <td>Foul Titanium Battle Axe</td>
      <td>3.26</td>
    </tr>
    <tr>
      <th>66</th>
      <td>Istydil92</td>
      <td>17</td>
      <td>Female</td>
      <td>152</td>
      <td>Darkheart</td>
      <td>3.37</td>
    </tr>
    <tr>
      <th>67</th>
      <td>Ilaststa70</td>
      <td>22</td>
      <td>Male</td>
      <td>58</td>
      <td>Freak's Bite, Favor of Holy Might</td>
      <td>4.69</td>
    </tr>
    <tr>
      <th>68</th>
      <td>Chanirrala39</td>
      <td>14</td>
      <td>Male</td>
      <td>155</td>
      <td>War-Forged Gold Deflector</td>
      <td>4.88</td>
    </tr>
    <tr>
      <th>69</th>
      <td>Chanastsda67</td>
      <td>39</td>
      <td>Male</td>
      <td>32</td>
      <td>Orenmir</td>
      <td>2.44</td>
    </tr>
    <tr>
      <th>70</th>
      <td>Indcil77</td>
      <td>16</td>
      <td>Male</td>
      <td>167</td>
      <td>Malice, Legacy of the Queen</td>
      <td>4.20</td>
    </tr>
    <tr>
      <th>71</th>
      <td>Eudaria62</td>
      <td>22</td>
      <td>Male</td>
      <td>82</td>
      <td>Nirvana</td>
      <td>1.50</td>
    </tr>
    <tr>
      <th>72</th>
      <td>Haisty72</td>
      <td>25</td>
      <td>Male</td>
      <td>39</td>
      <td>Betrayal, Whisper of Grieving Widows</td>
      <td>1.56</td>
    </tr>
    <tr>
      <th>73</th>
      <td>Tyaeduesu93</td>
      <td>15</td>
      <td>Male</td>
      <td>89</td>
      <td>Blazefury, Protector of Delusions</td>
      <td>2.11</td>
    </tr>
    <tr>
      <th>74</th>
      <td>Aelidru27</td>
      <td>31</td>
      <td>Male</td>
      <td>169</td>
      <td>Interrogator, Blood Blade of the Queen</td>
      <td>1.57</td>
    </tr>
    <tr>
      <th>75</th>
      <td>Isri59</td>
      <td>9</td>
      <td>Female</td>
      <td>133</td>
      <td>Faith's Scimitar</td>
      <td>4.27</td>
    </tr>
    <tr>
      <th>76</th>
      <td>Isurriarap71</td>
      <td>23</td>
      <td>Female</td>
      <td>69</td>
      <td>Frenzy, Defender of the Harvest</td>
      <td>3.96</td>
    </tr>
    <tr>
      <th>77</th>
      <td>Saerallora71</td>
      <td>25</td>
      <td>Male</td>
      <td>148</td>
      <td>Warmonger, Gift of Suffering's End</td>
      <td>4.41</td>
    </tr>
  </tbody>
</table>
<p>78 rows × 6 columns</p>
</div>




```python
total_players = len(players)
dict_players = [{"Total Players":total_players}]

df_players = pd.DataFrame(dict_players)
df_players
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1163</td>
    </tr>
  </tbody>
</table>
</div>




```python
unique_items = items["Item Name"].unique()
count_unique = len(unique_items)
average_price = purchased_items_pd["Price"].mean()
total_items = len(purchased_items_pd)
total_revenue = purchased_items_pd["Price"].sum()


df_items = pd.DataFrame({
    "Number Of Unique Items":[count_unique],
    "Average Price":[float(round(average_price, 2))],
    "Number Of Purchases":[total_items],
    "Total Revenue":[float(round(total_revenue, 2))]
})
df_items

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Price</th>
      <th>Number Of Purchases</th>
      <th>Number Of Unique Items</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3.01</td>
      <td>78</td>
      <td>186</td>
      <td>235.06</td>
    </tr>
  </tbody>
</table>
</div>




```python
percent_male = (100 * players["Gender"].eq('Male').mean())
percent_female = (100 * players["Gender"].eq('Female').mean())
percent_other = (100 - (percent_male + percent_female))
total_male = (players['Gender'] == "Male").sum()
total_female = (players['Gender'] == "Female").sum()
total_other = len(players) - (total_male + total_female)




df_gender = pd.DataFrame({
    "Percent Males":[float(round(percent_male, 2))],
    "Percent Females":[float(round(percent_female, 2))],
    "Percent Undisclosed":[float(round(percent_other, 2))],
    "Total Males":[total_male],
    "Total Females":[total_female],
    "Total Other":[total_other]
    #"Total Count":[players["Gender"].value_counts()]
})

df_gender

#print("---------------------------------")
#print(players["Gender"].value_counts())


```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Percent Females</th>
      <th>Percent Males</th>
      <th>Percent Undisclosed</th>
      <th>Total Females</th>
      <th>Total Males</th>
      <th>Total Other</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>15.99</td>
      <td>82.12</td>
      <td>1.89</td>
      <td>186</td>
      <td>955</td>
      <td>22</td>
    </tr>
  </tbody>
</table>
</div>




```python
male_purchase = (purchased_items_pd["Gender"] == "Male").sum()
female_purchase = (purchased_items_pd["Gender"] == "Female").sum()
other_purchases = len(purchased_items_pd) - (male_purchase + female_purchase)


df_gender_price = pd.DataFrame({
    "Male Purchases":[male_purchase],
    "Female Purchases":[female_purchase],
    "Other Purchases":[other_purchases]
})

df_gender_price


```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Female Purchases</th>
      <th>Male Purchases</th>
      <th>Other Purchases</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>13</td>
      <td>64</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
bins = [0, 10, 15, 20, 25, 30, 35, 40, 50, 60, 70]
groupnames = ['Child', 'Early Teen', 'Teen', 'Young Adult', 'Late Twenties', 'Adult', 'Late Thirties', 'Forties', 'Middle Aged', 'Older']
```
