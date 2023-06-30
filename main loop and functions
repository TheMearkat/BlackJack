#used to shuffle deck object
import random

#attributes of playing cards
suits = suits = ('Hearts', 'Diamonds', 'Spades', 'Clubs')
ranks = ranks = ('Two', 'Three', 'Four', 'Five', 'Six', 'Seven', 'Eight', 'Nine', 'Ten', 'Jack', 'Queen', 'King', 'Ace')
values = values = {'Two':2, 'Three':3, 'Four':4, 'Five':5, 'Six':6, 'Seven':7, 'Eight':8, 'Nine':9, 'Ten':10, 'Jack':10,
         'Queen':10, 'King':10, 'Ace':11}

#game variable used in main game loop
playing = True

#card object. has rank and suit attributes and a __str__ method to make it easy to read what each card is
class Card:
    
    def __init__(self, rank, suit):
        self.rank = rank
        self.suit = suit
        
    
    def __str__(self):
        return self.rank + " of " + self.suit

# Deck object with 52 cards, no Jokers
class Deck:
    
    def __init__(self):
        self.deck = []  # start with an empty list
        for suit in suits:
            for rank in ranks:
                self.deck.append(Card(rank, suit))
    
    def __str__(self):
        deck_list = ''
        for card in self.deck:
            deck_list += '\n' + card.__str__()
        return deck_list

    def shuffle(self):
        random.shuffle(self.deck)
        
    def deal(self):
        return self.deck.pop()

#this is the object that acts like cards in hand of player
class Hand:
    def __init__(self):
        self.cards = []  # start with an empty list as we did in the Deck class
        self.value = 0   # start with zero value
        self.aces = 0    # add an attribute to keep track of aces
    
    def add_card(self,card):
        self.cards.append(card)
        self.value += values[card.rank]
        
        #track aces
        if card.rank == 'Ace':
            self.aces += 1
    
    def adjust_for_ace(self):
        
        while self.value > 21 and self.aces:
            self.value -= 10
            self.aces -=1
            
class Chips:
    
    def __init__(self):
        self.total = 100  # This can be set to a default value or supplied by a user input
        self.bet = 0
        
    def win_bet(self):
        self.total += self.bet
    
    def lose_bet(self):
        self.total -= self.bet


def take_bet(chips):
    while True:
        try:
            chips.bet = int(input("Please enter the amount you would like to bet: "))

        except:
            print("Please Provide and Integer:")
        else:
            if chips.bet > chips.total:
                print(f"Sorry, you do not have enough chips! You have {chips.total}")
            else:
                break


def hit(deck,hand):
    
    hand.add_card(deck.deal())
    hand.adjust_for_ace()


def hit_or_stand(deck,hand):
    global playing 
    
    while playing:
        action = input("\nWould you like to Hit or Stand? Enter h or s: ")
        
        if action[0].lower() == 'h':
            hit(deck, hand)
            print("\nThe players cards are: ")
            for cards in hand.cards:
                print(cards)
            if hand.value > 21:
                break
        elif action[0].lower() == 's':
            print("Player Stands Dealer's Turn")
            playing = False
        else:
            print("Sorry I didn't understand that, please enter h or s only.")
            continue
                  

def show_some(player,dealer):
    print("\nThe players cards are: ")
    for cards in player.cards:
        print(cards)
        
    print("\nThe dealers revealed cards are: ")
    for i in range(1, len(dealer.cards)):
        print(dealer.cards[i])
    
def show_all(player,dealer):
    #show players hand and value
    print("\nThe players cards are: ")
    for cards in player.cards:
        print(cards)
    print(f"Value of Your hand is {player.value}")
        
    #show dealers hand and value
    print("\nThe dealers cards are: ")
    for cards in dealer.cards:
        print(cards)
    print(f"Value of Dealer's hand is {dealer.value}")
    

def player_busts(player, dealer, chips):
    print("Bust! You have lost!")
    chips.lose_bet()

def player_wins(player, dealer, chips):
    print("You Won!")
    chips.win_bet()

def dealer_busts(player, dealer, chips):
    print("Dealer Busted! Player Wins")
    chips.win_bet()
    
def dealer_wins(player, dealer, chips):
    print("Dealer wins!")
    chips.lose_bet()
    
def push():
    print("Dealer and Player tied!")

#MAIN GAME LOOP

while True:
    print('\nWelcome to BlackJack! Get as close to 21 as you can without going over!\n    Dealer hits until she reaches 17. Aces count as 1 or 11.')

    
    deck = Deck()
    deck.shuffle()
    
    player_hand = Hand()
    dealer_hand = Hand()
    
    for i in range(2):
        player_hand.add_card(deck.deal())
        dealer_hand.add_card(deck.deal())
        
    
    # Set up the Player's chips
    player_chips = Chips()
    # Prompt the Player for their bet
    take_bet(player_chips)
    # Show cards (but keep one dealer card hidden)
    show_some(player_hand, dealer_hand)
    

    
    while playing: #variable used in hit_or_stand function
        
        #prompt for player to hit or stand
        hit_or_stand(deck, player_hand)
        
        #show cards (one of dealers is hidden)
        show_some(player_hand, dealer_hand)
 
        
        # If player's hand exceeds 21, run player_busts() and break out of loop
        if player_hand.value > 21:
            player_busts(player_hand, dealer_hand, player_chips)
            break

    # If Player hasn't busted, play Dealer's hand until Dealer reaches 17
    if player_hand.value <= 21:
            while dealer_hand.value < 17:
                hit(deck, dealer_hand)
    
        # Show all cards
            show_all(player_hand, dealer_hand)
    
        # Run different winning scenarios
            if  dealer_hand.value > 21:
                dealer_busts(player_hand, dealer_hand, player_chips)
                
            elif player_hand.value > dealer_hand.value:
                player_wins(player_hand, dealer_hand, player_chips)
                
            elif player_hand.value < dealer_hand.value:
                dealer_wins(player_hand, dealer_hand, player_chips) 
                
            elif player_hand.value == dealer_hand.value:
                push()
    
    # Inform Player of their chips total 
    print(f"\nYou now have {player_chips.total}")
    # Ask to play again
    go_on = input("\nWould you like to play again? Enter Yes or No: ")
    
    if go_on[0].lower() == 'y':
        playing = True
        continue
    else:
        print("Thank you for playing!")
        break
