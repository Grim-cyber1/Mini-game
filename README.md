from tkinter import *

board = [[' ' for _ in range(3)] for _ in range(3)]
current_player = 'X'

def check_winner(player):
    for i in range(3):
        if board[i][0] == board[i][1] == board[i][2] == player or \
           board[0][i] == board[1][i] == board[2][i] == player or \
           board[0][0] == board[1][1] == board[2][2] == player or \
           board[0][2] == board[1][1] == board[2][0] == player:
            return True
    return False

def make_move(row, col):
    global current_player

    if board[row][col] == ' ':
        board[row][col] = current_player
        button = buttons[row][col]
        button.config(text=current_player)

        if check_winner(current_player):
            status_label.config(text=f'Игрок {current_player} выиграл!')
            disable_buttons()
        elif all([cell != ' ' for row in board for cell in row]):
            status_label.config(text='Ничья!')
            disable_buttons()
        else:
            current_player = 'O' if current_player == 'X' else 'X'
            status_label.config(text=f'Ход игрока {current_player}')

            if game_mode.get() == 1 and current_player == 'O':
                make_computer_move()

def make_computer_move():
    for i in range(3):
        for j in range(3):
            if board[i][j] == ' ':
                make_move(i, j)
                return

def disable_buttons():
    for row in buttons:
        for button in row:
            button.config(state=DISABLED)

window = Tk()
window.title('Крестики-нолики')

buttons = []
for i in range(3):
    row_buttons = []
    for j in range(3):
        button = Button(window, text=' ', font=('Arial', 20), width=5, height=2,
                        command=lambda r=i, c=j: make_move(r, c))
        button.grid(row=i, column=j)
        row_buttons.append(button)
    buttons.append(row_buttons)

game_mode = IntVar()
human_vs_human_radiobutton = Radiobutton(window, text='Человек против человека', variable=game_mode, value=0)
human_vs_human_radiobutton.grid(row=3, column=0, columnspan=3)
human_vs_computer_radiobutton = Radiobutton(window, text='Человек против компьютера', variable=game_mode, value=1)
human_vs_computer_radiobutton.grid(row=4, column=0, columnspan=3)

status_label = Label(window, text='Ход игрока X', font=('Arial', 12), pady=10)
status_label.grid(row=5, column=0, columnspan=3)

window.mainloop()
