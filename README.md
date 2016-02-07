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

[distribute]: ./images/distribute.png
