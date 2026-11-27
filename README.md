# Dorm Room Deals

My app helps a college student on a tight budget decide which Steam game to buy this week by finding well-reviewed games that fit their genre, price limit, and laptop, then showing which are on sale and which are hidden gems.

> A course project for **MIST 4600: Computer Programming in Business** at the University of Georgia's Terry College of Business (Fall 2026).

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/ishaanprasad1/dorm-room-deals/blob/main/dorm_room_deals_RECENT.ipynb)

## Problem

College students who play PC games usually have a small budget and a basic laptop that might run Windows, Mac, or Linux. Deciding which Steam game to buy this week is hard because the store has thousands of games, so a student has to check each game's reviews, price, and system support by hand, then visit other sites to see if it's on sale. Good lesser-known games also get buried under the big hits, so students often miss great games that would have been worth their money.

**My app helps a college student on a tight budget decide which Steam game to buy this week by finding well-reviewed games that fit their genre, price limit, and laptop, then showing which are on sale and which are hidden gems.**

## What it does

The student types in three choices at the top of the notebook: a genre, a price limit, and their laptop's system. My app answers three sub-questions, then puts the answers together into a recommendation.

| # | Sub-question | What the app tells the student |
|---|---|---|
| 1 | **Fit:** which highly rated games (85%+ positive reviews) match the student's genre and price limit and run on their laptop (Windows, Mac, or Linux)? | `find_fits` (with the helper `has_genre`) keeps only games with at least 85% positive reviews, the student's genre, a price at or under their limit, and support for their system. It tells the student how many games fit and lists them (pandas). |
| 2 | **Deals:** which of those games are on sale today, and how close is today's price to their lowest price ever? | `find_deals` takes the 5 best-reviewed paid games that fit and uses `.apply` with `get_today` and `get_lowest_ever` to look each one up in the CheapShark API by its Steam `appid`. Each game becomes a `DealPick` object that prints today's price, the regular price, and the lowest price ever, and the app lists the games on sale today. |
| 3 | **Hidden gems:** which of them are lesser known (under 1 million owners) but rated as highly by players as the big hits? | `compare_hidden_gems` splits the games that fit into hidden gems (under 1 million owners) and big hits, compares the two groups' average positive reviews, and lists the 5 best-reviewed hidden gems (pandas). |

**Together:** a recommendation of which game to buy this week: a well-reviewed game that fits the student's genre, budget, and laptop and is on sale today, or, if none is on sale, a hidden gem.

## How to run it

1. Click **Open in Colab** above, or open `dorm_room_deals_RECENT.ipynb` from this repository in [Google Colab](https://colab.research.google.com).
2. Put `steam_games.csv` (in this repository) in your Google Drive, in My Drive or a folder inside it. The setup cell finds it automatically.
3. No API key is needed: the [CheapShark API](https://apidocs.cheapshark.com/) is free and works without one, so there is nothing to add to Colab Secrets.
4. Optional: change `GENRE`, `MAX_PRICE`, and `SYSTEM` in Cell 1 to the student's own choices.
5. Choose **Runtime → Run all**. Allow access to Google Drive when Colab asks. Cell 5 asks CheapShark about 5 games, so it takes about 20 seconds.

## Example output

With `GENRE = "Indie"`, `MAX_PRICE = 20`, and `SYSTEM = "mac"`:

**Sub-question 1:** 154 games fit the student's choices. The first 10:

```
154 games fit the student's choices
                      name  price  pct_pos_total
6                 Terraria   9.99             97
8              Garry's Mod   5.99             96
13          Stardew Valley  14.99             98
17  Euro Truck Simulator 2  19.99             97
25                Unturned   0.00             91
31                 Valheim  19.99             94
35           Geometry Dash   4.99             93
36           Hollow Knight  14.99             97
39   Don't Starve Together  14.99             95
51         Project Zomboid  19.99             94
```

**Sub-question 2:** the 5 best-reviewed paid games that fit, with live prices from CheapShark (run on Nov 25, 2026; prices change every day):

```
Stardew Valley: $14.99 today, regular $14.99, lowest ever $5.99
OneShot: $9.99 today, regular $9.99, lowest ever $0.92
Vampire Survivors: $4.99 today, regular $4.99, lowest ever $2.69
Balatro: $10.04 today, regular $14.99, lowest ever $10.08
Katana ZERO: $8.99 today, regular $14.99, lowest ever $7.49
On sale today:
- Balatro
- Katana ZERO
```

Balatro is on sale for $10.04, which is even lower than the lowest price CheapShark had recorded ($10.08), so it is at a new all-time low.

**Sub-question 3:** hidden gems are rated slightly higher than the big hits:

```
Hidden gems: 27 games, average rating 94.9
Big hits: 121 games, average rating 93.4
```

The best hidden gems include Touhou Mystia's Izakaya ($5.99) and Baba Is You ($14.99), both 98% positive.

**Recommendation for the student:** buy a game from the "On sale today" list, especially one whose price today is at or near its lowest price ever. On Nov 25, 2026, that was **Balatro**: 98% positive reviews, $4.95 off its regular price, and at an all-time low. If none is on sale when the student checks, try a hidden gem instead: Touhou Mystia's Izakaya is 98% positive and costs only $5.99.

## Data

`steam_games.csv`: the 1,000 Steam games with the most reviews, from the [Steam Games Dataset](https://www.kaggle.com/datasets/artermiloff/steam-games-dataset) on Kaggle (collected March 2025), shared under the MIT License by its creator, artermiloff. Titles with adult content were removed for class. Prices in the file are from March 2025, so the app uses CheapShark for today's prices.

## Built with

- Python and [pandas](https://pandas.pydata.org/), in Google Colab
- The [CheapShark API](https://apidocs.cheapshark.com/), through the `requests` library: today's price and the lowest price ever for each game on the shortlist

## AI Use Disclosure

- **I used AI tools:** Yes
- **Tools:** Claude (Anthropic)
- **What I asked:** help choosing a project from the menu and writing my proposal, then writing the notebook's functions, class, text cells, and this README from my sub-questions.
- **What I added, changed, or fixed in the AI's output:** TODO: write this in your own words.
- **What I designed and wrote entirely myself:** TODO: write this in your own words.
- **I understand every line of code I am submitting:** TODO: Yes or No. Answer only after going through every line.

## Author

Ishaan Prasad · MIST 4600, Fall 2026
