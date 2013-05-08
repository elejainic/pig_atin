pig_atin
========
# # Topics
#
# * modules
# * strings
#
# # Pig Latin
#
# Pig Latin is a made-up children's language that's intended to be confusing. It obeys a few simple rules (below) but when it's spoken quickly it's really difficult for non-children (and non-native speakers) to understand.
#
# Rule 1: If a word begins with a vowel sound, add an "ay" sound to the end of the word.
#
# Rule 2: If a word begins with a consonant sound, move it to the end of the word, and then add an "ay" sound to the end of the word.
#
# (There are a few more rules for edge cases, and there are regional variants too, but that should be enough to understand the tests.)
#
# See <http://en.wikipedia.org/wiki/Pig_latin> for more details.
#
#

require "pig_latin_spec"

 
module PigLatin
  def translate_string(string)
    vowel = /\A[aeiouy]/
 
    until string =~ vowel
      if string[0,2] == "qu"
        string += string.slice!(0,2)
      else
        string += string.slice!(0)
      end
    end
 
    string + "ay"
  end
  def translate(string)
    symbol_end = /[\.:,;!\?]\Z/
 
    if string.split.size > 1
      string = string.split.map { |e| translate(e) }.join(' ')
    else
      capital_letter = true if string[0] == string[0].upcase
       word_punctuation = (string.slice!(-1) if string =~ symbol_end) || ''
 
      string = translate_string(string)
      string.downcase!.capitalize! if capital_letter 
      string + word_punctuation
    end
 
  end
end

describe "#translate" do
  include PigLatin

  it "translates a word beginning with a vowel" do
    s = translate("apple")
    s.should == "appleay"
  end

  it "translates a word beginning with a consonant" do
    s = translate("banana")
    s.should == "ananabay"
  end

  it "translates a word beginning with two consonants" do
    s = translate("cherry")
    s.should == "errychay"
  end

  it "translates two words" do
    s = translate("eat pie")
    s.should == "eatay iepay"
  end

  it "translates a word beginning with three consonants" do
    translate("three").should == "eethray"
  end

  it "counts 'sch' as a single phoneme" do
    s = translate("school")
    s.should == "oolschay"
  end

  it "counts 'qu' as a single phoneme" do
    s = translate("quiet")
    s.should == "ietquay"
  end

  it "counts 'qu' as a consonant even when it's preceded by a consonant" do
    s = translate("square")
    s.should == "aresquay"
  end

  it "translates many words" do
    s = translate("the quick brown fox")
    s.should == "ethay ickquay ownbray oxfay"
  end

  # Test-driving bonus:
  # * write a test asserting that capitalized words are still capitalized (but with a different initial capital letter, of course)
  # * retain the punctuation from the original phrase

end
