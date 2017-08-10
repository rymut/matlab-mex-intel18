# Matlab XML configuration for Intel Parallel Studio XE 2018 compiler
 
XML configuration files for compiling MATLAB MEX-files using
Intel Parallel Studio XE 2018 (Beta).

Tested on Windows 10 64-bit with MATLAB R2017 and Microsoft Visual Studio 2017
Professional with Intel Parallel Studio XE 2018 (Beta).

Configuration should also work fine with final release of Intel Parallel
Studio XE 2018.

## Install
First copy the files (intel_c_18_vs2017.xml, intel_cpp_18_vs2017.xml,
intel_fortran_18_vs2017.xml) to MATLABROOT\\bin\\win64\\mexopts (operation
might require administrator privileges).

## Configure
Next run `mex -setup C`, `mex -setup C++`, and `mex -setup FORTRAN` in MATLAB.  
Select 'Intel Parallel Studio XE 2017 with Microsoft Visual Studio 2017 (C)'
as C compiler, 'Intel Parallel Studio XE 2018 for C++ with Microsoft Visual
Studio 2017' as C++ compiler, and  'Intel Parallel Studio XE 2018 for Fortran
with Microsoft Visual Studio 2017' as FORTRAN compiler.

```
eval(['mex -setup:''' ...
  matlabroot '\bin\win64\mexopts\intel_c_18_vs2017.xml'' C'])
eval(['mex -setup:''' ...
  matlabroot '\bin\win64\mexopts\intel_cpp_18_vs2017.xml'' C++'])
eval(['mex -setup:''' ...
  matlabroot '\bin\win64\mexopts\intel_fortran_18_vs2017.xml'' FORTRAN'])
```

## Check if everything is working
Test the new settings with a sample MEX-file included with MATLAB:
```
% copy example file yprime.c
copyfile(fullfile(matlabroot,'extern','examples','mex','yprime.c'),'.','f')
copyfile(fullfile(matlabroot,'extern','examples','mex','yprimef.f'),'.','f')
copyfile(fullfile(matlabroot,'extern','examples','mex','yprimefg.f'),'.','f')
% make copy yprime.cpp of yprime.c file
copyfile('yprime.c', 'yprimecpp.cpp')

% compile command for C file using Intel C Compiler
mex -n -largeArrayDims yprime.c
% compile file
mex -v -largeArrayDims yprime.c
% test function correct ans is [2.0000, 8.9685, 4.0000, -1.0947]
T=1; Y=1:4; ansc = yprime(T,Y)

% clear mex
clear mex;

% compile command for CPP file using Intel C++ Compiler
mex -n -largeArrayDims yprimecpp.cpp
% compile file
mex -v -largeArrayDims yprimecpp.cpp
% test function correct ans is [2.0000, 8.9685, 4.0000, -1.0947]
T=1; Y=1:4; anscpp = yprimecpp(T,Y)


% compile command for CPP file using Intel FORTRAN Compiler
mex -n -largeArrayDims yprimef.f yprimefg.F
% compile file
mex -v -largeArrayDims yprimef.f yprimefg.F
% test function correct ans is [2.0000, 8.9685, 4.0000, -1.0947]
T=1; Y=1:4; ansfortran = yprimef(T,Y)
```
