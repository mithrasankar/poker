# Poker
from bakery import assert_equal
from random import randint
def convert_hand(number:int)->str:
    '''
    This function consumes the numerical value of a
    card and converts its value into a string.
    
    Args:
    number(int): numerical value of card
    
    Returns:
        str: converted string values
    '''
    return str(number)
def hand_to_string(hand:list[int])->str:
    '''
    This function consumes a hand and returns a string
    that can be read and understood by the human player.
    
    Args:
        hand (list[int]): the numerical values of the
        three cards.
   
    Returns:
        str: values with 10-14 replaced with a letter
   '''
    first = hand[0]
    second = hand[1]
    third = hand[2]
    if first is 10:
        first = "X"
    elif first is 11:
        first = "J"
    elif first is 12:
        first = "Q"
    elif first is 13:
        first = "K"
    elif first is 14:
        first = "A"
    else:
        first = convert_hand(first)
    if second is 10:
        second = "X"
    elif second is 11:
        second = "J"
    elif second is 12:
        second = "Q"
    elif second is 13:
        second = "K"
    elif second is 14:
        second = "A"
    else: 
        second = convert_hand(second)
    if third is 10:
        third = "X"
    elif third is 11:
        third ="J"
    elif third is 12:
        third = "Q"
    elif third is 13:
        third = "K"
    elif third is 14:
        third = "A"
    else:
        third = convert_hand(third)
    return first + " " + second + " " + third
assert_equal(hand_to_string([14,9,4]),"A 9 4")
assert_equal(hand_to_string([12,12,12]),"Q Q Q")
assert_equal(hand_to_string([14,14,14]),"A A A")
assert_equal(hand_to_string([10,10,10]),"X X X")
assert_equal(hand_to_string([13,13,13]),"K K K")
assert_equal(hand_to_string([11, 11,11]),"J J J")
assert_equal(hand_to_string([2,2,2]),"2 2 2")
assert_equal(hand_to_string([2,11,4]),"2 J 4")
def sort_hand (hand: list[int]) -> list[int]:
        '''
        This function consumes a hand and returns the same hand,
        but organized from greatest to smallest.

        Args:
        hand(list[int]): unorganized hand.

        Returns:
        list[int]: hand organized in descending order.
        '''
        first = hand[0]
        second = hand[1]
        third = hand[2]
        if hand[0] < hand[1]:
            first = hand[1]
            second = hand[0]
        if second < third:
            x = second
            second = hand[2]
            third = x
        if first < second:
            x = first
            first = second
            second = x
        return [first, second, third]
assert_equal(sort_hand([2,1,3]),[3,2,1])  
assert_equal(sort_hand([1,2,3]),[3,2,1])
assert_equal(sort_hand([4,7,2]),[7,4,2])
assert_equal(sort_hand([3,2,1]),[3,2,1])
def has_triple(hand:list[int]) -> bool: 
    ''' 
    This function consumes a hand and return True
    if and only if the hand is three of a kind.
    
    Args:
    hand(list[int]): A hand of cards. 
    
    Returns:
    bool: If the hand is three of a kind.
    '''
    if hand[0] == hand[1] == hand[2]:
        return True
    return False

assert_equal(has_triple([1,2,3]), False)
assert_equal(has_triple([2,2,3]), False)
assert_equal(has_triple([3,3,3]), True)
def has_straight(hand: list[int])->bool: 
    '''
    This function consumes a hand and returns True if and only if the hand is a straight.
    
    Args:
    hand(list[int]): A hand of cards.
    
    Returns: 
    bool: If the hand is in direct consecutive order from largest to smallest.
    '''
    if hand[0] - 1 == hand[1] and hand[1] - 1 == hand[2]:
        return True
    return False
assert_equal(has_straight([3,2,1]),True)
assert_equal(has_straight([5,2,1]),False)
assert_equal(has_straight([6,5,4]),True)
def has_pair(hand: list[int])->bool:
    '''
    This function consumes a hand and returns
    True if and only if the hand has a pair.
    
    Args:
    hand(list[int]): A hand of cards. 
    
    Returns:
    bool: If the hand of cards has two of the same number.
    '''
    if hand[0] == hand[1] or hand[1] == hand[2] or hand[0] == hand[2]:
        return True
    return False
assert_equal(has_pair([3,3,1]), True)
assert_equal(has_pair([4,3,1]), False)
assert_equal(has_pair([1,3,1]), True)
def score_hand(hand: list[int])->int: 
    '''
    This function consumes a hand and returns an integer 
    representing the "value" of a hand so that it can be 
    compared to other hands.
    
    Args:
    score_hand(list[int]): A hand of cards to be scored.
    
    Returns:
    int: The comparable, scored value of hand.
    '''
    if has_triple(hand):
        feature = 16
    elif has_straight(hand):
        feature = 15
    elif has_pair(hand):
        if hand[0] == hand[1] or hand[0] == hand[2]:
            feature = hand[0]
        elif hand[1] == hand[2]:
            feature = hand[1]
    else: 
        feature = 0
    return ((feature*(16**3))+(hand[0]*(16**2))+(hand[1]*(16))+(hand[2]))
assert_equal(score_hand([3,3,3]),66355)
assert_equal(score_hand([11,10,9]),64425)
assert_equal(score_hand([7,4,4]),18244)
assert_equal(score_hand([4,4,7]),17479)
assert_equal(score_hand([2,6,8]),616)
def dealer_plays(hand: list[int])->bool: 
    '''
    This function consumes a hand and returns True
    if the dealer should play the hand, False if 
    the dealer should fold.
    
    Args:
    hand(list[int]): A hand of cards.
    
    Returns: 
    bool: If the dealer should play the hand or fold.
   '''
    return score_hand(hand) > score_hand([12,3,2])

assert_equal(dealer_plays([2,6,8]), False)
assert_equal(dealer_plays([3,3,3]), True)
assert_equal(dealer_plays([5,4,3]), True)
def play_round()->int:
    '''
    This function plays one hand of cards and 
    determines the outcome. 
    
    Args: 
    None
    
    Returns:
    int: How many points are won, using negative for losses.
    '''
    player_hand = deal()
    print(hand_to_string(player_hand))
    points = 0
    option = get_choice()
    if option == "f":
        return points - 10
    else:
        dealer_hand = deal()
        print(hand_to_string(dealer_hand))
    if dealer_plays(dealer_hand) == False: 
        return points+ 10
    else: 
        dealer_plays(player_hand)
        if score_hand(player_hand) > score_hand(dealer_hand):
            return points + 20
        else:
            return points - 20
def get_choice() -> str:
    """
    Get user input and return either 'p' or 'f' depending on the player's choice.
    """
    answer= ' '
    while answer not in 'pf':
        answer=input("Please enter either 'p' or 'f':")
    return answer
def deal() -> list[int]:
    """
    Simple random card dealing function that returns three randomly chosen cards,
    represented as integers between 2 and 14.
    """
    return [randint(2, 14), randint(2, 14), randint(2, 14)]
score = 0
while True:
    score += play_round()
    print("Your score is", score, "- Starting a new round!")
#
