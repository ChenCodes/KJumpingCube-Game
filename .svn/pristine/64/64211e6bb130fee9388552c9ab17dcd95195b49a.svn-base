package jump61;

import static jump61.Side.*;
import static jump61.Square.square;
import java.util.ArrayList;
import java.util.Stack;

/** A Jump61 board state that may be modified.
 *  @author Yuxi Chen
 */
class MutableBoard extends Board {

    /** An N x N board in initial configuration. */
    MutableBoard(int N) {
        _newBoard = new Square[N][N];
        _stackBoard = new Stack<Square[][]>();
        _numPieces = this.size() * this.size();
        _possibleMoves = new ArrayList<Integer>();
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < N; j++) {
                _newBoard[i][j] = Square.square(WHITE, 1);
            }
        }
    }

    /** A board whose initial contents are copied from BOARD0, but whose
     *  undo history is clear. */
    MutableBoard(Board board0) {
        int N = board0.size();
        clear(N);
        _newBoard = new Square[N][N];
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < N; j++) {
                _newBoard[i][j] = board0.get(i + 1, j + 1);
            }
        }
    }

    /** This returns the _copyboard variable. */
    Square[][] getBoard() {
        return _copyBoard;
    }

    /** This sets the board to setBoard.
        @param board This is a board array.
        */
    void setBoard(Square[][] board) {
        _newBoard = board;
    }
    /** This pushes an array onto the stack.
        @param board This is a board array.
        */
    void pushStack(Square[][] board) {
        _stackBoard.push(board);
    }

    @Override
    void clear(int N) {
        _newBoard = new Square[N][N];
        _stackBoard = new Stack<Square[][]>();
        for (int i = 0; i < _newBoard.length; i++) {
            for (int j = 0; j < _newBoard.length; j++) {
                _newBoard[i][j] = Square.square(WHITE, 1);
            }
        }
        _numPieces = this.size() * this.size();
        announce();
    }

    @Override
    void copy(Board board) {
        _otherCopyBoard = new Square[board.size()][board.size()];
        for (int i = 0; i < board.size(); i++) {
            for (int j = 0; j < board.size(); j++) {
                _otherCopyBoard[i][j] = board.get(i + 1, j + 1);
            }
        }
        _newBoard = _otherCopyBoard;
    }

    /** Copy the contents of BOARD into me, without modifying my undo
     *  history.  Assumes BOARD and I have the same size. */
    private void internalCopy(MutableBoard board) {
        _undoCopyBoard = new Square[board.size()][board.size()];
        for (int i = 0; i < board.size(); i++) {
            for (int j = 0; j < board.size(); j++) {
                _undoCopyBoard[i][j] = board.get(i + 1, j + 1);
            }
        }
    }

    @Override
    int size() {
        int length = _newBoard.length;
        return length;
    }

    @Override
    Square get(int n) {
        int counter = 0;
        for (int i = 0; i < _newBoard.length; i++) {
            for (int j = 0; j < _newBoard.length; j++) {
                if (counter == n) {
                    counter = 0;
                    return _newBoard[i][j];
                } else {
                    counter += 1;
                }

            }
        }
        return null;
    }

    @Override
    int numOfSide(Side side) {
        int counter = 0;
        for (int i = 0; i < _newBoard.length; i++) {
            for (int j = 0; j < _newBoard.length; j++) {
                if (_newBoard[i][j].getSide() == side) {
                    counter += 1;
                }
            }
        }
        return counter;
    }

    @Override
    int numPieces() {
        return _numPieces;
    }

    @Override
    ArrayList<Integer> possibleMoves() {
        return _possibleMoves;
    }

    @Override
    void addSpot(Side player, int r, int c) {
        addSpot(player, sqNum(r, c));
    }

    /** This will end up calling my other addspot.
        * @param player This is my player.
        * @param r This is my row.
        * @param c This is my column.
        */
    void addSpot2(Side player, int r, int c) {
        addSpot2(player, sqNum(r, c));
    }

    /** This adds a spot to the right square.
        * @param player This is my player.
        * @param n This is the square that I want to add to.
        */
    void addSpot2(Side player, int n) {
        int bLen = _newBoard.length;
        if (!makeLegal) {
            internalCopy(this);
            _stackBoard.push(_undoCopyBoard);
        }
        int counter = 0;
        if (isLegal(player, n) || this.makeLegal) {
        outerloop:
            for (int i = 0; i < bLen; i++) {
                for (int j = 0; j < bLen; j++) {
                    if (counter == n) {
                        int curDot = _newBoard[i][j].getSpots() + 1;
                        if (i == 0 && j == 0 || i == (bLen - 1) && j == 0 || i
                            == 0 && j == bLen - 1 || i == bLen - 1 && j
                            == bLen - 1) {
                            if ((_newBoard[i][j].getSpots() + 1) > 2) {
                                _newBoard[i][j] = Square.square(player, curDot);
                                if (addHelper(bLen, player)) {
                                    break outerloop;
                                }
                                currNeighbors(i + 1, j + 1, player, curDot);
                                break outerloop;
                            } else {
                                _newBoard[i][j] = Square.square(player, curDot);
                            }
                        } else if (i == 0 || i == (bLen - 1)
                            || j == 0 || j == (bLen - 1)) {
                            if ((_newBoard[i][j].getSpots() + 1) > 3) {
                                _newBoard[i][j] = Square.square(player, curDot);
                                if (addHelper(bLen, player)) {
                                    break outerloop;
                                }
                                _newBoard[i][j] = Square.square(player, curDot);
                                currNeighbors(i + 1, j + 1, player, curDot);
                                break outerloop;
                            } else {
                                _newBoard[i][j] = Square.square(player, curDot);
                            }
                        } else {
                            int prevSpots = _newBoard[i][j].getSpots();
                            if ((_newBoard[i][j].getSpots() + 1) > 4) {
                                if (addHelper(bLen, player)) {
                                    break outerloop;
                                }
                                addHelper2(i, j, curDot, player);
                                break outerloop;
                            } else {
                                _newBoard[i][j] = Square.square(player,
                                    prevSpots + 1);
                            }
                        }
                    }
                    counter += 1;
                }
            }
        }
        announce();
    }


    /** Helper method for else case of addspots.
        @param bLen This is the size of the board.
        @param player This is the current player.
        @return
        */
    boolean addHelper(int bLen, Side player) {
        if (numOfSide(player) == bLen * bLen) {
            this.getWinner();
            return true;
        }
        return false;
    }

    /** Second helper method for else case of addspots.
        @param i This is the row.
        @param j This is the col.
        @param curDot This is the current number of spots.
        @param player This is the current player.
        */
    void addHelper2(int i, int j, int curDot, Side player) {
        _newBoard[i][j] = Square.square(player, curDot + 1);
        currNeighbors(i + 1, j + 1, player, curDot);
    }
    @Override
    void addSpot(Side player, int n) {
        _numPieces++;
        addSpot2(player, n);
        makeLegal = false;
    }

    /** Helper function that returns how many spots left on main square.
        @param r This is the row.
        @param c This is the column.
        @param player This is the player.
        @param spotsLeft This is the number of spots left.
        */
    void currNeighbors(int r, int c, Side player, int spotsLeft) {
        makeLegal = true;
        _newBoard[r - 1][c - 1] = Square.square(player, 1);
        if (r == 1 && c == 1) {
            this.addSpot2(player, r, c + 1);
            this.addSpot2(player, r + 1, c);
        } else if (r == _newBoard.length && c == 1) {
            this.addSpot2(player, r, c + 1);
            this.addSpot2(player, r - 1, c);
        } else if (r == 1 && c == _newBoard.length) {
            this.addSpot2(player, r + 1, c);
            this.addSpot2(player, r, c - 1);
        } else if (r == _newBoard.length && c == _newBoard.length) {
            this.addSpot2(player, r, c - 1);
            this.addSpot2(player, r - 1, c);
        } else if (c == 1) {
            this.addSpot2(player, r - 1, c);
            this.addSpot2(player, r + 1, c);
            this.addSpot2(player, r, c + 1);
        } else if (r == 1 && c != _newBoard.length) {
            this.addSpot2(player, r, c - 1);
            this.addSpot2(player, r, c + 1);
            this.addSpot2(player, r + 1, c);
        } else if (r == _newBoard.length && c != _newBoard.length) {
            this.addSpot2(player, r, c - 1);
            this.addSpot2(player, r, c + 1);
            this.addSpot2(player, r - 1, c);
        } else if (c == _newBoard.length) {
            this.addSpot2(player, r - 1, c);
            this.addSpot2(player, r + 1, c);
            this.addSpot2(player, r, c - 1);
        } else {
            this.addSpot2(player, r - 1, c);
            this.addSpot2(player, r + 1, c);
            this.addSpot2(player, r, c - 1);
            this.addSpot2(player, r, c + 1);
        }
    }

    /** This is a method to print all of my statements.
        @param o This can be anything.
        */
    public static void printt(Object o) {
    }
    @Override
    void set(int r, int c, int num, Side player) {
        internalSet(sqNum(r, c), square(player, num));
    }

    @Override
    void set(int n, int num, Side player) {
        internalSet(n, square(player, num));
        announce();
    }

    @Override
    void undo() {
        _newBoard = _stackBoard.pop();
    }

    /** Record the beginning of a move in the undo history. */
    private void markUndo() {
    }

    /** Set the contents of the square with index IND to SQ. Update counts
     *  of numbers of squares of each color.  */
    private void internalSet(int ind, Square sq) {
        int counter = 0;
    outerloop:
        for (int i = 0; i < _newBoard.length; i++) {
            for (int j = 0; j < _newBoard.length; j++) {
                if (counter == ind) {
                    _numPieces -= _newBoard[i][j].getSpots();
                    _numPieces += sq.getSpots();
                    _newBoard[i][j] = sq;
                    break outerloop;
                } else {
                    counter += 1;
                }

            }
        }
    }


    /** Notify all Observers of a change. */
    private void announce() {
        setChanged();
        notifyObservers();
    }

    @Override
    public boolean equals(Object obj) {
        if (!(obj instanceof MutableBoard)) {
            return obj.equals(this);
        } else {
            return false;
        }
    }

    @Override
    public int hashCode() {
        return 0;
    }

    /** This holds the array of my board. */
    private Square[][] _newBoard;

    /** This holds an array of the board I just copied. */
    private Square[][] _copyBoard;

    /** This holds a copy of the board with the undo history unchanged. */
    private Square[][] _undoCopyBoard;

    /** This holds a copy of another board. */
    private Square[][] _otherCopyBoard;

    /**This holds a stack of double array objects. */
    private Stack<Square[][]> _stackBoard;

    /** This holds the number of spots I have on my board. */
    private int _numPieces;

    /** This tells me if it's legal to add another spot. */
    private boolean makeLegal;

    /** This holds all of the possible moves that the next player can make. */
    private ArrayList<Integer> _possibleMoves;


}
