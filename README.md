using System;
using System.Linq.Expressions;
using System.Net.NetworkInformation;
using System.Runtime.CompilerServices;

namespace ConnectFour
{
    class Program
    {
        static void Main()
        {
            ConnectFourGame game = new ConnectFourGame();
            Console.WriteLine("Connect 4 Game Development Project: \n");
            game.Start();
        }
    }

    class ConnectFourGame
    {
        private const int DefaultRow = 6;
        private const int DefaultColumns = 7;

        private readonly char[,] board;
        private char cp;

        public ConnectFourGame()
        {
            board = new char[DefaultRow, DefaultColumns];
            cp = 'X';
            InitializeBoard();
        }

        private void InitializeBoard()
        {
            for (int row = 0; row < DefaultRow; row++)
            {
                for (int col = 0; col < DefaultColumns; col++)
                {
                    board[row, col] = ' ';
                }
            }
        }

        public void Start()
        {
            bool isGameOver = false;

            while (!isGameOver)
            {
                ShowBoard();

                int column = GetPlayerMove();
                bool checkmove = Moves(column);

                if (!checkmove)
                {
                    Console.WriteLine("Input is not correct, please eneter again");
                    continue;
                }

                if (winassign())
                {
                    ShowBoard();
                    Console.WriteLine($"It is connect 4. Player {cp} wins!");
                    isGameOver = true;
                }
                else if (fullboardcheck())
                {
                    ShowBoard();
                    Console.WriteLine("It's a draw!");
                    isGameOver = true;
                }
                else
                {
                    cp = (cp == 'X') ? 'O' : 'X';
                }
            }
        }

        private void RestartGame()
        {
            Console.WriteLine("Restart? Yes(1) No(0):");
            int y = Convert.ToInt32(Console.ReadLine());
            while (y != 0)
            {
                if (y == 1)
                {
                    Start();
                }
                else if (y == 0) Console.Clear();
                {
                    Environment.Exit(0);
                }
            }
        }

        private void ShowBoard()
        {

            // Console.Clear();

            for (int row = 0; row < DefaultRow; row++)
            {
                for (int col = 0; col < DefaultColumns; col++)
                {
                    Console.Write("# " + board[row, col] + " ");
                }

                Console.WriteLine("#");
                Console.WriteLine(" ");
            }
        }

        private int GetPlayerMove()
        {
            Console.WriteLine($"Player {cp}'s turn. Enter a column number (1-{DefaultColumns}):");

            while (true)
            {
                if (int.TryParse(Console.ReadLine(), out int column))
                {
                    if (column >= 1 && column <= DefaultColumns)
                    {
                        return column - 1;
                    }
                }

                Console.WriteLine("Input is not correct, please eneter again");
            }
        }

        private bool Moves(int column)
        {
            for (int row = DefaultRow - 1; row >= 0; row--)
            {
                if (board[row, column] == ' ')
                {
                    board[row, column] = cp;
                    return true;
                }
            }

            return false;
        }

        private bool winassign()
        {
            for (int row = 0; row < DefaultRow; row++)
            {
                for (int col = 0; col < DefaultColumns - 3; col++)
                {
                    if (board[row, col] != ' ' &&
                        board[row, col] == board[row, col + 1] &&
                        board[row, col] == board[row, col + 2] &&
                        board[row, col] == board[row, col + 3])
                    {
                        return true;
                    }
                }
            }

            for (int row = 0; row < DefaultRow - 3; row++)
            {
                for (int col = 0; col < DefaultColumns; col++)
                {
                    if (board[row, col] != ' ' &&
                        board[row, col] == board[row + 1, col] &&
                        board[row, col] == board[row + 2, col] &&
                        board[row, col] == board[row + 3, col])
                    {
                        return true;
                    }
                }
            }

            for (int row = 0; row < DefaultRow - 3; row++)
            {
                for (int col = 0; col < DefaultColumns - 3; col++)
                {
                    if (board[row, col] != ' ' &&
                        board[row, col] == board[row + 1, col + 1] &&
                        board[row, col] == board[row + 2, col + 2] &&
                        board[row, col] == board[row + 3, col + 3])
                    {
                        return true;
                    }
                }
            }

            for (int row = 0; row < DefaultRow - 3; row++)
            {
                for (int col = 3; col < DefaultColumns; col++)
                {
                    if (board[row, col] != ' ' &&
                        board[row, col] == board[row + 1, col - 1] &&
                        board[row, col] == board[row + 2, col - 2] &&
                        board[row, col] == board[row + 3, col - 3])
                    {
                        return true;
                    }
                }
            }

            return false;
        }

        private bool fullboardcheck()
        {
            for (int row = 0; row < DefaultRow; row++)
            {
                for (int col = 0; col < DefaultColumns; col++)
                {
                    if (board[row, col] == ' ')
                    {
                        return false;
                    }
                }
            }

            return true;
        }
    }
}
