import random
import math 
import tkinter as tk 
from tkinter import messagebox 
from words import words
import string

def games():
    a = input("What is your name?: ")
    print("Hello", a, "It's nice to meet you. I hope you're having a lovely day. What game would you like to play?");
    b = input("tic tac toe, hangman, or madlibs?: ")
    if b == "tic tac toe":
          class TicTacToe: 
            def __init__(self, root):
                 self.root = root 
                 self.root.title("Tic-Tac-Toe") 
                 self.current_player = "X" 
                 self.buttons = [[None for _ in range(3)] for _ in range(3)] 
                 self.create_board() 
    
            def create_board(self): 
             """Create a 3x3 grid of buttons for the game board.""" 
             for row in range(3): 
              for col in range(3): 
                button = tk.Button(self.root, text="", font="Arial 20 bold", width=5, height=2,
                                    command=lambda r=row, c=col: self.on_click(r, c)) 
                button.grid(row=row, column=col) 
                self.buttons[row][col] = button 
                
            def on_click(self, row, col): 
             """Handle button click."""
             if self.buttons[row][col]["text"] == "" and self.check_winner() is None:
               self.buttons[row][col]["text"] = self.current_player 
               if self.check_winner() is None: 
                 self.current_player = "O" if self.current_player == "X" else "X" 
               else: 
                self.show_winner() 
             elif self.check_winner() is None: 
               messagebox.showinfo("Invalid move", "This spot is already taken!") 
            
            def check_winner(self): 
             """Check for a winner or if the game is a tie.""" 
             for i in range(3): 
                # Check rows 
                if self.buttons[i][0]["text"] == self.buttons[i][1]["text"] == self.buttons[i][2]["text"] != "": 
                 return self.buttons[i][0]["text"] 
                # Check columns 
                if self.buttons[0][i]["text"] == self.buttons[1][i]["text"] == self.buttons[2][i]["text"] != "": 
                 return self.buttons[0][i]["text"] 
            
             # Check diagonals 
             if self.buttons[0][0]["text"] == self.buttons[1][1]["text"] == self.buttons[2][2]["text"] != "": 
               return self.buttons[0][0]["text"] 
             if self.buttons[0][2]["text"] == self.buttons[1][1]["text"] == self.buttons[2][0]["text"] != "": 
               return self.buttons[0][2]["text"] 
        
             # Check for a tie (if no empty spaces are left) 
             for row in self.buttons: 
               for button in row: 
                 if button["text"] == "": 
                    return None 
             return "Tie" 
            def show_winner(self): 
              """Display the winner or if the game is a tie.""" 
              winner = self.check_winner() 
              if winner == "Tie": 
               messagebox.showinfo("Game Over", "It's a tie!") 
              else: 
               messagebox.showinfo("Game Over", f"The winner is {winner}!") 
              self.reset_game() 
        
            def reset_game(self): 
               """Reset the game board for a new game.""" 
               for row in range(3): 
                 for col in range(3): 
                     self.buttons[row][col]["text"] = "" 
               self.current_player = "X" 
        
          # Create the main window 
          if __name__ == "__main__": 
            root = tk.Tk() 
            game = TicTacToe(root) 
            root.mainloop()


    elif b == "hangman":
      def get_valid_word(words):
          word = random.choice(words) # randomly chooses something from the list
          while '-' in word or ' ' in word:
              word = random.choice(words)

          return word.upper()


      def hangman():
          word = get_valid_word(words)
          word_letters = set(word) # letters in the word
          alphabet = set(string.ascii_uppercase)
          used_letters = set() # what the user has guessed

          lives = 6

          # getting user input
          while len(word_letters) > 0 and lives > 0:
              # letters used
              print('You have', lives, 'lives left and you have used these letters: ', ' '.join(used_letters))
        
              # what current word is
              word_list = [letter if letter in used_letters else '-' for letter in word]
              print('Current word: ', ' '.join(word_list))

              user_letter = input('Guess a letter: ').upper()
              if user_letter in alphabet - used_letters:
                  used_letters.add(user_letter)
                  if user_letter in word_letters:
                    word_letters.remove(user_letter)
                  else:
                    lives = lives - 1 # takes away a life if wrong
                    print('Letter is not in word')

              elif user_letter in used_letters:
                  print('You have already used that character. Please try again.')

              else:
                  print("Invalid character. Please try again.")

            # gets here when len(word_letters) == 0 OR when lives == 0
          if lives == 0:
              print('You died, sorry. The word was', word)
          else:
              print('You guessed the word', word, '!!')

      hangman();
    
    
    elif b == "madlibs":
      adj = input("Adjective: ")
      verb1 = input("Verb: ")
      verb2 = input("Verb: ")
      friends_name = input("Friends name: ")

      madlib = f"APUSH is so {adj}! It makes me want to {verb1} because it's too much work and isn't even intellectually stimulating. I honestly might {verb2} every time I have that class with {friends_name} because it's so boring!"

      print(madlib);


    else:
     print("Invalid input, try again")
     games();
    
games();
