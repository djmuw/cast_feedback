# CAST control reference #
---

Every CAST option consists of a `keyword` and one or more `value(s)` and can be provided within a line of a specific file or from the command line. The used keywords do only contain alphanumeric characters and no spaces, i.e. `MDheat`.

---

## Configuration file ##

The recognized file names for the CAST control file are:

- `CAST.txt` and
- `INPUTFILE`,

where the latter is only used if the current directory does not contain `CAST.txt`. Otherwise `INPUTFILE` is ignored.

The first contiguous string of alphanumeric characters in each of the files lines is treated as a `keyword` and the rest of the line as the respective `value`.

*Every option in CAST (except the `name` option) has a default value. Therefore the configuration file is facultative.*

Example:

    task MD

Sets the value for the option `task` to `MD`.

## Command line ##

Every option can also be adjusted by providing a command line switch in the form `-keyword=value` which overrules any entry with the same keyword in the configuration file.

This command

    cast.exe -task=MD

sets the value for the `task` option to `MD`, regardless of whether there was a respective line (setting the `task` option) inside the configuration file.

If `value` contains any whitespaces, CAST requires the command line to include quotation marks around the `value`.

#### Example: ####

    cast.exe -MOPACkey="PM6 MOZYME" 

*Note 1: If there is no configuration file, at least the `name` option must be provided via command line.*

## Shortlist of supported control sequences ##

- Default value is given between [ and ].
- Possible values or value constraints are given between ( and ), if applicable.
- The symbol `*` marks keywords which give rise to a certain program behvior by being present. They are absent by default. 

#### General ####

