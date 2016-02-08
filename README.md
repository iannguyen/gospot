# [GOSpot][site]

[Live][site]
[site]: http://thegospot.herokuapp.com/

![screenshot]

[screenshot]: ./images/gospot.gif

GOSpot is a web application inspired by [csgolounge][csgolounge], where users can bet their virtual in-game items on professional CS:GO matches.

[csgolounge]: http://csgolounge.com
[parimutuel]: https://en.wikipedia.org/wiki/Parimutuel_betting

### Features

- Single-page web application with a `Rails` RESTful API serving an `Ember.js` client-side server via the `active_model_serializers` gem
- Secure user authentication with `ember-simple-auth` and `devise` gem
- [Parimutuel Betting][parimutuel] system determines payout odds based on the total pool of each team as opposed to `fixed-odds` betting
- `Bin-Packing/Coin-Change` algorithm efficiently and quickly distributes winning payouts to users after the site takes a 5% rake (see code spotlight below)
- Users can `deposit` and `withdraw` their items instantly through their profile page
- Users can bet on matches of their choice, anytime between midnight and before the match starts
- View match score updates live when the match begins
- Custom scheduled `rake` tasks start matches, deposit winnings in the users profile, and re-seed random bets and items everyday.

### Code Spotlight

##### The Bin-Packing/Coin-Change Algorithm

![winner_returns]
![distribute]

[winner_returns]: ./images/winner_returns.png
[distribute]: ./images/distribute.png

- When a match is over, it starts by first creating `Payout` models for all the winners, initialized by returning the initial bets and items each `winner` made
- Along with the `Payout` models, `profits` are calculated and sorted in descending order, insuring that users with the highest bets are prioritized
- All the items betted on the losing team are collected and placed into the `PayoutTable` model, along with `Payout` and `profits` of each user, and the site `rake` amount.

![initialize]

[initialize]: ./images/initialize.png

- When the `PayoutTable` model initializes, it generates the `@skins_hash` (a frequency hash), and the `@bp_table`
- The `@bp_table` takes the `@max` profit and iterates from 1..@max, and iterates through the keys from the `@skins_hash` as the available distributable items.
- Through dynamic programming, it generates two arrays, the `item_count`, and `last_item` array, where the indices of the arrays represent the $ amount, and the value of the `item_count` and `last_item` arrays represent the optimal number of items, and the last item used to make change for that amount respectively.

![cashout]

[cashout]: ./images/cashout.png

- For each of the winning profits, `cashout` is called. `Cashout` finds the optimal payout for for the amount through the values generated in the `@bp_table`and deducts the respective item from the `@skins_hash`.
- While iterating through the optimal payout, if the `@skins_hash` doesn't have the necessary item, the `@bp_table` is recalculated, but not with the original `@max` profit. Every time an item is deducted from the current payout, the `@max` gets updated to the maximum of the current `remaining` profit, or the `next` profit, insuring that the the `@bp_table` doesn't compute values that have already been computed.
