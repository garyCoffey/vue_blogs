---
title: Plans for Egyptian Rat Screw
description: Going to discuss my idea for a coding a new card game called Egyptian Rat Screw
img: /thumbnails/egyptian.webp
alt: egyptian
type: blog
tags: [blogs, code, vue, game]
categories: [blogs, code, vue, game]
---

### Egyptian Rat Screw

[This game is really a thing....](https://bicyclecards.com/how-to-play/egyptian-rat-screw/)

If you have never heard of Egyptian Rat Screw.... Well now you have. I am really excited to create this game because I can remember playing this a lot as a kid and having a blast!

### How To Play

---

The basics of this card game is to deal the entire deck amongst the players. Then each player takes turn throwing a card down without looking at their cards until it is laid face up on a central pile. The game goes on like this until 1 of 2 things happen (at least in the simple version I played)

1. A face card is played
2. A double card plays

When a face card is laid down, the following player as a set # of chance to 'beat' it. Any other face card 'beats' a face card (Jack can beat Queen and vice versa). Each face card gives you the chances you have to beat that card (Ace - 4 chances, King - 3, Queen - 2, Jack - 1). This makes the Jack the strongest card in the game. If a players does beat the card, the original player that laid the face card can then try and beat that players card (and on and on until someones chances are up).  Whoever wins collects the entire stack of cards to that point.

The other condition is that a double is laid (meaning 2 cards of the same value are plaid on top of one another). Once this happens, any player can slap the cards, and pick up the stack to that point. This is valid in the case of face cards as well during a duel (if you will).

The way to win the game is to collect *ALL* of the cards. If a player runs out of cards, but the game is continuing (more than 2 players) the out player can slap back in.

### How to Code

---

This is the interesting part because I do not really know yet! I have not coded a card game before, but just thinking through some ideas, this sounds like a technically interesting/challenging project. I know I will have to create a deck. I will want to make the deck something that can be used in other future card games I might come up with on here. I am thinking that the game deck will be ran in an egyptianRatScrew component. Since the turn logic will be very specific to only this game, it makes sense to have that logic inside of there... The turn styling/animation would also only make sense inside of that component... So basically I can only think of having 2 components to make this work...

Obviously once I get into making the game, things could change drastically. I might notice something that should go into it's own separate piece and I will make those changes as they make sense.

The other cool thing I will have to make is how good the computer is at slapping. It could be made where the computer is impossible to beat... Which would not be very fun.. I think I will want to make the CSRT (computer slap response time) random each time. That way it keeps the game interesting.

I will also need to create the rules about how many chances are given and when. I don't think this will be too difficult, but we'll see...

### Conclusion

---

Overall I think this will be a fun project. I like playing this game, so I could see me spending time playing it for fun. It will also have some cool design challenges. To keep the right logic generic will be important to being able to easily design future card games.
