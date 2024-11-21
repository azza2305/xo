# xo
class Player:
    def __init__(self, name, symbol):
        self.name = name
        self.symbol = symbol


class Board:
    def __init__(self):
        self.board = [[" " for _ in range(3)] for _ in range(3)]

    def display(self):
        print("\n  0 1 2")
        for i, lig in enumerate(self.board):
            print(f"{i} {'|'.join(lig)}")
            if i < 2:
                print("  -----")

    def update(self, lig, col, symbol):
        if self.board[lig][col] == " ":
            self.board[lig][col] = symbol
            return True
        else:
            print("Case déjà occupée, réessayez.")
            return False

    def check_win(self, symbol):
    
        for lig in self.board:
            if all(cell == symbol for cell in lig):
                return True
        for col in range(3):
            if all(self.board[lig][col] == symbol for lig in range(3)):
                return True
        if all(self.board[i][i] == symbol for i in range(3)) or \
           all(self.board[i][2 - i] == symbol for i in range(3)):
            return True
        return False

    def check_draw(self):
        return all(cell != " " for lig in self.board for cell in lig)


class Game:
    def __init__(self):
        self.board = Board()
        self.players = []
        self.current_player_index = 0

    def setup(self):
        print(" Bienvenue a xo  ")
        for i in range(2):
            name = input(f"Entrez le nom du joueur {i + 1} : ")
            symbol = input(f"{name}, choisissez un symbole (ex : X ou O) : ").upper()
            self.players.append(Player(name, symbol))
        print(f"\n{self.players[0].name} joue avec {self.players[0].symbol}")
        print(f"{self.players[1].name} joue avec {self.players[1].symbol}")

    def play_turn(self):
        player = self.players[self.current_player_index]
        print(f"\nC'est au tour de {player.name} ({player.symbol})")
        while True:
            try:
                lig = int(input("Entrez la ligne (0-2) : "))
                col = int(input("Entrez la colonne (0-2) : "))
                if self.board.update(lig, col, player.symbol):
                    break
            except (ValueError, IndexError):
                print("Entrée invalide, réessayez.")

    def check_game_over(self):
        player = self.players[self.current_player_index]
        if self.board.check_win(player.symbol):
            self.board.display()
            print(f"Félicitations {player.name}, vous avez gagné !")
            return True
        elif self.board.check_draw():
            self.board.display()
            print("Match nul !")
            return True
        return False

    def switch_player(self):
        self.current_player_index = 1 - self.current_player_index

    def start(self):
        self.setup()
        while True:
            self.board.display()
            self.play_turn()
            if self.check_game_over():
                break
            self.switch_player()


if __name__ == "__main__":
    game = Game()
    game.start()
