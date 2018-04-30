Magic squares
=============

## Motivation

One of the biggest challenges of the neuromorphic engineering
community is to be able to build disruptive systems that can
efficiently perform complex tasks. Even though many tasks have been
tackled using traditional approaches based on digital electronics,
there is now the chance to achieve better results with less power
consumption and more robustness to noise of natural environments. The
Nengo software constitutes a powerful to explore the computational
capabilities of neural systems to perform complex cognitive tasks,
such as solving puzzles. In this work we chose the Magic Square and
Sudoku problems to explore the possibilities of neural architectures
using the NEF approach.

A magic square is an arrangement of squares in a table such that each
column and each row sums to some number. In this workgroup we chose to
implement a biologically plausible solver of the magic squares puzzle,
which consist of a magic square where some numbers are missing, using
the NEF approach and the Nengo software tools. There are several
variants of this game and several algorithms that can solve it. With
the NEF approach we aim at representing the problem in a vector space
that will be easily transferred to neural representations.

We decided to start with a variation of the magic squares that is
similar to the popular game Sudoku. The goal of the game we
implemented is to fill a 3 by 3 square with a set of 3 symbols such
that in each row and each column all the symbols of the vocabulary are
present.

## Implementation

Here is a brief sketch of the implementation in Nengo.

Each symbol of the vocabulary is represented by a vector in a high
dimensional space, and so are the representations every column and
every row with their content. The number of dimensions is chosen from
the trade-off between a high SNR (high dimensions) and a high speed of
computation (low dimensions). The number of dimensions will be
reflected into the number of neurons used for the representation of
the different quantities. The representation of the entire square is
split in row and columns for convenience and is achieved by binding
each symbol with the correspondent cell.

In the vector representation, it is easy to retrieve informations on
the content of the square by performing basic operations such as
convolutions and projections.

The system is composed by a vision module that provides the system
with the actual representation of the matrix. A saliency module
rapidly computes a saliency map of the matrix, selecting the most
constrained cell in the matrix, i.e. an empty cell whose row and
column contains the highest number of symbols. This is the cell for
which we can collect the most informations, so the system will most
likely be able to make a correct guess about which symbol to insert
there. Once the salient cell has been found, two estimator modules
will guess the symbol by looking at the content of the raw and column
of the salient cell. The guess is passed through a max operation so
that the highest guess is passed to the motor control who will modify
the content of the matrix.

## Code and howto

See `square_net.py`. In the upper part of the code there is the
initial matrix. The 0, 1, 2, 3 encode for blank, A, B, C. Modify that
to have a starting matrix. I didn't try too many initial matrices but
the engine seems to do a pretty reasonable job to solve a couple of
differeng starting conditions. The motor part is not active so you
have to manually adjust the matrix once the output tells you which
cell and which symbol to put. This is because there was a bug I didn't
have time to solve. It doesn't really matter, you can imagine that the
motor part is... a real motor activation!

Run with `./nengo-cl squares_net` from your `nengo` folder and look at
the printed output or the GUI plots. Press pause to easily read the
printed output.
