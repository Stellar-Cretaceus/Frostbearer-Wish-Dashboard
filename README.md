# Frostbearer's Wish - Genshin Impact Wish Dashboard

### Dashboard Link : https://app.powerbi.com/groups/me/reports/0be3af6b-8eb3-4618-a07f-def14afb3604?ctid=6f4432dc-20d2-441d-b1db-ac3380ba633d&pbi_source=linkShare
<p align="center">
  <img src="https://github.com/user-attachments/assets/6b816186-c02f-4b79-8b22-53094b59e7df" alt="Dashboard" />
</p>


## Introduction

This dashboard visualizes data from the Wishing (Gacha) feature of the game Genshin Impact. The data used in this dashboard comes from my personal wish history, which I have collected over 4 years (from 21/12/2021 to 17/03/2025) using https://paimon.moe/wish to scrape and store my data.

When we wish (or simply gacha) in Genshin Impact for our favorite characters, we spend Primogems to pull. We keep farming and grinding all the areas, but sometimes we wonder, "Where did all my Primogems go?" We might want to know the statistics on how much we've pulled on certain banners, but the History feature in Genshin doesn't fully cover that.

Typically, Genshin Impact players aim for 5-star characters, the highest rarity in the game. To understand how close we are to getting one, we need to know how many pulls we've made, because the probability of pulling a 5-star increases when reaching what's known as Soft Pity.

This visualization covers the Descriptive type from the four types of Data Analytics, as it visualizes past data and explains the pulling behavior on each banner. It may also lightly touch on the Predictive type by estimating the probability of getting a 5-star on the next wish using pity as a measure.

## About the dataset

The dataset that i have provided contains neat and clean information but there is a little bit of problem that i've discovered, some banners are not like the others.   
During 2.3, the Character Event banner introduced 'Wish 2' type, where instead of One character featuring in a banner (e.g. Ganyu for Adrift in the Harbor) , it would be Dual Characters (e.g. Albedo and Eula both featured in their banner at the same time) while those banners shared pity count, but 'Wish History' of Genshin Hadn't been update to seperate the count of each banner, so the name appeared on the data is not the 100% right and we may have to get around with it.

## Soft Pity System & Probability Calculation
Genshin Impact uses a dynamic probability system for 5-star drops, also known as Soft Pity. This means the chance of pulling a 5-star increases after a certain number of unsuccessful pulls (typically around the 73rd pull for character banners).

To calculate the correct probability and visualize it:

We will refer to community-sourced research and data:
- [The 5\* rate is not uniform - 0.6%, there is a soft pity (Reddit)](https://www.reddit.com/r/Genshin_Impact/comments/jo9d9d/the_5_rate_is_not_uniform_06_there_is_a_soft_pity/)
- [Soft pity mechanics for the Weapon Banner (Reddit)](https://www.reddit.com/r/Genshin_Impact/comments/1frlf11/what_is_the_soft_pity_for_the_weapon_banner/)


## Data Cleaning Approached

- We need to retrieve the correct names and types for each banner. This can be achieved using **Google Colab** to run **BeautifulSoup** and **Selenium** to scrape banner information from reliable sources:
  - [Banner History and Featured 5-Stars](https://genshin-impact.fandom.com/wiki/Wish/History)  
  - [List of Type 2 Character Event Wishes](https://genshin-impact.fandom.com/wiki/Category:Character_Event_Wishes-2)  
  (*Source: Genshin Impact Fandom Wiki*)

- Since all Wishing data in separate sheets for each types of banner share a similar structure (columns), we can append them together to reduce redundancy.

- To disambiguate banners with identical names but different periods (e.g., Secretum Secretorum have 3 reruns), we can count the number of reruns and assign an index or version number to separate them (e.g., Secretum Secretorum - 1).

- Some wish records have banner name collisions. We can clean this by identifying the time periods when banners overlap or merge. Then, we can apply the correct merged banner name and create a **new key table** to handle these relationships properly.

- Since the "Wish" history table contains **inaccurate banner names**, we need a robust method to distinguish which rolls belong to which banners. The primary matching criteria will be:
  - **Banner name** & **Time range**  
  We will validate each wish entry by checking if its timestamp falls within the correct banner period, ensuring accurate banner-to-pull mapping.

## Visualization
### 1. Total Roll Count
<p align="center">
  <img src="https://github.com/user-attachments/assets/b42b69ac-f193-4941-8103-932f47b165b8" alt="Total Roll Count" />
</p>

<p align="center">
  Calculated by counting the rows in the **All Rolls** table.
</p>

### 2. Pity Counter
<p align="center">
  <img src="https://github.com/user-attachments/assets/efcb92dd-09f1-4189-bebe-0e82ecbc68ad" alt="Pity Counter" />
</p>

<p align="center">
  Calculated by filtering each banner type, then sorting by **Time** and **Rolls**, and identifying when a 5-star or 4-star rank is obtained.
</p>

### 3. Wish by Time
<p align="center">
  <img src="https://github.com/user-attachments/assets/dc0904fd-bc61-4791-85e2-bef575debe2e" alt="Wish by Time" />
</p>

<p align="center">
  This graph visualizes wish activity over time, categorized by item rarity (3-star, 4-star, 5-star).  
  The time granularity is set to **daily**, showing detailed day-by-day trends.
</p>

### 4. Most Pulled Banner
<p align="center">
  <img src="https://github.com/user-attachments/assets/aa0d30f2-e47f-47e1-8e50-1ac326550206" alt="Most Pulled Banner" />
</p>

<p align="center">
  A dynamic table displaying the top banners with the highest pull counts.
</p>

### 5. Luckiest Pull
<p align="center">
  <img src="https://github.com/user-attachments/assets/e132d42c-a656-47f0-a570-d066e394b5ba" alt="Luckiest Pull" />
</p>

<p align="center">
  A leaderboard showing characters obtained with the **lowest pity count** in the **All Rolls** data.
</p>

### 6. Probability of Getting a 5-Star
<p align="center">
  <img src="https://github.com/user-attachments/assets/2d22ac24-84ee-4300-ad2b-6501ada2b9f6" alt="5-Star Probability" />
</p>

<p align="center">
  These cards use the **Pity Counter** to look up the 5-star probability calculated in the **Pull Probability** table.
</p>

### 7.  Wish Count by Wish category
<p align="center">
  <img src="https://github.com/user-attachments/assets/c60188ad-7631-42af-af47-2c5a773dec68" alt="Wish Count by Wish category" />
</p>
<p align="center">
  <img src="https://github.com/user-attachments/assets/68ee2ab0-346d-4558-a012-81d6315823f8" alt="Drilling down" />
</p>
<p align="center">
  Bar chart to display graph of pulling by category, it can also drill down to see each banner 
</p>

### 8.  Count of Items by Rarity
<p align="center">
  <img src="https://github.com/user-attachments/assets/33610f27-cc07-4fa4-9ca3-6f4396bb965c" alt="Item count by rarity" />
</p>
<p align="center">
  Card to display count of items, calculated by count of each item filtered by rarity
</p>



