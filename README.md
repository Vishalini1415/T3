#include <iostream>
using namespace std;

const char EMPTY = ' ';
const int SIZE = 3;

void initializeBoard(char board[SIZE][SIZE]) {
    for (int i = 0; i < SIZE; ++i)
        for (int j = 0; j < SIZE; ++j)
            board[i][j] = EMPTY;
}

void displayBoard(const char board[SIZE][SIZE]) {
    cout << "  1 2 3\n";
    for (int i = 0; i < SIZE; ++i) {
        cout << i + 1 << " ";
        for (int j = 0; j < SIZE; ++j) {
            cout << board[i][j];
            if (j < SIZE - 1) cout << '|';
        }
        cout << '\n';
        if (i < SIZE - 1) cout << "  -----\n";
    }
}

bool isBoardFull(const char board[SIZE][SIZE]) {
    for (int i = 0; i < SIZE; ++i)
        for (int j = 0; j < SIZE; ++j)
            if (board[i][j] == EMPTY)
                return false;
    return true;
}

bool checkWin(const char board[SIZE][SIZE], char player) {
    // Check rows and columns
    for (int i = 0; i < SIZE; ++i) {
        if ((board[i][0] == player && board[i][1] == player && board[i][2] == player) ||
            (board[0][i] == player && board[1][i] == player && board[2][i] == player))
            return true;
    }
    // Check diagonals
    if ((board[0][0] == player && board[1][1] == player && board[2][2] == player) ||
        (board[0][2] == player && board[1][1] == player && board[2][0] == player))
        return true;
    
    return false;
}

void playerMove(char board[SIZE][SIZE], char player) {
    int row, col;
    while (true) {
        cout << "Player " << player << ", enter your move (row and column): ";
        cin >> row >> col;
        row--; col--; // Adjust for 0-based index
        if (row >= 0 && row < SIZE && col >= 0 && col < SIZE && board[row][col] == EMPTY) {
            board[row][col] = player;
            break;
        } else {
            cout << "Invalid move. Try again.\n";
        }
    }
}

int main() {
    char board[SIZE][SIZE];
    char currentPlayer = 'X';
    bool gameWon = false;
    bool playAgain = true;

    while (playAgain) {
        initializeBoard(board);
        gameWon = false;

        while (!gameWon && !isBoardFull(board)) {
            displayBoard(board);
            playerMove(board, currentPlayer);
            if (checkWin(board, currentPlayer)) {
                displayBoard(board);
                cout << "Player " << currentPlayer << " wins!\n";
                gameWon = true;
            } else {
                currentPlayer = (currentPlayer == 'X') ? 'O' : 'X'; // Switch player
            }
        }

        if (!gameWon) {
            displayBoard(board);
            cout << "It's a draw!\n";
        }

        char response;
        cout << "Play again? (y/n): ";
        cin >> response;
        if (response == 'n' || response == 'N') {
            playAgain = false;
        } else {
            currentPlayer = 'X'; // Reset starting player for new game
        }
    }

    cout << "Thanks for playing!\n";
    return 0;
}
