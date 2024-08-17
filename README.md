# Custom-chess-game
#include <iostream>
#include <vector>
#include <string>
#include <cctype>

using namespace std;

class ChessBoard {
public:
    vector<vector<char>> board;
    bool whiteTurn = true;

    ChessBoard() {
        board = vector<vector<char>>(8, vector<char>(8, '.'));
        setupBoard();
    }

    void setupBoard() {
        string initialSetup[8] = {
            "rnbqkbnr",
            "pppppppp",
            "........",
            "........",
            "........",
            "........",
            "PPPPPPPP",
            "RNBQKBNR"
        };
        for (int i = 0; i < 8; i++) {
            for (int j = 0; j < 8; j++) {
                board[i][j] = initialSetup[i][j];
            }
        }
    }

    void printBoard() {
        for (int i = 0; i < 8; i++) {
            cout << 8 - i << " ";
            for (int j = 0; j < 8; j++) {
                cout << board[i][j] << " ";
            }
            cout << endl;
        }
        cout << "  a b c d e f g h" << endl;
    }

    bool isValidMove(int startX, int startY, int endX, int endY, char piece) {
        // Basic boundary check
        if (startX < 0 || startX >= 8 || startY < 0 || startY >= 8 ||
            endX < 0 || endX >= 8 || endY < 0 || endY >= 8) {
            return false;
        }

        char targetPiece = board[endX][endY];

        // Ensure you are not capturing your own piece
        if (isSameColor(piece, targetPiece)) return false;

        // Check if the move is valid based on the piece
        switch (tolower(piece)) {
            case 'p': return isValidPawnMove(startX, startY, endX, endY, piece);
            case 'r': return isValidRookMove(startX, startY, endX, endY);
            case 'n': return isValidKnightMove(startX, startY, endX, endY);
            case 'b': return isValidBishopMove(startX, startY, endX, endY);
            case 'q': return isValidQueenMove(startX, startY, endX, endY);
            case 'k': return isValidKingMove(startX, startY, endX, endY);
            default: return false;
        }
    }

    void makeMove(int startX, int startY, int endX, int endY) {
        char piece = board[startX][startY];
        if (isValidMove(startX, startY, endX, endY, piece)) {
            board[endX][endY] = piece;
            board[startX][startY] = '.';
            whiteTurn = !whiteTurn;
        } else {
            cout << "Invalid move!" << endl;
        }
    }

private:
    bool isSameColor(char piece1, char piece2) {
        return (isupper(piece1) && isupper(piece2)) || (islower(piece1) && islower(piece2));
    }

    bool isValidPawnMove(int startX, int startY, int endX, int endY, char piece) {
        int direction = isupper(piece) ? -1 : 1;

        // Moving forward
        if (startX + direction == endX && startY == endY && board[endX][endY] == '.') {
            return true;
        }

        // Moving forward two spaces from starting position
        if ((startX == 6 && direction == -1 || startX == 1 && direction == 1) &&
            startX + 2 * direction == endX && startY == endY && board[endX][endY] == '.') {
            return true;
        }

        // Capturing diagonally
        if (startX + direction == endX && abs(startY - endY) == 1 && board[endX][endY] != '.' &&
            !isSameColor(piece, board[endX][endY])) {
            return true;
        }

        return false;
    }

    bool isValidRookMove(int startX, int startY, int endX, int endY) {
        if (startX != endX && startY != endY) return false;

        // Check path
        if (startX == endX) {
            int direction = (endY > startY) ? 1 : -1;
            for (int i = startY + direction; i != endY; i += direction) {
                if (board[startX][i] != '.') return false;
            }
        } else {
            int direction = (endX > startX) ? 1 : -1;
            for (int i = startX + direction; i != endX; i += direction) {
                if (board[i][startY] != '.') return false;
            }
        }

        return true;
    }

    bool isValidKnightMove(int startX, int startY, int endX, int endY) {
        int dx = abs(endX - startX);
        int dy = abs(endY - startY);
        return (dx == 2 && dy == 1) || (dx == 1 && dy == 2);
    }

    bool isValidBishopMove(int startX, int startY, int endX, int endY) {
        if (abs(endX - startX) != abs(endY - startY)) return false;

        int xDirection = (endX > startX) ? 1 : -1;
        int yDirection = (endY > startY) ? 1 : -1;

        for (int i = 1; i < abs(endX - startX); i++) {
            if (board[startX + i * xDirection][startY + i * yDirection] != '.') {
                return false;
            }
        }

        return true;
    }

    bool isValidQueenMove(int startX, int startY, int endX, int endY) {
        return isValidRookMove(startX, startY, endX, endY) || isValidBishopMove(startX, startY, endX, endY);
    }

    bool isValidKingMove(int startX, int startY, int endX, int endY) {
        return abs(startX - endX) <= 1 && abs(startY - endY) <= 1;
    }
};

int main() {
    ChessBoard chessBoard;
    string input;
    while (true) {
        chessBoard.printBoard();
        cout << (chessBoard.whiteTurn ? "White's move: " : "Black's move: ");
        cin >> input;
        if (input == "quit") break;

        int startX = 8 - (input[1] - '0');
        int startY = input[0] - 'a';
        int endX = 8 - (input[3] - '0');
        int endY = input[2] - 'a';

        chessBoard.makeMove(startX, startY, endX, endY);
    }

    return 0;
}#include <iostream>
#include <vector>
#include <string>
#include <cctype>

