# circuitmacro
A wrapper for [CircuiTikz](https://github.com/circuitikz/circuitikz) with some customized symbols and support for three-phase circuits.

# Documentation
For all details, please refer to the [PDF document](https://github.com/giaccone/circuitmacro/blob/main/circuitmacro-guide.pdf) in this repository.

# Command list

```latex
\R* [<circuitikz options>]{<start coords>}{<end coords>}{<label>}
\L* [<circuitikz options>]{<start coords>}{<end coords>}{<label>}
\C* [<circuitikz options>]{<start coords>}{<end coords>}{<label>}
\B* [<circuitikz options>]{<start coords>}{<end coords>}{<label>}

\SwOpen* [<circuitikz options>]{<start coords>}{<end coords>}{<label>}
\SwClosed* [<circuitikz options>]{<start coords>}{<end coords>}{<label>}

\Short [<circuitikz options>]{<start coords>}{<end coords>}
\Open  [<circuitikz options>]{<start coords>}{<end coords>}

\Vs* [<circuitikz options>]{<start coords>}{<end coords>}{<label>}
\Is* [<circuitikz options>]{<start coords>}{<end coords>}{<label>}
\cVs* [<circuitikz options>]{<start coords>}{<end coords>}{<label>}
\cIs* [<circuitikz options>]{<start coords>}{<end coords>}{<label>}

\V*{<start coords>}{<end coords>}{<label>}
\I*{<start coords>}{<end coords>}{<label>}

\Nodes*{<coords>}...
\Terminal{<start coords>}{<end coords>}{<label>}
\Label[<tikz node options>]{<coords>}{<text>}

\PLoad[<top label>][<bottom label>]{<width>}{<height>}{<center coords>}

\YLoad* [<length scale>] [<component type>]{<coord1>}{<coord2>}{<offset>}{<label phase 3>}{<label phase 2>}{<label phase 1>}
\DLoad* [<length scale>] [<component type>]{<coord1>}{<coord2>}{<offset>}{<label branch 2>}{<label branch 1>}{<label branch 3>}

\triGen*  [<length scale>] [V|I]{<coord1>}{<coord2>}{<offset>}{<label phase A>}{<label phase B>}{<label phase C>}
\triLine* [<length scale>] [<component type>]{<coord1>}{<coord2>}{<offset>}{<label phase A>}{<label phase B>}{<label phase C>}
\triShort* [<circuitikz options>][<gap>]{<coord1>}{<coord2>}{<offset>}
```

Notes on the optionals (based on your definitions):

* Starred forms (`\Cmd*`) flip label/side.
* `<length scale>` defaults to 1 where present.
* <component type> defaults to B (generic) where present.
* `\triGen` source type defaults to V (voltage); I selects current sources.


# Example 1: first order circuit

```latex
\begin{circuitikz}
    \Vs{0,3}{0,0}{E_1}
    \R{0,3}{3,3}{R_1}
    \R{0,0}{3,0}{R_2}
    \Is[*-*]{3,0}{3,3}{A_1}
    \R[-*]{3,3}{6,0}{R_3}
    \R[-*]{3,3}{6,3}{R_4}
    \SwOpen{6,3}{9,3}{t=0}
    \C[v<=$v(t)$]{9,3}{9,0}{C_1}
    \Short{3,0}{9,0}
    \Short{6,0}{6,3}
\end{circuitikz}
```

<p align="center">
<img src="https://github.com/giaccone/circuitmacro/blob/main/img/example1.png?raw=true" width="800">
</p>

# Example 2: AC circuit

```latex
\begin{circuitikz}
    \Vs{0,6}{0,0}{\bar{E}_1}
    \Is[*-*]{3,3}{6,3}{\bar{A}_1}
    \B[*-, v<=$\bar{V}_1$]{3,6}{3,3}{\bar{Z}_1}
    \B[-*, i=$\bar{I}_2$]{3,3}{3,0}{\bar{Z}_2}
    \B[i^<=$\bar{I}_3$]{6,6}{6,3}{\bar{Z}_3}
    \B[v<=$\bar{V}_4$]{6,3}{6,0}{\bar{Z}_4}
    \Short{0,0}{6,0}
    \Short{0,6}{6,6}
\end{circuitikz} 
```

<p align="center">
<img src="https://github.com/giaccone/circuitmacro/blob/main/img/example2.png?raw=true" width="800">
</p>

# Example 3: Three-Phase circuit

```latex
\begin{circuitikz}
    % generator and line
    \triGen{0,4}{2,4}{1.8}{\bar{E}_1}{\bar{E}_2}{\bar{E}_3}
    \triLine{1}{2,4}{4.5,4}{1.8}{\bar{Z}_L}{}{}

    % load A
    \triShort{4.5, 4}{9, 4}{1.8}
    \PLoad[P_A][Q_A]{2}{4.5}{9,4}
    
    % load B
    \triShort[*-][1.8]{6.5, 5.8}{6.5, 1}{1.8}
    \PLoad[P_B][Q_B]{4.5}{2}{6.5,1}

    % current and voltages
    \I{8,5.8}{8.5,5.8}{\bar{I}_A}
    \I{4.7,3.5}{4.7,3}{\bar{I}_B}
    \V*{2,4}{2,5.8}{\bar{V}_g}
    \V{8.5,4}{8.5,5.8}{\bar{V}}
\end{circuitikz}
```

<p align="center">
<img src="https://github.com/giaccone/circuitmacro/blob/main/img/example3.png?raw=true" width="800">
</p>

