package jump61;

import java.util.ArrayList;


/** An automated Player.
 *  @author Yuxi Chen
 */
class AI extends Player {

    /** Time allotted to all but final search depth (milliseconds). */
    private static final long TIME_LIMIT = 15000;

    /** Number of calls to minmax between checks of elapsed time. */
    private static final long TIME_CHECK_INTERVAL = 10000;

    /** Number of milliseconds in one second. */
    private static final double MILLIS = 1000.0;

    /** Special number for best score. */
    private static final int BESTSCORE = -1000;

    /** Special random cutoff. */
    private static final int CUTOFF1 = 123123;

    /** Another special random cutoff. */
    private static final int CUTOFF2 = 1234;

    /** A new player of GAME initially playing COLOR that chooses
     *  moves automatically.
     */
    AI(Game game, Side color) {
        super(game, color);
        currGame = game;
        bestMoveSoFar = -1;
        bestScore = BESTSCORE;
        currMove = -1;
        _color = color;
    }
    /**;.*/
    @Override
    void makeMove() {
        boolean emptyBoard = true;
        bestScore = BESTSCORE;
        currMove = -1;
        bestMoveSoFar = -1;
        currBoard = currGame.getBoard();
        Side currSide;
        MutableBoard copiedBoard = new MutableBoard(currBoard);
        ArrayList<Integer> moves2 = new ArrayList<Integer>();
        minmax(_color, copiedBoard, 4, CUTOFF1, moves2);
        if (moves2.isEmpty()) {
            return;
        }
        bestMoveSoFar = moves2.get(0);
        getGame().makeMove(bestMoveSoFar);
        getGame().reportMove(_color, currBoard.row(bestMoveSoFar),
            currBoard.col(bestMoveSoFar));
    }

    /** Sets the bestMoveSoFar and various variables back to initial. */
    void setVariables() {
        bestMoveSoFar = -1;
        bestScore = BESTSCORE;
        currMove = -1;
    }


    /** This tells us the depth that we should start at. */
    private int depth = 4;

    /** This finds the best possible move that we can do.
        @param p This is a side.
        @param b This is a board.
        @param d This is the level that we start at.
        @param cutoff This is used for pruning.
        @param moves This holds the moves that we will be using.
        @return
        */
    private int minmax(Side p, Board b, int d, int cutoff,
                       ArrayList<Integer> moves) {
        ArrayList<Integer> newMoves = new ArrayList<Integer>();
        MutableBoard clone1 = new MutableBoard(b);
        if (b.getWinner() != null || d == 0) {
            return staticEval(_color, b);
        }
        int best = -1;
        if (d % 2 == depth % 2) {
            best = Integer.MIN_VALUE;
        } else {
            best = Integer.MAX_VALUE;
        }
        for (int i = 0; i < clone1.size(); i++) {
            for (int j = 0; j < clone1.size(); j++) {
                if (clone1.get(i + 1, j + 1).getSide() == p
                    ||
                    clone1.get(i + 1, j + 1).getSide() == Side.WHITE) {
                    Board clone2 = new MutableBoard(b);
                    clone2.addSpot(p, i + 1, j + 1);
                    int val = minmax(p.opposite(), clone2, d - 1,
                        CUTOFF2, moves);
                    if (d % 2 == depth % 2) {
                        if (val > best) {
                            best = val;
                            if (d == depth) {
                                moves.clear();
                                moves.add(clone2.sqNum(i + 1, j + 1));
                            }
                        }
                    } else {
                        if (val < best) {
                            best = val;
                        }
                    }
                }
            }
        }
        return best;
    }
    /** Returns heuristic value of board B for player P.
     *  Higher is better for P. */
    private int staticEval(Side p, Board b) {
        return b.numOfSide(p) - b.numOfSide(p.opposite());
    }

    /** This holds an array of the best moves possible. */
    private ArrayList<Integer> bestMoves;
    /** This holds the number of the best move so far. */
    private int bestMoveSoFar;

    /** This holds the number of my best score that I found. */
    private int bestScore;

    /** This holds the current move that I made. */
    private int currMove;

    /** This holds a copy of the current game object. */
    private Game currGame;

    /** This holds a board object of the current board. */
    private Board currBoard;

    /** This holds the color of my current side. */
    private Side _color;
}