- [`verbosity`](#verbosity)&emsp;[`1`]<br />*Amount of information CAST prints to the console;*

#### File in- and output ####

- [`name`](#name)&emsp;[&nbsp;]&emsp;(valid path to file)<br />*Path to the coordinates structure input file;*

- [`inputtype`](#inputtype)&emsp;[`TINKER`]&emsp;(`TINKER`)<br />*Type of the coordinate structure input file;*

- [`outname`](#outname)&emsp;[[`name`](#name)+`_out`]&emsp;(valid filename prefix)<br />*Filename prefix used by CAST to create output files;*

#### Theory level / energy interfaces ####

- [`interface`](#interface)&emsp;[`OPLSAA`]&emsp;(`AMBER`, `AMOEBA`, `CHARMM22`, `MOPAC`, `OPLSAA`, `TERACHEM`)<br />*Interface used to determine molecular energies;*


- [`preinterface`](#preinterface)&emsp;[`OPLSAA`]&emsp;(`AMBER`, `AMOEBA`, `CHARMM22`, `MOPAC`, `OPLSAA`, `TERACHEM`)<br />*Interface used for preoptimization during global optimization;*
- [`paramfile`](#paramfile)&emsp;[`oplsaa.prm`]&emsp;(valid path to file)&emsp;{supported for any internal FF `interface`}<br />*Path to file with tinker like force field paramters;´*

- [`MOPACpath`](#MOPACpath)&emsp;[`"C:\Program Files\mopac\MOPAC2012.exe"`]&emsp;(valid path to file)&emsp;{supported for `interface` == `MOPAC`}<br />*Path to MOPAC executable*

- [`MOPACversion`](#MOPACversion)&emsp;[`2012MT`]&emsp;(`2012`, `2012MT`, `7` `AVOID_HB`)&emsp;{supported for `interface` == `MOPAC`}<br />*MOPAC program version*

- [`MOPACkey`](#MOPACkey)&emsp;[`PM7 MOZYME`]&emsp;([MOPAC-Keywords](http://openmopac.net/Manual/allkeys.html))&emsp;{supported for `interface` == `MOPAC`}<br />*Keywords controlling MOPAC options*

#### Program Task ####

- [`task`](#task) &emsp;[`SP`]&emsp;(`SP`, `GRAD`, `TS`, `LOCOPT`, `RMSD`, `MC`, `DIMER`, `MD`, `NEB`, `CENTER`, `STARTOPT`, `WRITE`, `RDF`, `INTERACTION`, `INTERNAL`, `ADJUST`, `UMBRELLA`, `FEP`, `PATHOPT`, `PATHSAMPLING`, `XYZ`, `PROFILE`, `GOSOL`, `REACTIONCOORDINATE`, `GRID`, `ALIGN`, `ENTROPY`, `PCAgen`, `PCAproc`)<br />*Desired calculation task*

#### Options for internal force field calculations ####

- [`cutoff`](#cutoff)&emsp;[`10000.0`]&emsp;(`0.0` < value)&emsp;{supported for any internal FF `interface`}<br />*Cutoff radius for non bonded interactions*

- [`switchdist`](#switchdist)&emsp;[[`cutoff`](#cutoff) - `4.0`]&emsp;(`0.0` < value < [`cutoff`](#cutoff))&emsp;{supported for any internal FF `interface`}<br />*Radius to start switching function to kick in; scales interactions smoothly to zero at cutoff radius*

#### Local optimization options ####

- [`BFGSgrad`](#BFGSgrad)&emsp;[`0.001`]&emsp;(`0.0` < value)<br />*L-BFGS convergence threshold*

- [`BFGSmaxstep`](#BFGSmaxstep)&emsp;[`10000`]&emsp;(`1` < integral value)<br />*Maximum number of steps in L-BFGS routine*

#### Startopt routines ####

- *General*

 - [`SOtype`](#SOtype)&emsp;[`1`]&emsp;(`0` - Ringsearch, `1` - Solvadd, `2` Ringsearch+Solvadd)<br />*Select the startopt routine to be performed*
 
  - [`SOstructures`](#SOstructures)&emsp;[`0`]&emsp;(`0` < integral value)<br />*Number of generated structures in Startopt* 

- *Evolutionary ringsearch*

 - [`RSpopulation`](#RSpopulation)&emsp;[`12`]&emsp;(`1` < integral value)<br />*Population; number of individual, propagated structures* 
 
 - [`RSgenerations`](#RSgenerations)&emsp;[`20`]&emsp;(`1` < integral value)<br />*Number of propagated generations* 
 
 - [`RSchance_close`](#RSchance_close)&emsp;[`0.75`]&emsp;(`0.0` < value < `1.0`)<br />*Chance for initial ring closure* 
 
 - [`RSbias_force`](#RSbias_force)&emsp;[`0.1`]&emsp;(`0.0` < value)<br />*Force constant for ring closure *

- *SolvAdd*

 - [`SAhb`](#SAhb)&emsp;[`1.80`]&emsp;(`0.0` < value)<br />*Minimal hydrogen bond length*
  
 - [`SAlimit`](#SAlimit)&emsp;[`0`]&emsp;(`0` < integral value)<br />*Number of water molecules* 
 
 - [`SAboundary`](#SAboundary)&emsp;[`0`]&emsp;(`0` - layer, `1` - sphere, `2` - box)<br />*Boundary type*
 
 - [`SAradius`](#SAradius)&emsp;[`10.0`]&emsp;(`0.0` < value)&emsp;{normative if `SAlimit` == `0`}<br />*Desired final solvation radius (layer, sphere) or box width*

 - [`SAopt`](#SAopt)&emsp;[`1`]&emsp;(`0` - none, `1` - each shell, `2` - final structure, `3` - 1 + 2)<br />*Intermediate or final local optimization*
 
 - [`SAfixinit`](#SAfixinit)&emsp;[`1`]&emsp;(`0` - no, `1` - yes)<br />*Fixation of initial structure (solvate) during local optimization*

 - [`SAtypes`](#SAtypes)&emsp;[`53 54`]&emsp;(two integral values)<br />*Forcefield type indices for oxygen and hydrogen* 
  

#### Global optimization routines ####

- *General*

 - [`Iterations`](#Iterations)&emsp;[`1000`]&emsp;(`0` < integral value)<br />*Maximum number of iterations*
 
 - [`Temperature`](#Temperature)&emsp;[`298.15`]&emsp;(`0.0` < value)<br />*Absolute temperature value for metropolis criterion*
 
 - [`Tempscale`](#Tempscale)&emsp;[`1.0`]&emsp;(`0.0` < value <= `1.0`)<br />*`Temperature` is multiplied by `Tempscale` for every accepted minimum*
 
 - [`GOmetrolocal`](#GOmetrolocal)&emsp;[`0`]&emsp;(`0` - no, `1` - yes)<br />*Use current local or current lowest ('global') minimum for metropolis criterion*

 - [`GOerange`](#GOerange)&emsp;[`0.0`]&emsp;(`0.0` <= value)<br />*Tracked energy range above lowest minimum (written once for every lowest ('global') minimum)*

 - [`GOstartopt`](#GOstartopt)&emsp;[`0`]&emsp;(`0` - no, `1` - yes)<br />*Applied configured startopt routine prior to global optimization*
 
 - [`GOfallback`](#GOfallback)&emsp;[`LAST_GLOBAL`]&emsp;(`LAST_GLOBAL`, `FITNESS_ROULETTE`)<br />*Fallback procedure if metropolis criterion rejects a step*
 
 - [`GOfallback_limit`](#GOfallback_limit)&emsp;[`200`]&emsp;(`0` < integral value)<br />*Maximum number of times a minimum will be used as starting point for a step*
 
 - Options for `GOfallback == FITNESS_ROULETTE`
 
    - [`GOfallback_fr_fit`](#GOfallback_fr_fit)&emsp;[`LINEAR`]&emsp;(`LINEAR`, `EXPONENTIAL`)<br />*Function type for fitness based roulette selection of next starting structure*
 
    - [`GOfallback_fr_minima`](#GOfallback_fr_minima)&emsp;[`10`]&emsp;(`0` < integral value)<br />*Minima included in roulette selection of next starting structure*

    - [`GOfallback_fr_bounds`](#GOfallback_fr_bounds)&emsp;[`0.5 1.0`]&emsp;(two values, first < second, `0.0=` < both )<br />*Lower and upper bound for fitness value of included minima*

- Metropolis Monte-Carlo method (MC[M])

 - [`MCmovetype`](#MCmovetype)&emsp;[`1`]&emsp;(`0` - biased dihedral, `1` - dihedral, `2` - cartesian, `3` - water)<br />*Random system distortion method*
 
 - [`MCminimization`](#MCminimization)&emsp;[`1`]&emsp;(`0` - no, `1` - yes)<br />*Perform local minimization after random move (`1` makes MCM)*
 
 - [`MCstep_size`](#MCstep_size)&emsp;[`2.0`]&emsp;(`0.0` < value)<br />*Cartesian step size limit for random Cartesian move*
 
 - [`MCmax_dihedral`](#MCmax_dihedral)&emsp;[`30.0`]&emsp;(`0.0` < value)<br />*Maximum random dihedral distortion*
 
 - [`MCmovefreq`](#MCmovefreq)&emsp;[`0.75`]&emsp;(`0.0` < value <= `1.0`)<br />*Probability value for geometric distribution selecting number of adjusted degrees of freedom*

- Tabu-Search method

 - [`TSdivers_iter`](#TSdivers_iter)&emsp;[`50`]&emsp;(`0` < integral value)<br />*Number of 'diversification' steps in MCM routine*

 - [`TSdivers_threshold`](#TSdivers_threshold)&emsp;[`25`]&emsp;(`0` < integral value)<br />*Maximum number of steps allowed to fail in TS routine before `TSdivers_iter` MCM diversification steps are applied*

 - [`TSmc_first`](#TSmc_first)&emsp;[`0`]&emsp;(`0` - no, `1` - yes)<br />*Perform `TSdivers_iter` MCM steps before TS starts*
 
 - [`TSdivers_limit`](#TSdivers_limit)&emsp;[`50`]&emsp;(`0` < integral value)<br />*Maximum number of diversifications*
