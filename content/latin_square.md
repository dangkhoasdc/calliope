---
title: "Latin Squares"
date: 2018-02-01
---
This note draws primarily from the opening section of Knuth's *The Art of Computer Programming*, Volume 4A [@knuth_taocp4a]—a delightful mathematical playground where combinatorics meets practical computing.

# Definition

Given a square matrix of size $M$ where each element belongs to the set $\\{0, 1, \cdots, M-1\\}$, a *Latin square* is an arrangement where every element from this set appears exactly once in each row and each column.

### Examples

Consider 16 playing cards comprising four ranks (J, K, Q, A) and four suits ($\heartsuit, \diamondsuit, \clubsuit, \spadesuit$). The challenge: arrange them in a $4 \times 4$ grid such that every row and every column contains all four ranks and all four suits. This elegant puzzle appears in Jacques Ozanam's *Mathématiques et Physiques* (Paris, 1725).

![](https://i.imgur.com/opJooRI.png)

# Applications

When wading through abstract mathematics, finding real-world applications can be wonderfully motivating. As a pragmatically-minded computer scientist, I find particular satisfaction when elegant theory meets practical utility. Latin squares deliver on both fronts:

- [Sudoku](https://en.wikipedia.org/wiki/Sudoku): a variant of the Latin square is one of the most popular puzzles in the world.
- [Design of experiments](https://en.wikipedia.org/wiki/Blocking_(statistics)).
- [Error correcting codes](https://en.wikipedia.org/wiki/Latin_square#Error_correcting_codes).
- [Cryptography](http://elscorcho.50webs.com/latinsquares.pdf).

# A Mathematical War Story

In 1779, a puzzle reminiscent of our card arrangement captured the imagination of Leonhard Euler, one of history's most prolific mathematicians:

> Thirty-six officers of six different ranks, taken from six different
> regiments, want to march in a $6 \times 6$ formation so that each row and
> each column will contain one officer of each rank and one of each regiment.
> How can they do it?

Despite being nearly blind at the time, Euler attacked the puzzle with characteristic vigor, attempting to generalize it for matrices of any size $M = 1, 2, \ldots$. He conquered most cases but stumbled when $M \bmod 4 = 2$. Unable to find solutions for these stubborn values, he boldly conjectured that none existed.

Euler's puzzle differs subtly from our earlier definition. Let's decode it: assign $\heartsuit = 1, \diamondsuit = 2, \clubsuit = 3, \spadesuit = 4$, then separate ranks and suits into two distinct matrices. We obtain two Latin squares:

$$
\begin{bmatrix}
J& A& K& Q\newline
Q& K& A& J\newline
A& J& Q& K\newline
K& Q& J& A
\end{bmatrix}
$$

and

$$
\begin{bmatrix}
2& 1& 4& 3\newline
4& 3& 2& 1\newline
3& 4& 1& 2\newline
1& 2& 3& 4
\end{bmatrix}
$$

Fig. 1, when decomposed, becomes:

$$
\begin{bmatrix}
J2& A1& K4& Q3\newline
Q4& K3& A2& J1\newline
A3& J4& Q1& K2\newline
K1& Q2& J3& A4
\end{bmatrix}
$$

Two Latin squares are *orthogonal* if, when superimposing them element-wise, every resulting pair is unique. This reframes Euler's challenge: given one Latin square, can we construct another that is orthogonal to it?

The superimposed result is called a *Graeco-Latin square*—a name honoring the traditional practice of using Greek letters for one square and Latin letters for the other.

Proving—or disproving—Euler's conjecture consumed nearly 180 years of mathematical effort, eventually enlisting computational firepower. In 1957, Paige and Tompkins programmed SWAC to hunt for a counterexample at $M=10$. After ten fruitless hours, they surrendered.

Three years later, in 1960, Parker and colleagues delivered the knockout blow, proving that orthogonal Latin squares exist for all $M \geq 7$—demolishing Euler's conjecture. They also crafted a program for UNIVAC 1206 that cracked the $M=10$ case in under an hour—a task that had eluded SWAC:

```
0 1 2 3 4 5 6 7 8 9
1 8 3 2 5 4 7 6 9 0
2 9 5 6 3 0 8 4 7 1
3 7 0 9 8 6 1 5 2 4
4 6 7 5 2 9 0 8 1 3
5 0 9 4 7 8 3 1 6 2
6 5 4 7 1 3 2 9 0 8
7 4 1 8 0 2 9 3 5 6
8 3 6 0 9 1 5 2 4 7
9 2 8 1 6 7 4 0 3 5
```

One elegant solution, reproduced from TAoCP:

```
0 2 8 5 9 4 7 3 6 1
1 7 4 9 3 6 5 0 2 8
2 5 6 4 8 7 0 1 9 3
3 6 9 0 4 5 8 2 1 7
4 8 1 7 5 3 6 9 0 2
5 1 7 8 0 2 9 4 3 6
6 9 0 2 7 1 3 8 4 5
7 3 5 1 2 0 4 6 8 9
8 0 2 3 6 9 1 7 5 4
9 4 3 6 1 8 2 5 7 0
```

# Transversals

Parker utilized a concept called transversal which was previously proposed by Euler. A transversal is a way to choose a sequence of $M$ elements such that there is only 1 element in each row, each column, and each is unique in the sequence.

For example: 0859734216 means that we choose 0 in column 1, 8 in column 2, 5 in column 3, etc. Suppose that $k$ is the first item of a transversal; based on the following elements, we replace them by $k$. Given $M$ transversals of $k = 0, 1, \cdots, M-1$, we are able to construct an orthogonal matrix.

The algorithm below counts the number of transversals of a Latin square.

```cpp
typedef vector<vector<int>> mat;
class transversal {
private:
    int n;
    vector<bool> row;
    vector<bool> nums;
    mat input;
    int cnt;
public:
    transversal(const mat& in_): n(in_.size()), input(in_),
    row(n, false), nums(n, false), cnt(0) {
        assert(in_.size() == in_[0].size());
    }

    void print() { // debugging
        for (auto& rows: input) {
            for (auto& e: rows) cout << e << " ";
            cout << endl;
        }
    }

    int compute(int idx) {
        cnt = 0;
        fill(row.begin(), row.end(), false);
        fill(nums.begin(), nums.end(), false);
        nums[input[idx][0]] = true;
        row[idx] = true;
        comp(1);
        return cnt;
    }

private:
    void comp(int idx) { // idx = index of column
        if (idx == n) {
            cnt++;
            return;
        }

        for (int i = 0; i < n; ++i) {
            if (!row[i] && !nums[input[i][idx]] ) {
                row[i] = true;
                nums[input[i][idx]] = true;
                comp(idx+1);
                row[i] = false;
                nums[input[i][idx]] = false;
            }
        }

    }

};
```

From the above input, we get the output:

```
79 96 76 87 70 84 83 75 95 63
```

It is exactly the same as in the book. We can later modify the code to find all transversals given an input. After that, for every transversal $k$, we choose a set such that there are no duplicate elements, which becomes the output matrix. The details of the algorithm will be discussed in later posts about Dancing Links.
