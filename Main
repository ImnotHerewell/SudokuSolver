from pysat.solvers import Glucose4
from itertools import combinations, product, repeat


def rcd(row, column, digit):
    global n
    assert row in range(1, n)
    assert column in range(1, n)
    assert digit in range(1, n)
    return pow(10, int(n * 0.5)) * row + pow(10, int(n * 0.5) - 1) * column + pow(10, int(n * 0.5) - 2) * digit


def eoo(literals):
    clauses = [[l] for l in literals]
    for pair in combinations(literals, 2):
        clauses.append([-l for l in pair])
    return clauses


def sudokucell():
    global n
    clauses = []
    for row, column in product(range(1, n + 1), repeat=2):
        clauses += eoo([rcd(row, column, digit)] for digit in range(1, n + 1))
    return clauses


def sudokurow():
    global n
    clauses = []
    for row, digit in product(range(1, n + 1), repeat=2):
        clauses += eoo([rcd(row, column, digit)] for column in range(1, n + 1))
    return clauses


def sudokucolumn():
    global n
    clauses = []
    for column, digit in product(range(1, n + 1), repeat=2):
        clauses += eoo([rcd(row, column, digit)] for row in range(1, n + 1))
    return clauses


def sudokublock():
    global n
    clauses = []
    for row, column in product([i for i in range(1, n + 1, int(n ** 0.5))], repeat=2):
        for digit in range(1, n + 1):
            clauses += eoo([rcd(row + a, column + b, digit) for (a, b) in product(range(int(n ** 0.5) + 1), repeat=2)])
    return clauses


def solve(puzzle):
    global n
    assert len(puzzle) == n
    assert all(len(row) == n for row in puzzle)
    clauses = []
    clauses += sudokucell()
    clauses += sudokurow()
    clauses += sudokucolumn()
    clauses += sudokublock()
    for row, column in product(range(1, n + 1), repeat=2):
        if puzzle[row - 1][column - 1] != "*":
            digit = int(puzzle[row - 1][column - 1])
            assert digit in range(1, n + 1)
            clauses += [[rcd(row, column, digit)]]
    sudoku = Glucose4(clauses)
    sudoku.solve()
    if isinstance(sudoku, str):
        print("No solution")
        exit(0)
    assert isinstance(sudoku, list)
    for row in range(1, n + 1):
        for column in range(1, n + 1):
            for digit in range(1, n + 1):
                if rcd(row, column, digit) in sudoku:
                    print(digit, end="")
        print()


n = int(input())
puzzle = []
for i in range(n):
    puzzle.append(str(input()))
solve(puzzle)
