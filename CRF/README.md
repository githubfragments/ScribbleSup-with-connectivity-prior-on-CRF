# An ILP Solver for Multi-label MRFS with Connectivity Constraints
This code provides an implementation of the paper above (Shen, Ruobing, et al. "An ILP Solver for Multi-label MRFS with Connectivity Constraints." arXiv preprint arXiv:1712.06020 (2017), https://arxiv.org/pdf/1712.06020.pdf). Originally coded by Ruobing Shen and modified by Nicholas Bade.

The code does not only solve an ILP with Connectivity Constraints but has several models included (see below). Connectivity can be enforced for arbitrary labels. The coloring of the labels is adapted to fit to the Pascal VOC 2012 dataset. There is also an automated version which can be used together with a bash script to run over several images. A logfile and the output images will be saved automatically.


## Dependencies:
1) Gurobi (http://www.gurobi.com/)
2) Boost (http://www.boost.org/)
3) OpenCV (https://opencv.org/)

## Building the code:
Please edit the Makefile appropriately, and specify the directories of Boost and Gurobi.

The code uses C++ 2014 and needs at least G++-4.9. However, Gurobi officially only supports G++-4.8. But apparently it works with G++-4.9 as well but not with G++-5. Therefore, G++-4.9 is the only supported compiler!

You have two compiling options:

You can either use "make manual" to compile a more manual version of the code. This has the option to manually draw scribbles instead of loading them from file, also output is shown in the terminal as well.
Usage: ./main_manual image.jpg superpixel_file scribble_file brushwidth timelimit lambda scribble_option scribble_file(optional)
If you choose to manually select scribbles, you can use the left mouse button to enforce connectivity. Use the right mouse button if you do not want to enforce connectivity (e.g. background label). You can use scribble_option "2" to load from scribbles from file AND additionally draw new scribbles.


Use "make auto" for a more automated version. There is no check whether the call of main is correct. There is no output to the terminal. This might be a proper option if you need to run several images. The easiest way to do so is to use a bash script. There is an example included (see below).
Usage: ./main_auto image.jpg superpixel.csv scribble.txt brushwidth timelimit lambda

## Superpixel requirements:
A superpixel file can be generated using e.g.:
https://github.com/davidstutz/superpixel-benchmark
The superpixel csv file gives the superpixel ID for every pixel starting at 0.

## Typical options:
Brushwidth between "1" and "3". Lambda: "0.2" Time limit: "10"s.
One superpixel must only have one scribble/label on it. Otherwise the l0 heuristic will fail with a segfault. Occasionally, the l0 heuristic fails to converge or produces some arbitrary segfaults. Retry with reduced brushwidth. There is no check whether the provided input files exist. If they don't, the program will fail silently.

## Models available:
You have to define the models to run in the main files! You have the following options.
"0": ILP-C
"1": ILP-PC (automatically select when there is NO unconnected label)
"2": LP-PC
"3": ILP-P
"4": ILP-PCB (automatically added when there is at least one unconnected label (e.g. background))
The l0-heuristic will always be called at first.

## Example:
There are some example images, superpixel seg's and scribbles from the Pascal VOC set in the example sub folder. To run them automatically, "make auto", "chmod +x example.sh" and then execute the script "./example.sh".
