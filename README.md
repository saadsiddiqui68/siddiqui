using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.IO;
namespace ConsoleApplication112
{
    class KT
    {
        public int[] cx = new int[] { 1, 1, 2, 2, -1, -1, -2, -2 };
        public int[] cy = new int[] { 2, -2, 1, -1, 2, -2, 1, -1 };


        private bool flag = false;
        private int boardSize;
        private int nx, ny;
        private int[,] solutionMatrix;


        public KT(int b_s)
        {
            boardSize = b_s;
            solutionMatrix = new int[boardSize, boardSize];
        }

        public bool Valid_Move(int x, int y)
        {
            if (x < 0 || x >= boardSize) return false;
            if (y < 0 || y >= boardSize) return false;
            if (solutionMatrix[x, y] != -1) return false;

            return true;

        }

        public bool nextMove(int x, int y)
        {
            int count = 0;
            int min_onward_move = boardSize + 1;
            for (int i = 0; i < 8; i++)
            {
                if (Valid_Move(x + cx[i], y + cy[i]))
                {

                    count = 0;
                    for (int j = 0; j < 8; j++)
                    {
                        if (Valid_Move(x + cx[i] + cx[j], y + cy[i] + cy[j]))
                        {
                            count++;
                        }
                    }

                    if (count <= min_onward_move)
                    {
                        min_onward_move = count;

                        nx = x + cx[i];
                        ny = y + cy[i];
                    }
                }
            }

            if (min_onward_move == boardSize + 1)
            {
                return false;
            }

            return true;
        }

        public void set_the_array()
        {
            for (int i = 0; i < boardSize; i++)
            {
                for (int j = 0; j < boardSize; j++)
                {
                    solutionMatrix[i, j] = -1;
                }
            }
        }

        public void Mark_Next_Move()
        {
            int step_Count = 0;
            Random r = new Random();
            int x = r.Next(1, boardSize), y = r.Next(1, boardSize);
            for (int i = 0; i < boardSize * boardSize; i++)
            {
                solutionMatrix[x, y] = step_Count;
                if (!this.nextMove(x, y) && step_Count < boardSize * boardSize - 1)
                {
                    Console.WriteLine("Solution does not exits");
                    this.flag = true;
                    return;
                }
                x = nx; y = ny;
                step_Count++;
            }

            this.print_Solution();     //after finding steps will print the solution

        }

        public void print_Solution()
        {
            for (int i = 0; i < boardSize; i++)
            {
                for (int j = 0; j < boardSize; j++)
                {
                    Console.Write(solutionMatrix[i, j] + "\t");
                }
                Console.WriteLine();
            }
        }

        public void print_Sol_to_File()
        {
            string path = @"D:\Downloads\Documents\file.txt";

            FileStream fs = new FileStream(path, FileMode.Append, FileAccess.Write);
            StreamWriter sw = new StreamWriter(fs);
            sw.WriteLine("Solution: ");
            if (!flag)
            {
                for (int i = 0; i < boardSize; i++)
                {
                    for (int j = 0; j < boardSize; j++)
                    {
                        sw.Write(solutionMatrix[i, j] + "\t");
                    }
                    sw.WriteLine();
                }
            }
            else
            {
                sw.WriteLine("Solution does not exits");
            }
            sw.Close();
        }

    }
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Enter board Size: ");
            int boardSize = int.Parse(Console.ReadLine());
            KT obj = new KT(boardSize);
            obj.set_the_array();
            obj.Mark_Next_Move();
            obj.print_Sol_to_File();
            Console.ReadLine();
        }
    }
}
