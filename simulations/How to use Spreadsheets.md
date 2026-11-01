---
title: How to use Spreadsheets
---

# 1. Index

<!--toc:start-->

- [1. Index](#1-index)
- [2. How to use Spreadsheets](#2-how-to-use-spreadsheets)
  - [2.1. References and Commands](#21-references-and-commands)
  - [2.2. Example: drawing a function](#22-example-drawing-a-function)
  <!--toc:end-->

# 2. How to use Spreadsheets

Spreadsheets are a powerful tool for statistical analysis, data organization and
visualization.

We will use spreadsheets applying the same reasoning when programming, taking
advantages of a structured approach using references (`=A1`) and functions
(`=A1 + A3`, `=SUM(A1:C1)`).

This will be very useful to do a _first-glance_ analysis of our project results,
exporting the data as a `CSV` file from `OMNeT++` analysis file, and opening
it in a spreadsheet software like `LibreOffice Calc` or `Microsoft Excel`.

## 2.1. References and Commands

When we select a cell, we can reference the value of another cell by using **coordinates**.

Supposing we want to copy the value of the cell `A1` to the cell `B1`, in `B1` we
can write `=A1`.

We can perform operations with references, for example, if we want to sum the
values of the cells `A1` and `A2`, we can write in the cell `B1` the following formula:
`=A1 + A2`.

When we copy a cell containing a reference to another cell, the reference will
be updated according to the new position of the copied cell.

So, if we copy `B1` with `=A1` onto `B2`, the formula will be updated to `=A2`.

To lock the row/column of a reference, we can use the `$` symbol. For example,
if we wanted `=A1` to maintain the row we should have written `=A$1`.

## 2.2. Example: drawing a function

Suppose we want to draw the function $y = mx + c$ on a spreadsheet.

What we can do is set into two cells, like `B1` and `B2`, the values of `m`
and `c`, respectively.

Then, we can set the values of `x` in a column, for example, from `A3` to `A12`,
putting `1` in `A3`, and `=A3 + 1` in `A4`, and copying the formula down to `A12`.

Finally, we can set the formula for `y` in the corresponding cells of column `B`,
copying the formula down to `B12`.

To avoid shifting the references to the constants, the formula will be
`=B$1 * A3 + B$2`.

To plot the chart what we can do is select an empty cell and select from the menu:
`Insert > Chart`, and then select the type of chart we want to use.

In this case we will select a `Scatter` chart, and then:

- Create a new `Series`
- Select the X values range (`A3:A12`)
- Select the Y values range (`B3:B12`)

After that, we can customize the chart, adding titles and labels to the axes.

Every change to the data will be automatically reflected in the chart, so we can
change the values of `m` and `c` as we want.
