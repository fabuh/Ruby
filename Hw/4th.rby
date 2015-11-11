class Card
  def initialize rank, suit
    @rank = rank
    @suit = suit
  end
  attr_reader :rank, :suit
  def to_s
    "#{"#{rank}".capitalize} of #{suit.capitalize}"
  end
  def == (other)
    if (@suit == other.suit && @rank == other.rank) then true
    else false
    end
  end
  def compare_to other, ranks
    suits = [:spades, :hearts, :diamonds, :clubs]
    if suits.index(self.suit) > suits.index(other.suit) then 1
    elsif suits.index(self.suit) == suits.index(other.suit) and
      ranks.index(self.rank) < ranks.index(other.rank) then 1
    else -1
    end
  end
end

class Deck
  include Enumerable
  RANKS = [2, 3, 4, 5, 6, 7, 8, 9 ,10 , :jack, :queen, :king, :ace]
  SUITS = [:spades, :hearts, :diamonds, :clubs]
  def initialize deck = self.class::RANKS.product(SUITS).map{|x| Card.new(*x)}
    @deck = deck
  end
  def each
    @deck.each{|card| yield card}
  end
  def size
    count = 0
    @deck.each{|card| count += 1}
    count
  end
  def draw_top_card
    @deck.delete_at 0
  end
  def draw_bottom_card
   @deck.slice! (- 1)
  end
  def top_card
    return @deck[0]
  end
  def bottom_card
    return @deck.last
  end
  def shuffle
    @deck.shuffle!
  end
  def sort
    @deck.sort! {|first_card, second_card| first_card.compare_to second_card,
                                                            self.class::RANKS}
  end
  def to_s
     @deck.each{|card| puts card.to_s}
  end
  def deal how_many
    @deck.slice!(0...how_many)
  end
end

class WarDeck < Deck
  RANKS = [2, 3, 4, 5, 6, 7, 8, 9 ,10 , :jack, :queen, :king, :ace]
  def deal
    WarHand.new(super 26)
  end
  class WarHand
    def initialize hand
      @hand = hand
    end
    def size
      count = 0
      @hand.each{|card| count += 1}
      count
    end
    def play_card
      @hand.delete_at 0
    end
    def allow_face_up?
      if @hand.size <= 3 then true
      else false
      end
    end
  end
end

class Array
  def rank_check
    ranks = [7, 8, 9, :jack, :queen, :king, 10, :ace]
    max, one_after_another, prev = 0, 1, self.first
      self.each{|x| if ranks.index(x.rank) == ranks.index(prev.rank) - 1 then
        one_after_another += 1
        prev = x
      elsif one_after_another > max then max = one_after_another
      one_after_another = 1
      prev = x
      else one_after_another = 1
      prev = x end}
    if one_after_another > max then max = one_after_another end
    max
  end
end

class BeloteDeck < Deck
  RANKS = [7, 8, 9, :jack, :queen, :king, 10, :ace]
  def deal
    BeloteHand.new(super 8)
  end
  class BeloteHand
    RANKS = [7, 8, 9, :jack, :queen, :king, 10, :ace]
    def initialize hand
      @hand = hand
    end
    def size
      count = 0
      @hand.each{|card| count += 1}
      count
    end
    def highest_of_suit suit
      highest = Card.new(0, suit)
      @hand.each{|card| if highest.rank == 0 && card.suit == suit then
                highest = card
                elsif card.suit == suit &&
                RANKS.index(card.rank) > RANKS.index(highest.rank) then
                highest = card
                end}
      if highest.rank == 0 then puts "No cards from that suuit."
      else return highest
      end
    end
    def belote?
      checkup = []
	  flag = false
      @hand.each{|card| if card.rank == :king or card.rank == :queen then
                        checkup << card
                        end}
      checkup.permutation(2).to_a.map{|pear| if pear[0].suit ==
	                                    pear[1].suit then flag = true end}
      flag
    end
    def tierce?
      for_checkup, flag = [], false
      @hand.sort! {|first_card, second_card| first_card.compare_to second_card,
                                                            self.class::RANKS}
      sorted = @hand.group_by { |card|  card.suit }
      sorted.each_value {|value| for_checkup << value}
      for_checkup.map{|suit_array| if suit_array.rank_check >= 3 then
                                                      flag = true end}
      flag
    end
    def quarte?
      for_checkup, flag = [], false
      @hand.sort! {|first_card, second_card| first_card.compare_to second_card,
                                                            self.class::RANKS}
      sorted = @hand.group_by { |card|  card.suit }
      sorted.each_value {|value| for_checkup << value}
      for_checkup.map{|suit_array| if suit_array.rank_check >= 3 then
                                                      flag = true end}
      flag
    end
    def quint?
      for_checkup, flag = [], false
      @hand.sort! {|first_card, second_card| first_card.compare_to second_card,
                                                            self.class::RANKS}
      sorted = @hand.group_by { |card|  card.suit }
      sorted.each_value {|value| for_checkup << value}
      for_checkup.map{|suit_array| if suit_array.rank_check >= 3 then
                                                      flag = true end}
      flag
    end
    def carre_of_jacks?
      jack = 0
      @hand.each{|card| if card.rank == :jack then jack += 1 end}
      if jack == 4 then true
      else false
      end
    end
    def carre_of_nines?
      nine = 0
      @hand.each{|card| if card.rank == 9 then nine += 1 end}
      if nine == 4 then true
      else false
      end
    end
    def carre_of_aces?
      ace = 0
      @hand.each{|card| if card.rank == :ace then ace += 1 end}
      if ace == 4 then true
      else false
      end
    end
  end
end

class SixtySixDeck < Deck
  RANKS = [9, :jack, :queen, :king, 10, :ace]
  def deal
    SixtySixHand.new(super 8)
  end
  class SixtySixHand
    def initialize hand
      @hand = hand
    end
    def size
      count = 0
      @hand.each{|card| count += 1}
      count
    end
    def twenty? trump_suit
      checkup = []
      @hand.each{|card| if card.rank == :king or card.rank == :queen then
                        checkup << card
                        end}
      checkup.permutation(2).to_a.each{|pear| if pear[0].suit == pear[1].suit &&
                               pear[0].suit != trump_suit then return true
                                                          else false end}
    end
    def forty? trump_suit
      checkup = []
      @hand.each{|card| if card.rank == :king or card.rank == :queen then
                        checkup << card
                        end}
      checkup.permutation(2).to_a.each{|pear| if pear[0].suit == pear[1].suit &&
                               pear[0].suit == trump_suit then return true
                                                          else false end}
    end
  end
end