using namespace std;

class ChessBoard {
public:
    vector<vector<char>> board;
    bool whiteTurn = true;

    ChessBoard() {
        board = vector<vector<char>>(8, vector<char>(8, '.'));
        setupBoard();
    }

    void setupBoard() {
        string initialSetup[8] = {
            "rnbqkbnr",
            "pppppppp",
            "........",
            "........",
            "........",
            "........",
            "PPPPPPPP",
            "RNBQKBNR"
        };
        for (int i = 0; i < 8; i++) {
            for (int j = 0; j < 8; j++) {
                board[i][j] = initialSetup[i][j];
            }
        }
    }

    void printBoard() {
        for (int i = 0; i < 8; i++) {
            cout << 8 - i << " ";
            for (int j = 0; j < 8; j++) {
                cout << board[i][j] << " ";
            }
            cout << endl;
        }
        cout << "  a b c d e f g h" << endl;
    }

    bool isValidMove(int startX, int startY, int endX, int endY, char piece) {
        // Basic boundary check
        if (startX < 0 || startX >= 8 || startY < 0 || startY >= 8 ||
            endX < 0 || endX >= 8 || endY < 0 || endY >= 8) {
            return false;
        }

        char targetPiece = board[endX][endY];

        // Ensure you are not capturing your own piece
        if (isSameColor(piece, targetPiece)) return false;

        // Check if the move is valid based on the piece
        switch (tolower(piece)) {
            case 'p': return isValidPawnMove(startX, startY, endX, endY, piece);
            case 'r': return isValidRookMove(startX, startY, endX, endY);
            case 'n': return isValidKnightMove(startX, startY, endX, endY);
            case 'b': return isValidBishopMove(startX, startY, endX, endY);
            case 'q': return isValidQueenMove(startX, startY, endX, endY);
            case 'k': return isValidKingMove(startX, startY, endX, endY);
            default: return false;
        }
    }

    void makeMove(int startX, int startY, int endX, int endY) {
        char piece = board[startX][startY];
        if (isValidMove(startX, startY, endX, endY, piece)) {
            board[endX][endY] = piece;
            board[startX][startY] = '.';
            whiteTurn = !whiteTurn;
        } else {
            cout << "Invalid move!" << endl;
        }
    }

private:
    bool isSameColor(char piece1, char piece2) {
        return (isupper(piece1) && isupper(piece2)) || (islower(piece1) && islower(piece2));
    }

    bool isValidPawnMove(int startX, int startY, int endX, int endY, char piece) {
        int direction = isupper(piece) ? -1 : 1;

        // Moving forward
        if (startX + direction == endX && startY == endY && board[endX][endY] == '.') {
            return true;
        }

        // Moving forward two spaces from starting position
        if ((startX == 6 && direction == -1 || startX == 1 && direction == 1) &&
            startX + 2 * direction == endX && startY == endY && board[endX][endY] == '.') {
            return true;
        }

        // Capturing diagonally
        if (startX + direction == endX && abs(startY - endY) == 1 && board[endX][endY] != '.' &&
            !isSameColor(piece, board[endX][endY])) {
            return true;
        }

        return false;
    }

    bool isValidRookMove(int startX, int startY, int endX, int endY) {
        if (startX != endX && startY != endY) return false;

        // Check path
        if (startX == endX) {
            int direction = (endY > startY) ? 1 : -1;
            for (int i = startY + direction; i != endY; i += direction) {
                if (board[startX][i] != '.') return false;
            }
        } else {
            int direction = (endX > startX) ? 1 : -1;
            for (int i = startX + direction; i != endX; i += direction) {
                if (board[i][startY] != '.') return false;
            }
        }

        return true;
    }

    bool isValidKnightMove(int startX, int startY, int endX, int endY) {
        int dx = abs(endX - startX);
        int dy = abs(endY - startY);
        return (dx == 2 && dy == 1) || (dx == 1 && dy == 2);
    }

    bool isValidBishopMove(int startX, int startY, int endX, int endY) {
        if (abs(endX - startX) != abs(endY - startY)) return false;

        int xDirection = (endX > startX) ? 1 : -1;
        int yDirection = (endY > startY) ? 1 : -1;

        for (int i = 1; i < abs(endX - startX); i++) {
            if (board[startX + i * xDirection][startY + i * yDirection] != '.') {
                return false;
            }
        }

        return true;
    }

    bool isValidQueenMove(int startX, int startY, int endX, int endY) {
        return isValidRookMove(startX, startY, endX, endY) || isValidBishopMove(startX, startY, endX, endY);
    }

    bool isValidKingMove(int startX, int startY, int endX, int endY) {
        return abs(startX - endX) <= 1 && abs(startY - endY) <= 1;
    }
};

int main() {
    ChessBoard chessBoard;
    string input;
    while (true) {
        chessBoard.printBoard();
        cout << (chessBoard.whiteTurn ? "White's move: " : "Black's move: ");
        cin >> input;
        if (input == "quit") break;

        int startX = 8 - (input[1] - '0');
        int startY = input[0] - 'a';
        int endX = 8 - (input[3] - '0');
        int endY = input[2] - 'a';

        chessBoard.makeMove(startX, startY, endX, endY);
    }

    return 0;
}
