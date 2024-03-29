# Lotka-Volterra
Solution to Lota-Volterra prey-predator model using Fortran

Lotka Volterra prey-predator model, for an autocatalytic reaction. 

## File List
- [lvpp.f](/src/lvpp.f) - Main Routine 
- [derivs.f](/src/derivs.f) - ODE Sub Routine
- [rk4.f](/src/rk4.f) - 4th Order Runge-Kutta Method to solve a system of differential equations

**For the reaction**

$$A \rightarrow 2X$$

$$X + Y \rightarrow 2Y$$

$$Y \rightarrow B$$

Rate change for X and Y can be defined as:

$${{dX} \over {dt}} = \alpha[A][X] - \beta [X][Y]$$

$${{dY} \over {dt}} = \gamma[X][Y] - \delta [Y]$$

Using the 4th Order Runge-Kutta Method to solve the above system of differential equations:

$$k_1 = hf(t_n,x_n,y_n)$$

$$l_1 = hg(t_n,x_n,y_n)$$

$$k_2 = hf(t_n + {h \over 2}, x_n + {k_1 \over 2}, y_n + {l_1\over2} ) $$

$$l_2 = hf(t_n + {h \over 2}, x_n + {k_1 \over 2}, y_n + {l_1\over2} ) $$

$$k_3 = hf(t_n + {h \over 2}, x_n + {k_2 \over 2}, y_n + {l_2\over2} ) $$

$$l_3 = hf(t_n + {h \over 2}, x_n + {k_2 \over 2}, y_n + {l_2\over2} ) $$

$$k_4 = hf(t_n + h, x_n + k_3, y_n + l_3 ) $$

$$l_4 = hf(t_n + h, x_n + k_3, y_n + l_3 ) $$

$$t_{n+1} = t_n + h$$

$$x_{n+1} = x_n + {1\over6}(k_1 + 2k_2 + 2k_3 + k_4)$$

$$y_{n+1} = y_n + {1\over6}(l_1 + 2l_2 + 2l_3 + l_4)$$

Using the above algorithm, a Fortran program is [created](src) and values of $x_i$, $y_i$ are obtained from $x_0 - x_n$ and $y_0 - y_n$ at interval of time $h = 0.01$ for $n = 2000$. Taking the example values as follows:

$\alpha = 2.0$

$\beta = \gamma = 0.02$

$\delta = 1.0$

Initial values as:

$t_0 = 0.0$

$x_0 = 100.0$

$y_0 = 15.0$

**Output Result**

Output result file [lv-res.dat](/src/lv-res.dat) is included here for reference. 

# Plotting the Result
[gnuplot](http://www.gnuplot.info/) is used for plotting the result obtained from the simulation. After the result is obtained, gnuplot is set to output in svg/png. Sample plot is shown below. You can also check the svg files in [images](/images) directory. 

Settings for gnuplot to output svg files are as follows:

```
#!/usr/local/bin/gnuplot -persist

set terminal svg enhanced font 'Arial,11'
set output "rk.svg"
set title "[X]/[Y] vs Time Plot"
set xlabel "Time (t)"
set ylabel "[X]/[Y]"
set yrange [0:450]
plot "lv-res.dat" u (column(0)):2 w l lw 3 title "[X]","lv-res.dat" u (column(0)):3 w l lw 3 title "[Y]"

set terminal svg enhanced font 'Arial,11'
set title "[X] vs [Y] Plot"
set xlabel "[X]"
set ylabel "[Y]"
set output "rk2.svg"
plot "lv-res.dat" using 2:3 w l lw 3 title ""
```

This setting file is included as a shell script file [xy_svg.sh](/src/xy_svg.sh)

**[X],[Y] vs Time (t) plot**

<img src="/images/rk.png" width="100%" alt="[X],[Y] vs Time (t) plot">

**[X] vs [Y] Plot**

<img src="/images/rk2.png" width="100%" alt="[X] vs [Y] Plot">

# Notes
- The subroutine [rk4.f](/src/rk4.f) was adapted from [Numerical Recipes in Fortran 77, Second Edition (1992)](http://phys.uri.edu/nigh/NumRec/bookfpdf/f16-1.pdf)
