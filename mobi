"""
Made to fix simple mistakes in a mobi book I downloaded from project gutenberg.  Works on plain text atm.

Common mistakes were: conjoined words,
                      "m" sometimes actually being "in", and
                      "n" sometimes actually being "ri".
"""
class MobiFile(object):
    def __init__(self, filename):
        self.filename = filename
        self.english_dict = self.make_english_dict()

    def start(self):
        word_spagetti = []
        for word in self.file_contents():
            if word not in self.english_dict:
                word, next = self.modify_if_conjoined(word)
            if next:
                word, next = self.modify_if_misspelled(word)
            word_spagetti.append(word)

        # write output
        with open(self.filename + '.new.txt', 'w') as f:
            for w in word_spagetti:
                f.write(w + ' ')

    def find_all(self, input, pattern):
        """Return a list of integers corresponding to found indexes of a pattern in a string."""
        start = 0
        while True:
            start = input.find(pattern, start)
            if start == -1: return
            yield start
            start += len(pattern)

    def make_english_dict(self):
        with open(self.filename, 'r') as f:
            for line in f:
                self.english_dict[line.strip()] = None

    def file_contents(self):
        with open(self.filename, 'r') as f:
            for line in f:
                for word in line.split(' '):
                    yield word.strip()

    def modify_if_conjoined(self, word):
        """Split a word and check the dictionary to see if the split makes sense."""
        for i in range(len(word)):
            word1 = word[:i]
            word2 = word[i:]
            if word1 in self.english_dict and word2 in self.english_dict:
                return word1 + ' ' + word2, False
        return word, True

    def replace_letter(self, word, letter='n', replacement_letters='ri'):
        for i in self.find_all(word, letter):
            new_word = word[:i] + replacement_letters + word[i+1:]
            if new_word in self.english_dict:
                return new_word, False
        return word, True

    def modify_if_misspelled(self, word):
        """Attempt to fix some common misspellings in the mobi."""
        word, next = self.replace_letter(word, 'n', 'ri')
        if next:
            word, next = self.replace_letter(word, 'm', 'in')
        return word, next
